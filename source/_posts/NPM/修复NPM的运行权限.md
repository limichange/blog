---
title: 修复NPM的运行权限
date: 2016-09-04 10:48:11
tags: [npm, shell]
---

这个问题困扰了我好久，
我一直都没有理，
但是我现在意识到输密码这个动作浪费了我太多的时间，
必须要解决了（太烦人了。

官方已经给了解决的方法了（呆
https://docs.npmjs.com/getting-started/fixing-npm-permissions

```shell
$ npm config get prefix
/usr/local
```

根据官方给的命令我们得到一个值，
填到下面这个命令里。
```shell
# sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}
sudo chown -R limi /usr/local/{lib/node_modules,bin,share}
```

解决了🎉

如果不行的话试试下面这个。
http://stackoverflow.com/questions/16151018/npm-throws-error-without-sudo
```shell
# sudo chown -R $(whoami) ~/.npm
sudo chown -R limi ~/.npm
```
