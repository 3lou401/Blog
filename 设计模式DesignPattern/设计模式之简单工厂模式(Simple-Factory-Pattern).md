简单工厂模式其实并不是一个设计模式，反而像是一种比较常用的编程习惯！他还有个名字叫静态工厂方法（Static Factory Method）模式。

简单工厂模式应该是工厂模式家族中最简单的一种模式，同时也是很常用的一种模式。

我们一如既往的通过实际问题的模拟来学习简单工厂模式！

# 问题引出
假设我们要开一家披萨店，我们需要实现一个orderPizza的方法，根据不同的pizza种类。
大致代码如下：
```
Pizza orderPizza(String type) {
	Pizza pizza;
	
	if(type.equals("cheese")) {
		pizza = new CheesePizza();
	} else if (type.equals("greek")) {
		pizza = new GreekPizza();
	} else if (type.equals("cock")) {
		pizza = new CockPizza();
	}
	
	pizza.prepare();
	pizza.bake();
	pizza.cut();
	pizza.box();
	
	return pizza;
}
```
其中这部分代码都是在实现new一个pizza的工作，根据不同的种类
```
	Pizza pizza;
	
	if(type.equals("cheese")) {
		pizza = new CheesePizza();
	} else if (type.equals("greek")) {
		pizza = new GreekPizza();
	} else if (type.equals("cock")) {
		pizza = new CockPizza();
	}
```
很显然，这样是存在问题的，如果我们要增加一个pizza种类或者减少一个pizza种类，我们就要重新修改orderPizza方法。无法实现对修改关闭。

同时，我们意识到，new出一个pizza对象之后，我们之后所做的工作：
```
	pizza.prepare();
	pizza.bake();
	pizza.cut();
	pizza.box();
```
都是一致的！这一部分是不变的部分。所以我们考虑可以将变与不变的部分提取出来，达到封装的目的。

# 建立简单pizza工厂

我们将会变化的new对象的部分代码提取出来，将其用另一个对象封装起来，这个对象根据它的作用就是创建一个pizza，我们就称其为factory也就是工厂。
这样的话，orderPizza方法就变成工厂对象的客户，当需要一个pizza对象时，就像工厂对象请求做一个出来，这就是简单工厂的设计思想。

```
public class SimplePizzaFactory {
	public Pizza createPizza(String type) {
		Pizza pizza = null;
		
		if(type.equals("cheese")) {
			pizza = new CheesePizza();
		} else if (type.equals("greek")) {
			pizza = new GreekPizza();
		} else if (type.equals("cock")) {
			pizza = new CockPizza();
		}
		
		return pizza;
	}
}
```
这样就实现了简单工厂模式，简单工厂对象可以同时为多个pizza店提供pizza，所以当出现变化时，我们只需要修改这个类即可！

# 重构PizzaStore类
```
public class PizzaStore {
	SimplePizzaFactory factory;
	
	public PizzaStore(SimplePizzaFactory factory) {
		this.factory = factory;
	}
	
	Pizza orderPizza(String type) {
	Pizza pizza;
	
	pizza = factory.createPizza(type);
	
	pizza.prepare();
	pizza.bake();
	pizza.cut();
	pizza.box();
	
	return pizza;
}
}
```
看看我们设计的简单工厂模式的类图：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-5f83cca87de90609.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

PizzaStore是工厂的客户，他通过工厂对象取得pizza实例，pizza是工厂对象的产品，由工厂对象来完成实例化pizza实例的工作。

# 小结

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-7a92bddc5e7d7068.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

简单工厂模式严格的说不是一种设计模式，而是一种编程习惯，他的核心思想就是，将会发生变化的实例化代码抽离出来，新建一个简单工厂类将其封装起来！
