---
title: update-notifier
date: 2017-01-01 10:48:11
tags: nodejs
---

> Update notifications for your CLI app

https://www.npmjs.com/package/update-notifier

在np的源码里看到的，
用于提示当前工具升级的。

如果人家正在使用你的工具版本为0.0.1，
你发布了0.0.2，
那么用这个模块就可以了。

```js
const updateNotifier = require('update-notifier');
const pkg = require('./package.json');

updateNotifier({pkg}).notify();
```
