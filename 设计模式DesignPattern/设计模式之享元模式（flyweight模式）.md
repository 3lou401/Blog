> * 引入享元模式
* 享元模式的实例
* 享元模式的分析

# 引入享元模式
flyweight是轻量级的意思，指的是拳击比赛中选手体重最轻的等级。顾名思义，享元设计模式就是为了是对象更轻。
不过这里的轻的描述与现实中不一样。对于对象来说，重的对象代表对象占有的内存大，轻的对象代表对象内存占用小。
当我们需要大量对象的时候，使用new关键字来分配内存，就会消耗大量的空间。
享元模式和单例模式有点像。
当我们需要某个实例的时候，我们并不是总是new一个新的实例出来，而是看看是不是内存中已经有相应的实例了，如果已经有了，就直接从内存中取出那个实例来用，如果没有，才会new一个出来。
如果读者熟悉spring框架的话，也许会想到，spring的依赖注入机制，对于实现具体逻辑处理的service对象，并不需要读者显示的去new，因为那样可能由于开发者的不规范的new对象操作，导致出现很多重复的对象，浪费对象，而是直接在配置文件里设置或者标注，spring就会自动帮我new一个相应对象，而且只会存在一个，这样使用的时候直接使用就可以了，不仅帮我们解决了创建对象的过程，而且避免了生成过多对象。虽然依赖注入机制并不是使用的flyweight模式，但思想上会有相似之处。

# 享元模式的实例程序
我们假设我们有1，2，3，4，5，6，7，8，9的几个字符图形，这些字符对象就是大对象。

![image.png](http://upload-images.jianshu.io/upload_images/1234352-6af024c321c9a704.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-79c61899ee70a27a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-a21b61b310742d7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-ff7464ca0bde9d02.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们先把这些字符存在文件中，然后从文件读入存到string对象中，这些string对象就是重对象，假设我们现在要打印出12121212这些数字对应的大字符，如果我们new出每一个对象，就会占用很多内存，于是，这里我们就可以利用享元模式，将对象变轻，实现对象的共享，对于上面这个输出要求，我们只要new出1，2两个大字符就可以了。

我们看实例程序的类图

![image.png](http://upload-images.jianshu.io/upload_images/1234352-428c5f0acdf6507b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 首先bigchar类，从文件中读取大字符的内容

```
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class BigChar {
    // 字符名字
    private char charname;
    // 大型字符对应的字符串(由'#' '.' '\n'组成)
    private String fontdata;
    // 构造函数
    public BigChar(char charname) {
        this.charname = charname;
        try {
            BufferedReader reader = new BufferedReader(
                new FileReader("big" + charname + ".txt")
            );
            String line;
            StringBuffer buf = new StringBuffer();
            while ((line = reader.readLine()) != null) {
                buf.append(line);
                buf.append("\n");
            }
            reader.close();
            this.fontdata = buf.toString();
        } catch (IOException e) {
            this.fontdata = charname + "?";
        }
    }
    // 显示大型字符
    public void print() {
        System.out.print(fontdata);
    }
}
```

* BigCharFactory类
它实现共享实例的功能，用一个hashmap存储对应的实例，然后先判断是否已有实例，没有再新建。

```
import java.util.HashMap;

public class BigCharFactory {
    // 管理已经生成的BigChar的实例
    private HashMap pool = new HashMap();
    // Singleton模式
    private static BigCharFactory singleton = new BigCharFactory();
    // 构造函数
    private BigCharFactory() {
    }
    // 获取唯一的实例
    public static BigCharFactory getInstance() {
        return singleton;
    }
    // 生成（共享）BigChar类的实例
    public synchronized BigChar getBigChar(char charname) {
        BigChar bc = (BigChar)pool.get("" + charname);
        if (bc == null) {
            bc = new BigChar(charname); // 生成BigChar的实例
            pool.put("" + charname, bc);
        }
        return bc;
    }
}
```

* BigString表示由大型字符组成的字符串

```
public class BigString {
    // “大型字符”的数组
    private BigChar[] bigchars;
    // 构造函数
    public BigString(String string) {
        bigchars = new BigChar[string.length()];
        BigCharFactory factory = BigCharFactory.getInstance();
        for (int i = 0; i < bigchars.length; i++) {
            bigchars[i] = factory.getBigChar(string.charAt(i));
        }
    }
    // 显示
    public void print() {
        for (int i = 0; i < bigchars.length; i++) {
            bigchars[i].print();
        }
    }
}

```

最后main函数调用测试

```
public class Main {
    public static void main(String[] args) {
        if (args.length == 0) {
            System.out.println("Usage: java Main digits");
            System.out.println("Example: java Main 1212123");
            System.exit(0);
        }
        BigString bs = new BigString(args[0]);
        bs.print();
    }
}
```

运行结果

![image.png](http://upload-images.jianshu.io/upload_images/1234352-6d0d0e6e8dd0d966.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 享元模式分析

![image.png](http://upload-images.jianshu.io/upload_images/1234352-b5df12d5146cbb80.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

•Flyweight

— 描述一个接口，通过这个接口Flyweight可以接受并作用于外部状态。

• ConcreteFlyweight

— 实现Flyweight接口， 并为内部状态（ 如果有的话） 增加存储空间。

ConcreteFlyweight对象必须是可共享的。它所存储的状态必须是内部的；即，它必

须独立于Concrete Flyweight对象的场景。

• UnsharedConcreteFlyweight

— 并非所有的Flyweight子类都需要被共享。Flyweight接口使共享成为可能，但它并不强制共享。在Flyweight对象结构的某些层次， UnsharedConcreteFlyweight对象通常

将ConcreteFlyweight对象作为子节点（Row和Conum就是这样）。

• FlyweightFactory

— 创建并管理Flyweight对象。

— 确保合理地共享Flyweight。当用户请求一个Flyweight时，FlyweightFactory对象提供一个已创建的实例或者创建一个（如果不存在的话）。

• Client

— 维持一个对Flyweight的引用。

— 计算或存储一个（多个）Flyweight的外部状态。

享元模式的特点：
* 会对多个地方产生影响
由于实例是共享的，如果修改一个实例，就会对多给对方产生影响
