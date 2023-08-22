### 一、环境准备
1、centos 7.8
2、[docker容器安装](https://docs.docker.com/get-docker)
3、[docker-compose安装](https://docs.docker.com/compose/install)

### 二、Wordpress安装
1、直接使用docker[官方wordpress镜像](https://hub.docker.com/_/wordpress)的*.yml进行安装
2、运行并测试

stack.yml(yml文件放置位置自行定义)
*下面设置的8080是宿主机对外的访问端口，使用nginx可以不用打开防火墙，否则需要打开防火墙。*
``` yml
version: '3.1'

services:

  wordpress:
    image: wordpress:5.8
    restart: always
    ports:
      - 8080:80
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wp_db_user
      WORDPRESS_DB_PASSWORD: wp_db_password
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - ./www/wordpress:/var/www/html

  db:
    image: mysql:5.7
    restart: always
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wp_db_user
      MYSQL_PASSWORD: wp_db_password
      MYSQL_RANDOM_ROOT_PASSWORD: root_password
    volumes:
      - ./data/mysql:/var/lib/mysql

volumes:
  wordpress:
  db:

```
运行并执行命令
``` bash
$ mkdir -p /usr/src/wordpress
$ cd /usr/src/wordpress
$ touch stack.yml

# 编辑输入直接按[i],保存按[esc]然后输入[wq]回车，取消保存输入按[esc]然后输入[q!]回车。
$ vi stack.yml
$ docker-compose -f stack.yml up -d
```
命令执行完毕后等待完毕，无异常后即可在浏览区输入设备访问地址，在进行配置即可(语言选择，登录密码等等)。
### 三、nginx做反向代理(可选安装)
1、安装nginx镜像
2、获取默认配置文件
3、设置配置文件
4、启动容器nginx


详细步骤如下(其中路径/usr/src/nginx路径请自行定义)：
``` bash
# 下载镜像
$ docker pull nginx:1.19.7

# 拷贝默认配置，并关闭容器
$ docker run --name tmp-nginx-container -d nginx:1.19.7
$ docker cp tmp-nginx-container:/var/log/nginx /usr/src/nginx/log
$ docker cp tmp-nginx-container:/usr/share/nginx/html /usr/src/nginx/html
$ docker cp tmp-nginx-container:/etc/nginx /usr/src/nginx/config
$ docker rm -f tmp-nginx-container

# 设置配置
# 修改配置前，请先查看docker容器的wordpress容器的IP
$ docker container ps
$ docker inspect [容器id]

# 创建容器并运行
$ vi /usr/src/nginx/config/conf.d/default.conf
$ docker run --restart=always -e TZ="Asia/Shanghai" --name xt_nginx -v /usr/src/nginx/config/nginx.conf:/etc/nginx/nginx.conf -v /usr/src/nginx/config/conf.d/default.conf:/etc/nginx/conf.d/default.conf -v /usr/src/nginx/html:/usr/share/nginx/html -p 80:80 -d nginx:1.19.7
```
default.conf
``` txt
server {
......
}

# 前面为默认内容，下面是添加内容
server {
    listen       80;
	#www.xtpla.net 你的域名
    server_name  www.xtpla.net;
    location / {
		#http://172.19.0.3，wordpress容器IP
        proxy_pass   http://172.19.0.3;
    }
}

```

### 四、问题及解决方法
	安装过程问题多姿多彩，任何情况都先通过表象将异常范围缩小，然后再通过相关方、方法、记录、日志、google等定位问题，解决问题。
