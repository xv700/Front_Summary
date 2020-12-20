
<h1 align="center">前端跨域 Cross-Domain</h1>

## 定义    
   
> 首先什么是同源策略？同源策略/SOP（Same origin policy）是一种约定，由Netscape公司1995年引入浏览器，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到**XSS**、**CSFR**等攻击。所谓同源是指"协议+域名+端口"三者相同，即便两个不同的域名指向同一个ip地址，也非同源。
   
### 同源策略限制以下几种行为：

- Cookie、LocalStorage 和 IndexDB 无法读取  
- DOM和JS对象无法获得  
- AJAX 请求不能发送 

有些标签是允许跨域加载资源： 

```html  
<img src=XXX />  
<link href=XXX />  
<script src=XXX />   
<iframe src=XXX />   
```

### 出现跨域的场景

| http://www.example.com/detail.html | 与以下地址对比 |  
| --- | --- |  
| http://api.example.com/detail.html | 不同源，子域域名不同，根域名相同 |  
| http://nav.qianlimu.net/ | 不同源，域名完全不同 |  
| https://www.example.com/detail.html | 不同源，协议不同 |  
| http://www.example.com:8080/detail.html | 不同源，端口不同 |  
| http://www.example.com/other.html | 同源，只是目录不同 |  

- 1.如果是协议和端口造成的跨域问题“前台”是无能为力的。  
- 2.在跨域问题上，仅仅是通过“URL的首部”来识别而不会根据域名对应的IP地址是否相同来判断。“URL的首部”可以理解为“协议, 域名和端口必须匹配”。  

### 前端跨域解决方案   

https://segmentfault.com/a/1190000011145364



#### 一、 1、 通过jsonp跨域   

<p>通常为了减轻web服务器的负载，我们把js、css，img等静态资源分离到另一台独立域名的服务器上，在html页面中再通过相应的标签从不同域名下加载静态资源，而被浏览器允许，基于此原理，我们可以通过动态创建script，再请求一个带参网址实现跨域通信。</p>  

```html
<!--index.html-->
 <script>
    var script = document.createElement('script');
    script.type = 'text/javascript';

    // 传参一个回调函数名给后端，方便后端返回时执行这个在前端定义的回调函数
    script.src = 'http://www.domain2.com:8080/login?user=admin&callback=handleCallback';
    document.head.appendChild(script);

    // 回调执行函数
    function handleCallback(res) {

        console.log(res);//这里就输出了服务端返回的数据
    }
 </script>
```


<p>服务端返回如下（返回时即执行全局函数）：</p>  
```js
// 服务端的/login
handleCallback({"status": true, "user": "admin"})
```  

<p>后端node.js代码示例：</p>    

```js
var querystring = require('querystring');
var http = require('http');
var server = http.createServer();

server.on('request', function(req, res) {
    var params = qs.parse(req.url.split('?')[1]);
    var fn = params.callback;

    // jsonp返回设置
    res.writeHead(200, { 'Content-Type': 'text/javascript' });
    res.write(fn + '(' + JSON.stringify(params) + ')');

    res.end();
});

server.listen('8080');
console.log('Server is running at port 8080...');
```  


2、 document.domain + iframe跨域  
3、 location.hash + iframe  
4、 window.name + iframe跨域  
5、 postMessage跨域  
6、 跨域资源共享（CORS）  
7、 nginx代理跨域   
8、 nodejs中间件代理跨域  
9、 WebSocket协议跨域  

