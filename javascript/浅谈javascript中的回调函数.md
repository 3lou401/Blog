要理解javascript中的回调函数，首先我们就要对javascript中的函数有一定的理解，所以我们先从javascript中函数谈起，讲讲它与其他语言中的函数有什么不同。
***
# javascript中的函数
在javascript中，函数也是一种data，一种数据，只不过这种数据比较特殊，它里面存的是代码，而且这种data可以被调用执行。
自然，因为函数也是数据，所以就可以赋值给变量。
所以我们在javascript中经常看到这样的程序：
```
var f = function() {
      return 1;
}
```
我们将一个函数表达式赋值给了变量f，所以我们直接通过变量f来调用这个函数，只需要在f后面加上（）就行了。
** javascript中函数的调用特征就是后面跟一对括号，里面可以有参数 **

![js_function.PNG](http://upload-images.jianshu.io/upload_images/1234352-1d1329c1d1719ee0.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
如图中的执行代码，要调用某个函数，只需要在它的名字后面加一对括号即可，而且我们可以像变量一样使用函数，也就是说，我们可以将它拷贝给不同的变量。例如，我们将f拷贝给f2，那么f2也是一个函数变量，并且可以调用执行。

## 函数小结
现在我们javascript中的函数有以下特点：
* 函数也是一种data，一种数据
* 函数这种特殊的数据所包含的是代码
* 它们可以被调用执行

# 匿名函数
正如前文所提的，
```
var f = function() {
      return 1;
}
```
这样的函数我们称之为匿名函数。可以和非匿名函数对比一下
```
function f() {
     return 1;
}
```
匿名函数有种特殊的用法就是，跟其他数据data一样作为参数传递给其他函数，因为我们已经知道函数在javascript中和其他数据data是一样的额，所以将函数作为参数就不难理解了。这样使用函数，就是** 回调函数 **。

# 回调函数
既然函数与任何可以被赋值给变量的数据是相同的，那么它们当然可以像其他数据那样来定义，删除，拷贝，以及当成参数传递给其他函数。
例如下面一个简单的例子
```
function add(a, b)
{
	return a() + b();
}

function one() {
	return 1;
}

function two() {
	return 2;
}

add(one,two);
```
这就是一个简单的回调函数的实例。add中的参数是两个函数，我们将one，two两个函数传进去，在add中执行one和two两个函数，这就是回调函数。更简洁的，我们还可以直接用匿名函数来简化上述代码
```
function add(a, b)
	return a() + b();
}

add(
	function () {
		return 1;
	},
	function () {
		return 2;
	}
)
```
上述代码在控制台中运行的结果如下：

![js.PNG](http://upload-images.jianshu.io/upload_images/1234352-f14af771663a8912.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 回调函数的使用
知道了什么是回调函数，我们来看一下回调函数的使用。
回调函数有什么优势呢？也就是为什么要使用回调函数
* 它可以让我们在不做命名的情况下传递函数（这意味可以减少变量名的使用）
* 我们可以讲一个函数调用操作委托给另一个函数（这意味着可以节省一些代码编写工作）
* 有助于提升性能

# 回调函数实例
下面我们通过一个例子来看看回调函数使用和他的优势。

我们定义两个函数，一个是multiplyByTwo（）；这个函数一个循环将它接受的三个参数分别乘2，并以数组的形式返回结果；第二个函数addOne（）只接受一个值，然后将它加1并返回。
```
function multiplyByTwo(a, b, c, callback) {
  var ar = [
  ];
  for (var i = 0; i < 3; i++) {
    ar[i] = arguments[i] * 2;
  }
  return ar;
}
```
```
function addOne(a) {
  return a + 1;
}

```
现在假设我们有三个元素（1，2，3）我们现在摇奖它们先乘2，再分别加一，不考虑回调函数的话，那么我们可以这么做
```
var myarr = [
];
myarr = multiplyByTwo(1, 2, 3);
//然后再遍历数组，给每个元素加一
for (var i = 0; i < 3; i++) {
  myarr[i] = addOne(myarr[i]);
}
```
这段代码，显然可以工作，但还有一定的改善空间，特别是这里使用了两个循环，如果数据量一大，开销一定很大。因此，我们可以使用回调函数，将它们合二为一，这就要对multiplyByTwo函数做一些小改动，使其接受一个回调函数，并在每次迭代操作中调用它。
```
function multiplyByTwo(a, b, c, callback) {
  var ar = [
  ];
  for (var i = 0; i < 3; i++) {
    ar[i] = callback(arguments[i] * 2);
  }
  return ar;
}
```
这样，我们只需要调用一次函数就可以了。
```
var myarr = mutiplyByTwo(1, 2, 3, addOne);
myarr
```

# 总结
我们从javascript中的函数讲起，讲了函数在javascript中和数据一样，可以赋值，删除，拷贝，自然也可以作为函数的参数，这样就引出了回调函数的概念，我们先通过一个简单的例子，介绍了回调函数，然后通过一个例子说明了回调函数使用的优势，可以简化代码，提高效率，并且是代码易于修改维护！
