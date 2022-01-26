---
title: Promise + async + await 实现异步代码按顺序执行
date: 2022-01-13 14:26:53
tags:
  - Promise
  - async
  - await
  - 异步
categories:
  - 前端
---

![期望的执行流程](https://gitee.com/Rexiamu/image-hosting/raw/master/img/20220113135506.png)

在实际的工作中，我们常常会碰到这样的场景：调用接口 A  返回数据 a ，使用数据 a 作为请求参数调用接口 B 返回数据 b ，再使用数据 b 作为请求参数调用接口 C ... 

<!-- more -->

但是实际的执行情况可能并不如我们所愿。因为 ajax 请求是异步的，这意味着如果你没做处理，这几个请求会一起执行。而请求执行需要时间，这样一来后面的请求可能因为拿不到参数导致错误。

ES6 之后，出现了 Promise 和 async/await 这样的异步解决方案，我们可以使用它们实现异步代码按顺序执行。下面以三个定时器来模拟异步操作。<br />

使用前：
```javascript
setTimeout(() => {
  console.log("A");
}, 3000);

setTimeout(() => {
  console.log("B");
}, 2000);

setTimeout(() => {
  console.log("C");
}, 1000);

// 期望输出 A => B => C
// 实际输出 C => B => A
```
使用后：
```javascript
function printA() {
  return new Promise((re, rj) => {
    setTimeout(() => {
      re("A");
    }, 3000);
  });
}

function printB() {
  return new Promise((re, rj) => {
    setTimeout(() => {
      re("B");
    }, 2000);
  });
}

function printC() {
  return new Promise((re, rj) => {
    setTimeout(() => {
      re("C");
    }, 1000);
  });
}

async function print() {
  await printA().then(console.log);
  await printB().then(console.log);
  await printC().then(console.log);
}

print();

// 期望输出 A => B => C
// 实际输出 A => B => C
```
