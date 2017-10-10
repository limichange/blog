---
title: now.sh
date: 2016-12-16 13:13:42
tags: [shell, deploy]
---

# 场景

https://zeit.co/now

一个命令就将你的网站部署，
就一个命令，
什么都不用配，
http2，
https，
全球CDN（除了天朝），
都有。

# 安装
```bash
$ yarn global add now
# or
$ npm i -g now
```

# 部署
```bash
# Docker
$ my-app/ ls
Dockerfile  server.go
$ my-app/ now

# Node.js
$ my-api/ ls
package.json  index.js
$ my-api/ now

# Static Fils
$ my-site/ ls
index.html  logo.png
$ my-site/ now
```

部署完会给你一个网址。

# 资费
免费的够用，
如果有业务需求的话可以开通其他的Plan。

https://zeit.co/now#pricing
