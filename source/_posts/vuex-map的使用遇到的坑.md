---
title: vuex map的使用遇到的坑
date: 2017-04-07 01:22:00
tags: [学习,Vue.js]
categories: Vue.js
---

vuex的map有如下写法

```js
import { mapGetters } from 'vuex'

export default {
  // ...
  computed: {
  // 使用对象展开运算符将 getters 混入 computed 对象中
    ...mapGetters([
      'doneTodosCount',
      'anotherGetter',
      // ...
    ])
  }
}
```

其中的使用扩展符`...`来进行对象展开运算，标准的ES6只对数组有效，故一直报错`...`<!-- more -->

查明原因原来是Babel的不同等级支持度不一样，需要设置不同的stage

解决方案

```bash
# 安装babel-preset-stage-2
$ npm i babel-preset-stage-2 -D
```

```js
// 设置.babelrc，新增了stage-2
{
  "presets": [
    ["es2015", { "modules": false }],
    "stage-2"
  ]
}
```

再次启动程序`npm run dev`，不报错了

汗！真是折腾，官网vuex没有告知这一点

**参考：** [如何区分Babel中的stage-0,stage-1,stage-2以及stage-3（一）](http://www.cnblogs.com/chris-oil/p/5717544.html)