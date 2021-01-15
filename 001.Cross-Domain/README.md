
<h1 align="center">前端跨域 Cross-Domain</h1>

### 定义    
   
首先什么是同源策略？  
同源策略/SOP（Same origin policy）是一种约定，由Netscape公司1995年引入浏览器，它是浏览器最核心也最基本的安全功能，如果缺少了同源策略，浏览器很容易受到XSS、CSFR等攻击。  
所谓同源是指"协议+域名+端口"三者相同，即便两个不同的域名指向同一个ip地址，也非同源。   

那么什么是前端跨域呢？  
前端跨域是指一个域下的文档或者脚本试图去请求另外一个域下的资源。
   
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

#### 1、通过jsonp跨域(解决ajax跨域问题)   

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

window.name属性的独特之处：name值在不同的页面（甚至不同域名）加载后依旧存在，并且可以支持非常长的 name 值（2MB）。  

<p> a.html：(http://www.domain1.com/a.html)</p>   

```html   
var proxy = function(url, callback) {
    var state = 0;
    var iframe = document.createElement('iframe');

    // 加载跨域页面
    iframe.src = url;

    // onload事件会触发2次，第1次加载跨域页，并留存数据于window.name
    iframe.onload = function() {
        if (state === 1) {
            // 第2次onload(同域proxy页)成功后，读取同域window.name中数据
            callback(iframe.contentWindow.name);
            destoryFrame();

        } else if (state === 0) {
            // 第1次onload(跨域页)成功后，切换到同域代理页面
            iframe.contentWindow.location = 'http://www.domain1.com/proxy.html';
            state = 1;
        }
    };

    document.body.appendChild(iframe);

    // 获取数据以后销毁这个iframe，释放内存；这也保证了安全（不被其他域frame js访问）
    function destoryFrame() {
        iframe.contentWindow.document.write('');
        iframe.contentWindow.close();
        document.body.removeChild(iframe);
    }
};

// 请求跨域b页面数据
proxy('http://www.domain2.com/b.html', function(data){
    alert(data);
});
```   

proxy.html：(http://www.domain1.com/proxy.html)
中间代理页，与a.html同域，内容为空即可。    

<p>b.html：(http://www.domain2.com/b.html)</p>   

```html   
<script>
    window.name = 'This is domain2 data!';
</script>
```   

总结：通过iframe的src属性由外域转向本地域，跨域数据即由iframe的window.name从外域传递到本地域。这个就巧妙地绕过了浏览器的跨域访问限制，但同时它又是安全操作。  


5、 postMessage跨域    

点击查看详情：http://xv700.gitee.io/message-communication-for-web/#postMessage


### 以下方案都是借助服务端实现的  

6、 跨域资源共享（CORS）  

点击查看详情：http://www.ruanyifeng.com/blog/2016/04/cors.html  

7、 nginx代理跨域    

点击查看详情：https://segmentfault.com/a/1190000012859206 

8、 nodejs中间件代理跨域   

node中间件实现跨域代理，原理大致与nginx相同，都是通过一个代理服务器，实现数据的转发，也可以通过设置cookieDomainRewrite参数修改响应头中cookie中域名，实现当前域的cookie写入，方便接口登录认证。  

9、 WebSocket协议跨域  

