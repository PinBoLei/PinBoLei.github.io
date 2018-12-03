---
title: ES6部分常用知识整理
date: 2018-11-26 10:15:53
tags:
- ES6
- 前端
categories:
- ES6
copyright: true
---

>*在这整理了一些常用的ES6的知识，希望能够帮助开发者更加了解和运用ES6*

***
<!-- more -->

## 垃圾作用域

### 1.let取代var
ES6提出两个新的声明变量的命令 **let**,**const**，其中**let**完全可以取代**var**（两者语义相同）
注：`var命令存在变量提升作用，let命令没有这个问题`

### 2.全局变量和线程安全
在**let**和**const**之间，建议优先使用**const**，尤其在全局环境，不应该设置变量，只应设置常量。
**const**优于**let**的几个原因：

 1. const可以提醒阅读程序的人，这个变量不应该改变。
 2. const比较符合函数式编程思想，运算不改变值，只是新建值，而且这样也有利于将来的分布式运算。
 3. JavaScript编译器会对const进行优化，所以多使用const也有利于提高程序的运算效率。也就是说let和const的本质区别，其实是编译器内部的处理不同

## 字符串
静态字符串一律使用单引号或反引号，不使用双引号，动态字符串使用反引号
例：
```javascript
const a = 'foobar';
const b = `foo{a}bar`;
```
## 解构赋值
### 1.使用数组成员对变量赋值，优先使用解构赋值
例：
```javascript
const arr = [1,2,3,4];
// bad
const first = arr[0];
const second = arr[1];
// good
const [first,second] = arr;
```
### 2.函数的参数如果是对象的成员，优先使用解构赋值
例：
```javascript
// bad
function getFullName (user) {
  const firstName = user.firstName;
  const lastName = user.lastName;
}
// good
function getFullName (obj) {
  const {firstName,lastName} = obj;
}
// best
function getFullName ({firstName,lastName}) {
  ...
}
```
### 3.如果函数返回多个值，优先使用对象的解构赋值，而不是数组的解构赋值，这样有利于以后添加返回值，以及更改返回值的顺序
例：
```javascript
// bad
function processInput (input) {
  return [left,right,top,bottom];
}
// good
function processInput (input) {
  return {left,right,top,bottom};
  const {left,right} = processInput (input);
}
```
## 对象
### 1.单行定义的对象，最后一个成员不以逗号结尾，多行定义的对象，最后一个成员以逗号结尾
例：
```javascript
// bad
const a = {k1:v1,k2:v2,};
const b = {
  k1:v1,
  k2:v2
};
// good
const a = {k1:v1,k2:v2};
const b = {
  k1:v1,
  k2:v2,
};
```
### 2.对象尽量静态化，一旦定义，就不得随意添加新的属性，如果添加新属性不可避免，要使用Object.assign()方法
例：
```javascript
// bad
const a = {};
a.x = 3;
// if reshape unavoidable
const a = {};
Object.assign (a,{x:3});
// good
const a = {x:null};
a.x = 3;
```
### 3.如果对象的属性名是动态的，可以在创造对象的时候使用属性表达式定义
例：
```javascript
// bad
const obj = {
  id:5,
  name:'xiaolei',
};
obj[getKey('enabled')] = true;
// good
const obj = {
  id:5,
  name:'xiaolei',
  [getKey('enabled')]:true,
};
```
上面的代码中，对象obj的最后一个属性名，需要计算得到。这时最好利用属性表达式，在新建obj的时候，将该属性与其他属性定义在一起，这样，所有属性就在一个地方定义了。
### 4.对象的属性和方法尽量采用简洁表达法，这样易于描述和书写
例：
```javascript
var ref = 'some value';
// bad
const atom = {
  ref:ref,
  value:1,
  addValue:function (value) {
    return atom.value + value;
  },
};
// good
const atom = {
  ref, // 注意此处的写法
  value:1,
  addValue:function (value) {
    return atom.value + value;
  },
};
```
## 数组
### 1.使用扩展运算符（...）拷贝数组
例：
```javascript
// bad
const len = items.length;
const itemsCopy = [];
let i;
for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}
// good
const itemsCopy = [...items];
```
### 2.使用Array.form方法将类似数组的对象转为数组
例：
```javascript
const foo = document.querySelectorAll('foo');
const nodes = Array.from(foo);
```
## 函数
### 1.立即执行函数可以写成箭头函数的额形式
例：
```javascript
(() => {
  console.log('welcome to the Internet');
})();
```
### 2.那些需要函数表达式的场合，尽量使用箭头函数代替。因为这样更简洁，而且绑定了this.
例：
```javascript
// bad
[1,2,3].map(function (x) {
  return x * x;
});
// good
[1,2,3].map((x) => {
  return x * x;
});
// best
[1,2,3].map(x => x * x);
```
### 3.箭头函数形式取代Function.prototype.bind,不应再用self/_this/that 绑定this
例：
```javascript
// bad
const self = this;
const boundMethod = function (...params) {
  return method.apply(self.params);
};
// acceptable
const boundMethod = method.bind(this);
// best
const boundMethod = (...params) => method.apply(this.params);
```
### 4.所有的配置项都应该集中在一个对象，放在最后一个参数，布尔值不可以直接作为参数
例：
```javascript
// bad
function divide (a,b,option=false) {
  ...
};
// good
function divide (a,b,{option=false}) {
  ...
};
```
### 5.使用默认值语法设置函数参数的默认值
例：
```javascript
// bad
function handles (opts) {
  opts = opts || {};
};
// good
function handles (opts = {}) {};
```
### 6.不要在函数体内使用arguement变量，使用rest运算符(...)代替，因为rest运算明显表明你想要获取参数，而且arguement是一个类似数组的对象，而rest运算符可以提供一个真正的数组
例：
```javascript
// bad
function concatenateAl () {
  const args = Array.prototype.slice.call(arguements);
  return args.join('');
};
// good
function concatenateAl (...args) {
  return args.join('');
};
```
## Map结构
1.注意区分==Object==和==Map==,只有模拟现实世界的实体对象时，才使用Object。
2.如果只是需要key:value的数据结构，使用Map结构，因为Map有内建的遍历机制。
例：
```javascript
let map = new Map(arr);
for (let key of map.keys()) {
  console.log(key);
};
for (let value of map.values()) {
  console.log(value);
};
for (let item of map.entries()) {
  console.log(item[0],item[1]);
};
```
## Class
用class取代需要prototype的操作，因为class的写法更简洁，易于理解
例：
```javascript
// bad
function Queue (contents = []) {
  this._queue = [...contents];
};
Queue.prototype.pop = function () {
  const value = this._queue[0];
  this._queue.splice(0,1);
  return value;
};
// good
class Queue {
  constructor (contents = []) {
	this._queue = [...contents];
  }
  pop () {
	const value = this._queue[0];
	this._queue.splice(0,1);
	return value;
  }
}
```
## 模块
### 1.首先Module的语法是Javascript的标准写法，坚持使用这种写法，使用import代替require
例：
```javascript
// bad
const moduleA = require('moduleA');
const func1 = moduleA.func1;
const func2 = moduleA.func2;
// good
import {func1,func2} from 'moduleA';
```
### 2.使用export取代module.exports
例：
```javascript
// commonJs写法
var React = require('react');
var Breadcrumbs = React.createClass({
  render () {
	return <nav />;
  }
});
module.exports = Breadcrumbs;
// ES6写法
import React from 'react';
class Breadcrumbs extends React.Component{
  render () {
	return <nav />;
  }
};
export default Breadcrumbs;
```
如果模块只有一个输出值，就使用export default，如果模块有多个输出值，就不使用export default。
`export default 与普通的 export 不要同时使用`

### 3.不要在模块使用通配符，因为这样可以确保你的模块之中，只有一个默认输出（export default）
例：
```javascript
// bad
import * as myObject from './importModule';
// goood
import myObject from './importModule';
```
### 4.如果模块默认输出一个函数，函数名的首字母应该小写
例：
```javascript
function makeStyleGuide () {};
export default makeStyleGuide;
```
### 5.如果模块默认输出一个对象，对象名的首字母应该大写
例：
```javascript
const StyleGuide () {
  es6:{}
};
export default StyleGuide;
```