## 场景

当遇到创建文件夹的时候，
我们会遇见各种不同的情况，
如，
这个文件夹已经创建了怎么办，
这个时候fs就不是那么好用了，
此时可以用mkdirp。

## 使用

如下
```js
var mkdirp = require('mkdirp')

mkdirp('/tmp/foo/bar/baz', function (err) {
  if (err) {
    console.error(err)
  } else {
    console.log('pow!')
  }
})
```

扒了源码看了一下，
囫囵吞枣。
