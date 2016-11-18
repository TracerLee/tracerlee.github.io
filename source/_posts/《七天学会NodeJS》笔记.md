---
title: 《七天学会NodeJS》笔记
date: 2016-05-07 23:35:26
tags: 笔记
---

Node.js是一个运行javascript的环境，NodeJS的作者创造NodeJS的目的是为了实现高性能Web服务器。

> Node.js® is a JavaScript runtime built on [Chrome's V8 JavaScript engine](https://developers.google.com/v8/). Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. Node.js' package ecosystem, [npm](https://www.npmjs.com/), is the largest ecosystem of open source libraries in the world.
>
> —— [Node.js官网](https://nodejs.org/en/)



这篇文章主要摘录一些个人认为的重点，希望加深对Node.js的熟悉。



## 一、Node.js基础

#### exports

`exports`对象是当前模块的导出对象，用于导出模块公有方法和属性。别的模块通过`require`函数使用当前模块时得到的就是当前模块的`exports`对象。以下例子中导出了一个公有方法。

<!-- more -->

```javascript
exports.hello = function () {
	console.log('Hello World!');
};
```
> `exports`默认是`{}`，是`module.exports`的一个引用。
>

#### 模块初始化

一个模块中的JS代码仅在模块<u>**第一次被使用时执行一次**</u>，并在执行过程中初始化模块的导出对象。之后，<u>**缓存起来**</u>的导出对象被重复利用。

#### 主模块

通过命令行参数传递给NodeJS以启动程序的模块被称为主模块。主模块负责调度组成整个程序的其它模块完成工作。例如通过以下命令启动程序时，`main.js`就是主模块。

```bash
$ node main.js
```
> 说明多次`require`不会导致执行多次模块内的程序内容。
>



- NodeJS是一个JS脚本解析器，任何操作系统下安装NodeJS本质上做的事情都是把NodeJS执行程序复制到一个目录，然后保证这个目录在系统PATH环境变量下，以便终端下可以使用`node`命令。
- 终端下直接输入`node`命令可进入命令交互模式，很适合用来测试一些JS代码片段，比如正则表达式。
- NodeJS使用[CMD](http://wiki.commonjs.org/)模块系统，主模块作为程序入口点，所有模块在执行过程中只初始化一次。
- 除非JS模块不能满足需求，否则不要轻易使用二进制模块，否则你的用户会叫苦连天。

> Node.js使用CMD规范，Sea.js也是遵循CMD规范。
>



## 二、代码的组织和部署

### 模块路径解析规则

1. 内置模块
2. node_modules目录
3. NODE_PATH环境变量



### 包（package）

当模块的文件名是`index.js`，加载模块时可以使用模块所在目录的路径代替模块文件路径，因此接着上例，以下两条语句等价。

```javascript
var cat = require('/home/user/lib/cat');
var cat = require('/home/user/lib/cat/index');
```

这样处理后，就只需要把包目录路径传递给`require`函数，感觉上整个目录被当作单个模块使用，更有整体感。

> 所以就是`require`一个文件夹就等同于`require`里面的`index.js`模块，这个很方便。
>



#### package.json

```json
{
"name": "node-echo",
"main": "./lib/echo.js",
"dependencies": {
"argv": "0.0.2"
}
}
```
> `name`是包名称，`main`是入口模块（主模块），`dependencies`是依赖模块。
>



#### 版本号

语义版本号分为`X.Y.Z`三位，分别代表主版本号、次版本号和补丁版本号。当代码变更时，版本号按以下原则更新。

```
如果只是修复bug，需要更新Z位。
如果是新增了功能，但是向下兼容，需要更新Y位。
如果有大变动，向下不兼容，需要更新X位。
```


#### NPM

- 使用`npm help `可查看某条命令的详细帮助，例如`npm help install`。
- 在`package.json`所在目录下使用`npm install . -g`可先在本地安装当前命令行程序，可用于发布前的本地测试。
- 使用`npm update `可以把当前目录下`node_modules`子目录里边的对应模块更新至最新版本。
- 使用`npm update -g`可以把全局安装的对应命令行程序更新至最新版。
- 使用`npm cache clear`可以清空NPM本地缓存，用于对付使用相同版本号发布新版本代码的人。



#### 使用NodeJS编写代码前需要做的准备工作

- 编写代码前先规划好目录结构，才能做到有条不紊。
- 稍大些的程序可以将代码拆分为多个模块管理，更大些的程序可以使用包来组织模块。
- 合理使用`node_modules`和`NODE_PATH`来解耦包的使用方式和物理路径。





## 三、文件操作

#### 小文件拷贝

*代码片段*

```javascript
fs.writeFileSync(dst, fs.readFileSync(src)); // dst:目标路径,src:源路径
```

#### 大文件拷贝

*代码片段*

```javascript
fs.createReadStream(src).pipe(fs.createWriteStream(dst));
```

以上程序使用`fs.createReadStream`创建了一个源文件的只读数据流，并使用`fs.createWriteStream`创建了一个目标文件的只写数据流，并且用`pipe`方法把两个数据流连接了起来。连接起来后发生的事情，说得抽象点的话，水顺着水管从一个桶流到了另一个桶。

#### [Buffer（数据块）](http://nodejs.org/api/buffer.html)

#### [Stream（数据流）](http://nodejs.org/api/stream.html)

#### [File System（文件系统）](http://nodejs.org/api/fs.html)

`fs`模块的所有异步API都有对应的同步版本。

```javascript
// 以文件内容读取为例
// 异步写法
fs.readFile(pathname, function (err, data) {
    if (err) {
        // Deal with error.
    } else {
        // Deal with data.
    }
});
// 同步写法
try {
    var data = fs.readFileSync(pathname);
    // Deal with data.
} catch (err) {
    // Deal with error.
}
```

#### [Path（路径）](http://nodejs.org/api/path.html)

#### 遍历目录

目录是一个树状结构，在遍历时一般使用**深度优先+先序遍历算法**。使用这种遍历方式时，下边这棵树的遍历顺序是`A > B > D > E > C > F`。

```
          A
         / \
        B   C
       / \   \
      D   E   F
```

#### 同步遍历、异步遍历

**思路：**某个目录作为遍历的起点。遇到一个子目录时，就先接着遍历子目录。遇到一个文件时，就把文件的绝对路径传给回调函数。回调函数拿到文件路径后，就可以做各种判断和处理。

> 遍历的思路值得去好好琢磨。

#### 文件编码

常用的文本编码有`UTF8`和`GBK`，`UTF8`文件还可能带有BOM。在读取不同编码的文本文件时，需要将文件内容转换为JS使用的`UTF8`编码字符串后才能正常处理。

##### BOM移除

在不同的Unicode编码下，BOM字符对应的二进制字节如下：

```
    Bytes      Encoding
----------------------------
    FE FF       UTF16BE
    FF FE       UTF16LE
    EF BB BF    UTF8
```

移除UTF8 BOM

```javascript
function readText(pathname) {
    var bin = fs.readFileSync(pathname);
    if (bin[0] === 0xEF && bin[1] === 0xBB && bin[2] === 0xBF) {
        bin = bin.slice(3);
    }
    return bin.toString('utf-8');
}
```

##### GBK转UTF8（借助`iconv-lite`）

```javascript
var iconv = require('iconv-lite');
function readGBKText(pathname) {
    var bin = fs.readFileSync(pathname);
    return iconv.decode(bin, 'gbk');
}
```

##### 单字节编码

```javascript
function replace(pathname) {
    var str = fs.readFileSync(pathname, 'binary');
    str = str.replace('foo', 'bar');
    fs.writeFileSync(pathname, str, 'binary');
}
```

#### 小结

* 学好文件操作
* 掌握目录遍历和文件编码



## 四、网络操作

####  [HTTP](http://nodejs.org/api/http.html)

`http`模块提供两种使用方式：

- 作为服务端使用时，创建一个HTTP服务器，监听HTTP客户端请求并返回响应。
- 作为客户端使用时，发起一个HTTP客户端请求，获取服务端响应。

```javascript
// 创建HTTP服务器
http.createServer(function (request, response) {
    var body = [];
    console.log(request.method);
    console.log(request.headers);
    
	// chunk传输
    request.on('data', function (chunk) {
        body.push(chunk);
    });

    request.on('end', function () {
        body = Buffer.concat(body);
        console.log(body.toString());
    });
}).listen(80);

------------------------------------
POST
{ 'user-agent': 'curl/7.26.0',
  host: 'localhost',
  accept: '*/*',
  'content-length': '11',
  'content-type': 'application/x-www-form-urlencoded' }
Hello World
```

> `request`和`response`对象除了用于读写头数据外，都可以当作数据流来操作。

#### [HTTPS](http://nodejs.org/api/https.html)

#### [URL](http://nodejs.org/api/url.html)

##### URL的各组成部分

```
                           href
 -----------------------------------------------------------------
                            host              path
                      --------------- ----------------------------
 http: // user:pass @ host.com : 8080 /p/a/t/h ?query=string #hash
 -----    ---------   --------   ---- -------- ------------- -----
protocol     auth     hostname   port pathname     search     hash
                                                ------------
                                                   query
```

> 常用方法 `parse` `format` `resolve` 等

#### [Query String](http://nodejs.org/api/querystring.html)

> `querystring.parse('foo=bar&baz=qux')`
>
> `querystring.stringify({ foo: 'bar', baz: ['qux', 'quux'], corge: '' })`

#### [Zlib](http://nodejs.org/api/zlib.html)

`zlib`模块提供了数据压缩和解压的功能。

#### [Net](http://nodejs.org/api/net.html)

`net`模块可用于创建Socket服务器或Socket客户端。

> 通过`net`模块的Socket服务器与客户端可对HTTP协议做底层操作。



## 五、进程管理

#### [Process](http://nodejs.org/api/process.html)

`process`不是内置模块，而是一个全局对象。

> 应用：获取命令行参数、退出程序、控制输入输出、进程间通讯

#### [Child Process](http://nodejs.org/api/child_process.html)

使用`child_process`模块可以创建和控制子进程。该模块提供的API中最核心的是`.spawn`，其余API都是针对特定使用场景对它的进一步封装，算是一种语法糖。

#### [Cluster](http://nodejs.org/api/cluster.html)

`cluster`模块是对`child_process`模块的进一步封装，专用于解决单进程NodeJS Web服务器无法充分利用多核CPU的问题。使用该模块可以简化多进程服务器程序的开发，让每个核上运行一个工作进程，并统一通过主进程监听端口和分发请求。



## 六、异步编程

我们仍然回到JS是单线程运行的这个事实上，这决定了JS在执行完一段代码之前无法执行包括回调函数在内的别的代码。也就是说，即使平行线程完成工作了，通知JS主线程执行回调函数了，**回调函数也要等到JS主线程空闲时才能开始执行**。以下就是这么一个例子。

```javascript
function heavyCompute(n) {
    var count = 0,
        i, j;

    for (i = n; i > 0; --i) {
        for (j = n; j > 0; --j) {
            count += 1;
        }
    }
}

var t = new Date();

setTimeout(function () {
    console.log(new Date() - t);
}, 1000);

heavyCompute(50000);

-- Console ------------------------------
8520
```

可以看到，本来应该在1秒后被调用的回调函数因为JS主线程忙于运行其它代码，实际执行时间被大幅延迟。

#### 代码设计模式

##### 遍历数组

同步

```javascript
var len = arr.length,
    i = 0;

for (; i < len; ++i) {
    arr[i] = sync(arr[i]);
}

// All array items have processed.
```

异步（数组成员串行处理）

```javascript
(function next(i, len, callback) {
    if (i < len) {
        async(arr[i], function (value) {
            arr[i] = value;
            next(i + 1, len, callback);
        });
    } else {
        callback();
    }
}(0, arr.length, function () {
    // All array items have processed.
}));
```

异步（数组成员并行处理）

```javascript
(function (i, len, count, callback) {
    for (; i < len; ++i) {
        (function (i) {
            async(arr[i], function (value) {
                arr[i] = value;
                if (++count === len) {
                    callback();
                }
            });
        }(i));
    }
}(0, arr.length, 0, function () {
    // All array items have processed.
}));
```

##### 异常处理

```javascript
function async(fn, callback) {
    // Code execution path breaks here.
    setTimeout(function ()　{
        try {
            callback(null, fn());
        } catch (err) {
            callback(err);
        }
    }, 0);
}

async(null, function (err, data) {
    if (err) {
        console.log('Error: %s', err.message);
    } else {
        // Do something.
    }
});

-- Console ------------------------------
Error: object is not a function
```

在NodeJS中，几乎所有异步API都按照以上方式设计，回调函数中第一个参数都是`err`。因此我们在编写自己的异步函数时，也可以按照这种方式来处理异常，与NodeJS的设计风格保持一致。

#### [域（Domain）](http://nodejs.org/api/domain.html)

Node.js通过`process`对象提供了捕获全局异常的方法

```javascript
process.on('uncaughtException', function (err) {
    console.log('Error: %s', err.message);
});

setTimeout(function (fn) {
    fn();
});

-- Console ------------------------------
Error: undefined is not a function
```

使用`domain`模块创建一个子域（JS子运行环境）

```javascript
function async(request, callback) {
    // Do something.
    asyncA(request, function (data) {
        // Do something
        asyncB(request, function (data) {
            // Do something
            asyncC(request, function (data) {
                // Do something
                callback(data);
            });
        });
    });
}

http.createServer(function (request, response) {
    var d = domain.create();

    d.on('error', function () {
        response.writeHead(500);
        response.end();
    });

    d.run(function () {
        async(request, function (data) {
            response.writeHead(200);
            response.end(data);
        });
    });
});
```

> 子域可以独立处理抛出的异常，通过`.run`方法进入子域中运行代码

按照官方文档的说法，发生异常后的程序处于一个不确定的运行状态，如果不立即退出的话，程序可能会发生严重内存泄漏，也可能表现得很奇怪。

#### 小结

- 不掌握异步编程就不算学会NodeJS。
- 异步编程依托于回调来实现，而使用回调不一定就是异步编程。
- 异步编程下的函数间数据传递、数组遍历和异常处理与同步编程有很大差别。
- 使用`domain`模块简化异步代码的异常处理，并小心陷阱





## 参考资料

* [七天学会NodeJS](http://nqdeng.github.io/7-days-nodejs/)
* [Node.js官网](https://nodejs.org/en/)
* [7-days-nodejs](https://papahot.gitbooks.io/7-days-nodejs/content/)