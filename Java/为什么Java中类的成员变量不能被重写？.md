这篇文章讨论了Java面向对象概念中一个基本的概念--Field Hiding（成员变量隐藏）

# 成员变量在Java中能够被重写么？
Let’s first take a look at the following example which creates two Sub objects. One is
assigned to a Sub reference, the other is assigned to a Super reference.
我们看下面这个例子，我们创建了两个子对象，一个使用的是子对象的引用，一个使用的是父对象的引用。
```

public class FieldOverriding {

	public static void main(String[] args) {
		Sub c1 = new Sub();
		Super c2 = new Sub();
		System.out.println(c1.s);
		System.out.println(c2.s);

	}

}

class Super {
	String s = "Super";
}

class Sub extends Super {
	String s = "Sub";
}
```
程序的输出结果是：


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-ffec1b4b53a3e3d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

按照我们已有的多态的概念，第二个应该是输出sub才对，但却输出了super。这是为什么呢？

# 不会重写成员变量，而是隐藏成员变量
Java文档中对隐藏域的定义：

> Within a class, a field that has the same name as a field in the superclass hides the superclass’s field, even if their types are different. Within the subclass, the field in the superclass cannot be referenced by its simple name. Instead, the field must be accessed through super. Generally speaking, we don’t recommend hiding fields as it makes code difficult to read.

意思就是：

> 在一个类中，子类中的成员变量如果和父类中的成员变量同名，那么即使他们类型不一样，只要名字一样。父类中的成员变量都会被隐藏。在子类中，父类的成员变量不能被简单的用引用来访问。而是，必须从父类的引用获得父类被隐藏的成员变量，一般来说，我们不推荐隐藏成员变量，因为这样会使代码变得难以阅读。

其实，简单来说，就是子类不会去重写覆盖父类的成员变量，所以成员变量的访问不能像方法一样使用多态去访问。

# 访问隐藏域的方法

* 就是使用父类的引用类型，那么就可以访问到隐藏域，就像我们例子中的代码

* 就是使用类型转换```System.out.println(((Super)c1).s);```

翻译自[http://www.programcreek.com/simple-java/](http://www.programcreek.com/simple-java/)
