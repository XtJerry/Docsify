## git代理-查看

```shell
    git config --global --get http.proxy
    git config --global --get https.proxy
```

## git代理-设置
```shell
    git config --global http.proxy 127.0.0.1:7890
    git config --global https.proxy 127.0.0.1:7890
```

## git代理-取消
```shell
  git config --global --unset http.proxy
  git config --global --unset https.proxy
```


