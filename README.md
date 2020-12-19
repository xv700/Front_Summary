

<h1 align="center">前端技术总结</h1>

国内镜像:[码云](https://gitee.com/xv700/Message-communication-for-web)   
GITHUB:[git](https://github.com/xv700/Front_summary   


## 场景问题

###  001.前端跨域

-  跨域定义    

> 即请求的地址与被请求的地址协议头、域名、端口有一个不一样就叫跨域。相反,不跨域即叫同源。

建议右键新开页面阅读：[前端跨域](/001.Cross-Origin)  

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

## 原生js问题

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


