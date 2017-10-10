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

接下来我会事无巨细的为你介绍koa的方方面面，尽量对每一个函数和变量进行说明。当然，我没有办法保证所有的说明都是对的，如果你发现了错误，请在最后面留言，我会尽快的修改。

那么，让我们开始吧 ：）

## 版本

我们看的是2.3.0版本，我Fork了下来以保证和文章同步，因为这篇文章在未来也会过时。

https://github.com/limichange/koa

## 首先

这个是官方的上手例子，简单记忆一下。如果你对代码中的`async`和`=>`感到困惑，那么你需要先去学习一下[最新的js语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)。因为我们看的目前是最新版的koa，所以你可以忽略generator的知识，当然学一下也没有什么关系：）

```js
const Koa = require('koa');
const app = new Koa();

app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```

我们接下来的内容将会从这个例子展开。

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

当然官方文档里也说了。
> app.context is the prototype from which ctx is created from. You may add additional properties to ctx by editing app.context. This is useful for adding properties or methods to ctx to be used across your entire app, which may be more performant (no middleware) and/or easier (fewer require()s) at the expense of relying more on ctx, which could be considered an anti-pattern.

简单的说就是可以通过他添加一个全局的属性或者方法，这样比添加中间件方便而且性能更好。如下。

```js
app.context.db = db();

app.use(async ctx => {
  console.log(ctx.db);
});
```
我们暂且不要管三个变量的里面的具体实现，后面会慢慢讲。
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

