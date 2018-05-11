# 多继承
我们知道多继承是面向对象的语言中比较纠结的一个问题，有好处也存在缺陷。这方面我们不多讨论。就javascript而言，要实现多继承是比较简单的，因为javascript中函数可以接受任意个数目的参数，这就使问题变得简单了。

我们创建一个multi函数，接受任意数目的对象，实现方法就是在复制属性的循环外面包裹一层循环接收不同参数对象的函数。

```
function multi() {
var n = {}, stuff, j = 0, len = arguments.length;
for (j = 0; j <len; j++) {
stuff = arguments[j];
for (var i in stuff) {
if (stuff.hasOwnProperty(i)) {
n[i] = stuff[i];
}
}
}
return n;
}
```
下面我们测试这个函数，创建三个对象，一个shape,一个twodee,一个匿名对象，传递一些属性给Triangle。

```
var shape = {
name: 'Shape',
toString: function () {
return this.name;
}
};
var twoDee = {
name: '2D shape',
dimensions: 2
};
var triangle = multi(shape, twoDee, {
name: 'Triangle',
getArea: function () {
return this.side * this.height / 2;
},
side: 5,
height: 10
});
```
下面我们就在控制台测试一下，多继承函数是否起作用

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-fca8dbe0495e7f65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里的multi函数使用的是浅复制，当然也可以修改为深复制的版本。
同时要注意一个问题，** 如果传入的对象由同名属性，那么属性最后的值会和传入的最后一个对象相同 **

# 寄生式继承
寄生顾名思义，就是寄生在一个已有的对象，我们在创建对象的时候，寄生在已有的对象上，直接吸收其他对象已有的功能，扩展对象，并当作一个新对象返回。

下面创建一个对象
```
var twoD = {
name: '2D shape',
dimensions: 2
};
```
实现寄生式继承
```
function triangle(s, h) {
var that = object(twoD);
that.name ='Triangle';
that.getArea = function () {
return this.side * this.height / 2;
};
that.side = s;
that.height = h;
return that;
}
```
寄生式继承实现的步骤
* 首先将已有的对象作为新对象的原型，继承它的属性，我们调用了之前的objec函数
* 然后再给他添加其他属性与方法

# 借用构造函数
这种继承模式中，就是子对象的构造函数中调用父对象的构造函数，通过apply和call函数。
call和apply构造函数是什么呢？实际就是他们可以让一个一个对象去借用另一个对象的方法，并为己所用，这是一种非常简单的代码重用的方法，实质上就是去改变函数的this值。
下面看实例
```
function Shape(id) {
this.id = id;
}
Shape.prototype.name = 'Shape';
Shape.prototype.toString = function () {
return this.name;
};
```
上面这段代码首先定义了一个父类的构造函数
```
function Triangle() {
Shape.apply(this, arguments);
}
Triangle.prototype.name = 'Triangle';
```
我们调用父类的构造函数的apply方法，将triangle对象传入进去，并传入部分参数。
这样的话，triangle对象会继承Shape构造函数中的属性，但不会继承原型中的属性。
如果你想继承原型的属性其实很简单
```
function Triangle() {
Shape.apply(this, arguments);
}
Triangle.prototype = new Shape();
Triangle.prototype.name = 'Triangle';
```
但这样有一个缺点，我们通过调用父类的构造函数，继承了父类的自身属性，通过原型继承了父类的自身属性和原型，这样自身属性实际上就被覆盖了两次。这是不高效的。
下面这个模式就可以更好的解决这个问题

# 借用构造函数并且复制原型
其实解决上面那个自身属性被继承两次的问题也很简单，我们首先调用apply函数继承父类的自身属性，然后在复制原型属性就可以了，这个方法我们之前已经讨论过就是extend2方法，可以复制原型属性
```
function Shape(id) {
this.id = id;
}
Shape.prototype.name = 'Shape';
Shape.prototype.toString = function () {
return this.name;
};
function Triangle() {
Shape.apply(this, arguments);
}
extend2(Triangle, Shape);
Triangle.prototype.name = 'Triangle';
```
下面我们来测试一下

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-e5fea2089d1c9e83.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-0b6d280c5e5af43d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 以上
终于我们将该讲的继承方式都讲完了。
后续还有一片文章会将这些继承模式归类总结一下。
