给博客服务端写token的验证服务,
使用了passport作为处理工具,
配置了passport-http-bearer做为token的实现,
然而出现了坑,
摊手。

在谷歌上查了几个答案就不行,
遂,
翻开源码直接看。
```js
if (req.headers && req.headers.authorization) {
  var parts = req.headers.authorization.split(' ');
  if (parts.length == 2) {
    var scheme = parts[0]
      , credentials = parts[1];

    if (/^Bearer$/i.test(scheme)) {
      token = credentials;
    }
  } else {
    return this.fail(400);
  }
}

if (req.body && req.body.access_token) {
  if (token) { return this.fail(400); }
  token = req.body.access_token;
}

if (req.query && req.query.access_token) {
  if (token) { return this.fail(400); }
  token = req.query.access_token;
}
```

原来是这么检测的,
这个结构有点略奇葩。

必须写进http请求的header里面才可以,
key,
`Authorization`,
value,
`Bearer 85d052bf-10b8-4298-8c75-f4673eb5278a`。

get和post可以用各自的字段处理,
但最好还是写进header里,
一了百了。
