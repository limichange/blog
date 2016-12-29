自己做实验用到了`bcryptjs` ，
遂，
记一下。

> https://www.npmjs.com/package/bcryptjs
> Optimized bcrypt in plain JavaScript with zero dependencies. Compatible to 'bcrypt'.

起初是在Ghost的源码里看到的，
用来加密密码的。

用起来还是蛮简单的。
```js
var bcrypt = require('bcryptjs');

var salt = bcrypt.genSaltSync(10);
console.log(salt)
// -> $2a$10$b8N4nTRQ8xjZDqehvlQ8Z.

var hash = bcrypt.hashSync("mypassword", salt);
console.log(hash)
// -> $2a$10$b8N4nTRQ8xjZDqehvlQ8Z.dVkvTh9yHSYz9WbDqp8DCigx72.iyZ6

var hash2 = bcrypt.hashSync('mypassword', 10);
console.log(hash2)
// -> $2a$10$i1Nr.nKItHtDejf6ML8NduZAfFq0sTHhVO7ikRonJPZNY3Z5MG2Iy

console.log(bcrypt.compareSync("mypassword", hash));
// -> true

console.log(bcrypt.compareSync("mypassword", hash2));
// -> true
```

本着好奇心，
我去查了查这么方面的知识，
有用的我就列在下面了。
http://www.cnblogs.com/lixiong/archive/2011/12/24/2300098.html
https://segmentfault.com/a/1190000003024932
https://segmentfault.com/q/1010000003054250
https://segmentfault.com/a/1190000000401275

然后我知道了两件事：
 - CSDN的网站很渣
 - bcrypt比md5要慢很多，但更加安全
