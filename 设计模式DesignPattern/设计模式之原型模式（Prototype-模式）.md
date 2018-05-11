> * 引入原型模式
* 原型模式的实例
* 为什么需要使用原型模式

# 引入原型模式
如果读者很熟悉javascript的话，对原型这个词应该不会陌生。

原型是用来生成实例的，但不是利用类来生成实例，而是通过实例来生成实例。

## 为什么我们需要用过类来生成实例呢？
联想到浏览器中，如果我们生成了一个button实例，这个button实例经过一系列操作，携带了各种信息，比如button加颜色，加背景图，加文字，加事件等等。如果我们这时候需要和这个button实例完全一样的一个实例，仅仅通过类new 一个button出来是远远不够的，因为我们还要对它进行一系列的操作，所以这个生成一个完全一样的实例的过程是非常复杂的，所以这时候我们就想到可不可以直接根据这个实例，然后生成一个一模一样的实例呢？

实际上，这就是原型模式的基本思想，根据实例原型和实例模式来生成新的实例。

介绍完基本思想后，下面我们就通过一个实例来具体理解一下原型实例。

我们实现一段将字符串加上方框中打印出来或者是加入下划线显示出来。

Java中要实现原型模式，也就是实例的复制，我们可以直接利用clone方法，需要实现cloneable接口。

# 原型模式的实例
我们先定义一个接口，Product接口，这个接口是复制功能的接口，继承了cloneable接口
```
public interface Product extends Cloneable {
    public abstract void use(String s);
    public abstract Product createClone();
}
```
createClone是用来复制实例的方法，use方法是用来使用实例方法的。这些方法具体都交给子类去实现。

我们实现一个具体的子类，MessageBox，它的作用是可以给字符创添加类似方框的边界的图案。
这个类实现了product接口，createClone是用于复制自己，生成一个新的一模一样的实例，也就是原型模式的思想。use方法将结果显示出来。
```
public class MessageBox implements Product {
    private char decochar;
    public MessageBox(char decochar) {
        this.decochar = decochar;
    }
    public void use(String s) {
        int length = s.getBytes().length;
        for (int i = 0; i < length + 4; i++) {
            System.out.print(decochar);
        }
        System.out.println("");
        System.out.println(decochar + " "  + s + " " + decochar);
        for (int i = 0; i < length + 4; i++) {
            System.out.print(decochar);
        }
        System.out.println("");
    }
    public Product createClone() {
        Product p = null;
        try {
            p = (Product)clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return p;
    }
}
```

同样的我们实现可以给字符串添加下划线的类UnderlinePen
这个类同样实现了product接口，并且可以复制自己生成一个新的实例。

```
public class UnderlinePen implements Product {
    private char ulchar;
    public UnderlinePen(char ulchar) {
        this.ulchar = ulchar;
    }
    public void use(String s) {
        int length = s.getBytes().length;
        System.out.println("\""  + s + "\"");
        System.out.print(" ");
        for (int i = 0; i < length; i++) {
            System.out.print(ulchar);
        }
        System.out.println("");
    }
    public Product createClone() {
        Product p = null;
        try {
            p = (Product)clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return p;
    }
}
```

最后我们新建一个管理类Manager来管理这些对象的创建，复制和使用。
Manager类中有一个map字段，用来存储product对象。create方法根据对象的名字在map找到对象的实例，然后调用实例的createClone方法，从而复制出一个实例。

```
public class Manager {
    private HashMap showcase = new HashMap();
    public void register(String name, Product proto) {
        showcase.put(name, proto);
    }
    public Product create(String protoname) {
        Product p = (Product)showcase.get(protoname);
        return p.createClone();
    }
}
```

我们来测试一下：
```
public class Main {
    public static void main(String[] args) {

        Manager manager = new Manager();
        UnderlinePen upen = new UnderlinePen('~');
        MessageBox mbox = new MessageBox('*');
        MessageBox sbox = new MessageBox('/');
        manager.register("strong message", upen);
        manager.register("warning box", mbox);
        manager.register("slash box", sbox);

        Product p1 = manager.create("strong message");
        p1.use("Hello, world.");
        Product p2 = manager.create("warning box");
        p2.use("Hello, world.");
        Product p3 = manager.create("slash box");
        p3.use("Hello, world.");
    }
}
```
运行结果：


![image.png](http://upload-images.jianshu.io/upload_images/1234352-10194f66b28ca3d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


上述代码一个简易的类图：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-aa13ce1b23d6889f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 小结
下面我们来总结Prototype原型模式

首先，我们给出Prototype原型模式的类图

![image.png](http://upload-images.jianshu.io/upload_images/1234352-dfc1d8b2830b4d34.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通常会有一个原型接口或者类，定义clone方法，就像此例中的Product接口，然后会有具体的原型实例去实现或者继承自这个接口，就像这个例子中的UnderlinePen类和MessageBox

学到这里我们基本把原型模式讲完了。

# 为什么需要使用原型模式
但读者可能还能会有疑问，我们直接通过类new出一个实例不就可以了，为什么要搞这么复杂？
new Sth（）;
在本文的开头，我们就举了需要用到原型模式的例子，现在我们结合实例程序再来谈一下：

* 对象总类繁多，无法将他们整合到一个类中
例如例子中的有用‘~’作为下划线，有的用‘*’或者‘/’。
本例比较简单，只生成了三种样式，不过想要做，不论多少种样式都可以实现，但是如果为每个样式实现一个类，就会变得非常复杂。类的数量会非常多。

* 难以根据类生成实例
本例中可能感觉不到这一点。大家可以试想一下开发一个用户可以使用鼠标操作的，类似于图形编辑器的应用，假如我们想生成一个和用户通过一系列鼠标点击创建出来的实例，这个时候，显然无法根据类来生成实例，会变的非常复杂，但我们可以采取原型模式，根据实例生成实例，直接复制出一个实例就可以了。
