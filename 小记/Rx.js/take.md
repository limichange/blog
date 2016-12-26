## take

> Takes the first count values from the source, then completes.

确定一个数量，按数量提取。
下面这个例子就是只拿5次值。

```js
var interval = Rx.Observable.interval(1000);
var five = interval.take(5);
five.subscribe(x => console.log(x));
```
