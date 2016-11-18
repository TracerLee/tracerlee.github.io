---
title: npm实用模块
date: 2016-04-19 02:34:07
tags:
---

## nvm-windows: Node Version Manager(nvm) for Windows

这是一个windows系统下Node.js版本的管理工具。

可以直接下载可执行程序安装，[下载地址](https://github.com/coreybutler/nvm/releases)。

Github: https://github.com/coreybutler/nvm-windows

<!-- more -->

### 常用命令

安装Node.js

```bash
nvm install <version>
```

使用某个版本的Node.js

```bash
nvm use <version>
```

其它使用方式直接在命令行执行`nvm`即可查看。

### 安装过程遇到的问题

1. 使用nvm按照好Node.js最新版本后，切换到此版本，执行`node -v`，结果提示**”此应用无法在你的电脑上运行“**。 ![](http://ww2.sinaimg.cn/large/68731f4ajw1f492r3a3o2j20k305jt8v.jpg)


​	*解决过程*

* **确定是否是系统问题**，我的环境是win10 64bit，安装nvm前已有一个官方下载的Node.js版本v0.10.25。Google搜索和查看github issue后没有发现有人反馈这个问题。
* 发现使用nvm按照的v6.2.0和v4.3.0版本均无法执行，但是本机原来被拷贝到nvm目录下的v0.10.25版本是可用的，于是到nvm目录下设置`node.exe`程序为兼容模式尝试运行，仍然无效。
* **重装大法好**，在控制面板卸载了Node.js，然后`nvm uninstall`了所有版本，接着使用`nvm install`Node.js，结果就好啦，愉快地运行了`node -v`。
* **问题本质**应该是在安装nvm之前未卸载干净本地的Node.js造成的。

1. 使用`nvm install`安装Node.js下载很慢，这可怎么解决？

     因为nvm默认从官方源下载Node.js，国内下载慢是正常的，如果有VPN代理的话，可以使用nvm自带的代理功能`nvm proxy`，配置IP和端口就可以了。