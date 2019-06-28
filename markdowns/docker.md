# Docker

## What is Docker

> Docker is a set of coupled software-as-a-service and platform-as-a-service products that use operating-system-level virtualization to develop and deliver software in packages called containers based on [Cgroups](https://en.wikipedia.org/wiki/Cgroups).

## Getting started with a docker-compose - 一键启动 wordpress

```yml
version: '3.3'

services:
   db:
     image: mysql:5.7
     volumes:
       - db_data:/var/lib/mysql
     restart: always
     environment:
       MYSQL_ROOT_PASSWORD: somewordpress
       MYSQL_DATABASE: wordpress
       MYSQL_USER: wordpress
       MYSQL_PASSWORD: wordpress

   wordpress:
     depends_on:
       - db
     image: wordpress:latest
     ports:
       - "8000:80"
     restart: always
     environment:
       WORDPRESS_DB_HOST: db:3306
       WORDPRESS_DB_USER: wordpress
       WORDPRESS_DB_PASSWORD: wordpress
       WORDPRESS_DB_NAME: wordpress
volumes:
    db_data: {}
```

## Why containerization application - 为什么服务端会演变到容器化

![application](https://raw.githubusercontent.com/wujun4code/playground-t1w2jibp/master/assets/container_evolution.png)

## Containerization Application Development Workflow -  基于容器化应用开发的工作流

1. 选择一个后端语言
2. 选择对应语言的服务端框架
3. 写代码
4. 编写 Dockfile 引用项目源码
5. 编译镜像，推送到公司私有仓库
6. 编写 yaml 文件，引用项目镜像，交付给 DevOps
7. 等待部署，应用启动

总而言之，当今时代服务端工程师交付的就是一个 Docker Image，所有源码都会被编译成一个 Image ，而 Kubernetes 解决的就是高伸缩性的应用部署和运行时状态检测。

## Docker Swarm

多机器集群之间的 Docker 环境，是由 Docker 母公司官方开发的，目标是针对多机器（虚拟机/物理机）的统一的部署和运行环境，也有许多友好的功能，但是在与 Google 的直面竞争中失败，阿里云已与日前宣布停止支持 Docker Swarm 功能。

## Why Kubernetes won the game battled with Docker Swarm - 八卦小事

第一个核心原因是：

> Docker 母公司在还没有推出集群的解决方案的时候，就过早地开始商业化，大部分人力都倾注在企业服务和内部的集群管理工具的开发上，而开源版本的 Swarm 功能也被 Kubernetes 完全超越，不到 2 年的时候，全球的服务端业务需求大的公司纷纷站在了 Google 这一边。

另一个原因是，Swarm 本身是一个编排工具，并没有完整的运行时和管理功能提供给开发者，而 Kunbertes 自身的抽象层做的好，将许多接口以插件的形式开放给社区，比如 Docker 可以在 Kubernetes 里面被替代，用其他容器技术来运行，与此类似的是，Kubernetes 最强大一点就是，它只制定了 API 和组件之间的协议，并不强制要求使用者一定要使用某一个组件，包括操作系统，网络层，资源管理层都是开放出，交给社区是实现。
