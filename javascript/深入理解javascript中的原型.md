原型prototype是javascript中极其重要的概念之一，但也是比较容易引起混淆的地方。我们需要花费一些时间和精力好好理解原型的概念，这对于我们学习javascript是必须的。
***
# 原型的概念

真正理解什么是原型是学习原型理论的关键。很多人在此产生了混淆，没有真正理解，自然后续疑惑更多。

首先，我们明确原型是一个对象，其次，最重要的是，
** Every function has a prototype property and it contains an object **
这句话就是说，每个函数都有一个属性叫做原型，这个属性指向一个对象。
也就是说，原型是函数对象的属性，不是所有对象的属性，对象经过构造函数new出来，那么这个new出来的对象的构造函数有一个属性叫原型。明确这一点很重要。

** The prototype property is a property that is available to you as soon as you define the function. Its initial value is an "empty" object.
 **
每次你定义一个函数的时候，这个函数的原型属性也就被定义出来了，也就可以使用了，如果不对它进行显示赋值的话，那么它的初始值就是一个空的对象Object。
所以，综上我们知道我们讨论原型的时候，都是基于函数的，有了一个函数对象，就有了原型。切记这一点，讨论原型，不能脱离了函数，它是原型真正归属的地方，** 原型只是函数的一个属性 **！

```
function foo(a,b) {
    return a+b;
}
foo.prototype
foo.constructor
```
chrome控制台测试结果

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-96cea924e2c6dc06.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们可以看到函数foo的原型是空对象Object，所有函数的构造函数都是Function。
***
# 使用原型给对象添加方法和属性
不使用原型，使用构造函数给对象添加属性和方法的是通过this，像下面这样。
```
function Gadget(name, color) {
    this.name = name;
    this.color = color;
    this.whatAreYou = function() {
        return 'I am ' + this.color + '  ' + this.name; 
    }
} 
```
Gadget是一个构造函数，作为一个函数，它有一个属性，这个属性是原型，它指向一个对象，目前我们没有设置这个属性，所以它是一个空的对象。
** Adding methods and properties to the prototype property of the constructor
function is another way to add functionality to the objects this constructor produces **
当我们有了原型之后，我们可以给构造函数的原型对象添加属性和方法来。
像下面这样
```
Gadget.prototype.price = 100;
Gadget.prototype.rating = 3;
Gadget.prototype.getInfo = function() {
    return 'Rating: ' + this.rating +', price: ' + this.price;
}
```
给原型添加了属性和方法后，原型所指的对象也会更新

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-9363a11910f64fc3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
***
# 使用原型对象的属性和方法

我们使用原型的对象和方法不会在直接在构造函数上使用，而是通过构造函数new出一个对象，那么new出来的对象就会有构造函数原型里的属性和方法。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-471038db652d523e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里很容易造成误解，我们需要强调newtoy这个new出来的对象是没有原型的，原型只是函数对象的一个属性，newtoy是通过构造函数new出来的对象，所以他不是函数对象，也没有prototype属性，我们在chrome的控制台里自然也无法访问他的prototype属性。
但我们可以通过构造函数访问。
我们知道每个对象都有constructor属性，newtoy的constructor属性就指向Gadget，那么我们通过constructor可以访问到prototype。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-35443c4508a02382.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

到这里，我们对为什么要通过constructor.protptype访问属性应该清楚了。（笔者第一次接触原型就没看懂这个），切记，原型是函数对象的属性，只有函数对象才有原型就容易理解了。

## 原型的实时性
这里特别需要提出，原型是实时的，意思就是原型对象的属性和方法会实时更新。其实很好理解，javascript中对象是通过引用传递的，原型对象只有一份，不是new出一个对象就复制一份，所以我们对原型的操作和更新，会影响到所有的对象。这就是原型对象的实时性。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-c9ad1967b9af0b84.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 自身属性与原型属性
这里涉及到javascript是如何搜索属性和方法的，javascript会先在对象的自身属性里寻找，如果找到了就输出，如果在自身属性里没有找到，那么接着到构造函数的原型属性里去找，如果找到了就输出，如果没找到，就null。
所以，如果碰到了自身属性和原型属性里有同名属性，那么根据javascript寻找属性的过程，显然，如果我们直接访问的话，会得到自身属性里面的值。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-7e542ecd0c5f5255.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们加下来做一个小实验，寻找toString方法是谁的属性，一步步寻找

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-63dee589a984a269.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过实验我们可以发现，原来toString方法是object的原型对象的方法。

