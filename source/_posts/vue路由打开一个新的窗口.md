---
title: vue路由打开一个新的窗口
copyright: true
date: 2018-12-03 15:46:19
tags:
- router-link
- 路由
categories:
- vue
comments: true
keywords: router-link
---

>简单说一下vue路由如何打开一个新的窗口
***

<!-- more -->
### router-link标签

在vue的官方文档中

![图1-1](图一.png)

看到这大家应该会想，既然`router-link`不支持`target="_blank"`属性，那我们该怎么用`router-link`打开一个新的窗口呢？别急，继续往下看~

文档中还有一处描述
![图1-2](图二.png)

`router-link`添加`tag="li"`属性后，居然可以变成`li`标签渲染出来，真特么神奇哈，那可不可以写成`tag="a"`,从而去替代a标签呢？我们尝试着写一下

```html
<router-link tag="a" target="_blank" to="/about">新品</router-link>
```

### 编程导航

![图1-3](图三.png)

上图是官网的最新说法，vue2.0以后`router.go`和`router.push`就不支持新窗口打开的属性了，现在用一种新的方式`router.resolve`

```javascript
let routeData = this.$router.resolve({
  path: "/about",
  query: {
    name:'lei',
    age: 18,
    phoneNum:12345678901 
  }
});
window.open(routeData.href, '_blank');
```