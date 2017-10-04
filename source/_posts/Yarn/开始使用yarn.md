---
title: 开始使用yarn
date: 2017-01-01 10:48:11
tags:
---

## 说明
作为新的Nodejs包管理工具，
确实是比NPM好用很多。

### 官网
https://yarnpkg.com/

### 优点
 - 快
 - 命令友好
 - 离线安装（需要有缓存）
 - 自动锁定包版本

## 安装
https://yarnpkg.com/en/docs/install

```shell
$ brew update
$ brew install yarn

# 确认安装成功
$ yarn --version
```

## 使用
https://yarnpkg.com/en/docs/cli/

```shell
# 初始化
$ yarn init

# 升级包
$ yarn upgrade [package]

# 移除包
$ yarn remove [package]

# 安装包
$ yarn add [package]

# 全局安装
$ yarn global add [package]

# 安装项目下的依赖
$ yarn
# or
$ yarn install
```

## 建议
我觉得Yarn和NPM是补充关系，
基本的包处理命令其实差不多，
切换着用感觉没有什么成本，
建议使用。
