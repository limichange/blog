---
title: 使用postWindow在网页直接通信
date: 2017-10-16 10:21:50
tags: [js]
---

## 场景

有这么一个效果，就是需要在两个页面直接发信息。具体就是A页面点开第三方登录页面B，验证成功后，B页面关闭然后通知A页面。

通信的方法可以用`localStorage`来处理，用来监控有没有事件触发。然后看到了。如果是在移动端就挺好的。`postWindow`，感觉更适合就实验了一下。

## 项目

项目在github。
https://github.com/limichange/window.postMessage

## 用法

首先得有监听
```js
window.addEventListener('message', (event) => {
  console.log('received response:  ', event.data)
}, false)
```

然后打开一个网页
```js
var myPopup = window.open('http://localhost:8080', 'page')
```

开始发发发发
```js
setInterval(() => {
  var message = 'Hello!  The time is: ' + (new Date().getTime())
  console.log('blog.local:  sending message:  ' + message)
  myPopup.postMessage(message, 'http://localhost:8080')
}, 6000)
```

## 结尾

除了IE的兼容性都挺不错的，所以在国内还是老老实实用`localStorage`来处理。如果是在移动端就挺好的。

https://caniuse.com/#search=postMessage
