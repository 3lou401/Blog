一个enum就像其他的类一样，可以拥有一系列的实例。
下面我们会举几个简单的例子说明如何使用Java中的enum。

# 实例1
```
package Enum;

public class Test {

	public static void main(String[] args) {
		
		for(Color color : Color.values())
			System.out.println(color.toString());

	}

}

enum Color {
	RED, YELLOW, BLUE;	//each is a instance of color
}
```


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-99af93ab45742d54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 实例2（带构造函数）
```
package Enum;

public class Test {

	public static void main(String[] args) {
		
		for(Color color : Color.values())
			color.printColor();

	}

}

enum Color {
	RED(1), YELLOW(2), BLUE(3);	//each is a instance of color
	
	private int value;
	
	private Color() {}
	
	private Color(int value) {
		this.value = value;
	}
	
        //define instance method
	public void printColor() {
		System.out.println(this.value);
	}
}

```

# 什么时候使用Enum

我们知道Java中的enum的定义是像其他类一样，只是多了一系列预定义的实例。
一个适合的使用场景是：防止不可用参数，例如下面这个例子：
```
public void doSomethingWithColor(int color);
```
我们在使用函数的时候发现这个参数是很模糊的，我们不知道不同的颜色对应什么int值，所以传错参数，但我们如果使用enum，就可以使其变得简单易读：
```
public void doSomethingWithColor(Color color);
```
根据我们上面定义的enum color，我们就可以正确的传入参数，而且可读性也加强了！

原文：http://www.programcreek.com/simple-java/
