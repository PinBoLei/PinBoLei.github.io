---
title: Axios-指南
copyright: true
date: 2018-11-30 09:23:04
tags:
- axios
categories:
- axios
- ES6
- 前端
comments: true
---

*分享一下axios的相关知识*

***
<!-- more -->

## 一、axios
基于`promise`用于浏览器和`node.js`的http客户端
## 二、特点

 - 支持浏览器和node.js
 - 支持promise
 - 能拦截请求和响应
 - 能转换请求和响应数据
 - 能取消请求
 - 自动转换JSON数据
 - 浏览器端支持防止CSRF(跨站请求伪造)


## 三、安装
`npm安装`
```javascript
npm install axios
```
`bower安装`
```javascript
bower install axios
```
`cdn引入`
```javascript
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```
## 四、例子
1.发送一个`GET`请求
```javascript
// 通过给定的ID来发送请求
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
// 以上请求也可以通过这种方式来发送
axios.get('/user',{
  params:{
    ID:12345
  }
})
.then(function (response) {
  console.log(response);
})
.catch(function (error) {
  console.log(error);
});
```
2.发送一个`POST`请求
```javascript
axios.post('/user',{
  firstName: 'Fred',
  lastName: 'Flintstone'
})
.then(function (response) {
  console.log(response);
})
.catch(function (error) {
  console.log(error);
});
```
3.同时发起多个请求
```javascript
function getUserAccount () {
  return axios.get ('/user/12345');
};
function getUserPermissions () {
  return axios.get ('/user/12345/permissions');
};
axios.all([getUserAccount(),getUserPermissions()])
  .then(axios.spread (function (acct,perms) {
    // 当这两个请求都完成的时候会触发这个函数，两个参数分别代表返回的结果
  }));
```
## 五、axios的API
**（一）axios可以通过配置（`config`）来发送请求**
1.`axios(config)`
```javascript
// 发送一个`POST`请求
axios({
  method: 'POST',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }
});
```
```javascript
// 获取远程图片
axios({
  method:'get',
  url:'http://bit.ly/2mTM3nY',
  responseType:'stream'
})
  .then(function(response) {
  response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
});
```
2.`axios(url[, config])`

```javascript
// 发起一个GET请求（GET是默认的请求方法）
axios('/user/12345');
```
**（二）请求方法别名**
为了方便我们为所有支持的请求方法均提供了别名。
```javascript
axios.request(url);
axios.get(url[,config]);
axios.delete(url[,config]);
axios.head(url[,config]);
axios.options(url[,config]);
axios.post(url[,data[,config]]);
axios.put(url[,data[,config]]);
axios.patch(url[,data[,config]]);
```
 - **注意**：当使用以上别名方法时，`url`，`method`和`data`等属性不用在config重复声明。

