---
title: 前端vue项目国际化——vue-i18n
copyright: true
date: 2018-12-03 14:01:33
tags:
- vue-i18n
- 国际化
categories:
- vue
comments: true
keywords: vue-i18n
---

>有时候我们的项目需要支持多种语言，切换语言设置时，就自动切换整个项目的文字显示。

***

<!-- more -->
### 安装 vue-i18n

```html
// npm 安装
npm install vue-i18n

// script 引入
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-i18n/dist/vue-i18n.js"></script>
```
### 使用
在 main.js 中引入 vue-i18n
```javascript
import VueI18n from 'vue-i18n';
Vue.use(VueI18n);

const i18n = new VueI18n({
    locale: 'zh-CN',    // 语言标识
    // this.$i18n.locale // 通过切换locale的值来实现语言切换
    messages: {
      'zh-CN': require('./languages/lang/zh'),   // 中文语言包
      'en-US': require('./languages/lang/en')    // 英文语言包
    }
})

new Vue({
  el: '#app',
  i18n,  // 把 i18n 挂载到 vue 根实例上
  store,
  router,
  template: '<App/>',
  components: { App }
})
```
上面即是将 vue-i18n 引入 vue 项目中，引入以后，实现国际化，当然至少需要两种语言，我们假设需要中英文两种语言切换，那么我们就需要中英两套语言的文件，只需要两个 js 文件，通过 require 的形式引入到 main.js。

**项目下新增一个目录languages**
```
---src
    --languages
       --lang
          -- zh.js // 中文语言包
          -- en.js // 英文语言包
          -- .. // 其他语言，暂未实践
```

`zh`

```javascript
export const m = {
    common: {
        message: '消息'
    },
    xxx: {
		...
    }
}
```

`en`

```javascript
export const m = {
    common: {
        message: 'Messages'
    },
    xxx: {
		...
    }
}
```

### 数据渲染

```html
<template>
    ...
    // v-text
    <div v-text="$t('m.common.message')"></div>
    // {{}}
    <div>{{$t('m.common.message')}}</div>
    ...
</template>

// js
$t('m.common.message')
```

### 语言切换

如何实现中英文的切换呢？
```javascript
...
locale: 'zh-CN',    // 语言标识
    messages: {
      'zh-CN': require('./languages/lang/zh'),   // 中文语言包
      'en-US': require('./languages/lang/en')    // 英文语言包
    }
...
```
在main.js中，我们可以发现，当locale的值为`zh-CN`时，当前语言为中文，当locale的值为`en-US`时，当前语言为英文。

我们可以做一个切换按钮，点击来实现切换中英文。
```javascript
// 点击事件，切换语言
switchLang () {
  this.$confirm('您确定切换语言吗?', '提示', {
       confirmButtonText: '确定',
       cancelButtonText: '取消',
       type: 'warning'
    }).then(() => {
       const locale = this.$i18n.locale;
       locale === 'zh-CN' ? this.$i18n.locale = 'en-US' : this.$i18n.locale = 'zh-CN';
    }).catch(() => {
       this.$message({
           type: 'info',
       });          
  });
}
```