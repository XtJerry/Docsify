### 一、Docker安装
*此处仅介绍CentOS的Docker安装方式，Linux安装方式比较类似，Mac提供有可视化安装方法，Windows安装相对有点复杂。*


**准备材料**
1、CentOS系统环境
2、[Docker安装资料](https://docs.docker.com/get-docker)

### 二、安装步骤
1、进入[Docker文档Linux安装](https://docs.docker.com/engine/install)页面
2、找到Server平台下的[CentOS](https://docs.docker.com/engine/install/centos)并打开
3、根据文档进行操作

详情如下：
``` bash
# 如果之前安装过，请执行以下命令;
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine

# 设置安装仓库
$ sudo yum install -y yum-utils
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

# 安装最新Docker
$ sudo yum install docker-ce docker-ce-cli containerd.io

# 安装指定版本
# 查看Docker版本
$ yum list docker-ce --showduplicates | sort -r
> ......
> docker-ce.x86_64            3:18.09.1-3.el7                    docker-ce-stable  
> docker-ce.x86_64            18.06.3.ce-3.el7                   docker-ce-stable 
> ......


# 安装命令(下面是以<VERSION_STRING>为19.03.11-3.el7(新版本编号前面添加3:19.03.11-3.el7,请忽略:前面部分)
# sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
$ sudo yum install docker-ce-19.03.11-3.el7 docker-ce-cli-19.03.11-3.el7 containerd.io
```
到此，Docker安装完毕，下面简单说一下Docker安装后启动、测试及配置；

### 三、Docker启动和配置
``` bash
# 启动
$ sudo systemctl start docker

# 测试(执行下面命令并输出表示安装成功)
$ sudo docker run hello-world
> Hello from Docker!

# 设置开机启动
$ sudo systemctl enable docker.service

# 停止
$ sudo systemctl stop docker
```
### 四、Docker 卸载
``` bash
# 删除Docker
$ sudo yum remove docker-ce docker-ce-cli containerd.io

# 删除镜像、容器、配置等
$ sudo rm -rf /var/lib/docker
$ sudo rm -rf /var/lib/containerd

```


