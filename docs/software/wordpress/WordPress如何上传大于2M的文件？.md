### 方法
网上方法很多，需要根据自身WorrPress找到合适的方法；

### WordPress环境
Docker

### 解决办法
1、上传位置显示为2M?
方法1、直接修改php.ini配置
Note:修改后更新WordPress版本后可能会失效，需要重新设置

	进入容器(使用容器名,容器ID都可以，以下统一容器名)
	docker container exec -it 容器名 /bin/bash
	查看php.ini位置
	php -i |grep 'php.ini'
	查询出来可能是个目录，这时说明使用的是默认的配置，需需要将php.ini-product复制一份
	
	先将php.ini-product拷贝到宿主机(容器内可能不存在vi/vim)
	docker cp 容器名:/usr/local/etc/php/php.ini-product php.ini
	
	修改内容(vi php.ini)
	file_uploads = On
	upload_max_filesize = 300M
	post_max_size = 300M
	max_execution_time = 180
	
	覆盖容器内文件
	docker cp php.ini 容器名:/usr/local/etc/php/
	
	重启容器
	docker-compose -f wordpress.yml restart
	
	设置正确，上传文件将提示“最大上传文件大小：300 MB。”
方法2、使用插件配置(插件名称:WP Increase Upload)
暂不详解

2、413 Request Entity Too Large /nginx?
提示说明是nginx的问题
进入nginx配置目录

	修改
	vi nginx.conf
	
	在http{}添加以下内容
	http{
	...
	# 添加内容
	client_max_body_size 300M;
	...
	}
	
	重启容器
	docker container restart nginx容器ID


完毕！
