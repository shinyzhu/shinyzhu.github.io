---
title: "Self Hosting A Docker Registry To Pull Images From Docker Hub"
description: "Using pull through cache of Registry"
date: 2024-07-28T23:29:00+08:00
lastmod: 2024-07-31
tags: [devtools, devops, docker]
categories: Blog
featured_image: "/posts/2024/self-hosting-docker-registry/docker-registry-banner.jpg"
images: ["/posts/2024/self-hosting-docker-registry/og-self-hosting-docker-registry.jpg"]
---

This is a follow-up post to the [Using Docker in China 2024](../using-docker-in-china-2024/). To provide a private cache for Docker Hub with [Registry](https://distribution.github.io/distribution/).

## The Problem

I've updated the `registry-mirrors` to aliyun/tencent mirrors. But rencently I can't pull new images. The commands are successfully run but they returned 'old' `latest` images like built 2 years ago.

```sh
root@vm:~# docker pull busybox
Using default tag: latest
latest: Pulling from library/busybox
5cc84ad355aa: Pull complete 
Digest: sha256:5acba83a746c7608ed544dc1533b87c737a0b0fb730301639a0179f9344b1678
Status: Downloaded newer image for busybox:latest
docker.io/library/busybox:latest

root@vm:~# docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
busybox      latest    beae173ccac6   2 years ago   1.24MB

root@vm:~# docker inspect busybox
[
    {
        "Id": "sha256:beae173ccac6ad749f76713cf4440fe3d21d1043fe616dfbe30775815d1d0f6a",
        "RepoTags": [
            "busybox:latest"
        ],
        "RepoDigests": [
            "busybox@sha256:5acba83a746c7608ed544dc1533b87c737a0b0fb730301639a0179f9344b1678"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2021-12-30T19:19:41.006954958Z",
        "DockerVersion": "20.10.7",
#...
]
```

The real `latest` is [built](https://hub.docker.com/layers/library/busybox/latest/images/sha256-50aa4698fa6262977cff89181b2664b99d8a56dbca847bf62f2ef04854597cf8?context=explore) just 2 months ago.

The problem is that the mirror cached old images but didn't pull new ones. Or some mirrors are not providing cache services any more.

## The Solution

[Registry](https://hub.docker.com/_/registry) or [Distribution Registry](https://distribution.github.io/distribution/) is a service used for storing and distributing container images. I just need to run Registry as a [pull through cache](https://distribution.github.io/distribution/recipes/mirror/).

### Run Registry

I use `docker compose` to run Registry. Here is the file `docker-compose.yaml`:

```yaml
services:
  registry:
    image: registry
    restart: always
    ports:
      - 15000:5000
    environment:
      REGISTRY_PROXY_REMOTEURL: https://registry-1.docker.io
    volumes:
      - ./data:/var/lib/registry
```

Then use `docker compose -f docker-compose.yaml up -d` to run. 

And `docker logs --follow container_name` to tail the logs.

### Nginx Site

And I need to access it remotely so I set up an Nginx with SSL to handle the inbound requests.

File: `/etc/nginx/sites-avaiable/registry.conf`

Content:

```
server {
    listen 80;

    server_name registry.example.dev;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    listen [::]:443 ssl;

    server_name registry.example.dev;

    ssl_certificate     /etc/letsencrypt/live/registry.example.dev/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/registry.example.dev/privkey.pem;
    ssl_protocols       TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
    ssl_ciphers         HIGH:!aNULL:!MD5;

    location / {
        proxy_pass http://localhost:15000;
        include proxy_params;
    }
}
```

Then create a link to `sites-enabled`:

```sh
sudo ln -s /etc/nginx/sites-available/registry.conf /etc/nginx/sites-enabled/
```

Then reload and restart nginx:

```
sudo nginx -s reload
sudo systemctl restart nginx
```

### Docker Daemon Config

To use the registry. I updated the file `/etc/docker/daemon.json` :

```json
{
    "registry-mirrors": [
      "https://registry.example.dev"
    ]
}
```

Then you can pull images from Docker hub via the custom registry.

## To Do

### Authentication

How to implement authentication to auth users? 

Tried [htpasswd](https://distribution.github.io/distribution/about/configuration/#auth) of registry but cannot directly run `docker pull busybox`.

## Resources

- [Docker Official Registry Image](https://hub.docker.com/_/registry)
- Doc of [Distribution Registry](https://distribution.github.io/distribution/)
- [突破 DockerHub 限制，全镜像加速服务](https://moelove.info/2020/09/20/%E7%AA%81%E7%A0%B4-DockerHub-%E9%99%90%E5%88%B6%E5%85%A8%E9%95%9C%E5%83%8F%E5%8A%A0%E9%80%9F%E6%9C%8D%E5%8A%A1/) by [张晋涛](https://moelove.info/)
