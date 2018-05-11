javascript中的继承机制是建立在原型的基础上的，所以必须先对原型有深刻的理解，笔者在之前已经写过关于js原型的文章。

我们都知道，每个函数function都会有一个属性，这个属性就是原型prototype，它是一个引用，它指向一个对象object。当我们使用new操作符调用构造函数，创建一个新对象的时候，这个新对象就会拥有一个指向它的构造函数的原型对象的神秘链接，在浏览器中一般是__proto__,通常我们也称为它的原型对象。这就可以理解为，new出来的对象继承拥有了了它的构造函数的原型对象，这就隐约有一点继承的概念了。


#原型链继承机制

原型链的概念就是多个这样的对象通过proto相互关系起来

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-b947b7dcbae087af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图可以看到，对象A有一系列的属性，但其中的部分属性是藏在原型之中的，而这个原型又指向了对象B，所以对象A间接也拥有了对象B的部分属性，同理，对象B的部分属性是藏在proto中，而proto又指向了对象C.
这样一环一环的往上递加，显然最后会指向Object.prototype,它是所有对象的父对象。

下面我们就通过一个实例来说明，原型链继承机制的实现与原理
我们有三个构造函数，Shape，2DShape, Triangle。
很显然，它们之间的继承关系，应该是Shape，2DShape,Triangle.
下面分别定义三个构造函数：
```
function Shape(){
    this.name = 'Shape';
    this.toString = function () {
        return this.name;
    };
}

function TwoDShape(){
    this.name = '2D shape';
}

function Triangle(side, height){
    this.name = 'Triangle';
    this.side = side;
    this.height = height;
    this.getArea = function () {
        return this.side * this.height / 2;
    };
}
```
接下来实现它们之间的原型链继承关系：
```
TwoDShape.prototype = new Shape();
Triangle.prototype = new TwoDShape();
```
当我们覆盖原型对象的时候，不要忘了原型的陷阱（读者若不清楚，可以参考笔者介绍原型的博文）
```
TwoDShape.prototype.constructor = TwoDShape;
Triangle.prototype.constructor = Triangle;
```
这样我们就实现了原型链的继承关系。
每一个new出来的TwoDShape对象的proto属性都指向一个Shape对象，所以它可以拥有Shape对象的属性和方法，同理，每一个new出来的Triangle对象的proto属性都指向一个TwoDShape对象，而TwoDShape又继承至Shape,所以这样就形成了一个原型链。

下面我们对以上原型链关系进行测试

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-15d2f91fd2140925.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图我们可以看到清晰的一个原型链关系。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-e93e5b33360b6902.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们看到实现原型链继承关系之后，my作为子对象，同时都是父对象的一种，这是符合java等语言中继承的概念的。

# 将共有的属性放进原型中
如上个例子中的，name属性是三中对象共有的，上个例子每个单独的对象都会new出一个name属性，这样就造成了对空间的浪费。
所以我们将name属性移到原型中去
```
function Shape() {}
Shape.prototype.name = 'Shape';
```
就不用每次都new出一个name属性，而是共用原型属性里面的name属性。

下面我们就用这种思想完善之前的例子
```
// constructor
function Shape() {}
// augment prototype
Shape.prototype.name = 'Shape';
Shape.prototype.toString = function () {
    return this.name;
};
// another constructor
function TwoDShape() {}
// take care of inheritance
TwoDShape.prototype = new Shape();
TwoDShape.prototype.constructor = TwoDShape;
// augment prototype
TwoDShape.prototype.name = '2D shape';
function Triangle(side, height) {
    this.side = side;
    this.height = height;
}
// take care of inheritance
Triangle.prototype = new TwoDShape();
Triangle.prototype.constructor = Triangle;
// augment prototype
Triangle.prototype.name = 'Triangle';
Triangle.prototype.getArea = function () {
    return this.side * this.height / 2;
};
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-44b8455b518db9f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

将部分共享属性移到原型里去之后，原型链的继承关系如图，对比之前简洁了一些，因为没有多余的重复属性

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-24ac6afb8f3f4059.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里调用toString方法得到相同的结果，但与之前略有不同，这里要多搜索一次，因为toString方法是属于Shape的原型属性里的。于是效率就有所降低。

同时，这种模式还有一个缺陷看下面的例子

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-a94ffb2a8b75e19b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们访问Shape对象的name属性结果显示的确实Triangle，这是为什么呢？
其实很简单，因为我们所有的原型都指向同一个对象，而每个对象的原型属性只是取得了指向唯一的原型对象的指针，所以只要改变了它，所有的都会改变了
因为这句：
Triangle.prototype.name = 'Triangle';
所以会导致Shape，TwoDShape的name属性都都为Triangle。

所以在某些时候，就没法使用这种继承模式，这种将共享的属性移到原型中的模式，会产生子对象覆盖掉父对象共有属性的缺陷。
