---
title: 在nodejs环境实现phantomjs的截图功能
date: 2016-04-19 03:13:45
tags: 学习
---

以electron的nodejs上下文开发此截图功能

关键代码

```javascript
// web-side
var ipc = require('ipc');
var wrap = document.querySelector('.rap');
document.querySelector('button').onclick = function () {
  ipc.send('screenshot',{
    width: wrap.offsetWidth,
    height: wrap.offsetHeight
  });
};
```

<!-- more -->

```javascript
// server-side
let phInstance,sitepage,url="file:///C:/git/electron-quick-start/index.html",fname="./index.jpg";
ipc.on('screenshot', function (event,arg) {
  phantom.create()
      .then(function(instance) {
          console.log("1 - instance")
          phInstance = instance;
          return instance.createPage();
      })
      .then(function(page) {
          console.log("2 - page")
          sitepage = page;
          sitepage.property('viewportSize', { width: 2100, height: 1200 });
        return sitepage.open(url);
      })
      .then(function(status) {
            console.log("3 - render")
            sitepage.evaluate(function () {
              const wrap = document.querySelector('.android-wrap');
              return {
                left:wrap.offsetLeft,
                top:wrap.offsetTop
              }; 
            }).then(function (offset) {
              sitepage.property('clipRect', {top: offset.top, left: offset.left, width:arg.width, height:arg.height}).then(function() {
                sitepage.render(fname).then(function(finished) { 
                  console.log("\t\t\t---> finished");
                  sitepage.close();
                  phInstance.exit();
                  callback({msg: 'ok'})
                  phantom.exit();
                  return;
                });
              });
            });
      })
});
```

* 使用ipc作为客户端和服务端的通讯接口，不过还要改成客户端ipcMain的，旧接口将废弃

* `require('phantom')`引入node模块, github地址：https://github.com/amir20/phantomjs-node

* ```javascript
  page.property('plainText').then(function(content) {
    console.log(content);
  });
  ```

  使用`property`功能对`viewportSize`和`clipRect`进行设置，

* `page.render(fname)`对文件进行输出，`fname`为路径加名称

* `page.evaluate()`回调函数作用域可以访问客户端作用域（既可以访问`window`、`location`等对象）

## 参考资料

* [ipcMain](https://wizardforcel.gitbooks.io/electron-doc/content/api/ipc-main.html)
* [Access document when evaluating page #384](https://github.com/amir20/phantomjs-node/issues/384)
* [page#property](https://github.com/amir20/phantomjs-node#pageproperty)