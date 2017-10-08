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

## 构造函数

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

让我们看最后三个变量，这是为了向外部暴露一些内部的原型对象。使用[`Object.create`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/create)可以将原型复制一份。一开始我也对这三个变量感到困惑，为什么要这么做？于是我翻了一下koa的commit历史，找到了一次[提交](https://github.com/limichange/koa/commit/0be144211189c0ebd2ca059d7f1ee2e72b2ac6b9)。

> expose app-specific prototypes, cleanup/fix tests

很好，我们现在把最初的构建流程弄清楚了。

## app.use

现在我们看看这一部分，`use`函数。我们通过他塞入中间件。

```js
app.use(async ctx => {
  ctx.body = 'Hello World';
});
```

轻松找到[定义的地方](https://github.com/limichange/koa/blob/master/lib/application.js#L94-L115)。

```js
use(fn) {
  if (typeof fn !== 'function') throw new TypeError('middleware must be a function!');
  if (isGeneratorFunction(fn)) {
    deprecate('Support for generators will be removed in v3. ' +
              'See the documentation for examples of how to convert old middleware ' +
              'https://github.com/koajs/koa/blob/master/docs/migration.md');
    fn = convert(fn);
  }
  debug('use %s', fn._name || fn.name || '-');
  this.middleware.push(fn);
  return this;
}
```

首先他检查了fn是不是一个Generator函数，中间件都是通过函数的方式传入的。然后看看你传入的是不是v1版本的中间件，警告你一下，并贴心的帮你转换到v2。未来转化的这部分会废弃掉，就不详细讲了，用到的[koa-convert](https://github.com/koajs/convert#readme)和[is-generator-function](https://github.com/ljharb/is-generator-function#readme)可以自己详细看。

[debug](https://github.com/visionmedia/debug#readme)是很常用的调试库，功能很多，没有用过或不熟悉的一定要要看。

`middleware`就是我们前面看到的那个变量，和我们想的一样，中间件都被存到这个数组里面了。

最后返回this，因此可以做链式的调用。像这样。

```js
app
  .use(...)
  .use(...)
```

看样子这个函数，只是把中间件存了起来，并没有做什么特别的事情。

## app.listen

最后一个部分就是`app.listen`了。

```js
app.listen(3000)
```

这个函数就定义在[Application的构造函数的下面](https://github.com/limichange/koa/blob/master/lib/application.js#L65-L73)。

```js
/**
  * Shorthand for:
  *
  *    http.createServer(app.callback()).listen(...)
  *
  * @param {Mixed} ...
  * @return {Server}
  * @api public
  */
listen(...args) {
  debug('listen');
  const server = http.createServer(this.callback());
  return server.listen(...args);
}
```

是个快捷方式，把监听的流程简化了一下。这里就是用[`http.createServer`](https://nodejs.org/dist/latest-v8.x/docs/api/http.html#http_http_createserver_requestlistener)创建了一个http的服务，把参数直接的传入到`server.listen`。我们需要关注的是`this.callback()`。

> TODO
