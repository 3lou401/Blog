** 通过 HTML DOM，可访问 JavaScript HTML 文档的所有元素。**

HTML DOM 树

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-a19438485e2c415e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

DOM树很重要，特别是其中各节点之间的关系。因为有时候我们需要通过父节点寻找子节点等。

本文将会讲到以下内容：
通过可编程的对象模型，JavaScript 获得了足够的能力来创建动态的 HTML。
* JavaScript 能够改变页面中的所有 HTML 元素
* JavaScript 能够改变页面中的所有 HTML 属性
* JavaScript 能够改变页面中的所有 CSS 样式
* JavaScript 能够对页面中的所有事件做出反应

# JavaScript 能够改变页面中的所有 HTML 元素
首先，我们要知道如何查找HTML元素，通常有三种方法：
* id
* tag
* classs
就是分别通过id，tag，class的名字查找HTML元素。

## 通过ID查找HTML元素
```
<html>
	<body>
		<p id = "intro">hello world</p>
		<p>display <b>getElementById</b> method </p>
		
		<script>
			var x = document.getElementById("intro");
			document.write('<p>id = "intro" text in it is :' + x.innerHTML + '</p>');
		</script>
	</body>
</html>
``` 
## 通过tag查找HTML元素
```
<html>
<body>
	<p> hello world</p>
	<div id="main">
	<p>hello world </p>
	<p>display the <b>getElementBYTagName</b>  method</p>
	</div>
	
	<script>
	var x = document.getElementById("main");
	var y = x.getElementsByTagName("p");
	document.write('id is "main" first text is :' + y[0].innerHTML);
	</script>
</body>
</html>
```

## JavaScript 改变 HTML 元素的内容。

### 改变 HTML 输出流
```
<html>
<body>
    <script>
	document.write(Date());
	</script>
</body>
</html>
```

### 改变 HTML 内容
修改 HTML 内容的最简单的方法时使用 innerHTML 属性。
```
<html>
<body>
	<p id ="123">hello world</p>
    <script>
	document.getElementById("123").innerHTML="new html";
	</script>
</body>
</html>
```

### 改变 HTML 属性
```
<html>
<body>

<img id="image" src="C:\Users\chi\Pictures\The.Grandmaster[00_32_58][20160719-184823-0].BMP" />

<script>
document.getElementById("image").src="C:\Users\chi\Pictures\123.jpg";
</script>

<p>原始图片是郁金香（eg_tulip.jpg），但是已被修改为卢浦大桥（shanghai_lupu_bridge.jpg）。</p>

</body>
</html>
```

# 改变 HTML 样式
**HTML DOM 允许 JavaScript 改变 HTML 元素的样式。**
```
<html>
<body>

<p id="p1">Hello World!</p>
<p id="p2">Hello World!</p>

<script>
document.getElementById("p2").style.color="yellow";
document.getElementById("p2").style.fontFamily="Arial";
document.getElementById("p2").style.fontSize="larger";
</script>

<p>上面的段落已被一段脚本修改。</p>

</body>
</html>
```
# 
```
<html>
<body>

<p id= "a">hello world</p>

<button type="button" onclick = "document.getElementById('a').style.color='blue'">change color</button>

</body>
</html>
```

# ** JavaScript 有能力对 HTML 事件做出反应**

HTML 事件的例子：
* 当用户点击鼠标时
* 当网页已加载时
* 当图像已加载时
* 当鼠标移动到元素上时
* 当输入字段被改变时
* 当提交 HTML 表单时
* 当用户触发按键时

```
<html>
<body>

<h1 onclick="this.innerHTML='谢谢!'">请点击该文本</h1>

</body>
</html>
```

```
<html>
<head>
<script>
function changetext(id)
{
id.innerHTML="谢谢!";
}
</script>
</head>
<body>
<h1 onclick="changetext(this)">请点击该文本</h1>
</body>
</html>
```

```
<html>
<head>
</head>
<body>

<p>点击按钮就可以执行 <em>displayDate()</em> 函数。</p>

<button id="myBtn">点击这里</button>

<script>
document.getElementById("myBtn").onclick=function(){displayDate()};
function displayDate()
{
document.getElementById("demo").innerHTML=Date();
}
</script>

<p id="demo"></p>

</body>
</html> 
```

# **添加和删除节点（HTML 元素）**
```
<html>
<body>

<div id="div1">
<p id="p1">这是一个段落。</p>
<p id="p2">这是另一个段落。</p>
</div>

<script>
var para=document.createElement("p");
var node=document.createTextNode("这是新段落。");
para.appendChild(node);

var element=document.getElementById("div1");
element.appendChild(para);
</script>

</body>
</html>
```
这段代码创建新的 <p> 元素：
```
var para=document.createElement("p");
```
如需向 <p> 元素添加文本，您必须首先创建文本节点。这段代码创建了一个文本节点：
```
var node=document.createTextNode("这是新段落。");
```
然后您必须向 <p> 元素追加这个文本节点：
```
para.appendChild(node);
```
最后您必须向一个已有的元素追加这个新元素。
这段代码找到一个已有的元素：
```
var element=document.getElementById("div1");
```
这段代码向这个已有的元素追加新元素：
```
element.appendChild(para);
```

删除已有的 HTML 元素
如需删除 HTML 元素，您必须首先获得该元素的父元素：
```
var child=document.getElementById("p1");
child.parentNode.removeChild(child);
```
# 总结
在我们的 JavaScript 教程的 HTML DOM 部分，您已经学到了：
* 如何改变 HTML 元素的内容 (innerHTML)
* 如何改变 HTML 元素的样式 (CSS)
* 如何对 HTML DOM 事件作出反应
* 如何添加或删除 HTML 元素
