#容器脚本
``` bash
#!/bin/bash
source /etc/profile
#版本:v1.1
#作者:xtpla@126.com
#说明:对容器进行简单维护

#说明
#脚本全部功能有效，且其他已xt_开头的脚步皆是在该脚本上修改

#姓名名称(需要修改)
PRJ_NAME=test
#外部端口(需修改)
OUT_PORT=8080
#容器内部端口(需修改)
IN_PORT=6060

#前缀(对数据库,镜像有效)
PREFIX=xt_
#项目位置
PRJ_DIR=/usr/src/$PRJ_NAME
#容器名称
CON_NAME=$PREFIX$PRJ_NAME
#可执行文件名称(该名称一定要和Docerfile运行名称相同)
EXE_FILE=$PRJ_NAME
#新文件名称
NEW_FILE=$PRJ_NAME.linux
#备份目录
BACK_DIR=/usr/src/backup/$PRJ_NAME
#镜像
IMG_NAME=$PRJ_NAME:1.0.1
#镜像配置
IMG_DOCKER_FILE=Dockerfile_$PRJ_NAME

#运行目录
EXE_DIR=/usr/bin/app

#资源目录
SOURCE_DIR_NAME=source
#企业微信通知KEY
WEIXIN_KEY=a56facf0-e0c6-4ac0-a380-***********


#数据库容器ID
DB_CON_NAME=xt_mysql
#数据库账号
DB_USER=root
#数据库密码
DB_PWD=123456
#数据库名称
DB_NAME=$PREFIX_$PRJ_NAME
#主机名称
HOST_NAME=$(hostnamectl | grep hostname | awk -F: '{print $2}')
#脚本目录
SHELL_DIR=$(cd "$(dirname "$0")";pwd)

#初始化
function init() {
	#日志目录
	if [ ! -d "$PRJ_DIR/logs" ]; then
		mkdir -p "$PRJ_DIR/logs"
	fi
	#备份目录
	if [ ! -d "$BACK_DIR" ]; then
		mkdir -p "$BACK_DIR"
	fi
}

#镜像创建
function buildImage() {
  # -t 镜像的名字及标签
  # -f 指定要使用的Dockerfile路径
  if [ -f "$SHELL_DIR/$IMG_DOCKER_FILE" ];then
    echo -e "\033[32m Dockerfile:$SHELL_DIR/$IMG_DOCKER_FILE \033[0m"
    echo -e "\033[37m===================================== \033[0m"
    echo -e "\033[35m `cat "$SHELL_DIR/$IMG_DOCKER_FILE"` \033[0m"
    echo -e "\033[37m===================================== \033[0m"

    docker build -f $SHELL_DIR/$IMG_DOCKER_FILE -t $IMG_NAME .
  else
    echo -e "\033[33m File “$SHELL_DIR/$IMG_DOCKER_FILE” is not exist! \033[0m"
  fi
}

#创建容器
function createContainer() {
  CON_ID=`docker container ps -a|grep "^.*$CON_NAME$"`
  if [[ "$CON_ID" == "" ]];then
    docker run --restart=always -e TZ="Asia/Shanghai" -d --name $CON_NAME -p $OUT_PORT:$IN_PORT -v $PRJ_DIR:$EXE_DIR --link xt_mysql:mysql --link xt_redis:redis $IMG_NAME
  else
    echo -e "\033[33m Container “$CON_NAME” is exist! \033[0m"
  fi
}

#删除容器
function removeContainer() {
  CON_ID=`docker container ps -a|grep "^.*$CON_NAME$"`
  if [[ "$CON_ID" != "" ]];then
    docker container rm -f $CON_NAME
  else
    echo -e "\033[33m Container “$CON_NAME” is not exist! \033[0m"
  fi
}

#防火墙设置
function openFireWall() {
  PORT_STATE=`firewall-cmd --query-port=$OUT_PORT/tcp`
  if [[ "$PORT_STATE" == "no" ]];then
    firewall-cmd --zone=public --add-port=$OUT_PORT/tcp --permanent
    firewall-cmd --reload
  else
    echo -e "\033[33m Firewall port “$OUT_PORT” is open! \033[0m"
  fi
}

#防火墙关闭
function closeFireWall() {
  PORT_STATE=`firewall-cmd --query-port=$OUT_PORT/tcp`
  if [[ "$PORT_STATE" == "yes" ]];then
    firewall-cmd --zone=public --remove-port=$OUT_PORT/tcp --permanent
    firewall-cmd --reload
  else
    echo -e "\033[33m Firewall port “$OUT_PORT” is not open! \033[0m"
  fi
}

#更新
function update() {
	cd $PRJ_DIR
  #检查文件(该处需要对文件进行校验，否则正在上传的文件可能会导致出错)
	if [ -f $NEW_FILE ]; then
    #如果容器是运行的，则先关闭
    CON_ID=`docker container ps -a|grep "^.*$CON_NAME$"`
    if [[ "$CON_ID" != "" ]];then
      docker container stop $CON_NAME
    fi

    #删除当前运行程序
  	if [ -f $EXE_FILE ]; then
  		rm -f $EXE_FILE
  	fi

    #删除资源内容
  	if [ -d $SOURCE_DIR_NAME ]; then
  		rm -r -f $SOURCE_DIR_NAME
  	fi
    #从命名文件
  	mv $NEW_FILE $EXE_FILE

    #启动docker
    docker container start $CON_NAME
  fi
}

#拆分日志(00:00)
function saveLog() {
  CON_ID=`docker container ps -a|grep "^.*$CON_NAME$"`
  if [[ "$CON_ID" == "" ]];then
    exit 0
  fi

	LogFile=$PRJ_DIR/logs/$(date -d'1 day ago' +%F.log)
	#防止重复生成，导致日志被覆盖
	if [ ! -f "$LogFile" ]; then
		#容器简单ID
		ConId=$(docker container ps | grep $CON_NAME | awk '{print $1}')
		#容器完整ID
		ContainerId=$(docker container inspect --format="{{.Id}}" $CON_NAME)
		#复制文件
		StartDate=$(date -d'1 day ago' +%F)
		EndDate=$(date +%F)
		docker container logs --since $StartDate $ConId --until $EndDate >>$LogFile
		#清除内容
		echo "" >/var/lib/docker/containers/$ContainerId/$ContainerId-json.log
	fi
}

#发送错误(09:00发送异常)
function sendError() {
	LogFile=$PRJ_DIR/logs/$(date -d'1 day ago' +%F.log)
	ErrorContent=$(cat $LogFile | grep ' Error ' | awk 'NR==1')
	if [ "$ErrorContent" != "" ]; then
		weixinNotice="<font color=\\\"warning\\\">$HOST_NAME-异常</font>\n>Proj:<font color=\\\"comment\\\">$CON_NAME</font>\n>Desc:<font color=\\\"comment\\\">$ErrorContent</font>"
		curl "https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=$WEIXIN_KEY" -H "Content-Type: application/json" -d "{\"msgtype\": \"markdown\",\"markdown\": {\"content\": \"$weixinNotice\"}}"
	fi
}

#清理缓存(04:00)
function clearCache() {
  CON_ID=`docker container ps -a|grep "^.*$CON_NAME$"`
  if [[ "$CON_ID" == "" ]];then
    exit 0
  fi

	#删除90天前的日志
	find $PRJ_DIR/logs -mtime 180 -type f -name '*.log' | xargs rm -f

	#删除30天DB备份
	find $BACK_DIR -mtime 30 -type f -name '*.tar.gz' | xargs rm -f
}

#备份数据库(04:00)
function backupDB() {
  CON_ID=`docker container ps -a|grep "^.*$CON_NAME$"`
  if [[ "$CON_ID" == "" ]];then
    exit 0
  fi

	BACK_DB_NAME=$(date '+%Y%m%d')
	docker container exec -i $DB_CON_NAME mysqldump -u$DB_USER -p$DB_PWD --default-character-set=utf8 --databases $DB_NAME | gzip >$BACK_DIR/$BACK_DB_NAME.sql.gz
}

#恢复数据库
function restoreDB() {
  CON_ID=`docker container ps -a|grep "^.*$CON_NAME$"`
  if [[ "$CON_ID" == "" ]];then
    echo "Container is not running!"
    exit 0
  fi

	FILE_PATH=$1
	if [[ ! "$FILE_PATH" ]]; then
		echo "Please set filepath!"
	elif [ -f $FILE_PATH ]; then
		echo "Restore mysql db!"
		gunzip <$FILE_PATH | docker container exec -i $DB_CON_NAME mysql -u$DB_USER -p$DB_PWD
	else
		echo "File \"$FILE_PATH\" not exist!"
	fi
}

#备份
function backup() {
	backupDB
}

#推送
function broadcast() {
  a=1
  #curl --insecure -X POST -v https://api.jpush.cn/v3/push -H "Content-Type: application/json" -u "8160ac5c57003c258d81e6ed:470e8e472fbbcd184da10f53" -d '{"platform":"all","audience":"all","notification":{"alert":"Hi!"}}'
}

#初始化
init
#命令执行
case $1 in
"update")
	update
	;;
"saveLog")
	saveLog
	;;
"sendError")
	sendError
	;;
"clearCache")
	clearCache
	;;
"backup")
	backup
	;;
"backupDB")
	backupDB
	;;
"restoreDB")
	restoreDB $2
	;;
"broadcast")
	broadcast
  ;;
"buildImage")
	buildImage
  ;;
"createContainer")
	createContainer
  ;;
"removeContainer")
	removeContainer
  ;;
"openFireWall")
	openFireWall
  ;;
"closeFireWall")
  closeFireWall
  ;;
*)
	echo -e "\033[33m 运行参数 \033[0m"
	echo -e "\033[33m   *.sh \033[0m"
	echo -e "\033[33m     update          更新\033[0m"
	echo -e "\033[33m     saveLog         保存昨天\033[0m"
	echo -e "\033[33m     sendError       发送异常\033[0m"
	echo -e "\033[33m     clearCache      清理缓存,如过期备份\033[0m"
	echo -e "\033[33m     backup          备份\033[0m"
	echo -e "\033[33m     backupDB        备份数据库\033[0m"
	echo -e "\033[33m     restoreDB       恢复数据库\033[0m"
	echo -e "\033[33m     buildImage      编译镜像\033[0m"
	echo -e "\033[33m     createContainer 创建容器\033[0m"
	echo -e "\033[33m     removeContainer 删除容器\033[0m"
	echo -e "\033[33m     openFireWall    开启防火墙\033[0m"
	echo -e "\033[33m     closeFireWall   关闭防火墙\033[0m"

	;;
esac
exit 0

```
Dockerfile_test
``` bash
FROM	debian:buster-slim
/usr/bin/disin/Disinfection
EXPOSE 6060

ENTRYPOINT ["/usr/bin/app/test","/bin/bash"]
```

crontab -l
``` bash
#00:00
00 00 * * * /bin/sh /usr/src/sh/crontab_l.sh am0000

#04:00
00 04 * * * /bin/sh /usr/src/sh/crontab_l.sh am0400

#05:00
00 05 * * * /bin/sh /usr/src/sh/crontab_l.sh am0500

#08:00
00 08 * * * /bin/sh /usr/src/sh/crontab_l.sh am0800

#09:00
00 09 * * * /bin/sh /usr/src/sh/crontab_l.sh am0900

#每分钟
* * * * * /bin/sh /usr/src/sh/crontab_l.sh sp0001

#每3分钟
*/3 * * * * /bin/sh /usr/src/sh/crontab_l.sh sp0003

#每5分钟
*/5 * * * * /bin/sh /usr/src/sh/crontab_l.sh sp0005

#每10分钟
*/10 * * * * /bin/sh /usr/src/sh/crontab_l.sh sp0010

#每1小时
0 */1 * * * /bin/sh /usr/src/sh/crontab_l.sh sp0100
```
crontab_l.sh
``` bash
#根据自己需要编写
```
