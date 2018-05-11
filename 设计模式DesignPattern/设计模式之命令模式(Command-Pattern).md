“**行为请求者**”与“**行为实现者**”通常呈现一种“**紧耦合**”。比如要对行为进行“记录、撤销/重做、事务”等处理，这种无法抵御变化的紧耦合是不合适的。在这种情况下，如何将“行为请求者”与“行为实现者”解耦？将**一组行为抽象为对象**，**实现二者之间的松耦合**。这就是**命令模式（Command Pattern）。
 **
***
仔细看这个定义，我们知道一个命令对象通过在特定接受者上绑定一组动作来封装一个请求。要达到这一点，命令对象将动作和接受者抱紧对象中。这个对象只暴露出一个execute方法。当此方法被调用时，接收者就会进行这些动作，从外面来看，其他对象不知道接受者究竟哪个接受者进行了哪些动作，只知道如果调用execute方法，请求的目的就能达到。

要形象的说明命令模式，最好的方法就是模拟遥控器这个例子！

我们知道当我们使用遥控器，只需要按下每个命令按钮，电视机就会自动执行我们想要的动作，而我们不需要知道这些这些动作是怎么执行的以及究竟是谁执行的。

我们通过代码模拟遥控器的开关灯泡功能。

我们先定义一个Command接口，它封装了一个方法，所有的命令都需要实现自己的execute方法。
```
public interface Command {
	public void execute();
}
```
实现关灯命令的代码，它的execute方法直接调用了light对象的关灯方法
```
public class LightOffCommand implements Command {
	Light light;
	
	public LightOffCommand(Light light) {
		this.light = light;
	}
	
	@Override
	public void execute() {
		light.on();		
	}
}
```
同样我们可以实现开灯命令
```
public class LightOnCommand implements Command {

	Light light;
	
	public LightOnCommand(Light light) {
		this.light = light;
	}
	
	@Override
	public void execute() {
		light.on();		
	}

}
```
我们再实现实体的light类
```
public class Light {
	public void on() {
		System.out.println("light is on!");
	}
	
	public void off() {
		System.out.println("light is off!");
	}
}
```
最后我们实现遥控器类，并测试遥控器的模拟功能
```
public class SimpleRemoteControl {
	
	Command slot;
	
	public void setCommand(Command command) {
		slot = command;
	}
	
	public void btnWasPressed() {
		slot.execute();
	}
	
	public static void main(String args[]) {
		Light light = new Light();
		SimpleRemoteControl remote = new SimpleRemoteControl();
		LightOnCommand lightOn = new LightOnCommand(light);
		remote.setCommand(lightOn);
		remote.btnWasPressed();
	}
}
```

我们聚焦于测试代码，首先我们创建一个实体对象light，它具有自己的开启关闭的方法。我们将light对象传入，创建一个关灯的命令，这个命令对象封装了开灯的方法，然后我们创建一个遥控器对象，将开灯命令对象加载到遥控器对象上，最后我们只要在遥控器上触发相应的动作就可以实现开灯的动作。

这个简单的例子就很好的模拟了命令模式的原理。
