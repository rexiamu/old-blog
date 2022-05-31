---
title: vue 项目编译或打包时自动生成版本号
date: 2021-12-17 11:07:44
tags:
  - vue
  - 版本号
  - webpack
  - DefinePlugin
categories:
  - 前端
---

在项目的登录页中放置了版本号信息 **V 2.2 1639710093** ，每次发布都需要手动更换最后面的时间戳，这样操作很繁琐，于是乎找到了一种让项目自动编译时自动生成时间戳的方法。

<!-- more -->

## webpack.DefinePlugin

**webpack.DefinePlugin** 插件可以帮助我们配置项目中常用的全局变量，从而提升工作效率。具体配置信息如下：

```js
chainWebpack(config) {
  // 配置全局变量
  // 在 src 目录下的文件中的 js 文件或者 vue 文件中的 script 部分可以访问到定义的全局变量
  // second_timestamp 秒级时间戳 当做版本号后缀
  config
    .plugin('define')
    .tap(args => {
      args[0]['SECOND_TIMESTAMP'] = JSON.stringify(new Date().getTime().toString().slice(0, 10))
      args[0]['GLOBAL_VAR'] = JSON.stringify('GLOBAL_VAR')
      return args
    })
}
```

使用方法：

```js
// vue 文件中使用
data(){
  return {
    SECOND_TIMESTAMP,
    GLOBAL_VAR
  }
}
```
