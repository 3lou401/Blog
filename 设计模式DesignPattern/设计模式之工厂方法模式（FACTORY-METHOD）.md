**工厂方法模式定义了一个创建对象的接口，但由子类决定要实例化的类是哪一个。工厂方法让类把实例化推迟到子类。**
我们依然接着简单工厂模式提出的披萨店问题继续探讨

# 问题模拟
我们假设有多种不同的pizza店，比如纽约的pizza点，芝加哥的pizza店，他们都有自己制作的不同种类的pizza。
如果我们采用简单模式方法，那么我们就需要分别建立纽约pizzafactory和芝加哥的factory等等工厂，但这样做没有弹性。我们能不能将制作pizza的行为局限在旁pizzaStore类中，但同时又能让不同类的点去各自实例化自己的pizza类。
显然，我们可以将pizzaStore由一个具体类，变为一个抽象的接口：
```
public abstract class PizzaStore {
    public Pizza orderPizza(String type) {
      Pizza pizza;
      pizza=createPizza(type);
      pizza.prepare();
      pizza.bake();
      pizza.cut();
      pizza.box();
      return pizza;
  }
  abstract Pizza createPizza(String type);
}
```


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-364ba1e6c2737a6b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

定义了一个抽象的基类，里面有个抽象的create方法，我们让其他的纽约地区，芝加哥地区等等不同的继承自这个基类，让子类自己决定怎么创建pizza。

```
public class NYPizzaStore extends PizzaStore {
Pizza createPizza(String item) {
if (item.equals(“cheese”)) {
return new NYStyleCheesePizza();
} else if (item.equals(“veggie”)) {
return new NYStyleVeggiePizza();
} else if (item.equals(“clam”)) {
return new NYStyleClamPizza();
} else if (item.equals(“pepperoni”)) {
return new NYStylePepperoniPizza();
} else return null;
}
}
```

以上是我们实现的一个具体的纽约pizzastore类。他继承实现了基类的抽象方法。

然后我们继承实现抽象的pizza类和具体的pizza类
```
public abstract class Pizza {
String name;
String dough;
String sauce;
ArrayList toppings = new ArrayList();
void prepare() {
System.out.println(“Preparing “ + name);
System.out.println(“Tossing dough...”);
System.out.println(“Adding sauce...”);
System.out.println(“Adding toppings: “);
for (int i = 0; i < toppings.size(); i++) {
System.out.println(“ “ + toppings.get(i));
}
}
void bake() {
System.out.println(“Bake for 25 minutes at 350”);
}
void cut() {
System.out.println(“Cutting the pizza into diagonal slices”);
}
void box() {
System.out.println(“Place pizza in official PizzaStore box”);
}
public String getName() {
return name;
}
}
```
具体的产品类
```
public class NYStyleCheesePizza extends Pizza {
public NYStyleCheesePizza() {
name = “NY Style Sauce and Cheese Pizza”;
dough = “Thin Crust Dough”;
sauce = “Marinara Sauce”;
toppings.add(“Grated Reggiano Cheese”);
}
}
```

```
public class NYStyleCheesePizza extends Pizza {
public NYStyleCheesePizza() {
name = “NY Style Sauce and Cheese Pizza”;
dough = “Thin Crust Dough”;
sauce = “Marinara Sauce”;
toppings.add(“Grated Reggiano Cheese”);
}
}

public class ChicagoStyleCheesePizza extends Pizza {
public ChicagoStyleCheesePizza() {
name = “Chicago Style Deep Dish Cheese Pizza”;
dough = “Extra Thick Crust Dough”;
sauce = “Plum Tomato Sauce”;
toppings.add(“Shredded Mozzarella Cheese”);
}
void cut() {
System.out.println(“Cutting the pizza into square slices”);
}
}
```

最后测试我们的代码：
```
public class PizzaTestDrive {
public static void main(String[] args) {
PizzaStore nyStore = new NYPizzaStore();
PizzaStore chicagoStore = new ChicagoPizzaStore();
Pizza pizza = nyStore.orderPizza(“cheese”);
System.out.println(“Ethan ordered a “ + pizza.getName() + “\n”);
pizza = chicagoStore.orderPizza(“cheese”);
System.out.println(“Joel ordered a “ + pizza.getName() + “\n”);
}
}
```

# 工厂方法模式分析

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-0c64c3bb48fb1591.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

工厂方法模式常常分为两大类：一个创建产品的创建类，一个产品类，其中创建类定义一个抽象的接口，外加其余的继承自他的具体实现。产品类也是类似，定义一个抽象的产品类接口，具体的产品类实现继承自基类。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-e2b4ed1e7636bc82.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-99fa506ad2953021.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

把创建对象的代码集中在一个对象或者方法中，可以避免代码的重复，并且更方便的以后的维护，这意味着客户在实例化对象的时候，依赖的是接口，而不是具体的对象，而正是我们之前提到的具体设计原则中的一种，针对接口编程。

# 依赖倒置原则
这是我们提出的又一设计原则：**要依赖抽象，不要依赖具体实现 **
工厂方法模式中，就很好的应用这个原则：
 如果我们采用简单工厂模式，依赖关系是这样的：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-7a8dc42a31e30044.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而采用工厂方法模式，依赖关系如图：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-a4937bc2aed5d7d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这就是依赖倒置原则，高层组件依赖了底层的pizza组件！

# 小结
工厂方法模式对简单工厂模式进行了抽象。有一个抽象的Factory类（可以是抽象类和接口），这个类将不再负责具体的产品生产，而是只制定一些规范，具体的生产工作由其子类去完成。在这个模式中，工厂类和产品类往往可以依次对应。即一个抽象工厂对应一个抽象产品，一个具体工厂对应一个具体产品，这个具体的工厂就负责生产对应的产品。

工厂方法经常用在以下两种情况中:
* 第一种情况是对于某个产品，调用者清楚地知道应该使用哪个具体工厂服务，实例化该具体工厂，生产出具体的产品来。Java Collection中的iterator() 方法即属于这种情况。
* 第二种情况，只是需要一种产品，而不想知道也不需要知道究竟是哪个工厂为生产的，即最终选用哪个具体工厂的决定权在生产者一方，它们根据当前系统的情况来实例化一个具体的工厂返回给使用者，而这个决策过程这对于使用者来说是透明的。
