## Docker

#### 简介

Docker 是一个开源的应用容器引擎，基于 [Go 语言](https://www.runoob.com/go/go-tutorial.html) 并遵从 Apache2.0 协议开源。

Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

容器是完全使用沙箱机制，相互之间不会有任何接口（类似 iPhone 的 app），更重要的是容器性能开销极低。



#### 安装

**CentOS Docker 安装**

```shell
# yum 安装 docker
yum install docker
# 查看版本
docker -v
```

**Docker 设置国内镜像源**

```shell
# 编辑配置文件
vim /etc/docker/daemon.json
# 增加以下内容
{
  "registry-mirrors": [
    "https://registry.docker-cn.com"
  ]
}
```



## Docker-Compose

#### 简介

Docker-Compose 项目是 Docker 官方的开源项目，负责实现对 Docker 容器集群的快速编排。

最小的 Docker Compose 应用程序由三个组件组成：

1. 要构建的每个容器镜像的 Dockerfile。
2. Docker Compose 将用于从这些镜像启动容器并配置其服务的 YAML 文件 docker-compose.yml。
3. 构成应用程序本身的文件。



#### 安装

```shell
# 执行如下命令安装，安装完后 docker-compose 会被安装到 /usr/bin 目录下
curl -L https://github.com/docker/compose/releases/download/1.24.0-rc1/docker-compose-`uname -s`-`uname -m` -o /usr/bin/docker-compose
# 设置 docker-compose 可执行
chmod +x /usr/bin/docker-compose
# 查看 docker-compose 版本
docker-compose --version
```

