---
title: 使用axios上传文件
date: 2021-12-09 19:51:27
tags: 
  - axios 
  - 文件
categories: 
  - [前端]
  - [问题记录]
---

## 问题描述

在项目中使用 `axios` 上传文件时发现无法更改请求头默认的  `Content-Type` 默认值，从而导致文件上传失败。

<!-- more -->

## 解决方法

不使用统一的配置，对请求进行单独设置。

```js
const formData = new FormData();
formData.append('file', file);
formData.append('title', '文件');

this.$axios({
  method: 'post',
  url: '/api/uploadFile',
  data: formData,
  headers: {
    'Content-Type': 'multipart/form-data'
  }
})
  .then(function (res) {
    ...
  })
```
