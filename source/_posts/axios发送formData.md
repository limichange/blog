---
title: axios发送formData
date: 2017-10-06 13:13:42
tags: [axios, formData, nodejs]
---

这是一个坑，
因为axios无法在nodejs的环境中发送formData。

https://github.com/axios/axios#request-config

> `data` is the data to be sent as the request body
> Only applicable for request methods 'PUT', 'POST', and 'PATCH'
> When no `transformRequest` is set, must be of one of the following types:
> - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
> - Browser only: FormData, File, Blob
> - Node only: Stream, Buffer

我使用了一个叫form-data的库，
但没有什么效果，
于是就手动的写入了，
似乎效果更好一些。

```js
async function tweet (content) {
  return await axios({
    method: 'post',
    url: 'https://api.com',
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded;charset=UTF-8'
    },
    data: `content=${content}`
  })
}
```
