## 使用docker镜像与仓库

### 列出镜像

`docker images` 

`docker pull xxx`

`docker run -t -i --name xxx ubuntu:12.04 /bin/bash` 运行一个带标签的镜像

### 拉取镜像

`docker pull xxx:version`

### 查找镜像

`docker search xxx`

### 构建镜像

相关命令 `docker commit` `docker build` `Dockerfile`文件

### 用Dockerfile构建镜像

```dockerfile
# Version 0.0.1 
FROM ubuntu:14.04 # 基础镜像
MAINTAINER James Turnball "james@example.com" # 联系方式
RUN apt-get update && apt-get install -y nginx RUN echo 'Hi, I am in your container' > /usr/share/nginx/html/index.html # 执行命令
EXPOSE 80 # 开放端口
```

`docker build -t="username/imagename:version" .` 构建命令

`docker build --no-cache -t...` 不使用缓存

`docker history imageid` 查看如何构建

### 从新镜像启动容器

`sudo docker run -d -p 80:80 --name static_web cndvdocker/static_web nginx -g "daemon off;"`

​	`-p` 宿主机随机分配端口绑定在容器80端口

​	`docker port imageid 80` 查看宿主机哪个端口绑定了容器的端口

#### 命令

`docker login` 登录到docker hub

`docker commit imageid username/reponame` 提交定制容器

