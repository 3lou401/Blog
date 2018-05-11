# JavaScript对象
> **JavaScript 中的所有事物都是对象：字符串、数字、数组、日期，等等。**
**在 JavaScript 中，对象是拥有属性和方法的数据。**

JavaScript中的对象与java中和其他面向对象语言是基本一致的。如何访问对象，如何访问对象方法，如何新建对象等。都是相当一致的。

# JavaScript函数
>**函数是由事件驱动的或者当它被调用时执行的可重复使用的代码块。**

JavaScript函数的语法
`function myFunction(){
  函数内容
}`
与java语言中的函数一样，参数是可选的，返回值是可选的。当只需退出函数时，只需返回return；即可退出。函数中声明的变量都是局部变量，函数外声明的变量都是全局变量。当一个变量在未声明前就进行赋值时，那么该变量是全局变量。
`carname="Volvo";`
此处声明了一个全局变量，因为没声明就直接赋值。
可以直接理解为**变量前没有var就说明是全局变量**

# JavaScript运算符

JavaScript运算符基本与java一致，赋值，算术运算，等。基本可以直接通用
需要注意的一点是：
**如果把数字与字符串相加，结果将成为字符串。**

# JavaScript选择语句

JavaScript选择语句基本与Java是一致的。只需简单浏览一下就行。
在 JavaScript 中，我们可使用以下条件语句：
* if 语句 - 只有当指定条件为 true 时，使用该语句来执行代码
* if...else 语句- 当条件为 true 时执行代码，当条件为 false 时执行其他代码
* if...else if....else 语句 - 使用该语句来选择多个代码块之一来执行
* switch 语句 - 使用该语句来选择多个代码块之一来执行

# JavaScript循环语句

JavaScript 支持不同类型的循环：
* for* - 循环代码块一定的次数
* for/in* - 循环遍历对象的属性
* while* - 当指定的条件为 true 时循环指定的代码块
* do/while* - 同样当指定的条件为 true 时循环指定的代码块

与java中基本是完全一致的。

需要注意的是break和continue两个关键字，其作用基本与java也一致。
**break 语句用于跳出循环。**
**continue 用于跳过循环中的一个迭代**

JavaScript标签
通过标签，可以让break跳出任意指定的代码块
`cars=["BMW","Volvo","Saab","Ford"];
list:
{
document.write(cars[0] + "<br>");
document.write(cars[1] + "<br>");
document.write(cars[2] + "<br>");
break list;
document.write(cars[3] + "<br>");
document.write(cars[4] + "<br>");
document.write(cars[5] + "<br>");
}`

# JavaScript的异常处理

* try 语句测试代码块的错误。
* catch 语句处理错误。
* throw 语句创建自定义错误。

`try
  {
  //在这里运行代码
  }
catch(err)
  {
  //在这里处理错误
  `

throw 语句允许我们创建自定义错误。
正确的技术术语是：创建或抛出异常（exception）。
如果把 throw 与 try 和 catch 一起使用，那么您能够控制程序流，并生成自定义的错误消息。

``<script>
function myFunction()
{
try
  {
  var x=document.getElementById("demo").value;
  if(x=="")    throw "empty";
  if(isNaN(x)) throw "not a number";
  if(x>10)     throw "too high";
  if(x<5)      throw "too low";
  }
catch(err)
  {
  var y=document.getElementById("mess");
  y.innerHTML="Error: " + err + ".";
  }
}
</script>
`

以上就是JavaScript的基础部分。
