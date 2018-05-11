> * 内存泄漏
> * 内存泄漏发生的原因
> * 造成内存泄露的常见情形
> * 内存泄露的解决方案

Java的一个最显著的优势是内存管理。你只需要简单的创建对象而不需要负责释放空间，因为Java的垃圾回收器会负责内存的回收。然而，情况并不是这样简单，内存泄露还是经常会在Java应用程序中出现。

# 内存泄漏
内存泄露的定义：对于应用程序来说，当对象已经不再被使用，但是Java的垃圾回收器不能回收它们的时候，就产生了内存泄露。

要理解这个定义，我们需要理解对象在内存中的状态。如下图所示，展示了哪些对象是无用对象，哪些是未被引用的对象；


![image.png](http://upload-images.jianshu.io/upload_images/1234352-3e98eda54f540f3e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

未引用对象将会被垃圾回收器回收，而引用对象却不会。未引用对象很显然是无用的对象。然而，无用的对象并不都是未引用对象，有一些无用对象也有可能是引用对象，这部分对象正是内存泄露的来源。

# 内存泄漏发生的原因
如下图所示，对象A引用对象B，A的生命周期（t1-t4）比B的生命周期（t2-t3）要长，当B在程序中不再被使用的时候，A仍然引用着B。在这种情况下，垃圾回收器是不会回收B对象的，这就可能造成了内存不足问题，因为A可能不止引用着B对象，还可能引用其它生命周期比A短的对象，这就造成了大量无用对象不能被回收，且占据了昂贵的内存资源。

同样的，B对象也可能引用着一大堆对象，这些被B对象引用着的对象也不能被垃圾回收器回收，所有的这些无用对象消耗了大量内存资源。

![image.png](http://upload-images.jianshu.io/upload_images/1234352-5d086583e1fed262.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 造成内存泄露的常见情形
- 集合类，比如HashMap，ArrayList等，这些对象经常会发生内存泄露。比如当它们被声明为静态对象时，它们的生命周期会跟应用程序的生命周期一样长，很容易造成内存不足。
像HashMap、Vector等的使用最容易出现内存泄露，这些静态变量的生命周期和应用程序一致，他们所引用的所有的对象Object也不能被释放，因为他们也将一直被Vector等引用着。
```java
Static Vector v = new Vector(10);
for (int i = 1; i<100; i++)
{
Object o = new Object();
v.add(o);
o = null;
}
```
- 当集合里面的对象属性被修改后，再调用remove()方法时不起作用。
```java
package 校招;

import java.util.HashSet;
import java.util.Set;

public class MemoryOut {
	public static void main(String[] args) {
		Set<Person> set = new HashSet<Person>();
		Person p1 = new Person("唐僧","pwd1",25);
		Person p2 = new Person("孙悟空","pwd2",26);
		Person p3 = new Person("猪八戒","pwd3",27);
		set.add(p1);
		set.add(p2);
		set.add(p3);
		System.out.println("总共有:"+set.size()+" 个元素!"); //结果：总共有:3 个元素!
		p3.setAge(2); //修改p3的年龄,此时p3元素对应的hashcode值发生改变
		set.remove(p3); //此时remove不掉，造成内存泄漏
		set.add(p3); //重新添加，居然添加成功
		System.out.println("总共有:"+set.size()+" 个元素!"); //结果：总共有:4 个元素!
		for (Person person : set)
		{
		System.out.println(person);
		}
	}
}

class Person {
	
	int age;
	String name;
	String password;
	
	public Person(String name, String password, int age) {
		this.name = name;
		this.password = password;
		this.age = age;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + age;
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Person other = (Person) obj;
		if (age != other.age)
			return false;
		return true;
	}
	
}
```
- 监听器
在java 编程中，我们都需要和监听器打交道，通常一个应用当中会用到很多监听器，我们会调用一个控件的诸如addXXXListener()等方法来增加监听器，但往往在释放对象的时候却没有记住去删除这些监听器，从而增加了内存泄漏的机会。

- 各种连接
比如数据库连接（dataSourse.getConnection()），网络连接(socket)和io连接，除非其显式的调用了其close（）方法将其连接关闭，否则是不会自动被GC 回收的。对于Resultset 和Statement 对象可以不进行显式回收，但Connection 一定要显式回收，因为Connection 在任何时候都无法自动回收，而Connection一旦回收，Resultset 和Statement 对象就会立即为NULL。但是如果使用连接池，情况就不一样了，除了要显式地关闭连接，还必须显式地关闭Resultset Statement 对象（关闭其中一个，另外一个也会关闭），否则就会造成大量的Statement 对象无法释放，从而引起内存泄漏。这种情况下一般都会在try里面去的连接，在finally里面释放连接。

- 内部类和外部模块的引用
内部类的引用是比较容易遗忘的一种，而且一旦没释放可能导致一系列的后继类对象没有释放。此外程序员还要小心外部模块不经意的引用，例如程序员A 负责A 模块，调用了B 模块的一个方法如：
public void registerMsg(Object b);
这种调用就要非常小心了，传入了一个对象，很可能模块B就保持了对该对象的引用，这时候就需要注意模块B 是否提供相应的操作去除引用。

- 单例模式
不正确使用单例模式是引起内存泄漏的一个常见问题，单例对象在初始化后将在JVM的整个生命周期中存在（以静态变量的方式），如果单例对象持有外部的引用，那么这个对象将不能被JVM正常回收，导致内存泄漏，考虑下面的例子：
```java
class A{
public A(){
B.getInstance().setA(this);
}
....
}
//B类采用单例模式
class B{
private A a;
private static B instance=new B();
public B(){}
public static B getInstance(){
return instance;
}
public void setA(A a){
this.a=a;
}
//getter...
}
```
显然B采用singleton模式，它持有一个A对象的引用，而这个A类的对象将不能被回收。想象下如果A是个比较复杂的对象或者集合类型会发生什么情况.

# 内存泄露的解决方案
- 避免在循环中创建对象。
- 尽早释放无用对象的引用。 （最基本的建议）
- 尽量少用静态变量， 因为静态变量存放在永久代（方法区） ， 永久代基本不
参与垃圾回收。
- 使用字符串处理， 避免使用 String， 应大量使用 StringBuffer， 每一个 String
对象都得独立占用内存一块区域。
