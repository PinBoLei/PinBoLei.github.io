---
title: Chrome(谷歌)控制台，console实用教程
copyright: true
date: 2018-11-29 14:41:33
tags:
- console
- 前端
categories:
- console
- javascript
- 前端
comments: true
---

*大家在调试程序的时候，经常会用到控制台，在console下调试各种bug,在此整理了控制台console的一些用法，希望能够帮到你，话不多说，上干货*

### 一、先简单的介绍一下chrome的控制台

1.**Windows**：打开chrome浏览器，按f12就可以轻松的打开控制台（这里着重介绍下mac的，其实都一样，只是博主只有mac 😝）

2.**mac**:打开chrome浏览器，按fn+ f12就可以轻松的打开控制台（原谅我在此给百度打了一下广告，emmmm....我会考虑向他们收取广告费的..）

![图一](https://img-blog.csdnimg.cn/20181030150050919.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

如果此时你发现你的控制台并不向我的一样在右面，没关系，继续往下看

![图二](https://img-blog.csdnimg.cn/20181030150438372.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

首先，看箭头所指的地方有竖着的三个点，没错，就是他，请毫不犹豫的点击它

![图三](https://img-blog.csdnimg.cn/20181030150611884.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

这时你会发现，出来这么个东西，重点观察最里面的小红框中的几个小方块，从左依次往右，当你点击第一个，会弹出一个窗口，如下

![图四](https://img-blog.csdnimg.cn/20181030151247455.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

这就是将控制台作为一个窗口向我们展示，假如你关闭掉页面后，再次打开依然会是弹出框样式，此时不必惊慌，仔细发现，在这个弹出的页的右上角，还是有竖着的三个点，点击它会出现上一个图所示的情况，然后我们可以再点击第二个，会变成如下图所示

![图五](https://img-blog.csdnimg.cn/20181030152012423.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

此时你会发现控制台跑到左边去了，这时候你应该有种恍然大悟的感觉，是的，第三个第四个就是控制台在下面和在右面（剩下的就不贴图了，挺费事的）

*介绍完控制台，就该说一说console的用法了，终于可以好好说话了*！😂

**有小伙伴就问了为啥不用alert调试程序呢，设想一下，如果有一个数组，里面有超多的元素，但是你想知道这些元素都有哪些具体的值，如果此时用alert，那你真的会被自己整哭的，因为alert阻断线程运行，你不点击alert框的确定按钮下一个alert就不会出现。那如果用console呢？下面我们来做个测试：**

```javascript
let arr = [
  {name:'张三',age:13},
  {name:'李四',age:14},
  {name:'王五',age:15},
  {name:'小明',age:16},
  {name:'小华',age:17},
];
for (let item of arr) {
  console.log(item);
};
```
运行一下代码，发现要比alert好多了有木有~

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181030164036503.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

**注意**：刚打开控制台的时候，我们会发现控制台里有其他的东西，比如百度的彩蛋，其实就是招聘信息，这时我们并不想看到这些，怎么？你想看到吗？不，你不想...
那如何清除呢？

1.在控制台输入console.clear()或者直接输入clear(),运行（enter）一下，这时你发现控制台已经清空了

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181030155008651.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

2.你也可以通过点击左上角标出的标志，也可以清空控制台
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181030155503323.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

### 二、一般情况下我们用来输入信息的方法主要是用到如下五个

 - console.log &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&ensp;用于输出普通信息
 - console.info &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;用于输出提示性信息
 - console.error  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;用于输出错误信息
 - console.warn    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;用于输出警示信息
 - console.debug &nbsp;&nbsp;&nbsp;&nbsp;用于输出调试信息

![console](https://img-blog.csdnimg.cn/20181030153944190.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

 - 有小伙伴发现自己输入一个console方法后，想换行结果运行了，此时肯定一脸的懵逼😳，告诉你一个小技巧，```shift```+ ```return(enter)```就可以换行啦，开不开心，意不意外！😝

### 三、其实console还提供了其他的方法供我们使用，我们可以在控制台输入console打印一下查看

![console提供的方法](https://img-blog.csdnimg.cn/2018103017005462.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

### 四、console对象的上面5种方法，都可以使用printf风格的占位符。不过，占位符的种类比较少，只支持```字符（%s）```、```整数（%d或%i）```、```浮点数（%f）```和```对象（%o）```四种

例：
```javascript
console.log('%d年%d月%d日',2011,3,26); 
console.log('圆周率是%f',3.1415926);

```
输出如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181030170921340.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

**```%o```占位符，可以用来查看一个对象内部情况**

```javascript
let dog = {
	name:'金毛',
	color:'黄色',
};
console.log('%o',dog);
```

输出如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181030171441249.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

### 五、console.dirxml用来显示网页的某个节点（node）所包含的html/xml代码

例：
```javascript
<body>    
    <table id="mytable">        
        <tr>            
           <td>A</td>            
           <td>A</td>            
           <td>A</td>        
       </tr>        
       <tr>            
           <td>bbb</td>            
           <td>aaa</td>            
           <td>ccc</td>        
       </tr>        
       <tr>            
           <td>111</td>            
           <td>333</td>            
           <td>222</td>        
       </tr>    
    </table> 
</body> 
<script type="text/javascript">    
window.onload = function ()
{
  var mytable = document.getElementById('mytable');
  console.dirxml(mytable);   
} 
</script>
```

输出如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181030174759215.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

### 六、console.group输出一组信息的开头，console.groupEnd结束一组输出信息

例：
```javascript
console.group('aaa');
console.warn('aaa.aaa');
console.groupEnd();
```
输出如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181030175203223.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

### 七、console.assert对输入的表达式进行断言，只有表达式为false时，才输出相应的信息到控制台

例：
```javascript
let isDebug = false;
console.assert(isDebug,'为false时输出的信息');
```

输出如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/2018103017563363.png)

### 八、console.count 统计代码被执行的次数

例：
```javascript
function myFunction () {
  console.count('myFunction被执行的次数');
};
myFunction();
myFunction();
myFunction();
```
输出如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181030180148736.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

### 九、console.dir 直接将该DOM结点以DOM树的结构进行输出，可以详细查对象的方法发展等等

例：
```javascript
var myObject = {
  name:'aa',
  age:12,
  sex:'man',
  myFunc: function () {
    cpnsole.log('hello');
  }
};
console.dir(myObject);
```
输出如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181031092735658.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

### 十、console.time 计时开始,console.timeEnd 计时结束
例：
```javascript
// 用console.time来统计实例化1000000个对象所需时间
console.time('Array initialie');
var array = new Array(1000000);
for (var i = array.length - 1; i >= 0; i--) {
  array[i] = new Object();
};
console.timeEnd('Array initialie');
```
输出如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181031093421261.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

### 十一、再说下使用console.log的一些技巧

 1. 指定输出文字的样式
 2. 利用控制台输出图片

 例：
 ```javascript
// text
 console.log('%c 你看 ','color:red;font-size:5em;background-color:yellow');
 // 3D Text
 console.log("%c3D Text"," text-shadow: 0 1px 0 #ccc,0 2px 0 #c9c9c9,0 3px 0 #bbb,0 4px 0 #b9b9b9,0 5px 0 #aaa,0 6px 1px rgba(0,0,0,.1),0 0 5px rgba(0,0,0,.1),0 1px 3px rgba(0,0,0,.3),0 3px 5px rgba(0,0,0,.2),0 5px 10px rgba(0,0,0,.25),0 10px 10px rgba(0,0,0,.2),0 20px 20px rgba(0,0,0,.15);font-size:5em");
// Rainbow Text
console.log('%cRainbow Text ', 'background-image:-webkit-gradient( linear, left top, right top, color-stop(0, #f22), color-stop(0.15, #f2f), color-stop(0.3, #22f), color-stop(0.45, #2ff), color-stop(0.6, #2f2),color-stop(0.75, #2f2), color-stop(0.9, #ff2), color-stop(1, #f22) );color:transparent;-webkit-background-clip: text;font-size:5em;');
// Colorful CSS
console.log("%cColorful CSS","background: rgba(252,234,187,1);background: -moz-linear-gradient(left, rgba(252,234,187,1) 0%, rgba(175,250,77,1) 12%, rgba(0,247,49,1) 28%, rgba(0,210,247,1) 39%,rgba(0,189,247,1) 51%, rgba(133,108,217,1) 64%, rgba(177,0,247,1) 78%, rgba(247,0,189,1) 87%, rgba(245,22,52,1) 100%);background: -webkit-gradient(left top, right top, color-stop(0%, rgba(252,234,187,1)), color-stop(12%, rgba(175,250,77,1)), color-stop(28%, rgba(0,247,49,1)), color-stop(39%, rgba(0,210,247,1)), color-stop(51%, rgba(0,189,247,1)), color-stop(64%, rgba(133,108,217,1)), color-stop(78%, rgba(177,0,247,1)), color-stop(87%, rgba(247,0,189,1)), color-stop(100%, rgba(245,22,52,1)));background: -webkit-linear-gradient(left, rgba(252,234,187,1) 0%, rgba(175,250,77,1) 12%, rgba(0,247,49,1) 28%, rgba(0,210,247,1) 39%, rgba(0,189,247,1) 51%, rgba(133,108,217,1) 64%, rgba(177,0,247,1) 78%, rgba(247,0,189,1) 87%, rgba(245,22,52,1) 100%);background: -o-linear-gradient(left, rgba(252,234,187,1) 0%, rgba(175,250,77,1) 12%, rgba(0,247,49,1) 28%, rgba(0,210,247,1) 39%, rgba(0,189,247,1) 51%, rgba(133,108,217,1) 64%, rgba(177,0,247,1) 78%, rgba(247,0,189,1) 87%, rgba(245,22,52,1) 100%);background: -ms-linear-gradient(left, rgba(252,234,187,1) 0%, rgba(175,250,77,1) 12%, rgba(0,247,49,1) 28%, rgba(0,210,247,1) 39%, rgba(0,189,247,1) 51%, rgba(133,108,217,1) 64%, rgba(177,0,247,1) 78%, rgba(247,0,189,1) 87%, rgba(245,22,52,1) 100%);background: linear-gradient(to right, rgba(252,234,187,1) 0%, rgba(175,250,77,1) 12%, rgba(0,247,49,1) 28%, rgba(0,210,247,1) 39%, rgba(0,189,247,1) 51%, rgba(133,108,217,1) 64%, rgba(177,0,247,1) 78%, rgba(247,0,189,1) 87%, rgba(245,22,52,1) 100%);filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#fceabb', endColorstr='#f51634', GradientType=1 );font-size:5em");
// 输出动态图
console.log("%c ", "background: url(http://g.hiphotos.baidu.com/zhidao/wh%3D450%2C600/sign=7408a51e88d4b31cf0699cbfb2e60b49/c9fcc3cec3fdfc03aca05de5d73f8794a5c22696.jpg) no-repeat center;padding-left:640px;padding-bottom: 242px;");
```
输出如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181031100321100.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181031100551862.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181031100708375.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3BpbmJvbGVp,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181031110308636.gif)
