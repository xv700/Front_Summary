
<h1 align="center">前端跨域</h1>

## 定义    

> 即请求的地址与被请求的地址协议头、域名、端口有一个不一样就叫跨域。相反,不跨域即叫同源。

| http://www.example.com/detail.html | 与以下地址对比 |  
| --- | --- |  
| http://api.example.com/detail.html | 不同源，子域域名不同，根域名相同 |  
| http://nav.qianlimu.net/ | 不同源，域名完全不同 |  
| https://www.example.com/detail.html | 不同源，协议不同 |  
| http://www.example.com:8080/detail.html | 不同源，端口不同 |  
| http://www.example.com/other.html | 同源，只是目录不同 |  

### 同源策略限制以下几种行为：

- Cookie、LocalStorage 和 IndexDB 无法读取  
- DOM和JS对象无法获得  
- AJAX 请求不能发送  

-  前端跨域解决方案


