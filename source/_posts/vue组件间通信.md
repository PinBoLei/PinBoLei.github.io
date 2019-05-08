---
title: vue组件间通信
copyright: true
date: 2019-05-07 10:28:55
tags:
- 组件间通信
- prop
categories:
- vue
comments: true
keywords: prop
---
>简单说一下vue组件间的通信
***
<!-- more -->

## 1.父组件向子组件传递数据

**通过 Prop 向子组件传递数据**

1.在父组件中注册子组件
2.在子组件中声名props,接收从父组件传过来的值
3.在子组件的标签中使用props创建的属性
4.在父组件中，把要传给子组件的值赋值给props创建的属性

示例：

```html
<body>
    <div id="app">
        <child :message="message"></child>
    </div>
</body>
<script type="text/javascript">    
// 注册
Vue.component('child', {
  // 声明 props
  props: ['message'],
  // 同样也可以在 vm 实例中像 “this.message” 这样使用
  template: '<span>{{ message }}</span>'
})
// 创建根实例
new Vue({
  el: '#app',
  data:{
    message:'Hello'
  }
})
</script>
```
![父组件向子组件传递数据](pic1.png)


## 2.子组件向父组件传递数据

**子组件通过事件向父组件发送消息**

1.在子组件中以某种方式，触发一个自定义事件
2.利用$emit,将需要传的值作为第二个参数传过去，或者只是触发父组件中相对应的事件
3.父组件在使用子组件的地方直接用 v-on 来监听子组件触发的事件，通过 $event 访问到子组件传过来的值，或者这个事件处理函数是一个方法。

示例：

```html
<div id="app">
	<div id="counter-event-example">
	  <p>{{ total }}</p>
	  // 'v-on' 可用 '@' 代替，'v-bind' 可用 ':'代替
	  // 用 v-on 来监听子组件触发的事件
	  <button-counter @increment="incrementTotal"></button-counter>
	  <button-counter @increment="incrementTotal"></button-counter>
	</div>
</div>

<script>
// 子组件
Vue.component('button-counter', {
  template: '<button @click="incrementHandler">{{ counter }}</button>',
  data: function () {
    return {
      counter: 0
    }
  },
  methods: {
    incrementHandler: function () {
      this.counter += 1
      // 触发一个自定义事件
      this.$emit('increment')
    }
  },
})
new Vue({
  el: '#counter-event-example',
  data: {
    total: 0
  },
  methods: {
    incrementTotal: function () {
      this.total += 1
    }
  }
})
</script>
```

![子组件通过事件向父组件发送消息](pic2.png)

**有时我们也会同时用到这两种通信方式**

示例：

```html
<div id="app">
 <div id="counter-event-example">
   <p>我是子组件传递给父组件的数据:</p>
   <p>{{ total }}</p>
   <button-counter @increment="incrementTotal" :message='message'></button-counter>
 </div>
</div>
<script type="text/javascript">
 Vue.component('button-counter', {
   template: '<div><p>我是父组件传给子组件的信息:</p><span>{{message}}</span><button @click="incrementHandler">{{ counter }}</button></div>',
   props: ['message'],
   data: function () {
     return {
       counter: 0
     }
   },
   methods: {
     incrementHandler: function () {
       this.counter += 1;
       this.$emit('increment', this.counter);
     }
   },
 })
 new Vue({
   el: '#counter-event-example',
   data: {
     total: 0,
     message: '请点击按钮'
   },
   methods: {
     incrementTotal: function (counter) {
       this.total = counter;
     }
   }
 })
</script>
```

![子父组件间通信](pic3.png)
