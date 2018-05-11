# 堆栈追踪
想要知道异常的根源，以及多重方法调用下异常的传播，可以利用异常对象自动收集的堆栈的追踪来取得相关信息，例如，调用调用异常对象的printStacktrace()方法。
如下面这个例子
```
package ExceptionNote;

public class StackTraceDemo {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		try {
			c();
		} catch (NullPointerException ex) {
			ex.printStackTrace();
		}
	}
	
	static void c() {
		b();
	}
	
	static void b() {
		a();
	}
	
	static String a() {
		String text = null;
		return text.toUpperCase(null);
	}

}

```
显示的异常信息

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-b9d22a8c1bf089d8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以看到最前面的异常信息是调用方法的最里层，也就是实际发生异常的程序点。

重抛异常的时候，异常的追踪堆栈的起点仍是异常发生的根源，而不是重抛的异常的地方，露下面这个例子
```
package ExceptionNote;

public class StackTraceDemo2 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		try {
			c();
		} catch (NullPointerException ex) {
			ex.printStackTrace();
		}
	}
	
	static void c() {
		try {
			b();
		} catch (NullPointerException ex) {
			ex.printStackTrace();
                        throw ex;
		}
	}
	
	static void b() {
		a();
	}
	
	static String a() {
		String text = null;
		return text.toUpperCase(null);
	}

}

```
异常追踪堆栈的起点仍是异常发生的根源。
想要定位到重抛异常的地方，需要使用fillInStackTrace()方法。
像下面这样：
```
package ExceptionNote;

public class StackTraceDemo2 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		try {
			c();
		} catch (NullPointerException ex) {
			ex.printStackTrace();
		}
	}
	
	static void c() {
		try {
			b();
		} catch (NullPointerException ex) {
			ex.printStackTrace();
			Throwable t = ex.fillInStackTrace();
			
			throw (NullPointerException)t;
		}
	}
	
	static void b() {
		a();
	}
	
	static String a() {
		String text = null;
		return text.toUpperCase(null);
	}

}

```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-c88081ce331a4c4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# finally
当我们在写程序的时候，比如打开一个文件输入流，通常要关闭流，但如果在关闭流之前出现了异常，那么可能来不及关闭流，程序就发生异常中止，这样容易导致某些资源没有被正确的关闭，为了解决这个问题，trycatch语句还有一个finally关键字，它的作用就是不管你发没发生异常，都会执行最后finnally语句块里的代码，比如下面这个例子
```
package ExceptionNote;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class FileUtil2 {
	public static String readFile(String name) throws FileNotFoundException {
		StringBuilder text = new StringBuilder();
		Scanner console = null;
		try {
			console = new Scanner(new FileInputStream(name));
			while(console.hasNext()) {
				text.append(console.nextLine()).append('\n');
			}
		} catch (FileNotFoundException ex) {
			ex.printStackTrace();
			throw ex;
		}
		finally {
			if(console != null)
				console.close();
		}
		return text.toString();
	}
}

```

# 自动尝试关闭资源语法
jdk7之后为了方便，新增了尝试关闭资源语法，如示例
```
package IO;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStream;
import java.io.Reader;
import java.io.Writer;

public class CharUtil {
	public static void dump(Reader src, Writer dest) throws IOException {
		try(Reader input = new BufferedReader(src);
			Writer output = new BufferedWriter(dest)	) {
			char[] data = new char[1024];
			int length;
			while((length = input.read(data)) != -1) {
				output.write(data, 0, length);
			}
		}
	}

}
```
尝试关闭资源语法就是将想要自动关闭的对象，写在try之后的括号中，如果无需catch处理任何一场，就不用撰写。
