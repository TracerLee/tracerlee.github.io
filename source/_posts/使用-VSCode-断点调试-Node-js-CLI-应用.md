---
title: 使用 VSCode 断点调试 Node.js CLI 应用
date: 2020-03-07 23:24:47
tags:
---

使用 `commander.js` 写 Node.js CLI 应用已经是老生常谈的问题了，网上可以找到各种教程。

最近在写一个稍微有点复杂的 CLI，以前就是 `console.log` 一把梭的 debug。然而，看个变量都太费心了，如果能够断点调试就方便多了。

Google 一番后，对于 VSCode 强大的 Node.js 调试功能，果然不在话下。<!-- more -->

下面用一个简单的代码来演示调试效果。

## 使用 `commander.js` 创建一个 CLI

1. 新建一个文件夹，如 `my-cli`, 首先初始化 `package.json`，安装 `commander.js` 和你需要的其他 npm 包。

```bash
npm init -y
npm install -p commander
```

2. 新建主入口文件 `index.js`，这里写一点简单的测试代码来演示。

```javascript
#!/usr/bin/env node

'use strict'

const commander = require('commander')

program
  .command('test')
  .action(() => {
    console.log('hello world')
  })

program.parse(process.argv)
```

3. 修改 `package.json`，新增 `bin` 字段，执行 `npm link` 后，就可以使用 `my-cli` 执行命令了, 更多 `commander.js` 使用参见 [Github](https://github.com/tj/commander.js)。

```json
{
  "name": "my-cli",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "bin": {
    "my-cli": "./index.js"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "commander": "^4.1.1"
  }
}
```

```bash
npm link # 建立软链接

my-cli test # 输出 hello world
```

## 配置 VSCode

1. 如图所示，创建 launch.json 文件，新增 `args` `console`，前者表示运行 CLI 的参数，后者表示使用 VSCode 集成的终端来调试。其中，`args` 根据需要调整参数，也可以使用 `configurations` 来配置多个测试用例。

![截屏2020-03-0722.34.16.png](http://ww1.sinaimg.cn/large/68731f4agy1gclr6jypimj21ci0ysjzk.jpg)

```
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "启动程序",
      "skipFiles": [
        "<node_internals>/**"
      ],
      "program": "${workspaceFolder}/index.js",
      "args": ["test"],
      "console": "integratedTerminal"
    }
  ]
}
```

2. 在所需代码处添加断点后，F5 启动当前配置进行调试，如图所示，按照所期待的那样，代码在断点处暂停了，可以和往常一样方便地看到调用堆栈和变量。 


![截屏2020-03-0722.44.14.png](http://ww1.sinaimg.cn/large/68731f4agy1gclr6k4ojxj21ey17infg.jpg)

**本篇完**

代码参见：https://github.com/TracerLee/FE-demos/tree/master/blog/my-cli

## 参考

- https://stackoverflow.com/questions/29955126/debugging-node-js-cli-application-with-vscode
- https://code.visualstudio.com/docs/editor/node-debugging#_node-console