---
title: 请求参数类型之 x-www-form-urlencoded
date: 2022-03-18 14:54:16
tags:
  - http
  - axios
  - qs
  - x-www-form-urlencoded
categories:
  - [前端]
  - [问题记录]
---

对于后端接口规定使用 `x-www-form-urlencoded` 形式接收请求参数时，如果传递一个 json 数据过去，则是无法解析的。对于设置了 `x-www-form-urlencoded` 请求参数类型的接口，前端需要将 json 数据转化为键值对字符串的形式（eg:"name:zhangsan&age:18"）,并且设置请求头参数 `content-type` 为 `application/x-www-form-urlencoded`。

<!-- more -->

## 具体实现：

```js
// 引入 qs 库，用来将 json 数据转化为键值对字符串
// 如果安装了 axios 则可以直接引用
import qs from 'qs'

export function getData(data) {
  return request({
    url: '/api/getData',
    method: 'POST',
    headers: { 'content-type': 'application/x-www-form-urlencoded' },
    data: qs.stringify(data) // 使用 qs 库的 stringify 方法，将 json 数据转化为键值对字符串
  })
}
```

## 总结

- 使用较多的请求参数类型有 `application/json` 和 `application/x-www-form-urlencoded`
- `application/json` 接收 json 数据
- `application/x-www-form-urlencoded` 接收键值对字符串（eg:"name:zhangsan&age:18"）
- 后端接口请求参数只有一个对象时，使用 `@RequestBody` 进行注解，此时请求参数类型为 `application/json`
- 后端接口请求参数不是只有一个对象时，不使用 `@RequestBody` 进行注解，此时请求参数类型为 `application/x-www-form-urlencoded`
