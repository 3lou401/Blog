单件模式，也叫单例模式，可以说是设计模式中最简单的一种。顾名思义，就是创造独一无二的唯一的一个实例化的对象。

为什么要这样做呢？因为有些时候，我们只需要一个对象就够了，太多对象反而会引起不必要的麻烦。比如说，线程池，缓存，打印机，注册表，如果存在多个实例的话，反而会导致许多问题！

# 引出单例模式

我们通过一个小问题引出单例模式！
* 如何创建一个对象？我们都知道 `new MyObject();`
* 当我们需要创建与另外一个对象时，只需要再次`new MyObject();`即可
* 那么如下这样的代码是正确的么？

```
public MyClass{
  private MyClass() {}  
}
```
* 看过去这是合法的定义，没有什么语法错误。但仔细想想，含有私有构造器的话，只能在MyClass内调用构造器。因为必须有Myclass的实例才能调用构造器，但因为没有其他类可以取得它的实例，所以，我们无法实例化它，这像不像鸡生蛋还是蛋生鸡的问题？哈哈哈
* 为了解决这个问题，取得MyClass类的实例，我们创造一个静态方法

```
public MyClass{
  private MyClass() {}  
  public static MyClass getInstance() {
    return new MyClass();
  }
}
```
* 我们添加了一个静态的类方法，它可以返回一个对象实例，由于他是public，所以外部可以调用他。这实际上就实现了一个简单的单例模式。

# 经典单例模式的实现

```
public class Singleton {
	private static Singleton uniqueInstance;
	
	private Singleton(){}
	
	public static Singleton getInstance() {
		if (uniqueInstance == null) {
			uniqueInstance = new Singleton();
		}
		return uniqueInstance;
	} 
}
```
*  这里实现了一个概念，叫延迟实例化（lazy instance）。因为在我们不需要实例的时候，这个实例就永远不会被实例化。

# 定义单件模式
>单件模式的定义: 确保一个类只有一个实例，并提供一个全局访问点。

这定义应该很好理解，我们结合类图说明：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-46f80fd9bf14355c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 经典单件模式存在的问题
经典单件模式实际中存在这一定的问题，在第一次初始化实例的时候，如果同时有不同的线程访问，那么可能最后不只实例化出一个对象。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-f35abcdeb877e907.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如图所示，如果两个线程如图所示的顺序交错执行，那么最后会实例化两个对象！
这就是经典单例模式存在的多线程问题。

# 解决单例模式的多线程问题

## synchronize
显然最简单的一种解决方法就是同步getInstance方法。

```
public class Singleton {
	private static Singleton uniqueInstance;
	
	private Singleton(){}
	
	public static synchronized Singleton getInstance() {
		if (uniqueInstance == null) {
			uniqueInstance = new Singleton();
		}
		return uniqueInstance;
	} 
}
```
这样显然可以很好的解决问题，但是同步会降低效率。

## 急切实例化

```
public class Singleton {
	private static Singleton uniqueInstance = new Singleton();
	
	private Singleton(){}
	
	public static synchronized Singleton getInstance() {
		return uniqueInstance;
	} 
}
```

在任何线程访问uniqueInstance变量前，我们保证一定已经创建了这个实例。

## 双重检查加锁

```
public class Singleton {
	private volatile static Singleton uniqueInstance;
	
	private Singleton() {}
	
	public static Singleton getInstance() {
		if (uniqueInstance == null) {
			synchronized (Singleton.class) {
				if (uniqueInstance == null) {
					uniqueInstance = new Singleton();
				}
			}
		}
		return uniqueInstance;
	}
}
```
