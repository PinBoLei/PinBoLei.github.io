---
title: Promise对象用法简介
copyright: true
date: 2018-11-30 09:32:45
tags:
- Promise
categories:
- ES6
comments: true
---

>*原自 阮一峰老师的 ECMAScript 6 入门*

***
<!-- more -->

## 什么是promise对象

 1. `Promise`是异步编程的一种解决方案,ES6 将其写进了语言标准，统一了用法，原生提供了Promise对象。
 2. 从`Promise`对象可以获取异步操作的消息，`Promise `提供统一的 API，各种异步操作都可以用同样的方法进行处理。

## Promise对象的特点
 
 1. 对象的状态不受外界影响。`Promise`对象代表一个异步操作，有三种状态：`pending`（进行中）、`fulfilled`（已成功）和`rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。
 2. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。`Promise`对象的状态改变，只有两种可能：从`pending`变为`fulfilled`和从`pending`变为`rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 `resolved`（已定型）。如果改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。
 3. 
**`Promise`对象，可以将异步操作以同步操作的流程表达出来，避免层层嵌套的回调函数。此外，`Promise`对象提供统一的接口，使得控制异步操作更加容易。**

### Promise也有一些缺点:
**首先，无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。
其次，如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。
第三，当处于`pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。**

## 基本用法
ES6 规定，`Promise`对象是一个构造函数，用来生成`Promise`实例。
下面代码创造了一个`Promise`实例。
```javascript
const promise = new Promise(function(resolve, reject) {
  // ... some code
  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error); // 异步操作失败
  }
});
```
`Promise`构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject`。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

 - `resolve`函数的作用是，将`Promise`对象的状态从“`未完成`”变为“`成功`”（即从 `pending`变为 `resolved`），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；
 - `reject`函数的作用是，将`Promise`对象的状态从“`未完成`”变为“`失败`”（即从 `pending`变为`rejected`），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

`Promise`实例生成以后，可以用`then`方法分别指定`resolved`状态和`rejected`状态的回调函数。
```javascript
promise.then(function (value) {
  // success
},function (error) {
  // failure
})
```
`then`方法可以接受两个回调函数作为参数。第一个回调函数是`Promise`对象的状态变为`resolved`时调用，第二个回调函数是`Promise`对象的状态变为`rejected`时调用。其中，第二个函数是可选的，不一定要提供。这两个函数都接受`Promise`对象传出的值作为参数。

下面是一个`Promise`对象的简单例子。
```javascript
function timeout (ms) {
  return new Promise ((resolve,reject) => {
    setTimeout(resolve,ms,'done');
  })
};
timeout(100).then((value) => {
  console.log(value);
});
```
上面代码中，`timeout`方法返回一个`Promise`实例，表示一段时间以后才会发生的结果。过了指定的时间（ms参数）以后，`Promise`实例的状态变为`resolved`，就会触发`then`方法绑定的回调函数。

**`Promise `新建后就会立即执行。**
```javascript
let promise = new Promise(function(resolve,regect){
  console.log('Promise');
  resolve();
});
promise.then(function () {
 console.log(resolve);
});
// Promise
// Hi!
// resolved
```
上面代码中，`Promise `新建后立即执行，所以首先输出的是`Promise`。然后，`then`方法指定的回调函数，将在当前脚本所有同步任务执行完才会执行（因为`javascript`是单线程的），所以`resolved`最后输出。

**下面是异步加载图片的例子。**
```javascript
function loadImageAsync(url) {
  return new Promise(function(resolve, reject) {
    const image = new Image();
    image.onload = function() {
      resolve(image);
    };
    image.onerror = function() {
      reject(new Error('Could not load image at ' + url));
    };
    image.src = url;
  });
}
```
上面代码中，使用`Promise`包装了一个图片加载的异步操作。如果加载成功，就调用`resolve`方法，否则就调用`reject`方法。

**下面是一个用`Promise`对象实现的 Ajax 操作的例子。**
```javascript
const getJSON = function(url) {
  const promise = new Promise(function(resolve, reject){
    const handler = function() {
      if (this.readyState !== 4) {
        return;
      }
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
    const client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();
  });
  return promise;
};
getJSON("/posts.json").then(function(json) {
  console.log('Contents: ' + json);
}, function(error) {
  console.error('出错了', error);
});
```
上面代码中，`getJSON`是对 XMLHttpRequest 对象的封装，用于发出一个针对 JSON 数据的 HTTP 请求，并且返回一个`Promise`对象。需要注意的是，在`getJSON`内部，`resolve`函数和`reject`函数调用时，都带有参数。

如果调用`resolve`函数和`reject`函数时带有参数，那么它们的参数会被传递给回调函数。`reject`函数的参数通常是`Error`对象的实例，表示抛出的错误；`resolve`函数的参数除了正常的值以外，还可能是另一个 Promise 实例，比如像下面这样。
```javascript
const p1 = new Promise(function (resolve, reject) {
  // ...
});
const p2 = new Promise(function (resolve, reject) {
  // ...
  resolve(p1);
})
```
上面代码中，`p1`和`p2`都是 Promise 的实例，但是`p2`的`resolve`方法将`p1`作为参数，即一个异步操作的结果是返回另一个异步操作。


