

<h1 align="center">前端技术总结</h1>

国内码云地址:https://gitee.com/xv700/Front_Summary

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

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>事件冒泡</title>
</head>
<body>
	
 <div id="div1">
    <div id="div2">点这里666</div>
 </div>

</body>
</html>
<script>
var div1 = document.getElementById("div1");
var div2 = document.getElementById("div2");
   div2.onclick = function(){alert(1);};
   div1.onclick = function(){alert(2);};//父亲
</script>
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

### 010.文字和元素的垂直水平居中  

https://www.cnblogs.com/huchong/p/7875127.html  
https://www.jianshu.com/p/507fdba00e6c  

> 注：因为行内元素自身宽高为0，所以只有在块级元素前提下  

#### 文字的水平居中  


> 方法一：在文字所在的元素上添加样式 text-align:center  
> 方法二：在文字所在的元素上添加属性 align="center" 

#### 文字的垂直居中

> 1.在文字所在的元素上添加样式,使其样式的 line-height === height即可
> 2.在文字所在的元素高度不确定，CSS 给要居中元素设置一个伪元素   

~~~html
    <div class='father'>
        <div class="son">这是要居中的文字</div>
    </div>
~~~

~~~css
        .son{
            height: 91px;/*这里随机写任何一个像素*/
            background: blue;
            color: #fff;
        }

        .son::before{
            display: inline-block;
            content: "";
            height: 100%;
            vertical-align: middle;
        }
~~~


#### 元素
>在CSS中如果对元素进行水平居中是非常简单的：  
>如果它是一个行内元素， 就对它的父元素应用 text-align:center;
>如果它是一个块级元素，就对它自身应用 margin:auto。
>然而如果要对一个元素进行垂直居中是有些复杂的。  
1.首先，定义一个需要垂直居中的div元素，它的宽度和高度均为300px，背景色为蓝色。代码如下： 
~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
    <style>
        .content {
            width: 300px;
            height: 300px;
            background: #607588;
        }
    </style>
</head>
<body>
<div class="content"></div>
</body>
</html>
~~~

2.我们需要使得这个蓝色的div居中，到底该怎么办呢？首先我们实现水平居中，上面已经提到过了，可以通过设置margin: 0 auto实现水平居中，代码如下：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
    <style>
        .content {
            width: 300px;
            height: 300px;
            background: #607588;
            margin: 0 auto;
        }
    </style>
</head>
<body>
    <div class="content"></div>
</body>
</html>
~~~

3.现在已经实现水平居中了！接下来实现垂直居中。不过，在这之前，我们先要设置div元素的祖先元素html和body的高度为100%（因为他们默认是为0的），并且清除默认样式，即把margin和padding设置为0（如果不清除默认样式的话，浏览器就会出现滚动条）
~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
    <style>
        html, body {
            width: 100%;
            height: 100%;
            margin: 0; 
            padding: 0;
        }
        .content {
            width: 300px;
            height: 300px;
            background: #607588;
            margin: 0 auto; /*水平居中*/
        }
    </style>
</head>
<body>
    <div class="content"></div>
</body>
</html>
~~~

4.接下来，需要做的事情就是要让div往下移动了。我们都知道top属性可以使得元素向下偏移的。但是，默认情况下，由于position的值为static（静止的、不可以移动的），元素在文档流里是从上往下、从左到右紧密的布局的，我们不可以直接通过top、left等属性改变它的偏移。所以，想要移动元素的位置，就要把position设置为不是static的其他值，如relative,absolute,fixed等。然后，就可以通过top、bottom、right、left等属性使它在文档中发生位置偏移（注意，relative是不会使元素脱离文档流的，absolute和fixed则会！也就是说，relative会占据着移动之前的位置，但是absolute和fixed就不会）。设置了position: relative后的代码如下:

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
    <style>
        html, body {
            width: 100%;
            height: 100%;
            margin: 0;
            padding: 0;
        }
        .content {
            width: 300px;
            height: 300px;
            background: #607588;
            margin: 0 auto; /*水平居中*/
            position: relative; /*设置position属性*/
        }
    </style>
