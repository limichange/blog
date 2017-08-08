## 场景

异步流程优化。

## 例子
```js
function resolveAfter2Seconds(x) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(x);
    }, 2000);
  });
}

async function add1(x) {
  var a = resolveAfter2Seconds(20);
  var b = resolveAfter2Seconds(30);
  return x + await a + await b;
}

add1(10).then(v => {
  console.log(v);  // prints 60 after 2 seconds.
});

async function add2(x) {
  var a = await resolveAfter2Seconds(20);
  var b = await resolveAfter2Seconds(30);
  return x + a + b;
}

add2(10).then(v => {
  console.log(v);  // prints 60 after 4 seconds.
});
```
这个是MDN上的例子，
看得出，
使用上是在Promise的基础上做的增强。
结构上也很有趣，
分别介绍了并列请求和同步请求
可以实际的跑一下感受一下。

## 看法
有有优越性，
但很多时候Promise会更好，
catch错误和做其他的一些操作感觉不是很方便，
做高级的调用的时候很方便，
比如大量的异步做同步的时候还没有想到怎么更好的处理，
需要看看别人是怎么写的。

## 兼容性
最新版的chrome已经支持了，
其余的浏览器也都在支持的过程中，
但为了兼容性依旧使用babeljs来做转换。

## 相关
 - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function
 - http://babeljs.io/repl/
 - http://caniuse.com/#feat=async-functions
 - https://developers.google.com/web/fundamentals/getting-started/primers/async-functions
