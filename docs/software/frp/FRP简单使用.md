*场景回顾：今天阿逗领导说服务器要迁移，因公司自建异地服务器将要停用，需要把数据迁移到新服务器上，直接tar,scp带走，结束后修改一下域名映射就好。可这阿逗居然说域名不能重新指向新IP，这...，这么多客户端都是使用的这域名。于是就想到了FRP作端口映射，将旧服务器启动监听端口，访问新服务器数据，解决近期数据迁移的问题，并发布新版本使用新域名。吐槽完毕开工！*

### 一、环境
1、CentOS外(用户访问的服务器)
2、CentOS内(实际访问的服务器)
3、Docker环境
4、[FRP资料](https://gofrp.org/docs/overview/)

[FRPS](https://github.com/fatedier/frp)分为服务器和客户端，此处将服务器端安装在CentOS外，客户端安装在CentOS内。

### 二、FRPS安装
#### · 容器安装

``` bash
# 创建frps容器
$ docker run --restart=always --network host -d -v /etc/frp/frps.ini:/etc/frp/frps.ini --name frps snowdreamtech/frps
```
*创建容器可能出错，原因是/etc/frp/frps.ini是一个文件夹直接删除然后touch 一个文件就好*
服务器端web服务配置(文件:/etc/frp/frps.ini）
```
[common]
bind_port = 7000
# 可以指定访问端口(需要域名指向该服务器IP)
vhost_http_port = 8000
```
编辑配置完毕后重启容器
``` bash
$ docker container restart frps
```
#### · Window安装

1、直接下载对应的exe(86|arm)
2、放到想放置的文件夹(例如:C:\Program Files (x86)\Frp)
3、配置[frpc.ini]
4、启动frpc.exe，(需要在cmd中运行frpc.exe)
5、(可选)开机自动运行,自行任选其一vb,bat,sh。
5.1.a、创建vb脚本[start.vbs]
```vsscript
# 脚本测试未生效
# 本想使用bat脚本，由于不熟，就直接使用sh脚本了
createobject("wscript.shell").run "frpc.exe",0
```
5.1.b、创建sh脚本[start.sh],需要git支持
```
# 启动FRPC，需要git支持，或者在linux下运行
#  DIR根据运行程序自行修改

# 运行目录
DIR="/C/Program Files (x86)/Frp"
# 运行程序
EXE=$DIR/frpc.exe

# 切换目录
cd "$DIR"

# 判断程序是否已经启动
PID=`ps -ef|grep frpc|awk '{print $2}'`

# 程序正在运行，则输出
if [ "$PID" ];then
	echo pid=$PID
	echo frpc.exe already running!
else
	
	# 启动程序
	echo frpc.exe start...
	# 后台运行
	nohup "$EXE" >"$DIR/log.log" 2>&1 &
	sleep 3
	PID=`ps -ef|grep frpc|awk '{print $2}'`

	# 启动失败，再次启动(启动时未检测到网络将失败)
	while [ ! "$PID" ]
	do
		echo frpc.exe restart...
		nohup "$EXE" >"$DIR/log.log" 2>&1 &
		sleep 3
		PID=`ps -ef|grep frpc|awk '{print $2}'`
	done
	
	echo frpc.exe start up!
fi

# 退出
exit
```

5.2运行命令(win+r)，gpedit.msc，打开【本地组策略编辑器】
```
gpedit.msc
```
5.3找到【本地计算机 策略】->【计算机配置】->【脚本(启动/关机)】,点击【启动】按钮，【脚本】中添加【start.vbs】脚本即可。

```

```

### 三、FRPC安装
``` bash
# 创建frpc容器
$ docker run --restart=always --network host -d -v /etc/frp/frpc.ini:/etc/frp/frpc.ini --name frpc snowdreamtech/frpc
```
*如果出现异常，请参考二中的异常处理*
客户度web服务配置(文件:/etc/frp/frpc.ini
)
```
[common]
server_addr = x.x.x.x
server_port = 7000

[web]
type = http
local_port = 80
# Docker部署需要使用到
# 如果映射的是宿主机，需要查看宿主机IP
#
# Windows
# $ ipconfig 
# 查看vEthernet(DockerNAT)网卡
#
# Linux/Mac
# $ ip addr show docker0

local_ip = 127.0.0.1
# 需要域名服务器指向FRPS的IP
custom_domains = www.yourdomain.com

[web2]
type = http
local_port = 8080
custom_domains = www.yourdomain2.com
```
编辑配置完毕后重启容器
``` bash
$ docker container restart frpc
```

### 四、测试
1、可以通过容器日志查看连接情况
``` bash
# 查看服务器端运行情况
$ docker container logs -f --tail 20 frps

# 查看客户容器运行情况
$ docker container logs -f --tail 20 frpc
```
2、CentOS(内)配置的域名和端口访问进行测试
如：www.yourdomain.com
	　　www.yourdomain2.com:8080

### 五、二级多域名配置
1、[frps.ini]配置
```
[common]
bind_port = 27000	#接收frpc的连接端口
vhost_http_port =27009	#启用后才支持HTTP类型的代理，默认不启用(其他类型类似)
subdomain_host = f.aa.net	#二级域名后缀(需要在域名服务器配置*f.aa.net记录,可以自行配置二级或三级域名)

# 可选配置
authentication_method = token #鉴权方式,token(默认), oidc
token = token123 #token
dashboard_port = 27010 #看板
dashboard_user = admin
dashboard_pwd = admin

```
2、[frpc.ini]配置
```
[common]
server_addr = f.aa.net
server_port = 27009
token = token

# Http
[xm]
type = http
local_port = 9080
subdomain = xm	#子域名(xm.f.aa.net:27009)

# Http2
[xm2]
type = http
local_port = 9081
subdomain = xm2 #子域名(xm2.f.aa.net:27009)

```
3、nginx配置(可选)
```
需要提前在域名服务器商申请记录:*.f.aa.net
#配置文件../nginx/config/conf.d/f.aa.net.conf，文件名称随意写，nginx会自动加载配置目录的配置文件
server {
    listen       80;
    listen       443 ssl;
    server_name  *.f.aa.net;
    
    #可选证书配置ssl config
    ssl_certificate /etc/letsencrypt/live/aa.net/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/aa.net/privkey.pem;
   

    location / {
		proxy_pass  http://127.0.0.1:27009;	#frps中vhost_http_port =27009
		proxy_set_header Host $host;			#必须配置，否则frps地址能不识别
		#可选配置
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
4、访问测试
```
一、域名+端口(不配置nginx测试)
Http1访问：http://xm.f.aa.net:27009
Http2访问：http://xm2.f.aa.net:27009

二、域名(配置nginx测试)
Http1访问：http://xm.f.aa.net
Http2访问：http://xm2.f.aa.net

```
5、问题&解决
```
1、配置后访问无效
	A:登录配置的管理面板(dashboard_port = 27010),
	查看http://f.xtpla.net:27010/static/#/proxies/http，终端是否在线，不在线说明客户端未连接上，排查客户端frpc是否启动，配置是否正确，如有问题查看相关日志;
2、nginx配置后测试无效
	A：查看nginx配置，如果页面能正常看到frp信息，可能是终端未启动，或则nginx未配置的header导致frps为正确响应。
```
6、资料
[资料gofrp.org](https://gofrp.org/docs/features/http-https/subdomain)

### 六、多端口配置(IP配置非域名)
1、[frps.ini]配置
```
[common]
bind_port = 27000
token = token123

# 可选配置
authentication_method = token
dashboard_port = 27010
dashboard_user = admin
dashboard_pwd = @admin
```

2、[frpc.ini]配置
```
[common]
server_addr = 47.0.0.1 #frps访问地址
server_port = token123
token = token123

#Http
type = tcp		#注意是tcp非http
local_port = 9080
remote_port = 27001
#custom_domains = 47.96.177.11	#不需要配置

#Http2
type =tcp
local_port = 9081
remote_port = 27002

#Http ...

```

3、测试
```
	Http测试：http://47.0.0.1:27001
	Http2测试：http://47.0.0.1:27002
```

4、问题&解决
```
	使用云服务器，请打开对应的地址，使用linux需要开启防火墙端口。
	是否连接上，也可以通过面板地址查看连接终端情况。
```

### 七、问题总结
1、mqtt等是udp通信http是不适用的，在官网试用文档上没有查询到，后google寻得解决方法，下面就是如何配置如何配置upd。

服务端(CentOS外,frps.ini)
```
[common]
bind_port = 7000
```
客户端(CentOS内,frpc.ini)
```
[common]
server_addr = x.x.x.x
server_port = 7000

[udp]
type = udp
local_port = 9000
remote_port = 9001
```
2、服务器肯定是有多个端口，不同协议，如何优雅解决？

	由于这个一个临时项目使用，直接暴力使用多容器进行一一映射，同一协议可以使用一个容器。

3、部分网络不能穿透(其他网络可以用，到特定网络不能使用)
```
	解决：在[frpc].ini配置加密传输
	[common]
	#...
	tls_enable = true
	#...
```
*问题解决了，文章也写完，又提升了一点，周末愉快！！！*



