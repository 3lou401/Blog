* 什么是反射？
* 反射有什么用？
* 如何使用反射？

# 什么是反射？
反射被广泛运用在那些需要检查和控制改变在运行时的行为的程序中。反射的概念常常和自检（introspection）搞混。
维基百科中的自检（introspection）定义为：
* 自检（introspection）是程序能在运行时检查对象的类型和属性的能力
* 反射是程序在运行时检查同时改变对象的构造和行为的能力
从定义可以看出，introspection是反射的子集，反射除了检查，还可以控制改变，一些语言支持反射，一些语言支持反射：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-a1d93bd57cef4ed3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Introspection Example：
instanceof 可以判断一个对象是否属于一个特定class的实例
```
if(obj instanceof Dog){
   Dog d = (Dog)obj;
   d.bark();
}
```

Reflection Example:
Class.forName()方法可以返回根据给定类字符串的类object，我们还可以初始化出一个对象
```
// with reflection
Class<?> c = Class.forName("classpath.and.classname");
Object dog = c.newInstance();
Method m = c.getDeclaredMethod("bark", new Class<?>[0]);
m.invoke(dog);
```

在java中，反射可以理解为加强的Introspection，因为你无法改变一个对象的构造，但是可以改变对象的属性和方法的可见性

# 为什么我们需要反射？
有了反射，我们可以做以下事情：
* 在运行时检查一个对象
* 在运行时，根据一个class构造一个对象
* 在运行时，检查一个对象的属性和方法
* 在运行时，调用一个对象的任意一个方法
* 在运行时，改变对象的构造函数，属性，方法的可见性
* 等等

反射是很多框架的共有的方法：

* 例如JUnit，就是使用反射去找出那些带有@Test注解的方法，然后就利用反射在单元测试中调用这些方法
* 在web框架中，开发人员将他们定义实现的接口和类放到配置文件中，使用反射，他可以动态的在运行时自动初始化这些类和接口
例如，Spring中一般这样使用配置文件：
```
<bean id="someID" class="com.programcreek.Foo">
    <property name="someField" value="someValue" />
</bean>
```
当Spring读取到bean文件的时候，会调用Class.forName(String)方法"com.programcreek.Foo"来初始化这个类，然后在使用反射正确的get到所配置的属性的set方法，并把相应的值set进去。

Servlet web 也是使用这种反射技术：
```
<servlet>
    <servlet-name>someServlet</servlet-name>
    <servlet-class>com.programcreek.WhyReflectionServlet</servlet-class>
<servlet>
```

# 如何使用反射
具体的使用方法细节可以参考java API
下面我们介绍几种简单常用的反射的使用方法

## 从对象获取类名：
```
package myreflection;
import java.lang.reflect.Method;
 
public class ReflectionHelloWorld {
	public static void main(String[] args){
		Foo f = new Foo();
		System.out.println(f.getClass().getName());			
	}
}
 
class Foo {
	public void print() {
		System.out.println("abc");
	}
}
```
Output:
```
myreflection.Foo
```

## 调用位置对象的方法
```
package myreflection;
import java.lang.reflect.Method;
 
public class ReflectionHelloWorld {
	public static void main(String[] args){
		Foo f = new Foo();
 
		Method method;
		try {
			method = f.getClass().getMethod("print", new Class<?>[0]);
			method.invoke(f);
		} catch (Exception e) {
			e.printStackTrace();
		}			
	}
}
 
class Foo {
	public void print() {
		System.out.println("abc");
	}
}
```
output:
```
abc
```

## 从类创建对象
```
package myreflection;
 
public class ReflectionHelloWorld {
	public static void main(String[] args){
		//create instance of "Class"
		Class<?> c = null;
		try{
			c=Class.forName("myreflection.Foo");
		}catch(Exception e){
			e.printStackTrace();
		}
 
		//create instance of "Foo"
		Foo f = null;
 
		try {
			f = (Foo) c.newInstance();
		} catch (Exception e) {
			e.printStackTrace();
		}	
 
		f.print();
	}
}
 
class Foo {
	public void print() {
		System.out.println("abc");
	}
}
```

## 获取构造函数并创建实例
```
package myreflection;
 
import java.lang.reflect.Constructor;
 
public class ReflectionHelloWorld {
	public static void main(String[] args){
		//create instance of "Class"
		Class<?> c = null;
		try{
			c=Class.forName("myreflection.Foo");
		}catch(Exception e){
			e.printStackTrace();
		}
 
		//create instance of "Foo"
		Foo f1 = null;
		Foo f2 = null;
 
		//get all constructors
		Constructor<?> cons[] = c.getConstructors();
 
		try {
			f1 = (Foo) cons[0].newInstance();
			f2 = (Foo) cons[1].newInstance("abc");
		} catch (Exception e) {
			e.printStackTrace();
		}	
 
		f1.print();
		f2.print();
	}
}
 
class Foo {
	String s; 
 
	public Foo(){}
 
	public Foo(String s){
		this.s=s;
	}
 
	public void print() {
		System.out.println(s);
	}
}
```
output:
```
null
abc
```
同样的，既然可以得到构造函数，那么也可以通过Class instance去获取这个类实现的接口，它的父类，它的属性等等

## 运用反射改变数组的大小
```
package myreflection;
 
import java.lang.reflect.Array;
 
public class ReflectionHelloWorld {
	public static void main(String[] args) {
		int[] intArray = { 1, 2, 3, 4, 5 };
		int[] newIntArray = (int[]) changeArraySize(intArray, 10);
		print(newIntArray);
 
		String[] atr = { "a", "b", "c", "d", "e" };
		String[] str1 = (String[]) changeArraySize(atr, 10);
		print(str1);
	}
 
	// change array size
	public static Object changeArraySize(Object obj, int len) {
		Class<?> arr = obj.getClass().getComponentType();
		Object newArray = Array.newInstance(arr, len);
 
		//do array copy
		int co = Array.getLength(obj);
		System.arraycopy(obj, 0, newArray, 0, co);
		return newArray;
	}
 
	// print
	public static void print(Object obj) {
		Class<?> c = obj.getClass();
		if (!c.isArray()) {
			return;
		}
 
		System.out.println("\nArray length: " + Array.getLength(obj));
 
		for (int i = 0; i < Array.getLength(obj); i++) {
			System.out.print(Array.get(obj, i) + " ");
		}
	}
}
```
output:
```
Array length: 10
1 2 3 4 5 0 0 0 0 0 
Array length: 10
a b c d e null null null null null 
```
# 总结
我们简单的介绍了什么是反射，反射可以用来干什么，如何使用反射等问题，可以对反射有一个大致的了解，具体的概念细节还需要参考更多的资料
