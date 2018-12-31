---
title: Mac 设置 Terminal 代理的方式
date: 2018-12-31 23:59:59
tags: 工具
categories: Mac
---

> 通常在 Mac 系统中的 终端（即 Terminal 软件）是不跟随系统代理设置的，需要通过第三方工具或者其他方式来调整代理，本篇教程采用更改环境变量的形式来做到代理，简单易用。

## 在当次当前窗口实现代理访问

```bash
$ export http_proxy=http://127.0.0.1:1087 # 配置http
$ export https_proxy=http://127.0.0.1:1087 # 配置https
$ export socks5_proxy=socks5://127.0.0.1:1086 # 配置socks5
```

<!-- more -->

## 保持长期使用

以 zsh 为例，存到 .zshrc 文件中

```
alias proxy='export socks5_proxy=socks5://127.0.0.1:1086;export http_proxy=http://127.0.0.1:1087;export https_proxy=http://127.0.0.1:1087;'
alias unproxy='unset socks5_proxy http_proxy https_proxy'
```

source 生效

```
. ~/.zshrc 或 source ~/.zshrc
```

测试和使用

```
$ proxy
$ curl cip.cc
IP	: 35.189.*.*
地址	: 中国  台湾  彰化县
运营商	: cloud.google.com

数据二	: 美国 | 加利福尼亚州圣克拉拉县山景市谷歌公司

数据三	: 美国南卡罗来纳 | 谷歌

URL	: http://www.cip.cc/35.189.*.*
$ unproxy
$ curl cip.cc
IP	: 61.242.*.*
地址	: 中国  广东  广州
运营商	: 联通

数据二	: 广东省广州市 | 联通

数据三	: 中国广东省广州市 | 联通
```

**参考：**

 - https://github.com/mrdulin/blog/issues/18