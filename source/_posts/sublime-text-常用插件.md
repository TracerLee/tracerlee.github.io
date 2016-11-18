---
title: sublime text 常用插件
date: 2016-04-09 03:16:34
tags: [工具]
---

sublime text之所以强大其中一个原因就是强大的插件支持，具体如何安装Package Control请参考官网说明（https://packagecontrol.io/installation）。

此文默认读者拥有一定sublime text插件安装经验，只介绍我认为实用的一些插件。

# 语法检测

## SublimeLinter

只是一个管理工具

## SublimeLinter-jshint 

SublimeLinter的JavaScript检测的扩展插件<!-- more -->

依赖jshint，需要用node.js全局安装：

```bash
$ node i jshint -gd
```

参考配置:

```json
{
  "asi"      : true,  // 控制“缺少分号”的警告
  "browser"  : true,  // 预定义全局变量
  "eqeqeq"   : false, // 强制使用三等号
  "-W041"    : false, // 这个可以忽略没有使用全等于的警告 
  "eqnull"   : true,   
  "es3"      : true,  // 默认是es3语法检测，若要开启es其他版本，删除此行并设置"esversion":6
  "expr"     : true,
  "jquery"   : true,  // 定于全局变量
  "latedef"  : true,  // 禁止定义之前使用变量，忽略 function 函数声明
  "laxbreak" : true,
  "nonbsp"   : true,
  "strict"   : true,  // 严格模式
  "undef"    : true,  // 变量未定义
  "unused"   : true,  // 变量未使用
  "devel"    : true,  // 忽略console,alert警告
  "globals"   : {     // 全局变量忽略列表
    "seajs"   : true,
    "define"  : true
  }   
}
```

jshint配置默认依赖父深度为3的文件夹下.jshintrc文件的配置。

深度可在SublimeLinter用户设置中设置"rc_search_limit": 3。

__新建一个无文件名文件__的方法，比如新建".jshintrc"，就直接新建".jshintrc."，多了个"."就可以了。

## SublimeLinter-contrib-eslint

JS/ES6/JSX等语法检测插件，较为强大，缺点是速度比较慢

依赖：[eslint](http://eslint.org/)、babel-eslint，需要npm全局安装

参考配置：

```json
{
	"env": {
		"browser": true,
		"node": true,
		"es6": true
	},
	"parser": "babel-eslint",
	"ecmaFeatures": {
		"jsx": true
	},
	"rules": {
		"semi": [2, "always"],
		"quotes": [2, "single"]
	}
}
```

如果同时安装了eslint和jshint的话，在Sublimelinter设置中各自的@disable可以对其禁用，否则两个会同时检测语法。

ESLint规则官方文档http://eslint.org/docs/rules/

如图，每一个配置的代码中，注释说明了配置的名值对

![](http://ww1.sinaimg.cn/large/68731f4agw1f2r03o3xuvj20lg04m3z4.jpg)