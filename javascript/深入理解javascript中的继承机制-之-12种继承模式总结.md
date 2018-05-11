之前我们介绍了多种javascript中的继承方式，最后我们开始总结概括这些继承方式，先将javascript中的继承分类，根据不同的条件，可以分成不同的类别。
最常用的我们可以分为这两类：
* 基于构造器的继承模式
* 基于对象的继承模式

或者我们也可以如下分类：
* 是否使用原型
* 是否使用了属性拷贝
* 即使用了原型，也使用了属性拷贝

下面我们就来总结回顾一下javascript中的继承模式

# 原型链法
示例：
```
Child.prototype = new Parent();
```
分类：
使用了原型
基于构造器的继承模式

** 注意 **：
* 默认的继承机制
* 我们可以将需要重用的属性和方法移到原型中，不需要重用的则作为自身的属性

# 仅从原型继承
实例：
```
Child.prototype = Parent.prototype;
```
分类：
基于构造器模式
复制原型对象，没有原型链的关系，因为都共用一个原型对象
** 注意 **：
* 效率更高，没有多余的实例被new出来
* 原型属性查找更快，因为不存在原型链关系
* 由于都是基于同一个原型，所以对子对象的修改，会影响到父对象

# 临时构造器
实例
```
function extend(Child,Parent) {
var F = function(){};
F.prototype = Parent.prototype;
Child.prototype = new F();
Child.prototype.constructor = Child;
Child.uber = Parent.prototype;
}
```
分类：
基于构造器模式
使用了原型链
** 注意 **：
* 是前面几种方法的改进，只继承原型对象的属性和方法，自身属性和方法是不继承的
* 通过uber可以方便的访问到父对象

# 原型属性拷贝
实例：
```
function extend2(Child,Parent) {
var p = Parent.prototype;
var c = Child.prototype;
for (var i in p) {
c[i] = p[i];
}
c.uber = p;
}
```

分类：
基于构造器模式
基于属性拷贝的模式
使用了原型链

** 注意 **：
* 父原型的所有属性拷贝到子原型上
* 不用new出新的对象
* 更短的原型链

# 所有属性拷贝(浅拷贝)
实例：
```
function extendCopy(p) {
var c = {};
for (var i in p) {
c[i] = p[i];
}
c.uber = p;
return c;
}
```
分类：
基于对象的模式
属性拷贝

** 注意 **：
* 简单
* 不用使用原型

# 深拷贝
实例：
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
分类：
基于对象的模式
属性拷贝

** 注意 **：
* 简单
* 不用使用原型
* 对对象和数组也进行了复制

# 原型继承法
实例：
```
function object(o)
{
function F() {}
F.prototype = o;
return new F();
}
```
分类：
基于对象的模式
使用原型链
** 注意 **：
* 直接在对象之间构建继承关系

# 扩展与增强模式
实例：
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
分类：
基于对象的工作模式
使用原型链
属性拷贝模式
** 注意 **
* 此方法实际上是原型继承法与属性拷贝法的混合应用
* 同时实现继承和扩展

# 多重继承法
```
function multi() {
var n = {}, stuff,j = 0,
len = arguments.length;
for (j = 0; j<len; j++) {
stuff = arguments[j];
for (var i in stuff) {
n[i] = stuff[i];
}
}
return n;
}
```
分类：
基于对象的工作模式
属性拷贝模式
** 注意 **
* 一种混合插入式的继承实现
* 依照父对象出现的次序，执行属性全拷贝方法

# 寄生式继承
实例：
```
function parasite(victim) {
var that = object(victim);
that.more = 1;
return that;
}
```
分类：
基于对象的工作模式
使用原型链
** 注意 **
* 该方法通过一个类似构造函数的函数来创建对象
* 该函数会执行对象的拷贝，并可以进行扩展，然后返回对象

# 借用构造函数：
实例：
```
function Child() {
Parent.apply(this,
arguments);
}
```
分类：
基于构造函数的模式

** 注意 **：
* 仅继承自身属性
* 与方法一结和使用方便继承原型
* 方便于子对象继承某个对象的具体属性

# 构造器于属性拷贝
实例：
```
function Child() {
Parent.apply(this,arguments);
}
extend2(Child,Parent);
```
分类：
基于构造器模式
使用原型链
属性拷贝
** 注意 **
* 借用构造器与原型属性拷贝的结合
* 允许在不重复调用父对象构造器的情况下同时继承自身属性和原型属性
