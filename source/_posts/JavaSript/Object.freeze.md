---
title: Object.freeze
date: 2016-06-06 13:13:42
tags: [js, object]
---

今天照常逛逛Github,
发现vuex-router-sync更新了。
https://github.com/vuejs/vuex-router-sync

看了看更新的代码。
https://github.com/vuejs/vuex-router-sync/commit/

`Object.freeze(clone)`?
说实话，
我对这个函数没有啥印象，
遂学习一下。

先看看MDN上的文档。
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze

又翻了两篇文章。
http://www.cnblogs.com/dolphinX/p/3348467.html
https://segmentfault.com/a/1190000003894119

恩，
好像理解了。

```js
Object.freeze(obj);

obj.key = `value`; // no way!
```

在严格模式下会报类型错误（TypeError），
然后就是对象里面的对象不受影响（浅冻结），
需要遍历冻结内部的对象（深冻结）。

再看看兼容性，
IE9是支持的，
嗯，
没啥问题
good。
