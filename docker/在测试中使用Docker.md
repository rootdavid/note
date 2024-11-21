## 在测试中使用Docker

### 从Sample网站和Nginx镜像构建容器

`docker run -d -p 80 --name website -v $PWD/website:/var/www/html/website cndvdocker/nginx nginx`

`-v` 这个选项允许我们将宿主机的目录作为卷，挂载到容器里

### 构建Sinatra应用程序

```dockerfile
FROM ubuntu:latest
LABEL maintainer="x@sample.com"
ENV REFRESHED_AT=2024-11-01
RUN apt-get update -yqq && apt-get -yqq install ruby ruby-dev build-essential redis-tools
RUN gem install --no-document sinatra json redis
RUN gem install rackup
RUN mkdir -p /opt/webapp
EXPOSE 4567
CMD [ "/opt/webapp/bin/webapp" ]
```

#### 构建时注意事项

`gem install`  参数失效

启动失败无提示可从这几个方向排查

- 查看容器日志
- 检查 Dockerfile 和启动命令
- 进入容器进行调试
- 检查容器的退出代码
  - 找到 `State.ExitCode` 字段，通常会有以下含义
    - `0`：正常退出
    - `1`：一般性错误
    - `126`：命令不可执行
    - `127`：命令未找到
    - `137`：被 `SIGKILL` 信号终止，可能是内存不足
    - `139`：被 `SIGSEGV` 信号终止，通常是段错误
- 重启容器

### 将Sinatra应用应用程序链接到Redis容器

与服务器对话有三种方式

1. Docker内部网络
   1. 不太灵活，重启容器后IP会变
2. Docker Networking 以及 `docker network` 命令 **（推荐）**
   1. `docker network create name` 创建一个docker网络
   2. `docker inspect netname` 查看某个网络详细信息
   3. `docker network ls` 列出所有docker网络
3. Docker链接