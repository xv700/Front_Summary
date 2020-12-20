

<h1 align="center">前端技术总结</h1>

国内码云地址:https://gitee.com/xv700/Message-communication-for-web

GITHUB地址:https://github.com/xv700/Front_Summary


## 场景问题

###  001.前端跨域 

#### 前端跨域定义    

> 首先什么是同源策略？同源策略/SOP（Same origin policy）是一种约定，由Netscape公司1995年引入浏览器，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到**XSS**、**CSFR**等攻击。所谓同源是指"协议+域名+端口"三者相同，即便两个不同的域名指向同一个ip地址，也非同源。

建议右键新开页面阅读：[前端跨域](/001.Cross-Domain)  

### 002.网页消息传递

#### 定义  

> 在浏览器中，两个不同页面（A页面的window ！= B页面的window）网页之间的消息传递。

建议右键新开页面阅读：[网页消息传递详说](/002.Message_Communication)  

### 003.事件冒泡的问题

#### 定义   

> 当一个元素接收到事件的时候 会把他接收到的事件传给自己的父级，一直到window 。（注意这里传递的仅仅是事件并不传递所绑定的事件函数。所以如果父级没有绑定事件函数，就算传递了事件 也不会有什么表现 但事件确实传递了。）

```js
var div1 = document.getElementById("div1");
var div2 = document.getElementById("div2");
   div2.onclick = function(){alert(1);};
   div1.onclick = function(){alert(2);};//父亲
//html代码

 <div id="div1">

    <div id="div2"></div>
 </div>
```
### 003.浮点运算的精度

（例：0.1+0.7=0.7999999999999999）

https://www.html.cn/archives/7340/
https://www.cnblogs.com/powerwu/articles/13023859.html

### 004.获取浏览器当前页面的滚动条高度


的兼容写法


### 005.如何监听页面缩放？如何监听浏览器切换，包括切换tab、最小化

### 006.监听某个div滚动，div宽高变化

### 007.获取鼠标坐标

### 008.禁止表单记住密码自动填充

https://github.com/haizlin/fe-interview/issues/494

### 009.复制粘贴，粘贴截图

### 010.垂直水平居中

## 原生API用法

### 001.Blob的使用

### 002.Web Worker
 
### 003.IndexedDB 

### 004.Canvas

### 005.WebSocket

非原生的分离出去

## js加密

https://github.com/ricmoo/aes-js

## 图片压缩

https://github.com/fengyuanchen/compressorjs

## js库打包rollupjs

https://www.rollupjs.com/


## js操作PDF

## js操作excel

## js复制粘贴，粘贴截图