</head>
<body>
    <div class="content"></div>
</body>
</html>
~~~

5.刷新一下页面，发现跟之前是没有任何变化的，因为设置了元素的position=relative，但是还没开始移动他的垂直偏移。好，下面我们就让它偏移吧！垂直偏移需要用到top属性，它的值可以是具体的像素，也可以是百分数。因为我们现在不知道父元素（即body）的具体高度，所以，是不可以通过具体像素来偏移的，而应该用百分数。既然是要让它居中嘛！就让它的值为50%不就行了吗？问题真的那么简单，试一下，就设置50%试一下：
~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
    <style>
        html,body {
            width: 100%;
            height: 100%;
            margin: 0;
            padding: 0;
        }
        .content {
            width: 300px;
            height: 300px;
            background:  #607588;
            margin: 0 auto; /*水平居中*/
            position: relative; /*脱离文档流*/
            top: 50%; /*偏移*/
        }
    </style>
</head>
<body>
    <div class="content"></div>
</body>
</html>
~~~

6.普通的盒子是左右margin 改为 auto就可， 但是对于绝对定位就无效了定位的盒子也可以水平或者垂直居中，有一个算法。
这时候，我们可以使用通过margin-top属性来设置，因为div的自身高度是300，所以，需要设置他的margin-top值为-150。为什么是要设置成负数的呢？因为正数是向下偏移，我们是希望div向上偏移，所以应该是负数，如下：这时候，我们可以使用通过margin-top属性来设置，因为div的自身高度是300，所以，需要设置他的margin-top值为-150。为什么是要设置成负数的呢？因为正数是向下偏移，我们是希望div向上偏移，所以应该是负数，如下：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
    <style>
        html,body {
            width: 100%;
            height: 100%;
            margin: 0;
            padding: 0;
        }
        .content {
            width: 300px;
            height: 300px;
            background:  #607588;
            margin: 0 auto; /*水平居中*/
            position: relative;
            top: 50%; /*偏移*/
            margin-top: -150px; 
        }
    </style>
</head>
<body>
    <div class="content"></div>
</body>
</html>
~~~
7. 确实已经居中了。好兴奋！有木有？！除了可以使用margin-top把div往上偏移之外，CSS3的transform属性也可以实现这个功能，通过设置div的transform: translateY(-50%)，意思是使得div向上平移（translate）自身高度的一半(50%)。如下：
~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
    <style>
        html,body {
            width: 100%;
            height: 100%;
            margin: 0;
            padding: 0;
        }
        .content {
            width: 300px;
            height: 300px;
            background: #607588;
            margin: 0 auto; /*水平居中*/
            position: relative;
            top: 50%; /*偏移*/
            transform: translateY(-50%);
        }
    </style>
</head>
<body>
    <div class="content"></div>
</body>
</html>
~~~

8.上面的两种方法，我们都是基于设置div的top值为50%之后，再进行调整垂偏移量来实现居中的。如果使用CSS3的弹性布局（flex）的话，问题就会变得容易多了。使用CSS3的弹性布局很简单，只要设置父元素（这里是指body）的display的值为flex即可。

   ~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>index</title>
    <style>
        html,body {
            width: 100%;
            height: 100%;
            margin: 0;
            padding: 0;
        }
    
        body {
            display: flex;
            align-items: center; /*定义body的元素垂直居中*/
            justify-content: center; /*定义body的里的元素水平居中*/
        }
        .content {
            width: 300px;
            height: 300px;
            background: #607588;        
        }
    </style>
</head>
<body>
    <div class="content"></div>
</body>
</html>
~~~

 除了上面3中方法之外，当然可能还存在许多的可以实现垂直居中的方法。比如可以将父容器设置为display:table ，然后将子元素也就是要垂直居中显示的元素设置为 display:table-cell 。但是，这是不值得推荐的，因为会破坏整体的布局。如果用table布局，那么为什么不直接使用table标签了？那不更加方便吗？












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
