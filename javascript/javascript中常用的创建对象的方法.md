js中创建对象最简单的方法自然是直接new一个Object然后再为其添加属性和方法，例如一下代码：
```
var o = new Object();
o.name = "aaaa";
o.sayName = function() {
  alert(this.name);
}
```
但这样显然封装性太差，属性和方法分布在各个地方。
所以最容易想到的就是写一个函数来封装创建对象的过程，这就是设计模式中常用的工厂模式。
下面我们就先介绍一下工厂方法模式创建对象的方法
***
# 工厂模式
先直接上代码
```
function createStudent(no, name, age, class)
{
    var o = new Object();
    o.no = this.no;
    o.name = this.name;
    o.age = this.age;
    o.class = this.class;
    o.sayName = function() {
            alert(this.name);
    }
  return o;
}

var stu1 = createStudent(5,"chi",34,1);
stu1.sayName();
```
  上述代码就很好的像我们展示了如何用工厂模式创建对象，我们可以重复调用这个函数创建对象，每调用一次就会根传进去的参数，创建一个新的对象。但工厂模式存在的问题是无法识别生成的是哪个对象。
而构造函数模式就可以很好的解决这个问题
# 构造函数模式
类似java语言和其他面向对象语言的构造函数，构造函数模式如下：
```
function Student(name,no,age,class)
{
  this.name = name;
  this.no = no;
  this.age = age;
  this.class = class;
  this.sayName = function() {
  alert(this.name);
  }
}

var stu1= new Student("Nicholas", 29,34,4); 
```
与工厂模式比较，我们发现构造函数模式，直接赋值给this，不用在函数内创建一个Object，也需要return语句。
在使用构造函数模式创建对象的时候，只需要跟其他面向对象语言一样使用new操作符即可。
实际上，js在使用构造函数模式创建对象的过程中有以下的几个步骤：
* 创建一个新对象
* 将对象的作用域赋给新对象
* 调用构造函数中的代码为属性和方法赋值
* 返回新对象
其中，我们发现js帮我们封装了1，2，4等步骤，我们只需要专注于创建对象的属性和方法就行了。
**构造函数模式虽然好用，但也并非没有缺点。使用构造函数的主要问题，就是每个方法都要在每个实例上重新创建一遍。在前面的例子中， stu1 和 stu2 都有一个名为 sayName()的方法，但那两个方法不是同一个 Function 的实例。** 
而实际上呢，我们只需要一个sayName函数的实例就行了，因为它们的作用都是一样的，如果按构造函数模式，就会造成很多无用的浪费。
由此，我们就引出了下一种的方法，原型模式
# 原型模式
原型对象简而言之，就是每个构造函数创建的对象都有一个指针，这个指针指向它的原形对象，而原形对象也和普通对象一样具有属性和方法，但不同的事，原形对象的属性和方法是让所有实例共享的，也就是它只有一份，谁需要，都是调用这一份副本，而不会反复创建。
```
function Person(){}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
alert(this.name);
};
var person1 = new Person();
person1.sayName(); //"Nicholas"
var person2 = new Person(); 
```

![prototype.PNG](http://upload-images.jianshu.io/upload_images/1234352-5a574eba019670b3.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这就是原型模式创建对象的方法，它可以通过共享来避免重复创建多余的函数。
但原型模式，显然存在一个问题就是，并不是所有东西都是共享的，所以实际中，我们常常将原型模式与工厂模式或者构造函数模式结合起来。联合使用。对于那些需要共享的属性和方法，我们就把它加入到原型对象中。

** 需要注意的是，如果实例对象和原型对象中的存在相同的属性和方法，那么js会先从实例中搜寻，如果找到了就忽略原型对象中的，如果在实例中没有找到，就继续到原型中寻找 **

# 混合使用构造函数模式和原型模式

创建自定义类型的最常见方式，就是组合使用构造函数模式与原型模式。构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性。结果，每个实例都会有自己的一份实例属性的副本，但同时又共享着对方法的引用，最大限度地节省了内存。另外，这种混成模式还支持向构造函数传递参数；可谓是集两种模式之长。 
```
function Person(name, age, job)
{
  this.name = name;
  this.age = age;
  this.job = job;
  this.friends = ["Shelby", "Court"];
}
Person.prototype = {
  constructor : Person,
  sayName : function(){
  alert(this.name);
  }
}
var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor"); 
```

# 小结
以上就是js中主要用于创建对象的几种方法，工厂模式，构造函数模式，原型模式，构造函数模式和原型模式的组合使用。
