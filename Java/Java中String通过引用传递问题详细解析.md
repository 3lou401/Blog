This is a classic question of Java. Many similar questions have been asked on stackoverflow, and there are a lot of incorrect/incomplete answers. The question is simple
if you don’t think too much. But it could be very confusing, if you give more thought
to it.

这是一个非常经典的问题，许多类似的问题在stackoverflow上被提问，有很多不正确或者不完整的回答。如果你不考虑那么多，直接认为string是immutable的，那问题就很简单，如果你想要了解更多细节，问题就变的很复杂。

我们看下面这个非常容易混淆，同时又很经典的代码
```
public static void main(String[] args) {
String x = new String("ab");
change(x);
System.out.println(x);
}
public static void change(String x) {
x = "cd";
}
```
我们可能以为会输出cd，实际上输出的是ab

这是一个很容易混淆的问题，我们来看一下一个貌似很合理的解释：

x stores the reference which points to the "ab" string in the heap. So when x is passed
as a parameter to the change() method, it still points to the "ab" in the heap like the
following:
变量x存储的是引用，这个引用指向堆上的string“ab”，如下图所示：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-171fc2093d04468c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以x传递给change方法的x参数，然后新建一个string“cd”,然后x有指向这个新建的cd变量

![image.png](http://upload-images.jianshu.io/upload_images/1234352-f52f962c9d0329b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样的解释看上去一点问题没有，但为什么输出的结果又不对呢？

真正的代码执行过程应该是这样的：

When the string "ab" is created, Java allocates the amount of memory required to
store the string object. Then, the object is assigned to variable x, the variable is actually
assigned a reference to the object. This reference is the address of the memory location
where the object is stored.
当string变量‘ab’被创建出来的之后，java分配一块足够大小的内存去存储这个string对象，这个对象被分配给变量x，这个变量x实际上存储的是这个对象在内存中的地址。
The variable x contains a reference to the string object. x is not a reference itself! It
is a variable that stores a reference(memory address).
Java is pass-by-value ONLY. When x is passed to the change() method, a copy of
value of x (a reference) is passed. The method change() creates another object "cd" and
it has a different reference. It is the variable x that changes its reference(to "cd"), not
the reference itself.
这个变量x包含一个这个string对象的一个引用。切记 ** x不是引用本身 **，x只是一个变量存储了一个引用（这个引用其实就是内存的地址）。
**java只通过value传递**当x被传递给change方法的时候。会将x的一份拷贝传递给change方法中的局部变量x，这是另外一个x，虽然这个x存储的引用也就是地址的值是一样的，待会就被改变了，change方法新建一个对象“cd”,是局部变量里的x指向这个新建cd，所以原本的x还是指向ab。

![image.png](http://upload-images.jianshu.io/upload_images/1234352-433b61826165d632.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们可以测试其他引用类型的传递，会发现他们实际上都是通过值传递的，会在方法里新建一个引用，当我们对这个引用指向一个新对象时就要注意了
```
import java.util.ArrayList;
import java.util.List;

public class StringTest {

	public static void main(String[] args) {
		String x = new String("ab");
		change(x);
		System.out.println(x);
		
		Integer y = new Integer(5);
		change(y);
		System.out.println(y);
		
		List<Integer> a = new ArrayList<Integer>();
		
		a.add(1);
		a.add(4);
		change(a);
		
		System.out.println(a);
	}
	
	public static void change(String x) {
		x = "cd";
		}
	
	public static void change(Integer y) {
		y = 4;
	}
	
	public static void change(List<Integer> a) {
		//a.add(5);
		a = new ArrayList<Integer>();
		a.add(2);
		
	}
}

```
输出结果

![image.png](http://upload-images.jianshu.io/upload_images/1234352-e129f16d0306fe3d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

因为我们在方法中都是新建一个对象，所以局部变量的引用都改变了，无法改变原有的值，所以我们看到三个change方法都没有起到作用。

> 当我们向方法参数传递一个引用的时候要记住是传递的引用的值，而不是引用本身，当我们不让这个引用指向一个新对象的时候，不会出现问题，当我们在方法中将局部的引用赋给一个new出来的对象，那么我们要切记，这时候这个引用已经指向另一个对象了，它所操作的都不会反映在原有的对象上。

那么我们如何解决上面那个问题呢？
其实很简单，只要不在方法里新建一个对象就行了。保持方法中的那个局部变量的引用也在原有对象上操作
```
public static void main(String[] args) {
StringBuilder x = new StringBuilder("ab");
change(x);
System.out.println(x);
}

public static void change(StringBuilder x) {
x.delete(0, 2).append("cd");
}
```

> 我们总结一个关键的问题，Java中没有真正的按引用传递，所有变量都是按值value传递的，引用也是变量，只不过它的值是存的对象的地址。所以引用类型的变量在参数的传递过程中，也会新建一个局部变量，局部变量会得到和引用变量一样的值，也就是指向同一个对象。
