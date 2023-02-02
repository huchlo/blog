# Docker
-   **镜像（Image）**：Docker 镜像（Image），就相当于是一个 root 文件系统。比如官方镜像 ubuntu:16.04 就包含了完整的一套 Ubuntu16.04 最小系统的 root 文件系统。
-   **容器（Container）**：镜像（Image）和容器（Container）的关系，就像是面向对象程序设计中的类和实例一样，镜像是静态的定义，容器是镜像运行时的实体。容器可以被创建、启动、停止、删除、暂停等。
-   **仓库（Repository）**：仓库可看成一个代码控制中心，用来保存镜像。

## docker run
`docker run -i -t ubuntu:15.10`
`docker run -it ubuntu:15.10`
-   **-t** 在新容器内指定一个伪终端或终端。
-   **-i** 允许你对容器内的标准输入 (STDIN) 进行交互。
-   **-d** 后台模式。
-   **-P** 将容器内部使用的网络端口随机映射到我们使用的主机上。
-   **-p 80:8080** 本机80映射容器8080 。

- --name 
运行一个容器，没有则会创建一个
## docker ps
-   **-l** 查询最后一次创建的容器
-   **-a** 查询所有容器
查看容器
## docker logs
`docker logs 2b1b7a428627`
查看容器内的标准输出
## docker stop
停止容器
## docker pull
获取镜像
## docker start
启动一个停止的镜像
## docker attach
进入容器
exit后容器也会停止
## docker exec
进入容器
exit后容器不会停止
## docker export
`docker export 1e560fca3906 > ubuntu.tar`
导出
## docker import
`cat docker/ubuntu.tar | docker import - test/ubuntu:v1`
`docker import http://example.com/exampleimage.tgz example/imagerepo`
导入
## docker rm
`docker rm -f 1e560fca3906`
删除容器
## docker port
`docker port bf08b7f2cd89`
查看容器端口映射情况
## docker top
`docker top wizardly_chandrasekhar`
查看容器内的进程


docker hub：https://registry.hub.docker.com/

docker中文文档：https://www.widuu.com/docker/?_t=t

小猿取经docker$k8s教程：https://www.cnblogs.com/xiaoyuanqujing/p/11839932.html

centos下安装docker-ce

yum install -y yum-utils
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
yum install docker-ce
docker -v


docker的启动和停止
systemctl start docker
systemctl stop docker
systemctl restart docker
systemctl status docker
systemctl enable docker
docker info
docker --help

centos默认使用podman，而docker服务依赖containerd.io
解决方法1：安装新的containerd.io
 yum install https://download.docker.com/linux/fedora/30/x86_64/stable/Packages/containerd.io-1.2.6-3.3.fc30.x86_64.rpm

解决方法2：安装旧的docker
yum list docker-ce --showduplicates | sort -r
yum -y install  docker-ce-18.06.0.ce-3.el7

# mysql
```bash
docker pull mysql
```
```bash
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:latest
```
https://hub.docker.com/_/mysql

解决： 安装vi、vim
apt-get update
apt-get install vim

# nginx
html: /usr/share/nginx/html
conf: /etc/nginx/nginx.conf