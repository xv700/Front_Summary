
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
