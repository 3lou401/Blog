闭包可以说是javascript中最令人迷惑的概念了。需要我们在实践中去慢慢理解，在实际编码中，由于闭包的效率和会产生大量无法销毁的内存，所以原则是尽量少使用闭包，但是作为javascript中的一个特别的概念，理解闭包是很重要的。闭包像是一种突破javascript中作用域限制的利剑。下面我们就从javascript中的作用域链谈起，简单讲讲闭包的概念和理解。

# 作用域链
javascript中没有大括号级的作用域，但是javascript中拥有函数作用域。在某函数内部定义的变量，在函数外部是不可见的。
如下面这段代码：
```
var a=1;
function f() {
  var b = 1;
  return a;
}
f();
b
```
这段代码在函数外部访问了函数f中定义的变量的b，但是是不行的，会报如下错误：
```
ReferenceError: b is not defined
data:,/* 通过 Firebug 命令行执行的表达式: */%0Avar%20a%3D1%3B%0Afunction%20f()%20%7B%0A%20%20var%20b%20%3D%201%3B%0A%20%20return%20a%3B%0A%7D%0Af()%3B%0Ab
Line 8
```
下面这个例子，我们在outer中定义了另一个函数inner，那么在inner中可以访问的变量即来自他自己的作用域，也来自他的父亲作用域，也就函数outer，所以这样就形成了一条作用域链。
```
var global = 1;
function outer() {
  var outer_local = 2;
  function inner() {
    var inner_local = 3;
    return inner_local + outer_local + global;
  }
  return inner();
}
outer();
```
当我们访问执行outer函数的时候，返回的结果是6.
所以在这个例子中，可以通过inner访问到outer和global的变量，这就是作用域链。

# 引出闭包
我们看下面这段代码
```
var a = 'global variable';
var F = function () {
  var b = 'local variable';
  var N = function () {
    var c = 'inner local';
  };
};
```
我们定义了一个全局变量a，一个函数F，函数内部定义了变量b，以及一个私有函数N，私有函数内部定义了变量c。
我们通过图展示这些变量作用域之间的关系。
全局变量：


![closure1.png](http://upload-images.jianshu.io/upload_images/1234352-01d82397cc1d0fc3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

全部的变量：


![closure.png](http://upload-images.jianshu.io/upload_images/1234352-663999296d26045c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个图不太标准。但我们可以理解一下：
如果我们是a，那么我们就在全局作用域中，而如果是b，我们就位于函数f的作用域内，在这个作用域里，我们可以访问函数f中的变量也可以访问函数f外的全局作用域的变量，这就形成了一个作用域链，同样对于c点，是位于函数n中的变量，在c点的作用域我们可以访问图中所有的变量。根据图中，我们大致可以把整个空间分为 全局空间 ，F空间，和N空间。显然，a与b是不连通的，也就是说我们在a点是无法访问到b的，同理，显然a也是无法访问c点的。
这时候，通过闭包的话，我们可以把N与b连通起来。将N的空间扩展到F以外，并止步于全局空间。这就是** 闭包 **！


![closure2.png](http://upload-images.jianshu.io/upload_images/1234352-88bcd575a46c26f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用闭包后的结果就跟上图一样。
如果变成上图的这样的话，这样N就位于全局空间和a是在同一空间的，但是由于函数N还记得被定义时，所处的环境，因此他依然可以访问F空间并使用b，这有很有趣，因为这个时候，N与a处于同一空间，N可以访问b，而a却不能，这就是闭包的神奇作用。这就是让N突破了作用域链，跳到了全局空间，那么我们想象这是如何做到的，其实很简单，只要通过F将N返回出来，到全局空间就可以了。

# 利用闭包突破作用域链的三种方法

下面我们具体讲解三种使用闭包突破作用域链的方法。

## 闭包1
首先，我们对上面那个函数做一些修改。
```
var a = 'global variable';
var F = function () {
  var b = 'local variable';
  var N = function () {
    var c = 'inner local';
    return b;
  };
  return N;
};

var inner = F();
inner();
```
我们再函数F中返回了函数N，并在函数N中返回b。
函数N有自己的私有空间，同时也可以访问f空间和全局空间，所以b对他来说是可见的。因为F是可以在全局空间中被调用的。所以我们可以将它的返回值富裕另外一个全局变量inner，这样就生成了一个可以访问F私有空间的新的全局函数。

## 闭包2
第二种方法与第一种实现的方式不同，整体的思想还是一样的。
我们在全局声明一个变量inner，然后再在F中给他赋值，这样，相当于将N保存到全局作用域了。
```
var inner; // placeholder
var F = function () {
  var b = 'local variable';
  var N = function () {
    return b;
  };
  inner = N;
};
F();
inner();
```
## 闭包3
这次我们与前两个不同，使用函数的参数，该参数与函数的局部变量没什么不同，但他们是隐式创建的，我们让函数里的子函数返回函数的参数。这样成为全局作用域里的子函数，就可以访问到函数内部的参数了。
```
function F(param) {
  var N = function () {
    return param;
  };
  param++;
  return N;
}
```
我们像如下这样调用函数
```
> var inner = F(123);
> inner();
124
```
函数绑定的是作用域本身，而不是在函数定义时该作用域的变量或变量当前的返回值。

# 小结
看完上面三种创建闭包的方式，我们是不是对闭包有了一定的模糊的认识或者印象。

**　事实上每个函数都可以认为是闭包，因为每个函数都在其所在的作用域内维护了某种私有关系的联系。但大部分时候，该作用域在函数执行完之后就自行销毁了，除非像我们上面三种情况一样使用了闭包，返回了一个内部函数，导致作用域被保持。
现在我们可以说，如果一个函数会在其父级作用域返回之后留住对父级作用域的连接的话，相关的闭包就会被创建起来。　**
