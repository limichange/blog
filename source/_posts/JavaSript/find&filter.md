---
title: find 和 filter
date: 2016-12-12 19:42:02
tags: [js, array]
---

## 场景

从数组中根据条件过滤结果。

## 用法

### Find

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find

```js
function isBigEnough(element) {
  return element >= 15;
}

[12, 5, 8, 130, 44].find(isBigEnough); // 130
```

### Filter

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter

```js
function isBigEnough(value) {
  return value >= 10;
}

var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
// filtered is [12, 130, 44]
```

## 区别

一个返回数组，
一个返回第一个结果。
