*一般需要将apk作为系统签名的app都是定制化开发，需要获取系统的权限。*
### 一、准备
1、系统签名(platform.pk8和platform.x509.pem)
2、签名工具[signapk.jar](https://github.com/XtJoin/public/blob/main/android/SystemSign/signapk.jar)
3、java运行环境[JDK](https://www.oracle.com/cn/java/technologies/javase/javase-jdk8-downloads.html)或[OpenJDK](https://openjdk.java.net)
4、将AndroidManifest设置为系统app(android:sharedUserId="android.uid.system")
5、编译后的apk

### 二、操作

``` bash
# java -jar signapk.jar platform.x509.pem platform.pk8 Unsigned.apk Signed.apk
$ java -jar signapk.jar platform.x509.pem platform.pk8 Test2.1.apk Test.2.1.Signed.apk

# 未出错表示签名完成
```
上文命令中的[platform.x509.pem](https://github.com/XtJoin/public/blob/main/android/SystemSign/Firefly/platform.x509.pem)和[platform.pk8](https://github.com/XtJoin/public/blob/main/android/SystemSign/Firefly/platform.pk8)是Firefly(萤火虫)平板系统的系统签名文件。


### APK文件签名查看
```shell
keytool -list -printcert -jarfile app.apk
```

### JKS文件签名查看
```shell
keytool -v -list -keystore keystore.jks

```