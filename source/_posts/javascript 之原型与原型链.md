---
title: javascript 之原型与原型链
date: 2022-03-30 14:28:25
tags:
  - javascript
  - 原型
  - 原型链
categories:
  - [前端]
---

![javascript 原型链示意图](https://gitee.com/Rexiamu/image-hosting/raw/master/img/20220330130430.png)

在 java 语言中使用类的继承进行面向对象编程，而在 javaScript 语言中则是使用原型及原型链的方式进行面向对象编程。

<!-- more -->

## 原型

原型可以理解为实例对象的抽象，通过构造函数进行创建。

- 实例对象通过 `__proto__` 属性访问原型对象
- 构造函数通过 `prototype` 属性访问原型对象
- 原型对象通过 `constructor` 属性访问构造函数 
- 在使用我们所定义的构造函数时会自动创建原型对象（使用 `new Object()` 创建），
  所以实例对象的原型的原型其实就是 `Object.prototype`
- 12

### ES5 例子

```javascript
// 创建构造函数
function Person(name, age) {
  this.name = name;
  this.age = age;
}
// 定义原型方法
Person.prototype.say = function () {
  console.log("你好：" + this.name);
}
// 使用构造函数创建实例
const p = new Person("张三", 18);
console.log(p); // Person { name: '张三', age: 18 }
console.log(p.__proto__ === Person.prototype); // true
console.log(Person.prototype.__proto__ === Object.prototype); // true 
p.say(); // 你好：张三
```

### ES6 例子

```javascript
// 创建类
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  say() {
    console.log("你好：" + this.name);
  }
}
// 创建实例
const p = new Person("李四", 19);
console.log(p); // Person { name: '李四', age: 19 }
console.log(p.__proto__ === Person.prototype); // true
console.log(Person.prototype.__proto__ === Object.prototype); // true 
p.say(); // 你好：李四
```

## 原型链

当我们访问一个对象的属性时，首先会查看该实例对象是否有此属性，如果没有则会查看它的原型对象，如果还没有则会查看其原型对象的原型对象。
该过程会一直查下去，直到查到此属性或者查到 `Object.prototype` 的 `__proto__` 属性（值为 null 停止查找），如此便形成了原型链。

## 总结

- 每个构造函数对应一个原型对象，通过 `prototype` 属性访问
- 每个原型对象对应一个构造函数，通过 `constructor` 属性访问
- 每个实例对象通过 `__proto__` 属性访问其原型对象
- 实例对象的 `_proto` 属性和构造函数的 `prototype` 属性共同指向原型对象
- 构造函数 `function Object()` 的原型对象 `Object.prototype` 没有原型对象，
  即原型对象 `Object.prototype` 的 `__proto__` 属性为 `null`
- 所有的构造函数的原型 `__proto__` 属性指向原型对象 `Function.prototype` 
- 原型对象 `Function.prototype` 的原型 `__proto__` 属性指向原型对象 `Object.prototype`
- 原型对象 `Function.prototype` 的 `constructor` 属性指向构造函数 `function Function()`
- `javascript` 语言在初始化时会自动创建 `function Object()` 和 `function Function()` 以及他们各自的原型对象，
  同时建立它们之间的引用关系
