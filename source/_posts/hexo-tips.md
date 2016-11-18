---
title: Hexo Tips
date: 2016-04-03 22:45:32
tags: [工具,Tips系列]
---
记录一下部署Hexo的一些tips
## 安装

### 前提

* Node.js
* Git

### 开始安装

``` bash
$ npm install -g hexo-cli
```

<!-- more -->完成这个之后就可以愉快地使用hexo命令啦！

## 写作

一般的写作方式就不阐述了，自行看[文档](https://hexo.io/zh-cn/docs/writing.html)。

* 如何生成图片地址，这里介绍几种方式。

  * github项目存储，把图片存储到项目下面
  * 使用chrome插件“[新浪微博图床](https://chrome.google.com/webstore/search/%E6%96%B0%E6%B5%AA%E5%BE%AE%E5%8D%9A%E5%9B%BE%E5%BA%8A?hl=zh-CN)”

* 文章多标签（实质是遵照YAML语法）

  * ``` yaml
    tags: [tag1,tag2] 
    ```

  * ```yaml
    tags:
    - tag1
    - tag2
    ```

## 部署

* 域名映射

  到阿里云里面域名列表中“解析”，设置如下

  ![](http://ww2.sinaimg.cn/large/68731f4agw1f2jyrd0otzj20pz04hq3n.jpg)
* github项目文件覆盖(--force)问题，将自定义文件放到source即可。（BTW，通过修改源码node_modules\hexo-deployer-git\libdeployer.js:72可以根治问题，如图为未修改源码）

  ![](http://ww1.sinaimg.cn/large/68731f4agw1f2jyucy7gfj20ga03ywfa.jpg)
* README.md被编译成README.html的问题，通过设置_config.yml中skip_render: [README.md]可以解决问题

## 常用命令

初始化当前目录为Hexo项目

```bash
$ hexo i
```

新建文章

``` bash
$ hexo new 'My New Post'
```

生成静态页

``` bash
$ hexo g
```

启动服务

``` bash
$ hexo s
```

部署到远程站点

``` bash
$ hexo d
```

生成静态页并部署到远程站点

``` bash
hexo g -d
```

## 个性化

### 如何设置「阅读全文」？ 

在首页显示一篇文章的部分内容，并提供一个链接跳转到全文页面是一个常见的需求。 NexT 提供三种方式来控制文章在首页的显示方式。 也就是说，在首页显示文章的摘录并显示 **阅读全文** 按钮，可以通过以下方法：

1. 在文章中使用 `` 手动进行截断，Hexo 提供的方式 推荐

2. 在文章的 [front-matter](https://hexo.io/docs/front-matter.html) 中添加 `description`，并提供文章摘录

3. 自动形成摘要，在 主题配置文件 中添加：

   ```
   auto_excerpt:
     enable: true
     length: 150
   ```

   默认截取的长度为 `150` 字符，可以根据需要自行设定

建议使用 `<!-- more -->`（即第一种方式），除了可以精确控制需要显示的摘录内容以外， 这种方式也可以让 Hexo 中的插件更好的识别。



## 参考资料

* [Hexo官方中文文档](https://hexo.io/zh-cn/docs/)
* [GitHub Pages 绑定来自阿里云的域名](http://quantumman.me/blog/setting-up-a-domain-with-gitHub-pages.html)
* https://github.com/hexojs/hexo/issues/1146
* [常见问题](http://theme-next.iissnan.com/faqs.html)