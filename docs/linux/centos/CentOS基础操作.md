### 一、基本常用操作
``` bash
# ssh登录
# ssh user@ip [-p 端口](默认22)
$ ssh root@172.0.0.2 -p 6210

#修改用户密码
# passwd [user](默认当前用户)
$ passwd test

# 查看目录文件信息
# ls [-lh](查看文件)
$ ls -lh

# 预览文件
# cat filepath
$ cat test.txt

# 创建文件
# touch filename
$ touch test.txt

# 删除文件
# rm -f [-r|r](删除目录包括目录下全部文件)
$ rm -rf test

# 查看系统内存
# free -h
$ free -h
> Mem:           1.0G        527M         71M         40M        409M        273M
> Swap:          2.0G        326M        1.6G

# 查看系统当前运行端口
# netstat -ntlp
$ netstat -ntlp
> tcp        0      0 0.0.0.0:3268           0.0.0.0:*               LISTEN      973/sshd            
> tcp        0      0 127.0.0.1:25           0.0.0.0:*               LISTEN      1545/master    

# 端口查看
# lsof	-i:port
> lsof	-i:25

# 查看目录大小
> du -sm * | sort -n 

```
### 二、vi/vim操作
``` bash
# vi/vim filename
$ vi test.txt

# 编辑
> -- INSERT --
# 底部未显示“-- INSERT --”请按任意输入键，然后显示INSERT后方可输入

# 保存退出
# 底部无“-- INSERT --”输入:wq
$ :wq

# 退出
# 底部无“-- INSERT --”输入:q!
$ :q!

# 查找
# 底部无“-- INSERT --”输入/关键字，敲击回车
$ /keywork

# 显示行号
# set number
$ :set number

```

### 三、 SCP上传下载
``` bash
# 上传文件
# scp file user@ip:/path/file
$ scp .test.txt test@172.0.0.2:/test/test.txt

# 上传文件夹
# scp -r [-P ssh端口号] path user@ip:/path/
$ scp -r -P 6210 .testpath test@172.0.0.2:/test/

# 下载文件
# scp user@ip:/path/file .
$ scp test@172.0.0.2:/test/test.txt .

# 下载文件夹
# scp -r [-P ssh端口号] user@ip:/path/file .
$ scp -r test@172.0.0.2:/test/testpath .

```
**总结**

	scp [-r 目录] [-P ssh端口] 源  目的

### 四、防火墙设置
*防火墙设置需要先启动，初始化防火墙时，建议使用脚步(启动、添加ssh端口、更新防火墙)，否则可能导致直接对系统失联。*
``` bash
# 启动防火墙
$ systemctl start firewalld.service

# 添加放行端口
# firewall-cmd --zone=public --add-port={通行端口}/tcp --permanent
$ firewall-cmd --zone=public --add-port=6261/tcp --permanent

# 更新防火墙
$ firewall-cmd --reload

# 查看防火墙
# firewall-cmd --list-all 或 firewall-cmd --list-port
$ firewall-cmd --list-port

# 删除防火墙
# firewall-cmd --zone=public --remove-port={通行端口}/tcp --permanent
$ firewall-cmd --zone=public --remove-port=8080/tcp --permanent

# 开机自启
$ systemctl enable firewalld.service

# 查看防火墙状态
$ firewall-cmd --state

```

### 五、常见文件及处理
1、vi中文乱码，cat可正常显示
``` bash
# 编辑 ~/.vimrc
$ vi ~/.vimrc

# 添加
set fileencodings=utf-8,gbk,gb2312,gb18030
set fileencoding=utf8
set encoding=utf8
```
2、ssh登录提示(-bash: warning: setlocale: LC_CTYPE: ...)
``` bash
# 打开/etc/locale.conf
$ vi /etc/locale.conf

# 添加
LC_CTYPE=en_US.utf8
LC_ALL=en_US.utf8
LANG=en_US.UTF-8
```

### 六、文件压缩及解压
``` bash
# 压缩
# tar -cf [v 显示资源][z|j|J gzip|bzip2|zx格式] file.tar file|filepath
$ tar -cvf test.tar test.txt
$ tar -czvf test.tar.gz text.txt

# 解压
# tar -xf [v 显示资源][z|j|J gzip|bzip2|zx格式] file.tar [-C path]
$ tar -xvf test.tar
$ tar -xzf test.tar.gz -C /root
```
### 七、vi中文乱码
```shell
# vi 中文乱码
vi ~/.vimrc

# 添加
set fileencodings=utf-8,gbk,gb2312,gb18030
set fileencoding=utf8
set encoding=utf8
# 保存
:wq

```

### 八、linux权限
```shell
-rw------- (600) 只有拥有者有读写权限。
-rw-r--r-- (644) 只有拥有者有读写权限；而属组用户和其他用户只有读权限。
-rwx------ (700) 只有拥有者有读、写、执行权限。
-rwxr-xr-x (755) 拥有者有读、写、执行权限；而属组用户和其他用户只有读、执行权限。
-rwx--x--x (711) 拥有者有读、写、执行权限；而属组用户和其他用户只有执行权限。
-rw-rw-rw- (666) 所有用户都有文件读、写权限。
-rwxrwxrwx (777) 所有用户都有读、写、执行权限。
```

### *、 其他操作
	持续整理中...