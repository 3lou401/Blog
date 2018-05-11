
> * 引入责任链模式
* 责任链模式的实例
* 责任链模式的分析
* 责任链模式的优势
* 责任链模式的应用

# 引入责任链模式
责任链模式描述的就是如何推卸责任，说的简洁点，就是踢皮球哈哈。举个例子，有时候，出了某件事，我们去解决，找到A，结果A踢皮球，说这不关我的事，去找B解决，然后我们就去找B，结果B也说，这跟我没关系，快去找C，就这样，我们就被踢来踢去，这就是责任链模式的思想，在找到正确的人解决之前，我们被不断的踢给一个有一个人，就是推卸责任。
上面的例子，可能有点贬义，但在实际编程中，有时候确实存在需要推卸责任的情况，，比如，当我们接受到一个请求时，当前的程序暂时无法处理这个请求，于是就需要把请求给别人去处理。如果是web开发人员，对此应该很熟悉，当服务器收到一个客户端的请求时，首先会解析请求，action层不会处理请求，而是将请求的参数等信息进行简单的解析处理，然后根据请求的内容信息等将请求具体转发给service去处理。
当一个人被要求做一件事的时候，如果他自己可以做，那他就自己做了，如果他自己做不了，那就转发给另一个人做，另一个人也是一样，如果他自己可以做，就做，不可以做，就给别人做。。。。。。
这就是责任链模式的基本思想

# 责任链模式的实例
实例的类图

![image.png](http://upload-images.jianshu.io/upload_images/1234352-9e2836c3c0863af1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Support是一个抽象类，他的核心方法support中，如果当前support可以解决，就解决，如果不行，就交给next去解决。
```
package ChainOfResponse;

public abstract class Support {
	private String name;
	private Support next;
	
	public Support(String name) {
		this.name = name;
	}
	
	public Support setNext(Support next) {
		this.next = next;
		return next;
	}
	
	public final void support(Trouble trouble) {
		if(resolve(trouble)) {
			done(trouble);
		} else if (next != null) {
			next.support(trouble);
		} else {
			fail(trouble);
		}
	}
	
	public String toString() {              // 显示字符串
        return "[" + name + "]";
    }
	
	protected abstract boolean resolve(Trouble trouble);
	
	protected void done(Trouble trouble) {
		System.out.println(trouble + " is resolved by " + this + ".");
	}
	
	protected void fail(Trouble trouble) {  // 未解决
        System.out.println(trouble + " cannot be resolved.");
    }
}
```
然后我们实现几个具体的support类
NoSupport类是一个永远不解决问题的类
```
package ChainOfResponse;

public class NoSupport extends Support {

	public NoSupport(String name) {
		super(name);
	}

	@Override
	protected boolean resolve(Trouble trouble) {
		return false;
	}
	
}
```

LimitSupport类，解决指定范围内的问题
```
package ChainOfResponse;

public class LimitSupport extends Support {

	private int limit;
	
	public LimitSupport(String name, int limit) {
		super(name);
		this.limit = limit;
	}

	@Override
	protected boolean resolve(Trouble trouble) {
		if(trouble.getNumber() < limit)
			return true;
		return false;
	}
}

```
OddSupport类,解决奇数的问题
```
package ChainOfResponse;

public class OddSupport extends Support {

	public OddSupport(String name) {
		super(name);
	}

	@Override
	protected boolean resolve(Trouble trouble) {
		if(trouble.getNumber() % 2 == 1)
			return true;
		return false;
	}
}

```

```
package ChainOfResponse;

public class SpecialSupport extends Support {
	
	private int number;
	
	public SpecialSupport(String name, int number) {
		super(name);
		this.number = number;
	}

	@Override
	protected boolean resolve(Trouble trouble) {
		if(trouble.getNumber() == number)
			return true;
		return false;
	}
	
}

```
,最后实现一个main类：
```
package ChainOfResponse;

public class Main {

	public static void main(String[] args) {
		
		Support alice   = new NoSupport("Alice");
        Support bob     = new LimitSupport("Bob", 100);
        Support charlie = new SpecialSupport("Charlie", 429);
        Support diana   = new LimitSupport("Diana", 200);
        Support elmo    = new OddSupport("Elmo");
        Support fred    = new LimitSupport("Fred", 300);
        
        alice.setNext(bob).setNext(charlie).setNext(diana).setNext(elmo).setNext(fred);
        
        for(int i=0;i<500;i+=33) {
        	alice.support(new Trouble(i));
        }
	}

}

```
Main类中定义了一个责任链，将几个support对象连接在一起，组成了一条责任链，然后去处理问题
运行结果如下：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-404d73924f06e031.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 责任链模式的分析

首先，责任链模式中，存在着这么几个角色：

* Handler处理者
handler金额use定义了处理请求的接口，handler知道，下一个处理者是谁，如果自己无法处理请求，就转给下一个处理者。
在实例中对应的是，support类和support方法

* concreteHandler(具体的处理者)
具体的处理者是处理请求的具体角色。
在此实例中，由NoSupport角色和其他几个类扮演

* Client
请求者角色，就是向第一个具体的handler发送请求的角色，并连接好责任链，实例中对应的是main类的main方法。

责任链模式的类图如下：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-e34f882565fe16ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 责任链的作用

* 弱化了发出请求的人和处理请求的人之间的关系
发出请求的人只需要向第一个具体的处理者发送请求，然后就可以不用管了，处理者会在责任链上自己寻找处理的方法。
这样就解耦了处理者和请求者之间的关系。
如果我们不采取责任链模式，那么请求者就必须要很清楚哪个处理者能处理它的请求，就必须对所有的处理者都有所了解，类似于上帝视角，然而在实际中，要求请求这了解这么多是不实际的

* 可以动态的改变责任链
责任链还有的好处就是可以动态的改变责任，删除或者添加或者改变顺序。

* 让各个处理者专注于实现自己的职责
责任链模式同时还做到了处理者之间的解耦，处理者自己专注于自己的处理逻辑就好，不管其他处理者干什么。

* 推卸责任也可能导致处理延迟
我们可以责任链模式需要在责任链上传播责任，直至找到合适的处理对象。这样提高了程序的灵活性，但同时也出现了处理的延迟，因为有一个寻找的过程。所以需要低延迟的情况下，就不应该使用责任链模式

# 责任链模式的应用
在视窗系统中，经常会使用到责任链模式，尤其是事件的处理，熟悉javascript开发的朋友，可能会知道，浏览器中的事件有冒泡机制，，就是事件的是向父控件传播的，如果自己处理不了，就会传播给父控件去处理。
