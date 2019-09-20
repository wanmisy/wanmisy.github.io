---
title: docker
date: 2018-12-24 10:22:37
tags: [docker,java]
categories: docker
---
## Docker
##### 查看docker版本【docker version】
![docker version](/docker/WX20181224-104417@2x.png))

### 获取镜像
命令行`docker [image] pull NAME[:TAG]`  
注：NAME是仓库名称、TAG是镜像的标签（通常用来表示版本信息），不制定TAG，则默认为latest，镜像的内容会跟踪最新版本的变更而变化，内容不稳定，因此建议加加上TAG  
例如下载ubuntu:18.04  
```
docker image pull ubuntu:18.04
```
<!-- more -->
### 查看镜像信息
1. 列出镜像
```
docker images
```
或
```
docker image ls
```
2. 使用tag添加镜像标签
```
docker tag ubuntu:18.04 myubuntu:18.04
```
以后就可以用myubuntu:18.04来代表ubuntu:18.04这个镜像了,此时这两个的image_id相同，所以实际上他们指向同一个镜像文件
3. 使用inspest查看详细信息
查询ubuntu:18.04的详细信息
```
docker inspect ubuntu:18.04
```
4. 使用history查看历史镜像
```
docker history ubuntu:18.04
```
### 搜寻镜像
```
docker search [option] keyword
```

### 删除和清理镜像·
1. 使用标签删除镜像  
删除命令为docker rmi IMAGE[IMAGE...]或docker image rm IMG[IMG...]  
支持选项：  
  -f,--force 强制删除
	-no-prune 不要清理未带镜像的父标签  
注：docker rmi后面可以是TAG，也可以是IMAGE_ID
2. 清理镜像
docker image prune
支持选项
  * -a,-all 删除所有无用镜像
	* -filter filter 只删除符合给定过滤器的的镜像
	* -f,force 强制删除镜像  

### 创建镜像
1. 基于已有容器的创建
格式`docker [container] commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]`主要选项包括
* -a,--author="" 作者信息  
* -c，--change=[] 提交时执行Dockerfile命令
* -p,--pause=true执行时暂停是暂停容器运行
演示：启动一个镜像，并在其中进行修改操作
```(docker)
 missj@  ~  docker run -it ubuntu:18.04 /bin/bash                                                ✔  297  12:02:06
root@94b5b306ceba:/# touch test
root@94b5b306ceba:/# exit
exit
```
提交容器
```
 missj@  ~  docker container commit -m "add a new file" -a "missj" 94b5b306ceba test:0.1         ✔  303  12:16:14
sha256:00d5fabda363bc043eb6a13d6c1d89b9417e1f94fdd020efd6ce86675d4bf180
```
查看
```
 missj@  ~  docker images                                                                        ✔  304  12:16:23
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
test                0.1                 00d5fabda363        20 seconds ago      86.2MB
nginx               latest              568c4670fa80        3 weeks ago         109MB
ubuntu              18.04               93fd78260bd1        4 weeks ago         86.2MB
 missj@  ~ 
 ```
2. 基于本地模板导入
命令格式`docker [image] import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]`
演示OPENVZ模版下载，下载地址为(https://wiki.openvz.org/Download/template/precreated)
```
 missj@  ~/Downloads  cat ubuntu-14.04-x86-minimal.tar.gz | docker import - ubuntu:14.04
sha256:e1f1d8ba4d0ad4874c448de5e3d2013a225ed3debc6a78a570a5ca10a9f6f2ee
```
3. 基于Dockerfile创建创建

### 存出镜像[docker save]
存出ubuntu:18.04到本地文件
```
docker save -o ubuntu_18.04.tar ubuntu:18.04
```
### 载入镜像[docker load]
将存出的tar文件导入到本地镜像库存
```
docker load -i ubuntu_18.04.tar
```
或
```
docker load < ubuntu_18.04.tar
```
### 上传镜像
默认上传到docker hub官方仓库，需要登录，命令行为`docker push NAME[:TAG] [REGISTRY_HOST[:REGISTRY_PORT]/]NAME[:TAG]`

### 查看docker支持的镜像支持子命令
docker image help

## 创建容器
1. 新建容器
`docker create`
2. 启动容器
`docker start`
3. 新建并启动容器
`docker run`等价于先`docker create`再`docker start`
### 查看容器
1. 查看容器信息
`docker ps` 或 `docker container ls`
2. 查看容器输出
`docker logs [OPTIONS] CONTAINER`  
OPTIONS:
	* -detail 详细信息
	* -f,-follow 持续保持输出
	* -since string 输出从某个时间开始的日志
	* -tail string 输出最近的若干日志
	* -t,timestamps 显示时间戳信息
	* -until string 输出某个时间之前的日志

## 停止容器
1. 暂停容器
`docker pause CONTAINER [CONTAINER...]`  
恢复暂停的容器
`docker unpause CONTAINER [CONTAINER...]`
2. 终止容器
`docker stop [OPTIONS] CONTAINER [CONTAINER...]`
3. 重启容器
`docker restart [OPTIONS] CONTAINER [CONTAINER...]`

## 进入容器
使用-d启动容器后，将会后台运行，这个时候进入容器需要`docker attach [OPTIONS] CONTAINER`或 
`docker exec [OPTIONS] CONTAINER COMMAND [ARG...]`
```
Options:
  -d, --detach               Detached mode: run command in the background
      --detach-keys string   Override the key sequence for detaching a container
  -e, --env list             Set environment variables
  -i, --interactive          Keep STDIN open even if not attached
      --privileged           Give extended privileges to the command
  -t, --tty                  Allocate a pseudo-TTY
  -u, --user string          Username or UID (format: <name|uid>[:<group|gid>])
  -w, --workdir string       Working directory inside the container
```

## 删除容器
`docker rm`删除处于终止或退出状态的容器

## 导出容器
`docker export -o CONTAINER`
## 导入容器
`docker import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]`


## 查看容器
1. 查看容器内进程`docker top CONTAINER [ps OPTIONS]`
2. 查看当前运行中容器系统资源使用情况
`docker ststs`

### 容器操作命令
1. 复制文件
案例 本地data复制到/temp  
`docker cp data test:/tmp/`
2. 查看容器内数据变更
案例： 查看test数据变更
`docker container diff test`
3. 查看端口映射
查看test端口映射情况
`docker container port test`