我们通过一个简单的实例程序来了解一下什么是java中的异常处理

# 使用try,catch
看下面这个程序：
```
package ExceptionNote;

import java.util.Scanner;

public class Average {

	public static void main(String[] args) {
		
		Scanner console = new Scanner(System.in);
		double sum = 0;
		int count = 0;
		while(true) {
			int number = console.nextInt();
			if(number == 0)
				break;
			sum += number;
			count++;
		}
		System.out.printf("Average %.2f%n",sum/count);
	}

}

```
这个程序可以让用户输入任意个整数，以0作为结束的标志，最后会显示输入整数的平均值。
下面我们进行简单的测试
* 如果用户正确的输入每个整数，那么自然，程序会顺利显示结果

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-2121e8b7626d246b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 但如果用户输入错误呢，就会出现如下错误信息

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-d92b6632add6547d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

编译器提示main函数中出现了exception异常，异常是inputMismatchException
意思就是输入不符合，因为程序里要求输入int类型，我们却输入了aaa,String类型，所以就引发了InputMismatchexception

Java 中的所有异常错误信息都会被打包成对象，这时就轮到try catch派上用场了。

```
package ExceptionNote;

import java.util.InputMismatchException;
import java.util.Scanner;

public class Average2 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		try {
			Scanner console = new Scanner(System.in);
			double sum = 0;
			int count = 0;
			while(true) {
				int number = console.nextInt();
				if(number == 0)
					break;
				sum += number;
				count++;
			}
			System.out.printf("Average %.2f%n",sum/count);
			
		} catch(InputMismatchException ex) {
			System.out.println("please input the integer");
		}
	}

}

```
加上trycatch语句后，当我们再遇到错误输入时，就会像下面这样

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-f6f158380ac7f8ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
提示我们错误，而不是像之前一样报出一堆错误。

下面我们来分析一下trycatch，JVM会尝试执行try中的代码，如果发生错误，执行的流程会跳离错误的发生点，然后比较catch中的声明的错误类型，是否符合被抛出的错误对象的类型，如果符合就执行catch语句块的程序代码。

# 异常继承架构

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-24e2cb9308b362b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

很多人不理解当这段代码会提示错误

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-c77ba86771cc7cde.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这是因为编译器认为调用这个方法有可能发生错误，要求你一定要在程序中捕捉错误。这时有两种处理这个错误的方法，第一种就是使用之前的trycatch语句捕捉，第二种就是直接在函数的后面throw抛出这个错误。

但是同时问题也来了，之前的Average程序为什么就不强制让我们处理错误呢？

要解决这个问题，首先就得先了解那些错误对象的继承架构。

* 首先我们要了解所有的错误都会被包装成对象，这些错误的对象都继承自java.lang.Throwable类，Throwable类定义了取得错误信息，堆栈追踪等方法，他有两个子类，java.lang.error和java.lang.exception。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-625dbf30c1ebe7d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

error与其子类实例一般代表严重的系统错误，比如一些硬件层面的错误，ＪＶＭ错误
或者内存错误之类的信息，当然可以用ｔｒｙｃａｔｃｈ来处理，但是不建议这么做，因为当发生严重的系统错误时，程序本身是无力回天的。

** 如果抛出了throwable对象，而程序中没有任何catch捕捉到错误对象，最后由JVM捕捉到的话，那么JVM基本处理就是显示错误对象的打包信息并且中断程序。**

## 受检对象（checked exception）
Exception或其子对象，但非属于RunTimeException或其子对象，就是受检对象。意思就是受编译器检查的对象。这样做的目的是，在于API设计实现者要求实现某方法的时候，某些条件成立时会引发错误，而且认为调用方法的客户端有能力处理错误，要求编译程序提示客户端必须明确处理错误，不然不可以通过编译。

属于RuntimeException的衍生出来的类实例，代表API设计者实现某方法时，条件时会引发错误，需要好好检查，也叫做非受检异常。

同时还要注意捕捉异常对象的顺序，如果父类异常在子类异常之前，那么显然子类异常永远也不会被捕捉到。

# 该抓还是该抛
下面有一个例子，读取纯文本文档
```
package ExceptionNote;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class FileUtil {
	public static String readFile(String name)  {
		StringBuilder text = new StringBuilder();
		try {
			Scanner console = new Scanner(new FileInputStream(name));
			while(console.hasNext()) {
				text.append(console.nextLine()).append('\n');
			}
		} catch (FileNotFoundException ex) {
			ex.printStackTrace();
		}
		return text.toString();
	}
}

```

问题来了，如果这个函数是使用在web网站上，那么错误将会显示在控制台，web用户怎么看得到呢？所以直接在catch写死处理异常或输出错误信息并不符合需求。

这时候就可以考虑抛出异常，如果方法设计流程中发生异常，而你设计时并没有充足的信息知道该如何处理异常，就可以抛出异常，让调用方法的客户端来处理。

实际上可以同时使用try catch进行一部分的异常处理，剩下无法处理的可以再次抛出
```
package ExceptionNote;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class FileUtil {
	public static String readFile(String name) throws FileNotFoundException {
		StringBuilder text = new StringBuilder();
		try {
			Scanner console = new Scanner(new FileInputStream(name));
			while(console.hasNext()) {
				text.append(console.nextLine()).append('\n');
			}
		} catch (FileNotFoundException ex) {
			ex.printStackTrace();
			throw ex;
		}
		return text.toString();
	}
}

```
切记如果抛出的是受检异常，必须在方法上使用throws声明，如果是非受检异常则不用。
