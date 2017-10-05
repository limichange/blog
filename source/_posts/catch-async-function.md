---
title: catch async function
date: 2017-10-05 18:37:21
tags: nodejs
---

事实上，Promise和async的错误捕捉是可以相互转化的。
但看起来蛮奇怪的。

```js
function run () {
  return new Promise((resolve, reject) => {
    reject(new Error('🐞'))
  })
}

async function main () {
  await run()
}

main().catch(console.log)
```
