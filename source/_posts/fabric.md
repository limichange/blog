---
title: fabric 中的坑
date: 2017-11-01 10:02:18
tags: [js, canvas]
---

## 最近

需要做一个编辑器，所以用到了`fabric`，然后就遇到了不少坑，特此记录一下。

### 获取元素的宽高

```js
const width = k.getWidth()
const height = k.getHeight()
```

不要去直接的读取`width`和`height`属性，因为会有缓存，这两个属性值并不一定是最新的。

### 选取范围没有刷新

```js
rect.setCoords()
```

当你动态的设置了位置或者大小后，图形的选取范围会没有刷新，大小还是停留在上一个位置上。这个时候需要自己手动的刷新。

### 图像没有刷新

```js
canvas.renderAll()
```

如果碰到了什么没有刷新的情况，可以尝试使用这个方法。

### 放大镜

http://jsfiddle.net/powerc9000/G39W9/

### 边框会变粗

```js
canvas.on({
  'object:scaling': function (e) {
    let obj = e.target
    if (obj.myCustomOptionKeepStrokeWidth) {
      let newStrokeWidth = obj.myCustomOptionKeepStrokeWidth / ((obj.scaleX + obj.scaleY) / 2)
      obj.set('strokeWidth', newStrokeWidth)
    }
  }
})
```

似乎作者认为边框会随着变形变宽就是正确的。这是一种折中的方案，会尽量的缩小边框。
