---
title: vue组件keep-alive
copyright: true
date: 2019-01-04 11:17:53
tags:
- keep-alive
categories:
- vue
comments: true
keywords: keep-alive
---
>vue组件 keep-alive使用简介
***
<!--more-->

keep-alive适用于动态组件中，当在组件之间切换的时候，你有时会想保持这些组件的状态，以避免反复重渲染导致的性能问题。

keep-alive让组件实例能够被在它们第一次被创建的时候缓存下来。

**注意这个 `<keep-alive>` 要求被切换到的组件都有自己的名字，不论是通过组件的 name 选项还是局部/全局注册。**

### Props：
`include` - 字符串或正则表达式。只有名称匹配的组件会被缓存。
`exclude` - 字符串或正则表达式。任何名称匹配的组件都不会被缓存。
`max` - 数字。最多可以缓存多少组件实例。

### 用法：
`<keep-alive>` 包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。
`<keep-alive> `是一个抽象组件：它自身不会渲染一个 DOM 元素，也不会出现在父组件链中。
当组件在 `<keep-alive>` 内被切换，它的 `activated` 和 `deactivated` 这两个生命周期钩子函数将会被对应执行。

### 示例：
主要用于保留组件状态或避免重新渲染。

```html
<!-- 基本 -->
<keep-alive>
  <component :is="view"></component>
</keep-alive>

<!-- 多个条件判断的子组件 --> 
<keep-alive>
  <comp-a v-if="a > 1"></comp-a>
  <comp-b v-else></comp-b>
</keep-alive>

<!-- `<transition>` 一起使用 -->
<transition>
  <keep-alive>
    <component :is="view"></component>
  </keep-alive>
</transition>
```

**`include` and `exclude`**

`include` 和 `exclude `属性允许组件有条件地缓存。二者都可以用逗号分隔字符串、正则表达式或一个数组来表示：

```html
<!-- 逗号分隔字符串 -->
<keep-alive include="a,b">
  <component :is="view"></component>
</keep-alive>

<!-- 正则表达式 (使用 `v-bind`) -->
<keep-alive :include="/a|b/">
  <component :is="view"></component>
</keep-alive>

<!-- 数组 (使用 `v-bind`) -->
<keep-alive :include="['a', 'b']">
  <component :is="view"></component>
</keep-alive>
```
具体示例：

一个简单的tab切换,可以尝试把`<keep-alive>`去掉之后,对比一下,然后就会发现它的好处。

test.vue
```html
<template>
  <div class="test">
    <div class="testNav">
      <div :class="{'selected':tab === 1,'testTitle':true}" @click="toTab(1)">标题一</div>
      <div :class="{'selected':tab === 2,'testTitle':true}"  @click="toTab(2)">标题二</div>
    </div>
    <div class="container">
      <keep-alive>
        <Test1 v-if="tab === 1">
        </Test1>
        <Test2 v-else>
        </Test2>
      </keep-alive>
    </div>
  </div>
</template>
```
```javascript
<script>
  import Test1 from './test1.vue';
  import Test2 from './test2.vue';
  export default {
    data() {
      return {
          tab: 1,
      };
    },
    components: {
      Test1,
      Test2,
    },
    methods: {
      toTab(index) {
          this.tab = index;
      },
    },
  }
</script>

<style lang="less">
.test {
  width: 100%;
  .testNav {
    height: 60px;
    line-height: 60px;
    display: flex;
    border-bottom: 1px solid #e5e5e5;
    .testTitle {
      flex: 1;
      text-align: center;
    }
    .selected {
      color: red;
    }
  }
}
</style>
```
test1.vue

```javascript
<template>
  <div class="test1">
    test1
    {{testInfo1}}
  </div>
</template>

<script>
  export default {
    data() {
      return {
          testInfo1: '',
      };
    },
    activated() {
      console.log('测试1被激活');
    },
    deactivated() {
      console.log('测试1被缓存');
    },
    created() {
      setTimeout(() => {
        this.testInfo1 = '这是测试一的数据';
      }, 2000);
    },
  }
</script>
```

test2.vue

```javascript
<template>
  <div>
    test2
    {{testInfo2}}
  </div>
</template>

<script>
  export default {
    data() {
      return {
        testInfo2: '',
      }
    },  
    activated() {
      console.log('测试2被激活');
    },
    deactivated() {
      console.log('测试2被缓存');
    },
    created() {
      setTimeout(() => {
        this.testInfo2 = '这是测试二的数据';
      }, 2000);
    },
  }
</script>
```
运行代码，打开控制台，你会更直观的看到`keep-alive`的作用，以及`activated`和`deactivated`这两个函数被触发的时间

**用setTimeout模拟请求后端接口**

1.刚打开页面：
![图1](pic1.png)

2.点击标题二

![图2](pic2.png)

3.再次点击标题一，你会发现信息会快速显示出来：

![图3](pic3.png)

> 上述示例代码原引用作者 funnycoderstar，链接https://juejin.im/post/5ad56d86518825556534ff4b

以上是添加了`keep-alive`的情况下，如果去掉`keep-alive`，每次切换tab你会发现都会重新请求一次数据，感兴趣的可以尝试一下。

**注**：匹配首先检查组件自身的 name 选项，如果 name 选项不可用，则匹配它的局部注册名称 (父组件 components 选项的键值)。匿名组件不能被匹配。

**`max`**

最多可以缓存多少组件实例。一旦这个数字达到了，在新实例被创建之前，已缓存组件中最久没有被访问的实例会被销毁掉。

```html
<keep-alive :max="10">
  <component :is="view"></component>
</keep-alive>
```


