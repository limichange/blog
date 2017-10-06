---
title: 看Koa的源码
date: 2017-10-06 16:09:19
tags: [koa, nodejs]
---

## 版本

我们看的是2.3.0版本，我Fork了下来以免和文章发生冲突。

https://github.com/limichange/koa

## 首先看看

这个是官方的上手例子，记忆一下。
```js
const Koa = require('koa');
const app = new Koa();

app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```

## 如何看

首先要找到入口的文件，我们可以在koa包的package.json里找到。

```json
{
  "main": "lib/application.js"
}
```
好了，找到入口的文件，接着往下看。

### 细看

#### lib/application.js

我们先把文件简化一下，这样看。

```js

// 引入其他的模块

// Koa的Class定义
module.exports = class Application extends Emitter {
  // 具体的内容
};

// 一个辅助的函数
function respond(ctx) {
  // 具体的内容
}

```

这样看起来就很清晰。

> TODO
