## 一、Apk签名信息查看

---

### 1. APK文件签名查看

```shell
# 一、已签名apk签名信息查看

keytool -printcert -jarfile  app.apk

# 二、Jks签名文件查看

# 输入密码为签名文件密码
keytool -v -list -keystore keystore.jks
```
输出
```shell
输入密钥库口令:  
密钥库类型: JKS
密钥库提供方: SUN

您的密钥库包含 1 个条目

别名: your_certificate_alias
创建日期: 2019年12月16日
条目类型: PrivateKeyEntry
证书链长度: 1
证书[1]:
所有者: CN=zftg, OU=zftg, O=zftg, L=CD, ST=SC, C=ZH
发布者: CN=zftg, OU=zftg, O=zftg, L=CD, ST=SC, C=ZH
序列号: 7e128232
生效时间: Mon Dec 16 11:31:54 CST 2019, 失效时间: Fri Dec 09 11:31:54 CST 2044
证书指纹:
         SHA1: 2C:E4:6A:62:46:B7:6E:2A:B0:84:7F:57:34:F5:D8:FD:CA:70:D0:F4
         SHA256: 2A:3B:11:5F:9B:38:DA:34:5B:37:00:CE:9F:AC:E3:64:24:6A:50:5A:D8:D7:11:68:23:10:41:3D:57:6D:E7:34
签名算法名称: SHA256withRSA
主体公共密钥算法: 2048 位 RSA 密钥
版本: 3

扩展:

#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: 7D 27 85 6B 79 47 9B 71   88 3E 75 62 EE 4A 77 8C  .'.kyG.q.>ub.Jw.
0010: 33 23 A5 C1                                        3#..
]
]

*******************************************
*******************************************

```

### 2. 签名-公钥查看(String),部分平台需要使用

```shell
keytool -list -rfc -keystore your_keystore_file.jks
# 输入密码为签名文件密码

```

输出

``` shell
输入密钥库口令:  
密钥库类型: JKS
密钥库提供方: SUN

您的密钥库包含 1 个条目

别名: your_certificate_alias
创建日期: 2019年12月16日
条目类型: PrivateKeyEntry
证书链长度: 1
证书[1]:
-----BEGIN CERTIFICATE-----
MIIDRzCCAi+gAwIBAgIEfhKCMjANBgkqhkiG9w0BAQsFADBUMQswCQYDVQQGEwJa\
SDELMAkGA1UECBMCU0MxCzAJBgNVBAcTAkNEMQ0wCwYDVQQKEwR6ZnRnMQ0wCwYD
.
.
.
J7BD4TkqYto07i95ubQq0JrPLiwqWSM1/N7UM5dnR21k2JDaVJ4AzcMocfbUk4E+
4To17a67zqalJPKK7BJBTFWrkyWLawjFqjR/
-----END CERTIFICATE-----


*******************************************
*******************************************

```

### 3. 签名-指纹查看(MD5),部分平台需要使用

```shell
# 第一步，生成certificate.pem

# your_certificate_alias 别名
keytool -exportcert -alias your_certificate_alias -keystore your_keystore_file.jks -rfc -file certificate.pem
# 输入密码为别名密码

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

+ 系统签名(platform.pk8和platform.x509.pem)，(~~
  该处[platform.x509.pem](https://github.com/XtJoin/public/blob/main/android/SystemSign/Firefly/platform.x509.pem)
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

