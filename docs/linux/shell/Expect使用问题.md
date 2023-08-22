*expect是解决scp、ssh等访问服务器自动人机交互的工具，如自动scp同步数据等*
### expect自动scp遇到密码特殊字符串解决
``` bash
# 问题脚本
...
# 密码为abc'def"g
SSH_PWD="abc'def\"g"
...
expect -c "
    spawn scp -P ${SSH_PORT} ${SSH_NAME}@${SSH_HOST}:/usr/src/backup/workpass/${FILE_NAME}.sql.gz ${BACKUP_DIR}/
    expect {
            \"denied,\" {exit;}
            \"*assword\" {set timeout 300; send \"${SSH_PWD}\r\";exp_continue;}
            \"(yes/no)?\" {send \"yes\r\";}
        }
"
```
出错原因是使用send命令时本身是字符串(字符串)，执行时需要转换为脚本，如果直接使用send命令没有问题，但是像上面写法实际发送的字符串并非我们想要的，因此需要对其转义。
``` bash
...
expect -c "
    spawn scp -P ${SSH_PORT} ${SSH_NAME}@${SSH_HOST}:/usr/src/backup/workpass/${FILE_NAME}.sql.gz ${BACKUP_DIR}/
    expect {
            \"denied,\" {exit;}
            \"*assword\" {set timeout 300; send \"${SSH_PWD//\"/\\\}\r\";exp_continue;}
            \"(yes/no)?\" {send \"yes\r\";}
        }

# 将"替换为\\"说明
# echo ${SSH_PWD//\"/\\\"}
# 将"替换为\"，全部替换格式为${string//findString/replaceString},如${string//s/a}，下面其实是将///"其实是/"
```


