---
title: "如何在服务器上正常使用 Docker"
description: "解决 Docker 无法更新，镜像无法拉取的问题。"
date: 2024-06-12T13:29:00+08:00
lastmod: 2024-06-12
tags: [devtools, devops, docker]
categories: Blog
featured_image: "/posts/2024/using-docker-in-china-2024/using-docker-in-china-2024.jpg"
images: ["/posts/2024/using-docker-in-china-2024/ogc-using-docker.jpg"]
---

This post is to introduce a solution to the Docker issues that may only happen in China.

## 重新安装 Docker Engine

> 重要更新：不需要删除已安装的 Docker，只需要更换源即可。

我个人更喜欢用[官方来源](https://docs.docker.com/engine/install/ubuntu/)安装 Docker，但最近由于镜像和网络的问题，这种方法已经无法更新了：

```sh
W: Failed to fetch https://download.docker.com/linux/ubuntu/dists/focal/InRelease
Cannot initiate the connection to download.docker.com:443 (2a03:2880:f102:183:face:b00c:0:25de).
- connect (101: Network is unreachable)
Could not connect to download.docker.com:443 (104.244.46.185), connection timed out
```

### ~~卸载 Docker Engine~~

请直接跳到换源。

~~首先，需要把现在的 Docker 相关软件包卸载。~~

> 注意：这会丢失所有镜像和容器。记得先备份。

按照[官方文档](https://docs.docker.com/engine/install/ubuntu/#uninstall-docker-engine)，使用如下命令即可：

```sh
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
```

还有删掉数据：

```sh
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

### 换源 `apt` 安装 Docker

这里通过国内的 `apt` 源进行安装。

#### 阿里云

需要将官方的命令行部分更换为如下所示：

```sh
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

然后写入源列表：

```sh
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://mirrors.aliyun.com/docker-ce/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

#### 腾讯云

```sh
curl -fsSL https://mirrors.cloud.tencent.com/docker-ce/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

```sh
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://mirrors.cloud.tencent.com/docker-ce/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

再然后，就可以执行安装命令了：

```sh
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

至此，我们 Ubuntu 服务器上的 Docker Engine 就可以正常更新了。

## 配置 Docker daemon

要正常拉取镜像，还需要配置一下 `daemon.json`文件。

可以参考[官方文档](https://docs.docker.com/config/daemon/)。配置节是`registry-mirrors`。

### 使用阿里云容器镜像服务

如果你是使用的阿里云，可以在[容器镜像服务 ACR](https://cr.console.aliyun.com/?spm=5176.8351553.categories-n-products.dacr.3d2a1991DGZERv) 的**镜像工具**里找到[镜像加速器](https://cr.console.aliyun.com/cn-shanghai/instances/mirrors)。

> 注意：这个地址是私有的，不要公开出去。

```sh
root@aliecs2:~# tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://xxx.mirror.aliyuncs.com"]
}
EOF
```

### 使用腾讯云

腾讯云可以[参考文档](https://cloud.tencent.com/document/product/1207/45596)直接修改配置：

```json
{
   "registry-mirrors": ["https://mirror.ccs.tencentyun.com"]
}
```

修改后重新启动 Docker 服务。

然后可以使用 `docker info` 命令来查看是否有配置的镜像地址。

Enjoy!

