适配器模式（Adapter Pattern）在生活中的应用随处可见。最常见的，我们使用的转接头就是利用了适配器模式的思想，我们可能用type-c接口的手机，但现在只有普通接口的充电器，这时候我们买一个typec转普通接口的转接头就可以了。再比如，我们笔记本可能没有hdmi接口，但有usb接口，那么我们只要买一个usb转hdmi的接口就可以了。这些都是应用了适配器的思想。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-f6ef82b346159675.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当已有的系统接口和所需求的接口存在不适配的情况时，我们只需要实现一个转接功能的适配器使其可以适配原有的和新的接口。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-0a651b844669b436.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过一个实例来说明适配器模式：
假设我们现在有一个鸭子接口：
```
package Adapter;

public interface Duck {
	public void quack();
	public void fly();
}
```
定义了一个fly和quack方法。
我们实现一个MallardDuck的类
```
package Adapter;

public class MallardDuck implements Duck {

	public void quack() {
		System.out.println("Quack");
	}
	public void fly() {
		System.out.println("I’m flying");
	}

}
```

同时，还有一个土鸡的接口
```
package Adapter;

public interface Turkey {
	
	public void gobble();
	public void fly();

}

```
以及一个土鸡类：
```
package Adapter;

public class WildTurkey {
	public void gobble() {
		System.out.println("Gobble gobble");
	}
	public void fly() {
		System.out.println("I’m flying a short distance");
	}
}

```
通过上面的代码我们知道鸭子只会quack也就是鸭子叫，土鸡会gobble也就是土鸡叫，他们叫声是不同的。但是假设我们现在想要土鸡也看作一种鸭子，让它也能quack那应该怎么处理呢？

这是我们就可以实现一个适配器，让土鸡也会像鸭子一样quack

```
package Adapter;

public class TurkeyAdapter implements Duck {
	
	Turkey turkey ;
	
	public TurkeyAdapter(Turkey turkey) {
		this.turkey = turkey;
	}
	
	@Override
	public void quack() {
		turkey.gobble();
	}

	@Override
	public void fly() {
		turkey.fly();
	}

}

```

只要在适配器里加一个turkey的引用即可。是不是很简单？这就是适配器模式，很直观，因为在生活中有很多实例。



![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-d567cd6540babc96.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


适配器模式由三部分组成
client(客户端)，adapter（适配器）, adaptee（被适配者）
* 客户端通常都实现了一个目标接口
* 适配器也要实现同一个目标接口，并且持有一个被适配者的实例对象

# 适配器模式的定义

> ** Adapter Pattern 适配器模式** 将一个类的接口转换成调用者所期待的接口。适配器模式可以让不同的类在不相匹配的接口下也能正常工作。

可以看到适配器模式的类图清楚的说明：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-7a4333f2a946f274.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
