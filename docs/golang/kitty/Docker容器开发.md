## 一、Docker容器开发

---

### 1. Dockerfile

简单的Dockerfile编写

```dockerfile
FROM golang:1.21.0 AS go_dev
WORKDIR /go/src
```

### 2.创建&运行容器

```shell
# 创建镜像
docker build -t go_dev:1.0.1 .

# 运行容器
docker run --name go_dev -e TZ="Asia/Shanghai" -p 8080:8080 -p 8081:8081 -it --rm  -v C:/Users/Rock/IdeaProjects/Entertainment:/go/src go_dev:1.0.1
```

## 二、发布时简单容器设置

---

###  1.Dockerfile
```dockerfile
FROM alpine:3.14 AS go_release
# 工作目录
WORKDIR /go/src
# 拷贝app到镜像,app为编译好的alpine:3.14的镜像,template 相关文件，可以自行添加或删除
COPY app app
COPY templates templates

# 设置权限，可以不用的
RUN chmod -R 777 app

# 启动运行程序app
ENTRYPOINT  ["./app"]

```

### 2.创建镜像
```shell
# 创建镜像，名称go_release:1.0.1，版本1.0.1
docker build -t go_release:1.0.1 .

```

### 3.创建并启动容器
```shell
# xt-app 为容器名称
# -p 8080 宿主机对外的端口
# :8080 容器对外的端口
# -p 8080:8080 将宿主机的8080与容器8080做一下映射
# -e TZ 时区设置，默认不是+08:00
# --restart=always 总是自动启动
# -d 后台运行
docker run --name xt-app --restart=always -e TZ="Asia/Shanghai" -d -p 8080:8080 -p 8081:8081 go_release:1.0.1

```