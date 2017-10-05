---
title: 切换npm的源
date: 2016-06-24 10:48:11
tags: [npm]
---

对于刚刚写Nodejs的人来说，
NPM的下来速度让人抓狂，
往往在装NPM包的时候直接卡死。

解决的方法很简单，
直接换源。

但对于要在NPM发布模块的人来说，
这会引发一个新的问题，
发布的时候必须是NPM的官方源，
平时的装模块的时候又得改变源来获取速度，
这个时候就需要一个工具来做快速的切换，
你需要nrm。

> nrm
> NPM registry manager
> https://github.com/Pana/nrm
> https://www.npmjs.com/package/nrm

安装。
```shell
$ sudo npm install -g nrm
```

切换。
```shell
# 切换到淘宝源
$ nrm use taobao

# 切换到官方源
$ nrm use npm
```

简直离不开这个工具😆
