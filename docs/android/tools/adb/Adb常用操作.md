### adb命令常用操作
``` bash
# 连接设备
# adb connect ip[:port(默认5555)]
$ adb connect 192.168.11.1:5555

# 断开设备
# adb disconnect ip[:port]
$ adb disconnect 192.168.11.1

# 进入设备
$ adb shell

# 安装apk
# adb install [-r 覆盖] app.apk
$ adb install -r test.apk

# 卸载apk
# adb uninstall package
$ adb uninstall com.xtpla.test

# 推送文件(PC->Mobile)
# adb push pfile mfile
$ adb push test.txt /sdcard/test.txt

# 拉取文件(Mobile->PC)
# adb pull mfile pfile
$ adb pull /sdcard/test.txt .

# 端口转发(将Android端口映射到电脑)
# 将手机8080映射到PC8081,命令执行后会在PC创建8081服务
$ adb forward tcp:8081 tcp:8080

# 端口转发,将电脑端口映射到Android
# $将电脑9080映射到Android的9081
$ adb reverse tcp:9081 tcp:9080

# IP调试(需要先使用usb连接)
$ adb tcpip 5555
# 切换为usb
$ adb usb
# 使用adb connect ip:5555 连接
$ adb connect 192.168.10.91:5555

```