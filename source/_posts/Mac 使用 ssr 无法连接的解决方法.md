---
title: Mac 使用 ssr 无法连接的解决方法
date: 2022-03-30 15:11:05
tags: Others
---

## 问题
不知道什么原因mac系统下的ssr无法连接，不管是全局模式还是PAC模式，看了网上很多人都有反应这个问题，不管是用break安装的还是直接从Git下载来的。

## 解决办法

在命令行下操作进行代理，然后在通过socks5, 使用软件管理，或者直接在需要使用的软件上添加代理

## 步骤

1. 进入 shadowsocksX-NG-R8 的安装目录

	```
	cd /Users/用户名/Library/Application Support/ShadowsocksX-NG
	```

2. 进入目录后可以看到以`ss-local`开头的文件夹，后面的数据可能是内核号，不一定一样，比如本人文件夹名称就是 ss-local-2.5.6.9.static ，记住这个文件名，下面要用。

3. 编写配置文件 ss-local-config.json ，里面的具体参数按自己的服务器填写，但要注意的是 "local_port": #5500 这个端口号，是接下来电脑连接代理的socks5的端口，不能弄错。

4. 新建脚本文件 ssr.sh，保存于任何位置都可以

	```
	#!/usr/bin/env bash

	cd "$HOME/Library/Application Support/ShadowsocksX-NG/" || exit

	"#第2步得到的文件夹路径/ss-local" -c "ss-local-config.json" -f "ss-local-config.pid" -v
	```

现在只要运行ssr.sh脚本，就等于开启了代理，只需要连接上面设置的socks5端口就可以使用代理路线了。

最后提醒大家，科学上网，理性头脑，爱我中国！

## 引用

https://blog.haitianhome.com/mac-ssr-no-connection.html
