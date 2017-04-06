---
title: Mac在终端使用ss代理通过ssh连接vps
date: 2017-04-07 01:30:21
tags: 工具
categories: Mac
---

> 如果是国内vps（阿里云等），一般使用`ssh root@ip`去连上vps是不会有什么大问题的，但是国外vps连接就要看节点速度了，甚至墙太高导致无法得到响应。
> 如果手上有一个ss可以科学上网，那又如何使用ss在终端代理连接vps呢

1.安装`proxychains-ng`

```
brew install proxychains-ng
```
<!-- more -->
2.编辑配置

```
vim /usr/local/etc/proxychains.conf

#注释最后一行，在后面新增一行，根据ss的socks5端口配置，我的是1086，最终如下
-------------------------------
#socks4         127.0.0.1 9050
socks5  127.0.0.1 1086
-------------------------------
```

3.由于OSX10.11的限制，需要拷贝一个`ssh`执行文件到其他地方，这里拷贝到`~`

```
cp /usr/bin/ssh ~
```

4.连接vps，完成任务！

```
proxychains4 ~/ssh root@ip
```

5.其他

```
# 可以给proxychain4起别名
alias pc='proxychains4'
# 之后这样子调用就可以了
pc ~/ssh root@ip
```

**参考：**

- https://github.com/rofl0r/proxychains-ng/issues/78