为了解决前文提到的**将共有的属性放进原型中**这种模式产生的子对象覆盖掉父对象同名属性的问题，就出现了另一种模式，我们称作为**临时构造函数**模式

# 临时构造函数模式
我们具体通过代码来分析
```
function Shape() {}
// augment prototype
Shape.prototype.name = 'Shape';
Shape.prototype.toString = function () {
return this.name;
};
function TwoDShape() {}
// take care of inheritance
var F = function () {};
F.prototype = Shape.prototype;
TwoDShape.prototype = new F();
TwoDShape.prototype.constructor = TwoDShape;
// augment prototype
TwoDShape.prototype.name = '2D shape';
function Triangle(side, height) {
this.side = side;
this.height = height;
}
// take care of inheritance
var F = function () {};
F.prototype = TwoDShape.prototype;
Triangle.prototype = new F();
Triangle.prototype.constructor = Triangle;
// augment prototype
Triangle.prototype.name = 'Triangle';
Triangle.prototype.getArea = function () {
return this.side * this.height / 2;
};
```
从代码里可以看到，我们定义了一个临时的构造函数F,然后将Shape构造函数的原型对象赋给F的原型。为了实现继承关系，TwoDShape的原型就指向一个new出来的F对象。这样就打破了上一种模式中的原型都指向同一个对象的问题，同时，TwoDShape的原型对象的proto指向的是Shape的原型，然后我们再给这个new出来的F添加一些属性，也就是给TwoDShape的原型添加属性。
下面我们测试一下这种模式的结果

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-be73a2025aaaa894.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到父对象的属性没有被子对象所覆盖

与此同时，我们可以发现，这个模式，只有添加到原型里的属性和方法才会被继承，而自身的属性和方法是不会被继承的。所以这个模式也有缺陷，就是自身属性由于无法继承而导致无法被重用。

# Uber – 从子对象调用父对象的接口
传统的面向对象的编程语言都会有子对象访问父对象的方法，比如java中子对象要调用父对象的方法，只要直接调用就可以得到结果了。但在javascript中没有这样的语法，需要我们实现。
下面我们就来实现这个方法
先上示例代码
```
function Shape() {}
// augment prototype
    Shape.prototype.name = 'Shape';
    Shape.prototype.toString = function()
{
    var constr = this.constructor;
    return constr.uber
    ? constr.uber.toString() + ', ' + this.name
    :this.name; 
};
function TwoDShape() {}
// take care of inheritance
var F = function () {};
F.prototype = Shape.prototype;
TwoDShape.prototype = new F();
TwoDShape.prototype.constructor = TwoDShape;
TwoDShape.uber = Shape.prototype;
// augment prototype
TwoDShape.prototype.name = '2D shape';
function Triangle(side, height) {
this.side = side;
this.height = height;
}
// take care of inheritance
var F = function () {};
F.prototype = TwoDShape.prototype;
Triangle.prototype = new F();
Triangle.prototype.constructor = Triangle;
Triangle.uber = TwoDShape.prototype;
// augment prototype
Triangle.prototype.name = 'Triangle';
Triangle.prototype.getArea = function () {
return this.side * this.height / 2;
};
```
从代码可以发现，我们在维护继承关系的同时，给每个构造函数天价了一个uber属性，同时使他指向父对象的原型，然后更改了Shape的toString函数，更新后的函数，会先检查this.constructor是否有uber属性，当对象调用toString时，this.constructor就是构造函数，找到了uber属性之后，就调用uber指向的对象的toString方法，所以，实际就是，先看父对象的原型对象是否有同String，有就先调用它。
所以，当我们用下面代码测试时就会得到这样的结果

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-63bf5d08007f0e51.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 将继承部分封装成函数
下面，，我们就将所介绍的继承模式放到一个封装的extend函数里，实现复用
```
function extend(Child, Parent) {
var F = function () {};
F.prototype = Parent.prototype;
Child.prototype = new F();
Child.prototype.constructor = Child;
Child.uber = Parent.prototype;
}
```
然后我们测试一下这个函数
```
// inheritance helper
function extend(Child, Parent) {
var F = function () {};
F.prototype = Parent.prototype;
Child.prototype = new F();
Child.prototype.constructor = Child;
Child.uber = Parent.prototype;
}
// define -> augment
function Shape() {}
Shape.prototype.name = 'Shape';
Shape.prototype.toString = function () {
return this.constructor.uber
? this.constructor.uber.toString() + ', ' + this.name
: this.name;
};
// define -> inherit -> augment
function TwoDShape() {}
extend(TwoDShape, Shape);
TwoDShape.prototype.name = '2D shape';
// define
function Triangle(side, height) {
this.side = side;
this.height = height;
}
// inherit
extend(Triangle, TwoDShape);
// augment
Triangle.prototype.name = 'Triangle';
Triangle.prototype.getArea = function () {
return this.side * this.height / 2;
};
```
测试

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-cf0f74c461d4d5d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
