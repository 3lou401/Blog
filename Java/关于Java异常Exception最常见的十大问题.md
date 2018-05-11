# 1 受检异常 VS 非受检异常

简单的说，受检异常必须在方法中被显示的捕捉，或者在方法的throws语句中被抛出。
非受检异常是由哪些在程序编译时不能被解决的问题所引起的，常见的有除以0，空指针等等。
受检异常非常重要，因为你希望其他使用你的程序API的开发者知道如何去处理这些异常。例如，IOException是一个使用的很多的受检异常，RuntimeException则是一个最常见的非受检异常。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-f24bfb513229e7b9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 2 异常管理的最佳实践

**如果一个异常能够被正确的处理，那么他就该捕获，反之，则该被抛出**

# 3 为什么在try语句中定义的变量不能在catch和finally语句中使用？

In the following code, the string s declared in try block can not be used in catch clause.
The code does not pass compilation

下面这段代码，string s定义在try语句块中，然后却在catch语句中使用了s，这段程序是无法通过编译的
```
try {
		File file = new File("path");
		FileInputStream fis = new FileInputStream(file);
		String s = "inside";
	} catch (FileNotFoundException e) {
		e.printStackTrace();
		System.out.println(s);
	}
```

原因是因为，我们不知道在try语句块中的exception会在哪里被throw出去，比如这个例子，我们知道如果要抛出FileNotFoundException，也是在头两句代码中，那么如果跑出了异常，异常产生地方，其后的代码都不会被执行，所以s根本不会被声明初始化。这就是为什么try语句中定义的变量不能在catch和finally语句中使用。

# 4 为什么Double.parseDouble(null)和Integer.parseInt(null) throw不同的异常？
They actually throw different exceptions. This is a problem of JDK. They are developed
by different developers, so it does not worth too much thinking

他们确实会抛出不同的异常，按道理他们应该抛出一样的异常，这是JDK自身的一个bug，由于他们是由不同的开发者开发的。这个问题，我们不必过多纠结。
```
Integer.parseInt(null);
// throws java.lang.NumberFormatException: null
Double.parseDouble(null);
// throws java.lang.NullPointerException
```

# 5 Java中最常见的runtime异常，运行时异常

常见的有IllegalArgumentException ArrayIndexOutOfBoundsException等等，如下面这个情况
```
if (obj == null) {
throw new IllegalArgumentException("obj can not be null");
```

# 6 能在同一个catch语句中捕获多个异常么？

答案是可以的。只要这几个异常都属于同一个超类，我们只能使用同一个超类下的多个异常。

# 7 构造方法可以抛出异常么？

答案是可以的！
构造方法只是一种比较特殊的方法，所以，自然而来，他也能像其他方法一样抛出异常。
存在这样一种情况，一些对象已经被创建了而且被分配给静态的成员变量，但这时构造方法还没有执行。这种情况下，我们需要确保一致性。
下面是一个抛出异常的例子
```
class FileReader{
	public FileInputStream fis = null;
 
	public FileReader() throws IOException{
		File dir = new File(".");//get current directory
		File fin = new File(dir.getCanonicalPath() + File.separator + "not-existing-file.txt");
		fis = new FileInputStream(fin);
	}
}
```

# 8 在finally语句中抛出异常
```
public static void main(String[] args) {
		File file1 = new File("path1");
		File file2 = new File("path2");
		try {
			FileInputStream fis = new FileInputStream(file1);
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} finally {
		try {
			FileInputStream fis = new FileInputStream(file2);
		} catch (FileNotFoundException e) {
			e.printStackTrace();
			}
		}
	}
```

# 9 return语句可以在finally语句中使用么？

答案是可以的！

# 10 为什么开发者么总是默默的“消灭”异常？

像下面的代码
```
try {
		...
	} catch(Exception e) {
		e.printStackTrace();
	}
```
我们经常看到下面这种不处理异常的代码，为什么不仔细处理异常呢？
因为懒，虽然这样做是不对的，但这样做很容易，我们应该尽量避免这种写法。

转自[http://www.programcreek.com/simple-java/](http://www.programcreek.com/simple-java/)
