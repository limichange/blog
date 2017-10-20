---
title: UglifyJy一个报错
date: 2016-01-12 19:42:02
tags: [uglify, js, npm, es]
---

## 场景

我自己写了一个npm的包，
include了进入，
dev的时候没有什么问题。
但当我发布的时候，
报错了。

```bash
ERROR in static/js/vendor.6adc67b5894d6ddb4ff5.js from UglifyJs
SyntaxError: Unexpected token operator «=», expected punc «,» [./~/qiniu-image-view2/src/index.js:8,0]
```

触不及防。

> 科学上网中

原来是因为UglifyJs还没有支持ES6，
语法报错了。
然后我明白了，
babel没有处理这个包。

看看webpack的配置就明白了。
```js
{
  test: /\.js$/,
  loader: 'babel',
  include: [
    path.join(projectRoot, 'src')
  ],
  exclude: /node_modules/
}
```

自己亲手排除的。

只要自己再独立include就可以了，
网上有人是用的正则表达处理的，
但我老是报错，
于是只好用这个方法了，
还比较直观一点。
```js
{
  test: /\.js$/,
  loader: 'babel',
  include: [
    path.join(projectRoot, 'src'),
    path.join(projectRoot, 'node_modules/qiniu-image-view2')
  ],
  exclude: /node_modules/
}
```

看来开源库的代码还是得转成ES5才能通用。

有人告诉我这样最简单，
大智若愚。
```js
import qiniuImageView2 from 'babel!qiniu-image-view2'
```

## 相关
 - https://github.com/webpack/webpack/issues/2031
 - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp
 - http://stackoverflow.com/questions/33001237/webpack-not-excluding-node-modules
