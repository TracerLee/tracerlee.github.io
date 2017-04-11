---
title: 解决安装Node.js网速慢的问题
date: 2017-04-11 21:57:30
tags: Tips系列
categories: Node.js
---

国内安装各种环境依赖网速真的无法直视，除了翻飞机之外就是找到合适的源了，以下的核心就是切换Node.js和npm的源，感谢阿里！<!-- more -->

```
# 多版本安装要用nvm这个工具，windows下是nvm-windows
# Mac下nvm依赖git，先安装homebrew去安装git，这里不展示安装过程
# 安装好git之后
# 安装nvm
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash

# 安装完毕，设置nvm的Node.js源（阿里源），在~/.bash_profile里面新增一行，没有.bash_profile就touch一个
export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
# 更新配置
source ~/.bash_profile

# 安装Node.js
nvm install 6

# 安装完毕后，安装nrm来切换cnpm
npm --registry=https://registry.npm.taobao.org install nrm -g

#安装完毕后，切换cnpm
nrm use taobao

# 结束Node.js安装
```

**参考：** 
- http://www.w2bc.com/article/189615 
- nvm: https://github.com/creationix/nvm
- homebrew: https://brew.sh/index_zh-cn.html