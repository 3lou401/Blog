我们开始换一种思路实现继承，可不可以直接将父对象的属性直接复制给子对象，这样子对象不久也拥有了父对象的属性，相当于继承。

#属性复制
下面我们就实现这样一种继承方式，将父亲的原型对象的属性全部复制到子对象的原型属性中
```
function extend2(Child, Parent) {
var p = Parent.prototype;
var c = Child.prototype;
for (var i in p) {
c[i] = p[i];
}
c.uber = p;
}
```
显然这样实现的方法很简单，只需要简单的复制原型的属性即可，但显然是不高效的，因为很多属性被重复的存储了。
同时我们还要切记一点，我们实现的是浅复制，也就是直接复制的值，这样的话：
** 只有对于那些由原始数据类型构成的属性，才会被重复，那些对象的引用，只会复制引用，指向的还是同一个对象 **
下面我们使用上面实现的extend2继承函数。
```
var Shape = function () {};
var TwoDShape = function () {};
Shape.prototype.name = 'Shape';
Shape.prototype.toString = function () {
return this.uber
? this.uber.toString() + ', ' + this.name
: this.name;
};
extend2(TwoDShape, Shape);
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-75ecbadaa6a40b0d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

由于属性都是直接复制的，所以twoD会有自己的name属性，但由于toString不是原始类型，存储的是引用，所以它们指向的是同一个对象。

与之前extend函数比较，这种直接复制属性的方法，可能比较低效，但实际上，由于复制的只是原始数据类型的属性，真正的object类型的属性并没有被复制，，而且在另一方面，相对于extend找寻属性时，要绕着原型链搜索一番，所以实际应用中可能效率并不低。

# 对象之间的继承
extend2中，我们都是以构造器创建对象为基础的，我们将原型对象中的属性一一拷贝给子原型对象，而这两个原型本质上也是对象。现在我们考虑不通过原型，直接在对象之间拷贝属性。
```
function extendCopy(p) {
var c = {};
for (vari in p) {
c[i] = p[i];
}
c.uber = p;
return c;
}
```
首先我们创建一个空对象，然后逐步将属性添加到其中。
下面我们应用这个函数试试，
```
var shape = {
name: 'Shape',
toString: function () {
return this.name;
}
};

var twoDee = extendCopy(shape);
twoDee.name = '2D shape';
twoDee.toString = function () {
return this.uber.toString() + ', ' + this.name;
};

var triangle = extendCopy(twoDee);
triangle.name = 'Triangle';
triangle.getArea = function () {
return this.side * this.height / 2;
};
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-a20ab6ec57912c49.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们可以看到这种直接复制对象，不通过原型和构造器，的继承模式比较简单，直接复制，子对象有需要添加的属性，直接更改或添加就可以了。但显然有不足，继承关系不明显，而且triangle的初始化，不能通过构造器，这样封装性不好。

# 深复制
前面介绍的复制的方法都是浅复制，也就是只对于原始数据类型的属性会复制出副本，而对于引用类型的对象则只是复制出引用。这样造成的问题就是，当操作新对象时，可能会无意识的覆盖改变旧对象。·

深复制的实现其实并不复杂，也是逐一的复制属性，唯一的不同就是，当遇到引用类型的属性时，再次调用复制函数复制，他就会将引用对像的属性也复制过来。

```
function deepCopy(p, c) {
c = c || {};
for (var i in p) {
if (p.hasOwnProperty(i)) {
if (typeof p[i] === 'object') {
c[i] = Array.isArray(p[i]) ? [] : {};
deepCopy(p[i], c[i]);
} else {
c[i] = p[i];
}
}
}
return c;
}
```

下面我们测试一下这个深复制函数
```
var parent = {
numbers: [1, 2, 3],
letters: ['a', 'b', 'c'],
obj: {
prop: 1
},
bool: true
};
```

我们同时用深复制和浅复制实现一下，并进行对比：
```
var mydeep = deepCopy(parent);
var myshallow = extendCopy(parent);
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-9bfa916e113bda6c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 原型继承
下面我们介绍一种在ES5中被采纳的继承方式，称作原型继承，Object.create（object）可以调用他。
他的实现方式就是，接受一个对象，返回一个以该对象为原型的新对象。

```
function object(o) {
function F() {}
F.prototype = o;
return new F();
}
```
如果要设置访问父对象的uber属性
```
function object(o) {
var n;
function F() {}
F.prototype = o;
n = new F();
n.uber = o;
return n;
}
```
使用这个函数和extendCopy函数一样
```
var triangle = object(twoDee);
triangle.name = 'Triangle';
triangle.getArea = function () {
return this.side * this.height / 2;
};
```
这种继承方式被叫做原型继承，因为在这里，我们将父对象设置成了子对象的原型。

# 原型继承与属性复制的混合使用
我们知道实现继承就是将已有的功能归为所有，我们在new一个新对象的时候，应该继承于现有对象，然后再为其添加额外的属性与方法。
* 原型继承可以在新建一个对象的时候，将已有对象设置为新的对象的原型。
* 属性拷贝，就是在新建一个对象之后，将另一个已有对象的属性拷贝过来。
我们将这两项功能放在一个函数中。
具体实现如下
```
function objectPlus(o, stuff) {
var n;
function F() {}
F.prototype = o;
n = new F();
n.uber = o;
for (var i in stuff) {
n[i] = stuff[i];
}
return n;
}
```
接受两个对象，一个用于原型继承，一个用于属性拷贝，这里使用的是浅拷贝，也可以改成深拷贝。
下面来看这个混合继承方式的应用：
```
var shape = {
name: 'Shape',
toString: function () {
return this.name;
}
};
```
```
var twoDee = objectPlus(shape, {
name: '2D shape',
toString: function () {
return this.uber.toString() + ', ' + this.name;
}
});
```
```
var triangle = objectPlus(twoDee, {
name: 'Triangle',
getArea: function () {
return this.side * this.height / 2;
},
side: 0,
height: 0
});
```
实现继承关系后，我们新建一个对象
```
var my = objectPlus(triangle, {
side: 4, height: 4
});
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-7696f0c7d31e829f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们看到调用toString的时候，出现了两次triangle，这是因为，my又是继承自Triangle,所以多了一个继承层次，我们可以更改name属性，在测试。实际上这种原型继承方式抛弃了构造器，但没有除去原型。
