# docker

*一句话就是docker解决了运行环境不一致所带来的问题*

> 其实偏向于运维的工具，简单了解

## docker 简介

> Docker（中文名为“容器”）是一个开源平台，允许开发人员在软件容器内自动化部署和管理应用程序。容器提供了一个轻量级、隔离的环境，用于运行应用程序及其依赖项，使得软件可以更容易地打包和分发到不同的系统中。
>
> 以下是与Docker相关的关键概念：
>
> 1. **容器**：容器是轻量级且隔离的环境，用于打包应用程序及其依赖项。它们提供了在不同系统上一致的行为，确保应用程序在不同的基础设施上运行一致。
>
> 2. **镜像**：Docker镜像是容器的构建块。它们是只读模板，包含运行应用程序所需的一切，包括代码、运行时、库和系统工具。镜像是从Dockerfile创建的，Dockerfile指定了构建镜像的步骤。
>
> 3. **Dockerfile**：Dockerfile是一个文本文件，包含构建Docker镜像的指令。它定义了基础镜像，设置环境，安装依赖项，将文件复制到镜像中，并指定容器启动时要运行的命令。
>
> 4. **Docker Hub**：Docker Hub是一个公共注册表，托管各种Docker镜像。它作为一个中央仓库，供您查找和与社区共享Docker镜像。Docker Hub还允许您创建私有仓库来存储自己的镜像。
>
> 5. **Docker Compose**：Docker Compose是一个用于定义和运行多容器Docker应用程序的工具。它允许您在一个YAML文件中定义应用程序所需的服务、网络和卷。通过一个简单的命令，您可以启动和停止整个应用程序堆栈。
>
> 6. **Docker Swarm**：Docker Swarm是Docker的原生集群和编排解决方案。它允许您创建和管理一组Docker节点，将它们转化为一个单一的虚拟Docker主机。Swarm提供了服务扩展、滚动更新和负载均衡等功能。
>
> 7. **Docker CLI**：Docker提供了一个命令行界面（CLI），允许您与Docker守护程序进行交互和管理Docker资源。CLI提供了构建镜像、运行容器、管理网络等操作的命令。
>
> Docker因其简化了在不同环境中打包、分发和运行应用程序的过程而变得流行起来。它提倡一致性和可重复性，使得可靠地部署应用程序变得更加容易。



## docker 安装

### 安装过程

+ 安装依赖

```bash
yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

+ 设置仓库

```bash
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

+ 安装docker

```
yum install docker-ce docker-ce-cli containerd.io
```

+ 启动并且加入启动项

```
systemctl start docker

systemctl enable docker
```

+ 验证

```
[root@glnode07 docker]# docker version
```

```
[root@glnode07 docker]# docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
719385e32844: Pull complete 
Digest: sha256:88ec0acaa3ec199d3b7eaf73588f4518c25f9d34f58ce9a0df68429c5af48e8d
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/

```



### 关于在内网环境中的代理配置

##### 背景

可能在很多单位、公司，只有特定服务器可以上网，需要配置代理才可以访问外网

##### 问题

即使配置好了加速镜像，执行 hello-world 时也依然会出现下面错误，并且我的这台服务器已经配置了代理，可以正常访问外网，但是docker需要单独配置代理。

![image-20231030180932580](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310301809658.png)

> 如果您的本机配置了代理，但是 Docker 客户端没有配置代理，那么 Docker 容器默认情况下将无法直接访问外部互联网。Docker 容器的网络设置是独立于主机的，不会继承主机的代理配置。
>

##### docker 代理配置

"docker pull" 命令是由 dockerd 守护进程执行。而 dockerd 守护进程是由 systemd 管理。因此，如果需要在执行 "docker pull" 命令时使用 HTTP/HTTPS 代理，需要通过 systemd 配置。

- 为 dockerd 创建配置文件夹。

```
sudo mkdir -p /etc/systemd/system/docker.service.d
```

- 为 dockerd 创建 HTTP/HTTPS 网络代理的配置文件，文件路径是 /etc/systemd/system/docker.service.d/http-proxy.conf 。并在该文件中添加相关环境变量。

```bash
[Service]

Environment="HTTP_PROXY=http://172.16.4.71:3128/"

Environment="HTTPS_PROXY=http://172.16.4.71:3128/"

Environment="NO_PROXY=localhost,127.0.0.1,.example.com"
```

- 刷新配置并重启 docker 服务。

```
sudo systemctl daemon-reload

sudo systemctl restart docker
```



## docker 命令

**一图通**

![image-20231030220107349](https://raw.githubusercontent.com/Quinlan7/pic_cloud/main/img/202310302201028.png)



### 命令介绍

| **命令**       | **说明**                       | **文档地址**                                                 |
| :------------- | :----------------------------- | :----------------------------------------------------------- |
| docker pull    | 拉取镜像                       | [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) |
| docker push    | 推送镜像到DockerRegistry       | [docker push](https://docs.docker.com/engine/reference/commandline/push/) |
| docker images  | 查看本地镜像                   | [docker images](https://docs.docker.com/engine/reference/commandline/images/) |
| docker rmi     | 删除本地镜像                   | [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/) |
| docker run     | 创建并运行容器（不能重复创建） | [docker run](https://docs.docker.com/engine/reference/commandline/run/) |
| docker stop    | 停止指定容器                   | [docker stop](https://docs.docker.com/engine/reference/commandline/stop/) |
| docker start   | 启动指定容器                   | [docker start](https://docs.docker.com/engine/reference/commandline/start/) |
| docker restart | 重新启动容器                   | [docker restart](https://docs.docker.com/engine/reference/commandline/restart/) |
| docker rm      | 删除指定容器                   | [docs.docker.com](https://docs.docker.com/engine/reference/commandline/rm/) |
| docker ps      | 查看容器                       | [docker ps](https://docs.docker.com/engine/reference/commandline/ps/) |
| docker logs    | 查看容器运行日志               | [docker logs](https://docs.docker.com/engine/reference/commandline/logs/) |
| docker exec    | 进入容器                       | [docker exec](https://docs.docker.com/engine/reference/commandline/exec/) |
| docker save    | 保存镜像到本地压缩文件         | [docker save](https://docs.docker.com/engine/reference/commandline/save/) |
| docker load    | 加载本地压缩文件到镜像         | [docker load](https://docs.docker.com/engine/reference/commandline/load/) |
| docker inspect | 查看容器详细信息               | [docker inspect](https://docs.docker.com/engine/reference/commandline/inspect/) |



[黑马docker](https://b11et3un53m.feishu.cn/wiki/MWQIw4Zvhil0I5ktPHwcoqZdnec)











### 参考

[docker入门(利用docker部署web应用)](https://blog.csdn.net/q610376681/article/details/90483576?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522169836669616800227447553%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=169836669616800227447553&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-90483576-null-null.142^v96^control&utm_term=docker&spm=1018.2226.3001.4187)

[Docker从入门到实践](https://yeasy.gitbook.io/docker_practice/advanced_network/http_https_proxy)——very good
