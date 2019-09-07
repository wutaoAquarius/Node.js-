# docker常用命令记录
## docker 命令查看
- 查看 docker 所有命令
`docker`
- 更深入查看docker命令
`docker command --help` 例如：`docker stats --help`

## 运行hello world
`docker run ubuntu:15.10 /bin/echo "Hello world"`  

参数解析
- docker: Docker 的二进制执行文件。
- run:与前面的 docker 组合来运行一个容器。
- ubuntu:15.10指定要运行的镜像，Docker首先从本地主机上查找镜像是否存在，如果不存在，Docker 就会从镜像仓库 Docker Hub 下载公共镜像。
- /bin/echo "Hello world": 在启动的容器里执行的命令  

以上命令完整的意思可以解释为：Docker 以 ubuntu15.10 镜像创建一个新容器，然后在容器里执行 bin/echo "Hello world"，然后输出结果。

## 运行交互式的容器
通过docker的两个参数 -i -t，让docker运行的容器实现"对话"的能力
```
runoob@runoob:~$ docker run -i -t ubuntu:15.10 /bin/bash
root@dc0050c79503:/#
```
参数注释
- i：允许你对容器内的标准输入 (STDIN) 进行交互。
- t: 在新容器内指定一个伪终端或终端。  

运行命令后，就能够进入 ubuntu15.10的容器，在这里能执行Ubuntu系统的各种命令；
通过 ctrl+D 或者 exit 推出；

## 启动容器（后台模式）
创建一个以进程方式运行的容器
```sh
runoob@runoob:~$ docker run -d ubuntu:15.10 /bin/sh -c "while true; do echo hello world; sleep 1; done"
2b1b7a428627c51ab8810d541d759f072b4fc75487eed05812646b8534a2fe63
```
输出的长字符串叫做容器ID，对每个容器来说都是唯一的，我们可以通过容器ID来查看对应的容器发生了什么。
- d: 使容器后台运转

首先，我们需要确认容器有在运行，可以通过 docker ps 来查看
`docker ps`
输出结果对应解释：
```
CONTAINER ID：容器ID 
IMAGE：镜像名称
COMMAND：执行的命令
CREATED：创建时间
STATUS：状态
PORTS：映射端口
NAMES：自动分配的容器名称
```
详细解释：[docker 命令解析之 ps](https://bingohuang.com/docker-ps/)

查看容器运行标准输出：`docker logs 容器ID` 或者 `docker logs 容器名称`
停止容器运行：`docker stop 容器ID` 或者 `docker stop 容器名称`

## 运行一个Web应用
载入镜像：`docker pull 镜像名称`
运行镜像：`docker run -d -P 镜像名称 执行命令 启动文件`
```sh
docker pull training/webapp
docker run -d -P training/webapp python app.py
```
查看容器运行
```sh
runoob@runoob:~#  docker ps
CONTAINER ID        IMAGE               COMMAND             ...        PORTS                 
d3d5e39ed9d3        training/webapp     "python app.py"     ...        0.0.0.0:32769->5000/tcp
```
PORTS 显示 0.0.0.0:32769->5000/tcp
Docker 开放了 5000 端口（默认 Python Flask 端口）映射到主机端口 32769 上。
如果你的服务器开放了的 32769 端口,那么在 通过 服务器IP或域名:32769 就能访问应用
http://testip:32769

