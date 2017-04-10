---
title: 图标base64代码简易导出方案
date: 2017-04-10 14:25:41
tags: Tips系列
categories: 移动端
---

有时候需要使用`base64`去输出简单的`icon`，举个例子：

``` css
liked{ background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACQAAAAkCAYAAADhAJiYAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAASJJREFUeNpi/P//P8NgAkwMgwyMOogQYKFEc/9ZcxMglQTEfEBcBMSvSDWj0PgkdUII6BhTIDUNiM2AWAOIJwAx20BGWSKafjUgjhtIB6lgEQsYSAfxYhEToTRdUuKgFzgyicJAOegqDnHDgXLQWRzi+UCsOBAO2g/Ev7CIcwBxCt0dBCzQPgCplzikLahWUgMLvLlASp/C3MsPxKAiuBGItwHxGTxqTQiFEKWOgQFmINajRpT9G2y1fQQQvxs0DgIm1ntA6s6gcRAwUddDa/BBE2W+oy1GAg66MtgcBGp4XRhMDuIGYsHB1MjfAMQCgymEBAZbGno2GKuOvYOp6vgGpI4OtoJxMxDPAuJPFJgNak2eI1UT4+hwzKiDhpuDAAIMAIapMxQx+M1OAAAAAElFTkSuQmCC);}
```

得到以上代码只需将图片拖到`chrome浏览器`中查看源码

![](http://p1.bqimg.com/567571/3ab758a741d031a2.jpg)

复制`Sources`里面的图片`base64`代码设置在css背景就可以了