**（三）同时发生的请求（Concurrency）**
以下两个用来处理同时发生多个请求的辅助函数
```javascript
// iterable是一个可以迭代的参数,如数组等
axios.all(iterable);
// callback要等到所有请求都完成才会执行
axios.spread(callback)
```
**（四）创建一个实例**
你可以创建一个拥有通用配置的`axios`实例
1.`axios.creat([config])`
```javascript
var instance = axios.cerate({
  baseURL:'https://some-domain.com/api/',
  timeout:1000,
  headers:{'X-Custom-Header':'foobar'}
});
```
2.实例的方法
以下是所有可用的实例方法，额外声明的配置将与利用create创建的实例配置合并
```javascript
axios#request(config)
axios#get(url[, config])
axios#delete(url[, config])
axios#head(url[, config])
axios#options(url[, config])
axios#post(url[, data[, config]])
axios#put(url[, data[, config]])
axios#patch(url[, data[, config]])
```
## 六、请求的配置（request config）
以下就是请求的配置选项，只有`url`选项是必须的，如果`method`选项未定义，那么它默认是以`GET`的方式发出请求
```javascript
{
  // `url`是请求的服务器地址
  url:'/user',
  //`method`是请求资源的方式
  method:'get'//default
  //如果`url`不是绝对地址，那么`baseURL`将会加到`url`的前面
  //当`url`是相对地址的时候，设置`baseURL`会非常的方便
  baseURL:'https://some-domain.com/api/',
  //`transformRequest`选项允许我们在请求发送到服务器之前对请求的数据做出一些改动
  //该选项只适用于以下请求方式：`put/post/patch`
  //数组里面的最后一个函数必须返回一个字符串、-一个`ArrayBuffer`或者`Stream`
  transformRequest:[function(data){
    //在这里根据自己的需求改变数据
    return data;
  }],
  //`transformResponse`选项允许我们在数据传送到`then/catch`方法之前对数据进行改动
  transformResponse:[function(data){
    //在这里根据自己的需求改变数据
    return data;
  }],
  //`headers`选项是需要被发送的自定义请求头信息
  headers: {'X-Requested-With':'XMLHttpRequest'},
  //`params`选项是要随请求一起发送的请求参数----一般链接在URL后面
  //他的类型必须是一个纯对象或者是URLSearchParams对象
  params: {
    ID:12345
  },
  //`paramsSerializer`是一个可选的函数，起作用是让参数（params）序列化
  //例如(https://www.npmjs.com/package/qs,http://api.jquery.com/jquery.param)
  paramsSerializer: function(params){
    return Qs.stringify(params,{arrayFormat:'brackets'})
  },
  //`data`选项是作为一个请求体而需要被发送的数据
  //该选项只适用于方法：`put/post/patch`
  //当没有设置`transformRequest`选项时dada必须是以下几种类型之一
  //string/plain/object/ArrayBuffer/ArrayBufferView/URLSearchParams
  //仅仅浏览器：FormData/File/Bold
  //仅node:Stream
  data {
    firstName:"Fred"
  },
  //`timeout`选项定义了请求发出的延迟毫秒数
  //如果请求花费的时间超过延迟的时间，那么请求会被终止

  timeout:1000,
  //`withCredentails`选项表明了是否是跨域请求
  
  withCredentials:false,//default
  //`adapter`适配器选项允许自定义处理请求，这会使得测试变得方便
  //返回一个promise,并提供验证返回
  adapter: function(config){
    /*..........*/
  },
  //`auth`表明HTTP基础的认证应该被使用，并提供证书
  //这会设置一个authorization头（header）,并覆盖你在header设置的Authorization头信息
  auth: {
    username:"zhangsan",
    password: "s00sdkf"
  },
  //返回数据的格式
  //其可选项是arraybuffer,blob,document,json,text,stream
  responseType:'json',//default
  //
  xsrfCookieName: 'XSRF-TOKEN',//default
  xsrfHeaderName:'X-XSRF-TOKEN',//default
  //`onUploadProgress`上传进度事件
  onUploadProgress:function(progressEvent){
    //下载进度的事件
onDownloadProgress:function(progressEvent){
}
  },
  //相应内容的最大值
  maxContentLength:2000,
  //`validateStatus`定义了是否根据http相应状态码，来resolve或者reject promise
  //如果`validateStatus`返回true(或者设置为`null`或者`undefined`),那么promise的状态将会是resolved,否则其状态就是rejected
  validateStatus:function(status){
    return status >= 200 && status <300;//default
  },
  //`maxRedirects`定义了在nodejs中重定向的最大数量
  maxRedirects: 5,//default
  //`httpAgent/httpsAgent`定义了当发送http/https请求要用到的自定义代理
  //keeyAlive在选项中没有被默认激活
  httpAgent: new http.Agent({keeyAlive:true}),
  httpsAgent: new https.Agent({keeyAlive:true}),
  //proxy定义了主机名字和端口号，
  //`auth`表明http基本认证应该与proxy代理链接，并提供证书
  //这将会设置一个`Proxy-Authorization` header,并且会覆盖掉已经存在的`Proxy-Authorization`  header
  proxy: {
    host:'127.0.0.1',
    port: 9000,
    auth: {
      username:'skda',
      password:'radsd'
    }
  },
  //`cancelToken`定义了一个用于取消请求的cancel token
  //详见cancelation部分
  cancelToken: new cancelToken(function(cancel){
  })
}
```
>作者：FunnySeeker
链接：https://www.jianshu.com/p/df464b26ae58
來源：简书

