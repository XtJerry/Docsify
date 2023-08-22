# 一环境
1、域名是domains.google.com购买
2、服务器是centos 7.8(不重要),使用docker容器；
3、证书使用为nginx，将wordpress配置为https;

# 二、certbot使用
1、Docker安装，在此略过
2、镜像拉取([官方](https://hub.docker.com/r/certbot/certbot "官方"))
```
# 拉取镜像
docker pull certbot/certbot
```

```
# 镜像使用
docker run --rm -it --name certbot --network host -v /usr/src/nginx/letsencrypt:/etc/letsencrypt certbot/certbot certonly --manual --preferred-challenges=dns

* 注意事项
* 1、由于这边是将多个域名设置相同证书，使用了同配置*.xtpla.net
* 2、验证key时，需要先在domain.google.com添加一条文本的路由记录(路由信息在提示信息中存在)，再敲击回车；

#--rm 关闭后自动销毁容器
```
```
# 更新
docker run --rm -it --name certbot -v /usr/src/nginx/letsencrypt:/etc/letsencrypt certbot/certbot renew
```

# 三、Nginx配置
1、nginx配置文件内容
```
server {
	listen       80;
    listen       443 ssl;
    server_name  blog.xtpla.net;
    
    #该处的路径为nginx容器的访问路径,不是宿主机的路径
    ssl_certificate /etc/letsencrypt/live/xtpla.net/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/xtpla.net/privkey.pem;
   

    location / {
        proxy_redirect   off;
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header X-Forwarded-Host $server_name;
		proxy_set_header X-Forwarded-Proto https;
	
		#对应的wordpress访问路径
		proxy_pass http://172.20.0.3:80;
    }
}

```
2、重启容器
```
#重启容器
docker container restart 容器名称

#查看容器日志(容器最近10条记录并持续查看)
docker container logs 容器名称 -f --tail 10

```
# 四、wordpress设置
进入wordpress管理页面，【设置】->【常规】，将访问地址设置为https路径即可。

# 五、常见问题
1、certbot尝试次数是有限的，建议先了解流程后再操作；
2、nginx配置后不清楚使用哪个证书,其实可以直接使用*.pem,不用再去转换；
3、nginx proxy_set_header参数设置不正确，导致wordpress部分资源不能正常访问；
4、wordpress部分资源不能识别，主要是网站使用了http绝对引用导致，如头像；

