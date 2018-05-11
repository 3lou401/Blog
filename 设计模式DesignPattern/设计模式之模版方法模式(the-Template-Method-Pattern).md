> ** The Template Method Pattern ** defines the skeleton of an algorithm in a method, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm’s structure.
** 模版方法模式 ** 在一个方法中定义了算法的基本框架和结构，然后将部分具体的步骤留给子类去具体实现。模版方法模式允许子类去自定义自己的算法的特定的步骤，但是又不改变整体的算法的结构，这样就可以实现代码的复用。

下面我们就通过一个简单的实例来讲讲什么是模版方法模式？

假设我们需要制作一杯咖啡和一杯茶，首先我们看看制作咖啡的几个简单的步骤：
* boilWater
* brewCoffeeGrinds
* pourInCup
* addSugarAndMilk

同时我们看看制作一杯茶所需要的步骤：
* boilWater
* steepTeaBag
* pourInCup
* addLemon

我们发现制作茶和咖啡的基本步骤有两步是一样的，如果我们直接实现，也就是各实现各的算法，那么显然，会产生重复的代码，也就是boilWater和pourInCup是重复的。
```
public class Coffee {
void prepareRecipe() {
boilWater();
brewCoffeeGrinds();
pourInCup();
addSugarAndMilk();
}
public void boilWater() {
System.out.println(“Boiling water”);
}
public void brewCoffeeGrinds() {
System.out.println(“Dripping Coffee through fi lter”);
}
public void pourInCup() {
System.out.println(“Pouring into cup”);
}
public void addSugarAndMilk() {
System.out.println(“Adding Sugar and Milk”);
}
}
```
```
public class Tea {
void prepareRecipe() {
boilWater();
steepTeaBag();
pourInCup();
addLemon();
}
public void boilWater() {
System.out.println(“Boiling water”);
}
public void steepTeaBag() {
System.out.println(“Steeping the tea”);
}
public void addLemon() {
System.out.println(“Adding Lemon”);
}
public void pourInCup() {
System.out.println(“Pouring into cup”);
}
}
```
我们显然可以想到一个简单的方法就是设计一个超类将重复的两个方法封装起来，这样子类就不用实现了，只需要继承超类的方法即可。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-6df648e10a278d00.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但这样其实还不够好，我们仔细观察，可以发现，制作茶和咖啡不同的两个步骤，我们可以抽象为超类中的一个方法，并设置为抽象方法，让子类去对应的实现自己的相应方法，这样就更好的封装和复用。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-4355ee4e70a09f73.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们定义一个超类：
```
public abstract class CaffeineBeverage {
final void prepareRecipe() {
boilWater();
brew();
pourInCup();
addCondiments();
}
abstract void brew();
abstract void addCondiments();
void boilWater() {
System.out.println(“Boiling water”);
}
void pourInCup() {
System.out.println(“Pouring into cup”);
}
}
```
我们可以看到我们将算法的步骤封装在prepareRecipe，为了不让子类去修改这个方法，我们设置为final，同时对于子类公用的方法，我们直接在超类中实现，这样子类直接调用即可，对于需要不同实现的特定方法，我们在超类中定义一个抽象方法，让子类去自己实现。
这样子类的代码就变的很简洁，因为只要实现两个抽象方法
```
public class Tea extends CaffeineBeverage {
public void brew() {
System.out.println(“Steeping the tea”);
}
public void addCondiments() {
System.out.println(“Adding Lemon”);
}
}
```
```
public class Coffee extends CaffeineBeverage {
public void brew() {
System.out.println(“Dripping Coffee through filter”);
}
public void addCondiments() {
System.out.println(“Adding Sugar and Milk”);
}
}
```

> 模版方法模式定义了算法的步骤，同时允许子类去自定义的实现其中的一个或者多个步骤

模版方法模式为一个算法创造一个实现的模版。什么是模版呢？
就是一个算法需要实现的一系列步骤，这些步骤可以分别用一系列的方法封装起来。在模版方法中，部分这些方法定义为抽象的，由具体的子类去实现，这样就保证了虽然可能实现不同，但是整体的算法框架是不变的，都需要经过相同的步骤。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-1808e16ba1a2a57b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们考虑一种情况，即有时候，子类可能并不需要实现模版方法中定义的全部方法，可能其中一个方法，有的子类需要实现，有的子类却不需要实现，那么我们该如何解决这样的需求问题呢？
** 使用hook **
```
public abstract class CaffeineBeverageWithHook {
final void prepareRecipe() {
boilWater();
brew();
pourInCup();
if (customerWantsCondiments()) {
addCondiments();
}
}
abstract void brew();
abstract void addCondiments();
void boilWater() {
System.out.println(“Boiling water”);
}
void pourInCup() {
System.out.println(“Pouring into cup”);
}
boolean customerWantsCondiments() {
return true;
}
}
```
即增加一个方法，通常返回bool类型的变量对是否使用进行判断。我们形象的称它为钩子，hook
其中的子类一种实现的例子，如下：
```
public class CoffeeWithHook extends CaffeineBeverageWithHook {
public void brew() {
System.out.println(“Dripping Coffee through filter”);
}
public void addCondiments() {
System.out.println(“Adding Sugar and Milk”);
}
public boolean customerWantsCondiments() {
String answer = getUserInput();
if (answer.toLowerCase().startsWith(“y”)) {
return true;
} else {
return false;
}
}
private String getUserInput() {
String answer = null;
System.out.print(“Would you like milk and sugar with your coffee (y/n)? “);
BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
try {
answer = in.readLine();
} catch (IOException ioe) {
System.err.println(“IO error trying to read your answer”);
}
if (answer == null) {
return “no”;
}
return answer;
}
}
```
这样用户可以更灵活的控制，通过钩子是否调用其中的某个方法。

hook就是起到这样的作用，它使得子类的可以更灵活的选择某些超类模版方法中的方法。

策略模式和模版方法模式在某些程度上是很相似的，但策略模式是为了避免继承，采用接口，组合的形式，而模版方法模式是通过继承实现的
同时，沃恩也可以发现，工厂模式其实就是模版方法模式的一种，特殊的模版方法模式，专用于创建新的对象。
