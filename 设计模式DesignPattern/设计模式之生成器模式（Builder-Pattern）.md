>生成器模式的核心是 ** 当构建生成一个对象的时候，需要包含多个步骤，虽然每个步骤具体的实现不同，但是都遵循一定的流程与规则 **

举个例子，我们如果构建生成一台电脑，那么我们可能需要这么几个步骤
* 需要一个主机
* 需要一个显示器
* 需要一个键盘
* 需要一个鼠标
* 需要音响等

虽然我们具体在构建一台主机的时候，每个对象的实际步骤是不一样的，比如，有的对象构建了i7cpu的主机，有的对象构建了i5cpu的主机，有的对象构建了普通键盘，有的对象构建了机械键盘等。
但不管怎样，你总是需要经过一个步骤就是构建一台主机，一台键盘。
对于这个例子，我们就可以使用生成器模式来生成一台电脑，他需要通过多个步骤来生成。

所以，我们可以将生成器模式理解为，假设我们有一个对象需要建立，这个对象是由多个组件（Component）组合而成，每个组件的建立都比较复杂，但运用组件来建立所需的对象非常简单，所以我们就可以将构建复杂组件的步骤与运用组件构建对象分离，使用builder模式可以建立。

生成器模式的类图如下：


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-dafed33b138cb013.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下面我们就根据这个例子来实现一个生成器模式，生成一台电脑

首先我们需要一个电脑类：
```
package Builder;

public class Computer {
	public String master;
	public String screen;
	public String keyboard;
	public String mouse;
	public String audio;
	public void setMaster(String master) {
		this.master = master;
	}
	public void setScreen(String screen) {
		this.screen = screen;
	}
	public void setKeyboard(String keyboard) {
		this.keyboard = keyboard;
	}
	public void setMouse(String mouse) {
		this.mouse = mouse;
	}
	public void setAudio(String audio) {
		this.audio = audio;
	}
}
```

然后我们建立一个抽象的builder类：
```
package Builder;

public abstract class ComputerBuilder {
	
	protected Computer computer;
	
	public Computer getComputer() {
		return computer;
	}
	
	public void buildComputer() {
		computer = new Computer();
		System.out.println("生成了一台电脑！！！");
	}

	public abstract void buildMaster();
	public abstract void buildScreen();
	public abstract void buildKeyboard();
	public abstract void buildMouse();
	public abstract void buildAudio();
}
```
然后我们实现两个具体的builder类，分别是惠普电脑的builder和戴尔电脑的builder
```
package Builder;

public class HPComputerBuilder extends ComputerBuilder {

	@Override
	public void buildMaster() {
		// TODO Auto-generated method stub
		computer.setMaster("i7,16g,512SSD,1060");
		System.out.println("(i7,16g,512SSD,1060)的惠普主机");
	}

	@Override
	public void buildScreen() {
		// TODO Auto-generated method stub
		computer.setScreen("1080p");
		System.out.println("(1080p)的惠普显示屏");
	}

	@Override
	public void buildKeyboard() {
		// TODO Auto-generated method stub
		computer.setKeyboard("cherry 青轴机械键盘");
		System.out.println("(cherry 青轴机械键盘)的键盘");
	}

	@Override
	public void buildMouse() {
		// TODO Auto-generated method stub
		computer.setMouse("MI 鼠标");
		System.out.println("(MI 鼠标)的鼠标");
	}

	@Override
	public void buildAudio() {
		// TODO Auto-generated method stub
		computer.setAudio("飞利浦 音响");
		System.out.println("(飞利浦 音响)的音响");
	}
}
```
```
package Builder;

public class DELLComputerBuilder extends ComputerBuilder {
	
	@Override
	public void buildMaster() {
		// TODO Auto-generated method stub
		computer.setMaster("i7,32g,1TSSD,1060");
		System.out.println("(i7,32g,1TSSD,1060)的戴尔主机");
	}

	@Override
	public void buildScreen() {
		// TODO Auto-generated method stub
		computer.setScreen("4k");
		System.out.println("(4k)的dell显示屏");
	}

	@Override
	public void buildKeyboard() {
		// TODO Auto-generated method stub
		computer.setKeyboard("cherry 黑轴机械键盘");
		System.out.println("(cherry 黑轴机械键盘)的键盘");
	}

	@Override
	public void buildMouse() {
		// TODO Auto-generated method stub
		computer.setMouse("MI 鼠标");
		System.out.println("(MI 鼠标)的鼠标");
	}

	@Override
	public void buildAudio() {
		// TODO Auto-generated method stub
		computer.setAudio("飞利浦 音响");
		System.out.println("(飞利浦 音响)的音响");
	}


}
```

然后我们实现一个director类：
```
package Builder;

public class Director {
	
	private ComputerBuilder computerBuilder;

	public void setComputerBuilder(ComputerBuilder computerBuilder) {
		this.computerBuilder = computerBuilder;
	}
	
	public Computer getComputer() {
		return computerBuilder.getComputer();
	}
	
	public void constructComputer() {
		computerBuilder.buildComputer();
		computerBuilder.buildMaster();
		computerBuilder.buildScreen();
		computerBuilder.buildKeyboard();
		computerBuilder.buildMouse();
		computerBuilder.buildAudio();
	}

}
```

最后我们测试一下代码：
```
package Builder;

public class ComputerCustomer {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Director director = new Director();
		
		ComputerBuilder hp = new HPComputerBuilder();
		
		director.setComputerBuilder(hp);
		
		director.constructComputer();
		
		//get the pc
		Computer pc = director.getComputer();
	}
}
```


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-f7b71a87b9f639e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 生成器模式的优缺点

## 优点
* 将一个对象分解为各个组件
* 将对象组件的构造封装起来
* 可以控制整个对象的生成过程

## 缺点
对不同类型的对象需要实现不同的具体构造器的类，这可能回答大大增加类的数量

# 生成器模式的实际应用
Builder pattern has been used in a lot of libraries. However, there is a common mistake here. Consider the following example of StringBuilder which is a class from Java standard library. Does it utilize the Builder pattern?

生成器模式在许多类库中都使用了。但是严格来说，却有些错误。
比如这个例子，我们考虑java标准库中的StringBuilder类，它使用了生成器模式么？

```
StringBuilder strBuilder= new StringBuilder();
strBuilder.append("one");
strBuilder.append("two");
strBuilder.append("three");
String str= strBuilder.toString();
```
在标准库中，StringBuilder继承自AbstractStringBuilder
append方法是这个生成过程中的一步，就像我们构建电脑时，先构建主机这样的步骤一样。
toString方法也是生成过程中的一步，而且是构建过程中的最后一步。然而，这里的不同是没有director，所以严格来说这不是一个标准的生成器模式。我们程序的调用者好像就是director可以生成我们自己的String。

# 生成器模式与工厂模式的不同

生成器模式构建对象的时候，对象通常构建的过程中需要多个步骤，就像我们例子中的先有主机，再有显示屏，再有鼠标等等，生成器模式的作用就是将这些复杂的构建过程封装起来。
工厂模式构建对象的时候通常就只有一个步骤，调用一个工厂方法就可以生成一个对象。
