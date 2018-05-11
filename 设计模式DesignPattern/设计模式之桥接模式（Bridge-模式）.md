> * 类的功能层次
* 类的实现层次
* 桥接模式的具体事例
* 小结

Bridge的意思是桥梁，作用就是将两边连接起来。桥接模式的作用也是如此，桥接模式分别**类的功能层次**和**类的实现层次**连接起来。

这里出现了两个可能有点陌生的词汇，类的功能层次和类的实现层次。

所以我们先来介绍这两种的层次结构，因为桥接模式就是为了连接这两种层次结构。

# 类的功能层次
用于添加的新的功能，假如现在有一个类，我们想在这个类中添加一个新的功能，同时又不改变原有的类，那么我们可以采用继承的方法，继承自这个类，然后在继承的类中添加一个具体的新的方法。这就是类的功能层次。

* 父类拥有基本的功能
* 子类对类的功能进行扩展，添加的新的功能

注意：类的功能层次不能太深

# 类的实现层次
用于添加新的实现。假如我们现在有一个抽象类或者接口，里面定义了相应的方法，但是没有实现，对于不同的具体的实现我们需要继承这个抽象类或者实现接口，这就是类的实现层次。

* 父类通过声明抽象方法来定义接口
* 子类通过实现具体方法来实现接口

# 类的层次结构的混杂与分离
所以学习了类的功能层次和实现层次之后，我们在编写子类的就可以考虑一个问题，我们要添加功能还是添加实现。当类的层次结构只有一层的时候，功能层次结构与实现层次结构是混在一起的，这样就容易是类的层次结构变得复杂难以理解。
因此，我们需要将类的功能层次和实现层次分离为两个独立的层次结构，但又不能的简单的分开，分开之后又要添加某种联系，这种联系就是桥梁，也就是我们本文要讲的桥接模式。

# 桥接模式的具体实例
这个实例的功能就是打印显示某个东西。

* 我们先考虑类的功能层次

类的功能层次只需要考虑具体需要考虑哪些，具体的实现交给实现层次去实现，那么功能层次为了调用实现层次，就需要持有一个实现层次的对象，就是委托。

功能层次的基类Display
```
package Bridge;

public class Display {
	private DisplatImpl impl;
	
	public Display(DisplatImpl impl) {
		this.impl = impl;
	}
	
	public void open() {
		impl.rawOpen();
	}
	
	public void print() {
		impl.rawPrint();
	}
	
	public void close() {
		impl.rawClose();
	}
	
	public final void display() {
		open();
		print();
		close();
	}
}
```

然后我们给这个类添加功能，可以多次显示的功能
COuntPlay:
```
package Bridge;

public class CountDisplay extends Display{

	public CountDisplay(DisplatImpl impl) {
		super(impl);
	}
	
	public void multiDisplay(int times) {
		open();
		for(int i=0;i<times;i++)
			print();
		close();
	}
}

```

* 类的实现层次

首先，类的实现层次的基类应该是一个接口或者抽象类，他定义了需要实现的方法
```
package Bridge;

public abstract class DisplatImpl {
	public abstract void rawOpen();
	public abstract void rawPrint();
	public abstract void rawClose();
}
```

然后我们看一个真正的具体实现，实现了上述的接口
```
public class StringDisplayImpl extends DisplayImpl {
    private String string;                              // 要显示的字符串
    private int width;                                  // 以字节单位计算出的字符串的宽度
    public StringDisplayImpl(String string) {           // 构造函数接收要显示的字符串string
        this.string = string;                           // 将它保存在字段中
        this.width = string.getBytes().length;          // 把字符串的宽度也保存在字段中，以供使用。
    }
    public void rawOpen() {
        printLine();
    }
    public void rawPrint() {
        System.out.println("|" + string + "|");         // 前后加上"|"并显示
    }
    public void rawClose() {
        printLine();
    }
    private void printLine() {
        System.out.print("+");                          // 显示用来表示方框的角的"+"
        for (int i = 0; i < width; i++) {               // 显示width个"-"
            System.out.print("-");                      // 将其用作方框的边框
        }
        System.out.println("+");                        // 显示用来表示方框的角的"+"
    }
}
```

最后我们来调用这两个层次：
```
public class Main {
    public static void main(String[] args) {
        Display d1 = new Display(new StringDisplayImpl("Hello, China."));
        Display d2 = new CountDisplay(new StringDisplayImpl("Hello, World."));
        CountDisplay d3 = new CountDisplay(new StringDisplayImpl("Hello, Universe."));
        d1.display();
        d2.display();
        d3.display();
        d3.multiDisplay(5);
    }
}
```

运行结果：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-10837c15ff603da8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上述实例的类图：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-22d6130b3a418ba0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Bridge模式的类图也是类似的：


![image.png](http://upload-images.jianshu.io/upload_images/1234352-3139c3b3f60223a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 小结
* 分开后更容易扩展
桥接模式的特点是将类的功能层次和实现层次分开。分开之后的好处就是有利于对它们进行扩展，当要添加新的功能的时候，只要在功能层次添加类就可以了。不必对类的实现层次做任何修改。而且增加的功能可以被所有的实现使用。

例如，如果我们程序中依赖操作系统的部分划分为max，windows和linux版，我们就可以利用类的桥接层次中的实现层次来表现这些依赖操作系统的部分。