## 六、响应组成（请求返回的内容）
1.`response`由以下几部分信息组成
```javascript
{
  // 服务端返回的数据
  data: {},
  // 服务端返回的状态码
  status: 200,
  // 服务端返回的状态信息
  statusText: 'OK',
  // 响应头,所有的响应头名称都是小写
  headers: {},
  // axios请求配置
  config: {},
  // 请求
  request: {}
}
```
2.用`then`接收以下响应信息
```javascript
axios.get('/user/12345')
  .then(function(response) {
    console.log(response.data);
    console.log(response.status);
    console.log(response.statusText);
    console.log(response.headers);
    console.log(response.config);
  });
```
## 七、默认配置
1.**全局修改axios默认配置**
```javascript
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```
2.**自定义的实例默认设置**
```javascript
// 创建实例时修改配置
var instance = axios.create({
  baseURL: 'https://api.example.com'
});
// 实例创建之后修改配置
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;
```
3.**配置优先级**
配置项通过一定的规则合并，`request config `> `instance.defaults`> `系统默认`，优先级高的覆盖优先级低的。
```javascript
// 创建一个实例，这时的超时时间为系统默认的 0
var instance = axios.create();
// 通过instance.defaults重新设置超时时间为2.5s，因为优先级比系统默认高
instance.defaults.timeout = 2500;
// 通过request config重新设置超时时间为5s，因为优先级比instance.defaults和系统默认都高
instance.get('/longRequest', {
  timeout: 5000
});
```
## 八、拦截器
1.你可以在`then`和`catch`之前拦截请求和响应。
```javascript
// 添加一个请求拦截器
axios.interceptors.request.use(function (config) {
    // Do something before request is sent
    return config;
  }, function (error) {
    // Do something with request error
    return Promise.reject(error);
  });

// 添加一个响应拦截器
axios.interceptors.response.use(function (response) {
    // Do something with response data
    return response;
  }, function (error) {
    // Do something with response error
    return Promise.reject(error);
  });
```
2.移除拦截器
```javascript
var myInterceptor = axios.interceptors.request.use(function () {/*...*/});
axios.interceptors.request.eject(myInterceptor);
```
3.为axios实例添加一个拦截器
```javascript
var instance = axios.create();
instance.interceptors.request.use(function () {/*...*/});
```
## 九、错误处理
```javascript
axios.get('/user/12345')
  .catch(function (error) {
    if (error.response) {
      // 发送请求后，服务端返回的响应码不是 2xx   
      console.log(error.response.data);
      console.log(error.response.status);
      console.log(error.response.headers);
    } else if (error.request) {
      // 发送请求但是没有响应返回
      console.log(error.request);
    } else {
      // 其他错误
      console.log('Error', error.message);
    }
    console.log(error.config);
  });
```
可以用`validateStatus`定义一个http状态码返回的范围
```javascript
axios.get('/user/12345', {
  validateStatus: function (status) {
    return status < 500; // Reject only if the status code is greater than or equal to 500
  }
})
```
## 十、取消请求

 - 你可以通过`cancel token`来取消一个请求
 1.你可以通过`CancelToken.source`工厂函数来创建一个`cancel token`
```javascript
ar CancelToken = axios.CancelToken;
var source = CancelToken.source();

axios.get('/user/12345',{
  cancelToken: source.token
}).catch(function(thrown){
  if(axios.isCancel(thrown)){
    console.log('Request canceled',thrown.message);
  }else {
    //handle error
  }
});

//取消请求（信息的参数可以设置的）
source.cance("操作被用户取消");
```
2.你可以给`cancelToken`构造函数传递一个`executor function`来创建一个`cancel token`
```javascript
var cancelToken = axios.CancelToken;
var cance;
axios.get('/user/12345',{
  cancelToken: new CancelToken(function(c){
    //这个executor函数接受一个cancel function作为参数
    cancel = c;
  })
})
//取消请求
cancel();
```
