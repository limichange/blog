---
title: 看Koa的源码
date: 2017-10-06 16:09:19
tags: [koa, nodejs]
---

## 说明



## 版本

我们看的是2.3.0版本，我Fork了下来以免和文章发生冲突。

https://github.com/limichange/koa

## 首先

这个是官方的上手例子，记忆一下。
```js
const Koa = require('koa');
const app = new Koa();

app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```

## 找入口

首先要找到入口的文件，我们可以在koa包的package.json里找到。

```json
{
  "main": "lib/application.js"
}
```
好了，找到入口的文件，接着往下看。

## 细看

首先大致的看一下文件，然后我们先把文件简化一下。

```js
// 引入其他的模块
const Emitter = require('events');

// Koa的Class定义
module.exports = class Application extends Emitter {
  // 具体的内容
};

// 一个辅助的函数
function respond(ctx) {
  // 具体的内容
}
```

这样看起来就很清晰了。

[events](https://nodejs.org/dist/latest-v8.x/docs/api/events.html)是nodejs的内置基本库。如果不是很熟悉，可以去看看官方的文档。

接着看这个Class的构造函数。让我们稍微还原一下文件，填上我们要看的内容。

```js
// 引入其他的模块
const response = require('./response');
const context = require('./context');
const request = require('./request');
const Emitter = require('events');

// Koa的Class定义
module.exports = class Application extends Emitter {
  constructor() {
    super();

    this.proxy = false;
    this.middleware = [];
    this.subdomainOffset = 2;
    this.env = process.env.NODE_ENV || 'development';
    this.context = Object.create(context);
    this.request = Object.create(request);
    this.response = Object.create(response);
  }
  // 其他的内容
};
```

看样子是初始化了一些内部的变量，我们挨个看看。其中的三个我们可以从文档中找到解释。
 - `env` 是运行时的环境变量，默认是`development`
 - `proxy` 如果使用代理模式的话，就设为`true`
 - `subdomainOffset` offset of .subdomains to ignore [2]

另外`middleware`从名字可以看出来，是用来存储中间件的。如何使用我们会在后面详细的分析下。


> TODO
