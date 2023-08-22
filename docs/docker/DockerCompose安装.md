### 准备
1、CentOS 7.8
2、[安装资料](https://docs.docker.com/compose/install)

### 安装
1、根据文件直接下载
2、设置可执行文件权限
3、测试安装结果

``` bash
# 下载文件
$ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# 设置权限
$ sudo chmod +x /usr/local/bin/docker-compose

# 测试
$ docker-compose --version
> docker-compose version 1.29.2, build 1110ad01
#安装完成
```

### 三、删除
删除很简单，直接rm -f
``` bash
# 删除命令
$ sudo rm /usr/local/bin/docker-compose
```