## 一、Apk签名信息查看

---

### 1. APK文件签名查看

```shell
# 一、已签名apk签名信息查看
keytool -printcert -jarfile  app.apk

# 二、Jks签名文件查看
keytool -v -list -keystore keystore.jks
```

### 1. 指纹获取(MD5),部分平台需要使用

```shell
# 第一步，生成certificate.pem
keytool -exportcert -alias your_certificate_alias -keystore your_keystore_file.jks -rfc -file certificate.pem

# 第二步，查看指纹
# Mac
openssl x509 -noout -fingerprint -md5 -in certificate.pem

# Windows(Git Bash工具)
openssl x509 -noout -fingerprint -md5 -in certificate.pem

```

## 二、系统Apk签名

---

*一般需要将apk作为系统签名的app都是定制化开发，需要获取系统的权限。*

### 1、准备

+ 系统签名(platform.pk8和platform.x509.pem)，(~~该处[platform.x509.pem](https://github.com/XtJoin/public/blob/main/android/SystemSign/Firefly/platform.x509.pem)
  和[platform.pk8](https://github.com/XtJoin/public/blob/main/android/SystemSign/Firefly/platform.pk8)是Firefly(萤火虫)
  平板系统的系统签名文件。~~)
+ 签名工具[signapk.jar](https://github.com/XtJoin/public/blob/main/android/SystemSign/signapk.jar)
+ java运行环境[JDK](https://www.oracle.com/cn/java/technologies/javase/javase-jdk8-downloads.html)
或[OpenJDK](https://openjdk.java.net)
+ 将AndroidManifest设置为系统app(android:sharedUserId="android.uid.system")
+ 编译后的apk

### 2、操作

``` bash
# java -jar signapk.jar platform.x509.pem platform.pk8 Unsigned.apk Signed.apk
$ java -jar signapk.jar platform.x509.pem platform.pk8 Test2.1.apk Test.2.1.Signed.apk

# 未出错表示签名完成
```

