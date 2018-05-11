近期开始接触学习extjs框架。该框架是基于JavaScript的。为了更好地理解学习extjs，必然需要先对JavaScript有一个较好的理解。
***
从最著名的web技术学习网站W3C开始。
如下图：
![捕获.PNG](http://upload-images.jianshu.io/upload_images/1234352-7b95796d4c53c727.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
花了几个小时将JavaScript的初级教程大致过了一遍。下面做些总结归纳。
***

>**JavaScript 是属于网络的脚本语言！**
**JavaScript 被数百万计的网页用来改进设计、验证表单、检测浏览器、创建cookies，以及更多的应用。**
**JavaScript 是因特网上最流行的脚本语言。**
**JavaScript 很容易使用！你一定会喜欢它的！**

这是W3C上介绍JavaScript的四句话，JavaScript的语言类型，作用，用途，地位，特点等。

# JavaScript的简介

>**JavaScript 是脚本语言**
JavaScript 是一种轻量级的编程语言。
JavaScript 是可插入 HTML 页面的编程代码。
JavaScript 插入 HTML 页面后，可由所有的现代浏览器执行。
JavaScript 很容易学习。

# JavaScript的使用
上面提到，JavaScript是可插入HTML的代码。那么如何插入使用JavaScript呢？
一般有两种方法：
+ 一种直接将JavaScript插入在<script> 与 </script> 标签之间
`<!DOCTYPE html>
<html>
<body>
.
.
<script>
document.write("<h1>This is a heading</h1>");
document.write("<p>This is a paragraph</p>");
</script>
.
.
</body>
</html>`
+ 把脚本保存到外部文件中。在 <script> 标签的 "src" 属性中设置该 .js 文件
`<!DOCTYPE html>
<html>
<body>
<script src="myScript.js"></script>
</body>
</html>`

## 关于JavaScript的使用还有几个要注意的小点：
* 可以在 HTML 文档中放入不限数量的脚本。
* 可位于 HTML 的 <body> 或 <head> 部分中，或者同时存在于两个部分中。
* ** 通常的做法是把函数放入 <head> 部分中，或者放在页面底部。这样就可以把它们安置到同一处位置，不会干扰页面的内容。**
* 外部脚本不能包含 <script> 标签。

# JavaScript输出
JavaScript操作输出主要有两种方式：
* 操作HTML元素
从 JavaScript 访问某个 HTML 元素，使用 document.getElementById(id) 方法
`<!DOCTYPE html><html><body><h1>My First Web Page</h1><p id="demo">My First Paragraph</p><script>document.getElementById("demo").innerHTML="My First JavaScript";
</script></body></html`

* 直接写到文档输出
使用 document.write() 仅仅向文档输出写内容。
如果在文档已完成加载后执行 document.write，整个 HTML 页面将被覆盖.

#javascript语句
与C，Java等语言类似。句尾分号，大小写敏感等。有其他语言基础的，此处可浏览一遍即可。
作为脚本语言，浏览器会在读取代码时，逐行地执行脚本代码。而对于传统编程来说，会在执行前对所有代码进行编译。

#JavaScript注释
JavaScript注释与Java语言相同。“//”用于单行注释；“/*”用于多行注释

#JavaScript变量
`var pi=3.14;
var name="Bill Gates";
var answer='Yes I am!';`
变量的声明简单，var关键字。变量是存储信息的容器。
在计算机程序中，经常会声明无值的变量。未使用值来声明的变量，其值实际上是 undefined。
变量可以使用短名称（比如 x 和 y），也可以使用描述性更好的名称（比如 age, sum, totalvolume）。
* 变量必须以字母开头
* 变量也能以 $ 和 _ 符号开头（不过我们不推荐这么做）
* 变量名称对大小写敏感（y 和 Y 是不同的变量）

#JavaScript数据类型
JavaScript拥有动态类型。这意味着相同的变量可用作不同的类型。
`var x                // x 为 undefined
var x = 6;           // x 为数字
var x = "Bill";      // x 为字符串`
* 字符串
* 数字
* 布尔
* 数组
数组的声明方式有三种：
`var cars=new Array();
cars[0]="Audi";
cars[1]="BMW";
cars[2]="Volvo";`

`var cars=new Array("Audi","BMW","Volvo");`

`var cars=["Audi","BMW","Volvo"];`
* 对象
对象由花括号分隔。在括号内部，对象的属性以名值对的形式 (name : value) 来定义。属性由逗号分隔：
`var person={
firstname : "Bill",
lastname  : "Gates",
id        :  5566
};`
* Null
可以通过将变量的值设置为 null 来清空变量。
* Undefined
Undefined 这个值表示变量不含有值

第一部分的js基础就到基础。
总结一下，我们先简单学习了JavaScript的特点用途；然后学会两种将JavaScript嵌入HTML的方法；JavaScript输出一般有两种方式，分别是通过id操作HTML元素输出，以及直接写到文档输出；JavaScript的语句与注释与Java语言和c语言基本相同；JavaScript变量的声明使用；JavaScript的数据类型主要有7种，数字，字符串，数组，布尔，对象，null，undefined.
下一部分我们将继续介绍js基础内容，分别是：
* JS 对象
* JS 函数
* JS 运算符
* JS 选择语句
* JS 循环语句
* JS 错误异常处理
* JS 验证
