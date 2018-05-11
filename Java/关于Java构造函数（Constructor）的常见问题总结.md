这篇文章总结了Java使用构造函数中最常遇到的五个问题！

# 1 为什么调用子类的构造方法的时候，默认会调用父类的构造方法

看下面这个简单的例子：
```
package cc;

public class Sub extends Super {
	public Sub() {
		System.out.println("Sub");
	}
	
	public static void main(String[] args) {
		Sub sub = new Sub();
	}
}

class Super {
	public Super() {
		System.out.println("Super");
	}
}
```


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-0ddfd7f4cd516e4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当继承自一个类的时候，构造方法就会首先调用super()方法。如果没有显式的写这个语句，那么编译器就会自动插入这个语句。这就是为什么我们上面的那个例子程序会先调用super的构造方法。
但要切记，** 虽然调用了父类的构造方法，但只创建了一个对象也就是子对象。**
之所以要调用父类的构造方法，是因为super类可能需要构造函数来初始化一些私有的成员变量。
编译器自动插入super构造方法后，子类的构造函数就会像下面这样：
```
public Sub(){
    super();
    System.out.println("Sub");
}
```

# 2 常见错误：Implicit super constructor is undefined for default constructor. Must define an explicit constructor

> Implicit super constructor is undefined for default constructor. Must define an explicit constructor

这个错误是很多开发者经常遇到的错误，错误原因就是找不到超类中的默认构造函数。

看下面的代码：
```
package cc;

public class Sub extends Super {
	
	public Sub(String s) {
		
	}
	
	public static void main(String[] args) {
		Sub sub = new Sub();
	}
}

class Super {
	String s;
	public Super(String s) {
		this.s = s;
	}
}

```
上面这段代码会报错：
Implicit super constructor Super() is undefined. Must explicitly invoke another constructor。

编译器错误是因为默认的super()无参的构造函数是没有定义的。在Java中，如果一个类没有定义构造函数，编译器会自动插入一个默认的无参的构造函数。
但是，如果类中定义了一个构造函数，编译器就不会自动插入无参的构造函数了，所以如果我们不显示定义一个无参的构造函数，那么这个构造函数就不存在。

上一小节，我们知道，如果子类的构造函数中，没有显示的调用父类的构造函数，那么，编译器就会插入super()，也就是自动调用无参的构造函数。但是此时，父类没有无参的构造函数，所以就会报错了。

解决这个问题很简单，我们可以给父类插入一个无参的构造函数，或者在子类构造函数中显示的调用的父类有参构造函数。

# 在子类的构造函数中显示的调用父类的构造函数

下面的代码是正确的。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-a3bb3214f6ffbb00.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 构造函数的使用规则

简单的说，在使用的时候，子类的构造函数必须调用父类的构造函数，不管有没有显示的声明。所以，被调用的父类构造函数，一定在定义好！

# 为什么Java在一个类已经实现了一个带参的构造函数的时候，不实现默认的无参构造函数？

这是个很有趣的问题。我们知道如果在一个类中没有声明一个构造函数，那么编译器会隐式的帮我们实现一个无参的构造函数，但如果我们一旦一个构造函数，不管带不带参数，那么编译器都不会提供默认的构造函数，所以这么做的原因是为什么呢？

有一个原因就是，如果我们给所有的类都自动实现一个无参的构造函数，就可能出现问题，会打破类的设计原则。
比如说，考虑这个Scanner类，他有几个构造函数，你可以通过这几个构造函数，声明你想要读取数据的来源，如果编译器增加了无参的构造函数，那么你不给定读取的数据源，就会报错，程序无法执行，因为我们不能不指定一个数据源就让他去读取数据。