## isPrototypeOf()
Object的原型里还有这样一个方法isPrototypeOf(),这个方法可以返回一个特定的对象是不是另一个对象的原型，实际这里不准确，因为我们知道只有函数对象有原型属性，普通对象通过构造函数new出来，自动继承了构造的函数原型的属性方法。但这个方法是可以直接判断，而不需要先取出constructor对象再访问prototype。看下面的例子：
```
function Human(name) {
    this.name = name;
}

var monkey = {
    hair:true,
    feeds:'banana',
}

Human.prototype = monkey;

var chi = new Human('chi');
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-8c2f9944b429c645.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们知道chi这个对象是没有原型属性的，它有的是他的构造函数的原型属性monkey。但isPrototypeOf直接判断，实际上是省略了获取构造函数的过程，搞清楚这里面的区别。
object还有一个getPrototypeOf方法，基本用法和isPrototype一样，参考下面的代码：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-09c3025f68fe706c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 神秘的proto链接
我们之前访问对象的原型，都要先取得构造函数然后访问prototype
```
chi.constructor.prototype;
newtoy.constructor.prototype;
```
这样是不是特别别扭，所以各个浏览器一般都会给出一个__proto__属性，前后分别有双下划线，对象的这个属性可以直接访问到构造函数的原型。这就很方便了。所以proto与prototype是有很大区别的。区别就在此。proto是实例对象用来直接访问构造函数的属性，prototype是函数对象的原型属性。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-557dc27db5ec5c65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
chi.constructor.prototype == chi.__proto__
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-6ad7fe6d04547f3b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
显然现在已经很容易弄清楚了proto和prototype的区别了。

# 原型的陷阱
原型在使用的时候有一个陷阱：
** 在我们完全替换掉原型对象的时候，原型会失去实时性，同时原型的构造函数属性不可靠，不是理论上应该的值。**
这个陷进说的是什么呢？好像不太明白
举个例子我们就懂了
```
function Dog() {
    this.tail = true;
}

var benji = new Dog();
var rusty = new Dog();

Dog.prototype.say = function () {
    return 'Woof!';
};
```
我们进行测试：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-d9ebb0075e1d14ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

直到这里一切都是正常的
接下来我们将原型对象整个替换掉
```
Dog.prototype = {
    paws: 4,
    hair: true
};
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-e538ed4fd7f2ab70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过测试我们发现，我们没法访问刚刚更新的原型对象，却能访问之前的原型对象，这说明没有实现实时性。

我们继续测试

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-6c0086739f3a57a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们发现这时新建的对象可以访问更新后的原型，但是构造方法又不对了，本来constructor属性应该指向dog，结果却指向了Object。这就是javascript中的原型陷阱。

我们很容易解决这个问题，只要在更新原型对象后面，重新指定构造函数即可。
```
Dog.prototype.constructor = Dog;
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-5be6521e397938b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这样所有就按正常的运行了

** 所以我们切记在替换掉原型对象之后，切记重新设置constructor.prototype **

# 小结
我们大概介绍了原型中容易混淆的问题，主要有以下几方面:
* 所有函数都有一个属性prototype，这就是我们指的原型，他的初始值是一个空的对象
* 你可以原型对象添加属性和方法，甚至直接用另一个对象替换他
* 当你用构造函数new出一个对象之后，这个对象可以访问构造函数的原型对象的属性和方法
* 对象的自身属性搜索的优先级比原型的属性要高
* proto属性的神秘连接及其同prototype的区别
* prototype使用中的陷阱
