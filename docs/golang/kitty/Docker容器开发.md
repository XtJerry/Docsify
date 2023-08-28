### Docker容器开发

### Dockerfile

简单的Dockerfile编写

```dockerfile
FROM golang:1.21.0 AS go_dev
WORKDIR /go/src
```
### 创建&运行容器
```shell
# 创建镜像
docker build -t go_dev .

# 运行容器
docker run --name go_dev -e TZ="Asia/Shanghai" -p 8080:8080 -p 8081:8081 -it --rm  -v C:/Users/Rock/IdeaProjects/Entertainment:/go/src go_dev
```
