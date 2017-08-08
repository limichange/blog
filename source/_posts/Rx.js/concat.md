# concat

印象中是用来处理数组合并的，
rxjs里是把Observable做合并，
将多个流处理成一个流。

```js
var numbers = Rx.Observable.of(10, 20, 30);
var letters = Rx.Observable.of('a', 'b', 'c');
var interval = Rx.Observable.interval(1000);
var result = numbers.concat(letters).concat(interval);
result.subscribe(x => console.log(x));
```
