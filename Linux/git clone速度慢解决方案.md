# git clone速度慢解决方案

目前值得尝试的就两种，改host文件对我没用

## gitee曲线救国

1. 注册码云账号[https://gitee.com/](https://gitee.com/)
2. 在右侧个人头像那里，点击`+`号，新建仓库
3. 点击从`github`导入
4. 填好信息导入
5. 导入后`clone`即可



## socks5代理(只针对https的clone)

大前提是使用了`ss`，然而就算使用了，你也会发现。

我喵喵喵的`clone`还是那么慢

原因是`clone`没走 `ss`

我们需要设置它走`ss`，`ss`用的是`socks5`端口

为了防止误伤了国内的码云仓库，我们要这样搞，只针对`github`的域名进行代理

```shell
git config --global http.https://github.com.proxy socks5://127.0.0.1:1080
git config --global https.https://github.com.proxy socks5://127.0.0.1:1080
```

尾部的1080是你的`ss`本地监听端口，你的端口多少可以看看你的`ss`设置，不要看我～

这样就可以啦。

既然写了如何上贼船，还要教教怎么下车。取消上面的代理：

```shell
git config --global --unset http.proxy
git config --global --unset https.proxy
```



