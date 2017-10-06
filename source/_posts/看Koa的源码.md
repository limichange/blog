---
title: 看Koa的源码
date: 2017-10-06 16:09:19
tags: [koa, nodejs]
---

## 说明

如果你对说明没兴趣，可以直接看下面一章节。

起因是我看到了一篇招聘说明，上面要求应试者对koa的源码了如指掌。很惭愧，我虽然使用koa有一段时间了，但的确没有仔细的看过koa的源码。于是我就到网上搜索了一些关于koa深入的说明或教程。但很遗憾，大多数的文章较老，停留在`Genetator`的版本，或只是提及了几个关键的核心，并不全面。

所以，为什么不自己写一篇呢？

如果你已经使用koa一段时间了，且对一些API比较熟悉了，现在想更加的精进一些。又或是你是刚用koa的新手，想从中学些比较有用的知识。那么这篇文章很适合你。

接下来我会事无巨细的为你介绍koa的方方面面，对每一个函数和变量进行说明。当然，我没有办法保证所有的说明都是对的，如果你发现了错误，请在最后面留言，我会尽快的修改。

那么，让我们开始吧 ：）

## 版本

我们看的是2.3.0版本，我Fork了下来以保证和文章同步，因为这篇文章在未来也会过时。

https://github.com/limichange/koa

## 首先

这个是官方的上手例子，简单记忆一下。如果你对代码中的`async`和`=>`感到困惑，那么你需要先去学习一下[最新的js语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)。

```js
const Koa = require('koa');
const app = new Koa();

app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```

## 找入口

首先要找到入口的文件，不然我们就不知道从何看起了。我们可以在koa包的[package.json](https://github.com/limichange/koa/blob/master/package.json#L5)里找到。

```json
{
  "main": "lib/application.js"
}
```
好了，我们找到入口的文件了，接着往下看。

## 细看

让我们把目光转到[lib/application.js](https://github.com/limichange/koa/blob/master/lib/application.js)。首先大致的看一下文件，然后我们先把文件简化一下。

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

另外`middleware`从名字可以看出来，是用来存储中间件的。至于是如何运作的，我们会在后面详细的分析下。

让我们看最后三个变量，通过一个例子就能让你立马明白他们是做什么的。是不是感觉很亲切？
```js
app.use(async ctx => {
  ctx;          // context
  ctx.request;  // request
  ctx.response; // response
});
```

好了，我们把构造函数里的发生了什么都搞清了。
等一下，[`Object.create`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)是什么？为什么不是`new`？

看，我们就要学到新东西了。其实当我第一次看的时候，我也感到了困惑。所以我觉得有必要在此详细的说明一下。


> TODO
