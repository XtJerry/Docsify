### Dockerfile

Dockerfile 为创建Docker镜像的文件。

### Dockerfile
```dockerfile
FROM alpine:3.14 AS go_release
# 运行目录
WORKDIR /go/src
# 拷贝可运行的程序(根据需要)
COPY app app
# 拷贝模板(根据需要)
COPY templates templates

# 启动容器后运行程序
ENTRYPOINT  ["./app"]
```
app在alpine中需注意build参数，否则不一定能运行，需要添加`--tags netgo`
```shell
# linux 下编译命令(alpine)
go build --tags netgo -o app src/main.go

```

### 创建命令

```shell
docker build -t go_dev:1.0.1 .

# -t  go_dev      #go_dev为镜像名称,后可以写版本如(go_dev:1.0.1)
# -f  Dockerfile  #Dockerfile文件路径
```

### 根据镜像创建容器

```shell
docker run --name my-running-app -e TZ="Asia/Shanghai" -p 8080:8080 -p 8081:8081 -it --rm -v C:/Users/Rock/IdeaProjects/Entertainment:/go/src go_dev:1.0.1

# C:/Users/Rock/IdeaProjects/Entertainment  #宿主机的目录

```
