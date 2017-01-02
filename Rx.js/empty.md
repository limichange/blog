## empty

http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html#static-method-empty

> Just emits 'complete', and nothing else.

```js
var result = Rx.Observable.empty().startWith(7);
result.subscribe(x => console.log(x));
```

```js
var interval = Rx.Observable.interval(1000);
var result = interval.mergeMap(x =>
  x % 2 === 1 ? Rx.Observable.of('a', 'b', 'c') : Rx.Observable.empty()
);
result.subscribe(x => console.log(x));
```