先大致的看一下[callback的实现](https://github.com/limichange/koa/blob/master/lib/application.js#L117-L140)。这里是整个koa的核心，是处理请求的地方。通过注释知道，这个函数是返回一个请求处理回调给nodejs的http使用，就前面listen里的用法。首先我们大致的理清里面的流程。

```js
/**
  * Return a request handler callback
  * for node's native http server.
  *
  * @return {Function}
  * @api public
  */

callback() {
  // 把中间件处理成一个函数
  const fn = compose(this.middleware);

  // 绑定默认的错误处理
  if (!this.listeners('error').length) this.on('error', this.onerror);

  // 首先是原生的`req`和`res`传进来
  const handleRequest = (req, res) => {

    // 默认状态码是404
    res.statusCode = 404;

    // 然后创建ctx
    const ctx = this.createContext(req, res);

    // 创建错误处理的函数
    const onerror = err => ctx.onerror(err);

    // 响应处理
    const handleResponse = () => respond(ctx);

    // 调用on-finished库处理
    onFinished(res, onerror);

    // 用中间件处理ctx，然后处理响应，最后捕捉错误
    return fn(ctx).then(handleResponse).catch(onerror);
  };

  // 返回处理函数
  return handleRequest;
}
```

> TODO

### createContext

```js
createContext(req, res) {
  // 还记得开头看到的那个构造函数吗？里面最后的三行。这里就直接的使用了那三个对象。
  const context = Object.create(this.context);
  const request = context.request = Object.create(this.request);
  const response = context.response = Object.create(this.response);

  // 接下来就是各种赋值，把原生`req`和`res`存起来，给对象做别名。
  context.app = request.app = response.app = this;
  context.req = request.req = response.req = req;
  context.res = request.res = response.res = res;
  request.ctx = response.ctx = context;
  request.response = response;
  response.request = request;
  context.originalUrl = request.originalUrl = req.url;

  // 创建`cookies`，平时你的`keys`就是给这里用的。
  context.cookies = new Cookies(req, res, {
    keys: this.keys,
    secure: request.secure
  });

  // 然后就是ip的判断。
  request.ip = request.ips[0] || req.socket.remoteAddress || '';

  // 调用accepts
  context.accept = request.accept = accepts(req);

  // 初始化state
  context.state = {};

  // 处理完毕后把ctx返回
  return context;
}
```

这里面用到了两个库[`cookies`](https://github.com/pillarjs/cookies#readme)和[accepts](https://github.com/jshttp/accepts#readme)。前者就是用来操作`cookie`的，后者是处理`Negotiation`。

> TODO

### respond

koa在这里引入了[`statuses`](https://github.com/jshttp/statuses#statuses)

> HTTP status utility for node.

`statuses`提供了很多http状态的管理方法，平时处理http的时候会用得着。这里koa用来判断该请求是否需要把body清空，[比如这样](https://github.com/jshttp/statuses#statusemptycode)。
```js
status.empty[200] // => undefined
status.empty[204] // => true
status.empty[304] // => true
```

接下来看本体。
```js
function respond(ctx) {
  // 没有respond的话就不处理
  if (false === ctx.respond) return;

  // 把原生的res取出来
  const res = ctx.res;

  // 是否可写
  if (!ctx.writable) return;

  // 把body和状态码取出来
  let body = ctx.body;
  const code = ctx.status;

  // 判断该状态是否需要一个空的body
  if (statuses.empty[code]) {

    // 把body清空
    ctx.body = null;

    // 返回
    return res.end();
  }

  // 如果http的方法是 `HEAD`
  if ('HEAD' == ctx.method) {
    if (!res.headersSent && isJSON(body)) {
      ctx.length = Buffer.byteLength(JSON.stringify(body));
    }

    // 返回
    return res.end();
  }

  // body为空
  if (null == body) {

    // 把message或者状态码放进去
    body = ctx.message || String(code);
    if (!res.headersSent) {
      ctx.type = 'text';
      ctx.length = Buffer.byteLength(body);
    }

    // 返回
    return res.end(body);
  }

  // responses
  if (Buffer.isBuffer(body)) return res.end(body);
  if ('string' == typeof body) return res.end(body);
  if (body instanceof Stream) return body.pipe(res);

  // 如果所有的判断都通过了，就当json来处理
  body = JSON.stringify(body);
  if (!res.headersSent) {
    ctx.length = Buffer.byteLength(body);
  }

  // 返回
  res.end(body);
}
```

这里是[`res.end`的解释](https://nodejs.org/api/http.html#http_response_end_data_encoding_callback)
> This method signals to the server that all of the response headers and body have been sent

`headersSent`和

#### writable

`writable`是一个很有趣的变量，我们先找到[定义的地方](https://github.com/koajs/koa/blob/13c7ca61392acecff026a079623ab0d928ea0d93/lib/response.js#L487-L505)。然后再看一看他的[单元测试](https://github.com/koajs/koa/blob/d394724200d0e36e6af6db2edb524820212da915/test/response/writable.js)。

> TODO

### koa-compose

首先`compose`把中间件处理了一下，看看他是如何实现的。
```js
const compose = require('koa-compose');
```

是一个独立的库。这里说一下，我们可以在命令行通过一个命令快速的找到一个库的实现。
```shell
$ npm home koa-compose
```

是不是很方便？我们把[定义的文件](https://github.com/koajs/compose/blob/master/index.js)打开看看。为了方便讲解，我把其中的注释和部分类型判断删掉了。

确保你对`Promise`已经很熟悉了，不然你会对接下里的讲解很困惑。

为了更好的理解下面的运行原理，先让我们复习一下koa的中间件运行顺序，只要弄明白下图是怎么回事就可以了。
![middleware.gif](https://github.com/koajs/koa/blob/master/docs/middleware.gif)

很好，那我们开始。

```js
function compose (middleware) {

  // 首先会返回一个函数，就是createContext里的fn
  return function (context, next) {
    let index = -1
    return dispatch(0)

    // 
    function dispatch (i) {

      // 你不可以再一次请求里多次调用next
      if (i <= index) return Promise.reject(new Error('next() called multiple times'))
      index = i
      let fn = middleware[i]
      if (i === middleware.length) fn = next
      if (!fn) return Promise.resolve()
      try {
        return Promise.resolve(fn(context, function next () {
          return dispatch(i + 1)
        }))
      } catch (err) {
        return Promise.reject(err)
      }
    }
  }
}
```



> TODO
