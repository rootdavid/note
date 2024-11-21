## 使用Docker构建服务

### Jekyll基础镜像

```dockerfile
FROM ubuntu:latest
LABEL maintainer=x@x.com
ENV REFRESHED_AT=2024-11-01
RUN apt-get -yqq update
RUN apt-get -yqq install ruby ruby-dev libffi-dev build-essential nodejs
RUN gem install --no-document jekyll -v 4.2.0
VOLUME /data
VOLUME /var/www/html
WORKDIR /data
ENTRYPOINT [ "jekyll", "build", "--destination=/var/www/html" ]
```

- `sudo docker run -v /home/wsl/james_blog:/data/ --name james_blog cndvdocker/jekyll`
  - 启动一个名为**james_blog**容器，映射本地文件夹**james_blog**到卷**data**目录

卷在Docker宿主机***/var/lib/docker/volumes***目录中

`sudo docker run -d -P --volumes-from james_blog cndvdocker/apache`

- **`-d`**：以“分离”模式运行容器，容器会在后台运行
- **`-P`**：自动将容器的所有公开端口（`EXPOSE` 声明的端口）映射到宿主机的随机端口
- **`--volumes-from james_blog`**：将另一个容器 `james_blog` 中的卷（如 `/data` 和 `/var/www/html`）挂载到当前容器中，使其可以共享数据
- **`cndvdocker/apache`**：使用 `cndvdocker/apache` 作为镜像来创建并启动新的 Apache 容器

### 备份Jekyll卷

