---
title: "Using GitHub Action and Systemd to Deploy Express App"
description: "Notes on setting up continuous deployment of a REST API service."
date: 2023-12-15T18:09:00+08:00
lastmod: 2023-12-21
tags: [github, linux, nodejs]
categories: Blog
featured_image: "/posts/2023/using-github-action-and-systemd-to-deploy-express-app/using-github-action-head.jpg"
images: ["/posts/2023/using-github-action-and-systemd-to-deploy-express-app/og-card.jpg"]
---

This post is to take notes about how to automatically continuous deploy an Express app on GitHub to a virtual machine host by ssh and GitHub Action.

Resources I have:

1. A virtual machine with SSH enabled. Hosting the API service.
2. The API Service is an Express app whose code is managed by GitHub.
3. GitHub Action for automatic CI/CD.

## The API Service (ExpressJS)

I'm building the app on the WeChat Mini Program platform. It's the front-end of an app that requires an API service backend.

Here I'm using [Express](https://expressjs.com/) to do that. So I can just use JavaScript to write the full stack of the whole app.

I initiated the project with [Express application generator](https://expressjs.com/en/starter/generator.html). It will use file `bin/www` to launch the server.

File: `package.json`

```json
"scripts": {
  "start": "node ./bin/www"
},
```

I also use `.env` to store secrets and configurations.

## The Hosting (Virtual Machine)

[TencentCloud Lighthouse](https://cloud.tencent.com/product/lighthouse) is an affordable hosting especially during promotion seasons. I'm using one comes with 2 cores CPU and 4GB RAM. And built with Ubuntu20.04-Docker20 image.

The API service is hosted as a node service. I don't want to run it in docker.

And I'm using Nginx + SSL to handle requests and as the reverse proxy to the API service.

### Long Running Express App as Systemd Service

To [run the Express app as a service](https://expressjs.com/en/advanced/pm.html). I use Systemd to do that. (Not pm2 here).

Here is the service file: `/etc/systemd/system/ibkapi.service`

```ini
[Unit]
Description=IBKAPI Server

[Service]
ExecStart=/opt/node/bin/node /var/www/api/bin/www
# Required on some systems
WorkingDirectory=/var/www/api
Restart=always
# Restart service after 10 seconds if node service crashes
RestartSec=10
# Output to syslog
StandardOutput=syslog
StandardError=syslog
SyslogIdentifier=ibkapi
#User=<alternate user>
#Group=<alternate group>
EnvironmentFile=/var/www/.env.production

[Install]
WantedBy=multi-user.target
```

### Node Production Environment Variables

As you can see above. I put a `.env.production` manually into the root `www` folder. If I have a new config to add. I need to `ssh` to the server and edit them in place.

> / ? \
>
> So what is your practice for maintaining `.env` in production? Comments are welcome.
>

## Continuous Deploying by GitHub Action

Here is the workflow file:

```yaml
name: Deployment
on:
  push:
    branches:
      - main
  pull_request:
    branches: 
      - main
jobs:
  deployment:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - uses: actions/checkout@v4

      - name: Install SSH Key
        uses: shimataro/ssh-key-action@v2
        with:
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          known_hosts: never_know

      - name: Add Known Hosts
        run: ssh-keyscan -p ${{ secrets.SSH_PORT }} -H ${{ secrets.SSH_HOST }} >> ~/.ssh/known_hosts
            
      - name: Deploy With Rsync
        run: rsync -avz --delete -e "ssh -p ${{ secrets.SSH_PORT }}" ./ ${{ secrets.SSH_USER }}@${{ secrets.SSH_HOST }}:${{ secrets.PUB_PATH }}

      - name: Run Commands
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.SSH_HOST }}
          username: ${{ secrets.SSH_USER }}
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          port: ${{ secrets.SSH_PORT }}
          script: |
            cd ${{ secrets.PUB_PATH }}
            npm ci
            sudo systemctl restart ibkapi
```

I followed the tutorial [here](https://zellwk.com/blog/github-actions-deploy/). First I need to add credentials for logging into the host. Then use `rsync` to send files. And run some commands in the host. Restart the service then the job is done.

I failed several times. Here is why:

I installed `node` mannually to `/opt/node` and set `PATH` in file `.bashrc`. It works for `ssh` login but failed to set path correctly when running from a worker.

The solution is simple:

```sh
sudo ln -s /opt/node/bin/node /usr/bin/node
sudo ln -s /opt/node/bin/npm /usr/bin/npm
sudo ln -s /opt/node/bin/pm2 /usr/bin/pm2
```

> / ? \
>
> What is your approach to manage path?

## Conclusion (Maybe)

This is my setup for deploying and keeping it running of an Express app.

It makes me feeling good for making code changes.

## Background Story

My wife is running a bakery shop. It has been 7 years since 2016.

It's hard to believe that we survived the COVID-19 and lockdowns. But the business is keeping going down.

It depends on the online booking service e.g. built on [WeChat Mini Program](https://mp.weixin.qq.com/cgi-bin/wx?token=&lang=en_US) which is running just inside WeChat without installing another app. And channels such as [MeiTuan](https://www.meituan.com/en-US/about-us)/Dianping we don't spend much time on them so almost no orders there.

Back to the WeChat Mini Program. We have purchased 2 (or 3) apps from different vendors. For online ordering for cakes and breads and managing membership. Why did it come out like this?

1. Business processes are not clearly defined. Cakes are pre-order. Breads are date limited and so on.
2. Services (or features) from vendors are complex and not easy to use. And some providers give very poor service experience. 

As one of them are expiring. So we decided building the app on our own.

Let me make long story short here. The notes for deploying the API service and setup the CI/CD in this post is for that I can focus on designing and implementing the business logic. That is extremely important for a **one-person team**. (Can one person be called a team? And, **full stack** is not about the **{front end + business layer + data}** but **{product + dev + ops}** :) - another story)
