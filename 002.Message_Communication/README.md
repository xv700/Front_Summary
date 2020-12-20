
<h1 align="center">网页消息传递</h1>

## 定义
> 在浏览器中，两个不同页面（A页面的window ！= B页面的window）网页之间的消息传递。

## 可能出现的场景

1.浏览A页面内通过window.open打开N>=2个以上的tab页面，A页面与打开的tab页面互相通信的场景(包含跨域的情况)  
2.浏览A页面内通过非window.open(如a连接)  打开N>=2个以上的tab页面，A页面与打开的tab页面互相通信的场景(包含跨域的情况)  
3.浏览A页面内使用了N>=1个以上iframe，A页面与打开的iframe页面互相通信的场景(包含跨域的情况)  
4.浏览A页面内使用了N>=1个以上Webwork，A页面与打开的Webwork页面互相通信的场景(不能跨域)  

## 文档  

右键新打开页面阅读：https://xv700.gitee.io/message-communication-for-web/  

### WebSocket （可跨域）  

### Server-Sent Events  

### postMessage（可跨域）  

### Worker之SharedWorker  

### localStorage  

### Cookies  

### iframe 之 (contentWindow/contentDocument）  

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>菜鸟教程(runoob.com)</title>
<script>
function changeStyle(){
	var x=document.getElementById("myframe");
	var y=(x.contentWindow || x.contentDocument);
	if (y.document)y=y.document;
	y.body.style.backgroundColor="#0000ff";
}
</script>
</head>
<body>
	
<iframe id="myframe" src="demo_iframe.htm">
<p>你的浏览器不支持iframes。</p>
</iframe>
<br><br>
<input type="button" onclick="changeStyle()" value="修改背景颜色">

</body>
</html>
```  

### BroadcastChannel  

