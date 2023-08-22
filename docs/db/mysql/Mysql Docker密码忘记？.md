1、进入容器
``` bash
docker container exec -it [容器ID/名称] /bin/bash
```
2、跳过权限表
``` bash
echo "skip-grant-tables" >> /etc/mysql/conf.d/docker.cnf
```
3、退出容器
``` bash
exit
```
4、重启容器
``` bash
docker container restart [容器ID/名称]
```
5、进入容器(另见1)，并修改root密码
``` bash
mysql -uroot
alter user 'root'@'localhost' identified by '密码';
flush privileges;
exit;
```
6、恢复权限表
``` bash
sed -i "s/skip-grant-tables/ /" /etc/mysql/conf.d/docker.cnf
exit
```
7、重启容器(另见4)
