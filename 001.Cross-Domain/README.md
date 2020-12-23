
<h1 align="center">前端跨域 Cross-Domain</h1>

## 定义    
   
> 首先什么是同源策略？同源策略/SOP（Same origin policy）是一种约定，由Netscape公司1995年引入浏览器，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到XSS、CSFR等攻击。所谓同源是指"协议+域名+端口"三者相同，即便两个不同的域名指向同一个ip地址，也非同源。
   
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
还有样式中background:url()、@font-face()等文件外链
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
https://www.jianshu.com/p/66fcfac9ea33?utm_campaign


#### 1、通过jsonp跨域   

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
// 服务端 http://www.domain2.com:8080/login?user=admin&callback=handleCallback 输出
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


#### 2、document.domain + iframe跨域  

此方案仅限主域相同，子域不同的跨域应用场景。  
实现原理：两个页面都通过js强制设置document.domain为基础主域，就实现了同域。   

<p>父窗口：(http://www.domain.com/a.html)</p>   

```html  
 
<iframe id="iframe" src="http://child.domain.com/b.html"></iframe>
<script>
    document.domain = 'domain.com';
    var user = 'admin';
</script>
```  

<p>子窗口：(http://child.domain.com/b.html)</p>    

```html   
<script>  
    document.domain = 'domain.com';  
    // 获取父窗口中变量  
    console.log('get js data from parent ---> ' + window.parent.user);//输出父传给子窗口的参数  
</script>  
```    

#### 3、location.hash + iframe    

实现原理： a欲与b跨域相互通信，通过中间页c来实现。 三个页面，不同域之间利用iframe的location.hash传值，相同域之间直接js访问来通信。  

具体实现：A域：a.html -> B域：b.html -> A域：c.html，a与b不同域只能通过hash值单向通信，b与c也不同域也只能单向通信，但c与a同域，所以c可通过parent.parent访问a页面所有对象。  

<p> a.html：(http://www.domain1.com/a.html)</p>     

```html    
<iframe id="iframe" src="http://www.domain2.com/b.html" style="display:none;"></iframe>
<script>
    var iframe = document.getElementById('iframe');

    // 向b.html传hash值
    setTimeout(function() {
        iframe.src = iframe.src + '#user=admin';
    }, 1000);
    
    // 开放给同域c.html的回调方法
    function onCallback(res) {
        alert('data from c.html ---> ' + res);
    }
</script>
```   

<p> b.html：(http://www.domain2.com/b.html)</p>   

```html   
<iframe id="iframe" src="http://www.domain1.com/c.html" style="display:none;"></iframe>
<script>
    var iframe = document.getElementById('iframe');

    // 监听a.html传来的hash值，再传给c.html
    window.onhashchange = function () {
        iframe.src = iframe.src + location.hash;
    };
</script>
```   

<p> c.html：(http://www.domain1.com/c.html)</p>   

```html   
<script>
    // 监听b.html传来的hash值
    window.onhashchange = function () {
        // 再通过操作同域a.html的js回调，将结果传回
        window.parent.parent.onCallback('hello: ' + location.hash.replace('#user=', ''));
    };
</script>
```   

#### 4、window.name + iframe跨域    

5、 postMessage跨域  

6、 跨域资源共享（CORS）  

8、 nodejs中间件代理跨域  

7、 nginx代理跨域   
9、 WebSocket协议跨域  

