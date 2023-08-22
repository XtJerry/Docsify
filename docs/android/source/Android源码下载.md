*最近打算看一下Android源码，想通过官方下载，不料搞了一下午才下载下来，把遇到的问题记录一下*
### 一、准备资料
1、[Google官方资料](https://source.android.google.cn/setup/downloading)

### 二、根据官方资料操作
``` bash
# 1、安装 Repo
$ mkdir ~/bin
$ PATH=~/bin:$PATH

# 2、下载 Repo 工具，并确保它可执行
$ curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
$ chmod a+x ~/bin/repo

# 3、创建源码保存目录
$ mkdir -p /Volumes/AndroidSource/AndroidSource5.1
$ cd /Volumes/AndroidSource/AndroidSource5.1

# 4、设置Git信息(需要google账号关联的电子邮箱)
$ git config --global user.name "Your Name"
$ git config --global user.email "you@example.com"
# git config --global user.name Join
# git config --global user.email join@**.com

# 5、repo 初始化(使用代理本处要出异常，下面讲解)
# 5.1获取最新
$ repo init -u https://android.googlesource.com/platform/manifest
# 5.2获取指定版本(本处源码版本为:android-5.1.1_r25)
$ repo init -u https://android.googlesource.com/platform/manifest -b android-5.1.1_r25

# 6、下载Android当前分支源代码
$ repo sync -c -j8

```

### 三、异常处理
1、不能访问异常
``` bash
# 异常信息如下
Downloading Repo source from https://gerrit-google.tuna.tsinghua.edu.cn/git-repo
fatal: Cannot get https://gerrit-google.tuna.tsinghua.edu.cn/git-repo/clone.bundle
fatal: error [Errno 8] nodename nor servname provided, or not known
fatal: cloning the git-repo repository failed, will remove '.repo/repo' 
# 或则
Downloading Repo source from https://gerrit.googlesource.com/git-repo
fatal: Cannot get https://gerrit.googlesource.com/git-repo/clone.bundle
fatal: error [Errno 60] Operation timed out
fatal: cloning the git-repo repository failed, will remove '.repo/repo' 

# 问题解决
# A:如果浏览器不能访问https://gerrit-google.tuna.tsinghua.edu.cn/git-repo/clone.bundle，不能访问或则代理失效
# B:如果浏览器可以访问https://gerrit-google.tuna.tsinghua.edu.cn/git-repo/clone.bundle，则配置一下：
# export http_proxy=IP:Port
# export https_proxy=IP:Port
# Mac配置
$ export http_proxy=127.0.0.1:1087
$ export https_proxy=127.0.0.1:1087
```
2、证书验证失败
``` bash
# 异常信息如下
Downloading Repo source from https://gerrit.googlesource.com/git-repo
fatal: Cannot get https://gerrit.googlesource.com/git-repo/clone.bundle
fatal: error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:1129)
fatal: cloning the git-repo repository failed, will remove '.repo/repo' 


# 问题解决
# repo是使用python编写的，可以直接通过vi或则文本编辑软件打开
$ vi repo
# 找到“import sys”,并在下面添加
import sys
# 下面为添加内容
import ssl
ssl._create_default_https_context = ssl._create_unverified_context

```

3、Python运行异常或则版本不对
``` bash
# 直接删除python重新安装
# Mac运行可能不是默认的python，可以根据提示及路径直接删除
```




