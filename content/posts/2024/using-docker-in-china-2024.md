---
title: "如何解决网络问题实现正常使用 Docker"
description: "解决 Docker Engine 无法更新，Docker Hub 镜像无法拉取的问题。"
date: 2024-06-12T13:29:00+08:00
lastmod: 2024-07-30
tags: [devtools, devops, docker]
categories: Blog
featured_image: "/posts/2024/using-docker-in-china-2024/using-docker-in-china-2024.jpg"
images: ["/posts/2024/using-docker-in-china-2024/ogc-using-docker.jpg"]
---

This post is to introduce a solution to the network issues of Docker engine and Docker Hub that may only happen in China.

## 更新 apt 源解决 Docker Engine 的安装和更新问题

我是根据[官方安装文档](https://docs.docker.com/engine/install/ubuntu/#installation-methods)安装的 Docker，这种方法会直接从 `download.docker.com` 下载软件包，最近由于网络问题，已经无法更新了：

```sh
W: Failed to fetch https://download.docker.com/linux/ubuntu/dists/focal/InRelease
Cannot initiate the connection to download.docker.com:443 (2a03:2880:f102:183:face:b00c:0:25de).
- connect (101: Network is unreachable)
Could not connect to download.docker.com:443 (104.244.46.185), connection timed out
```

### 更换为可访问的 `apt` 源

通过替换为国内的一些课访问的 `apt` 源，这样就可以更新 Docker Engine 了。

> 请注意这也取决于源的更新频率，某些源的软件包可能不是最新的。

我这里通过阿里云和腾讯云两种方式作为演示。

#### 阿里云

首先运行如下命令更新 GPG：

```sh
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

然后更新 apt 源列表文件：

```sh
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://mirrors.aliyun.com/docker-ce/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

#### 腾讯云

更新 GPG：

```sh
curl -fsSL https://mirrors.cloud.tencent.com/docker-ce/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

更新 apt 源：

```sh
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://mirrors.cloud.tencent.com/docker-ce/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

#### 更新 Docker Engine

现在我们就可以执行更新命令了：

```sh
sudo apt-get update
```

## 配置 Docker Daemon 解决 Docker Hub 镜像拉取问题

要正常拉取 Docker Hub 上的镜像，可以在运行 Docker 的机器上配置 `/etc/docker/daemon.json`文件。配置节是`registry-mirrors`，表示通过镜像加速服务访问 Docker Hub。请参考[官方文档](https://docs.docker.com/config/daemon/)。

我这里也使用阿里云和腾讯云作为演示。

### 使用阿里云容器镜像服务

如果你是使用的阿里云，可以在[容器镜像服务 ACR](https://cr.console.aliyun.com/?spm=5176.8351553.categories-n-products.dacr.3d2a1991DGZERv) 的**镜像工具**里找到[镜像加速器](https://cr.console.aliyun.com/cn-shanghai/instances/mirrors)。

> 注意：这个地址是私有的，不要公开出去。

```json
{
  "registry-mirrors": ["https://xxx.mirror.aliyuncs.com"]
}
```

### 使用腾讯云

### 使用腾讯云

腾讯云可以[参考文档](https://cloud.tencent.com/document/product/1207/45596)直接修改配置：

```json
{
   "registry-mirrors": ["https://mirror.ccs.tencentyun.com"]
}
```

修改保存后重新启动 Docker 服务。

```sh
sudo systemctl restart docker
```

然后可以使用 `docker info` 命令来查看是否有配置的镜像地址。

### Enjoy!

