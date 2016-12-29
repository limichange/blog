## defer
http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#static-method-defer

> Creates the Observable lazily, that is, only when it is subscribed.

下面的基本逻辑是根据条件生成发送。
就是说如果随机数大于0.5，
那么就会匹配第一个发送源，
如果是小于0.5，
那么就匹配第二个。

```js
var clicksOrInterval = Rx.Observable.defer(function () {
  if (Math.random() > 0.5) {
    return Rx.Observable.fromEvent(document, 'click');
  } else {
    return Rx.Observable.interval(1000);
  }
});
clicksOrInterval.subscribe(x => console.log(x));
```
