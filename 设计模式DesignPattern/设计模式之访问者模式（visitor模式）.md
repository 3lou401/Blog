> * 引入访问者模式
* 访问者模式的实例
* 访问者模式分析

# 引入访问者模式
Visitor是访问者的意思。
数据结构中保存着元素。一般我们需要对元素进行处理，那么处理元素的代码放在哪里呢？最显然的方法就是放在数据结构的类中，在类中添加处理的方法。但是如果有很多处理，就比较麻烦了，每当增加一种处理，我们就不得不去修改表示数据结构的类。
visitor模式就是用来解决这个问题的，visitor模式将数据结构的定义和处理分离开。也就是会新增一个访问者的类，将数据元素的处理交给访问者类，这样以后要新增处理的时候，只需要新增访问者就可以了。

# visitor模式的实例

我们在这个实例中会结合composite模式[http://www.jianshu.com/p/685dd6299d96]中的实例基础上进行增改，文件夹和文件表示我们要访问的数据结构，然后我们实现访问者将文件夹和文件的内容输出。

先看一下类图：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-a8ae7c182c508127.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们看到对于数据结构那部分有一个接口element，这个接口就是用来接受访问者的一个接口，里面只有一个方法就是接受一个访问者，然后子类具体的数据结构回去实现这个这个方法。

```
package Visitor;

public interface Element {
    public abstract void accept(Visitor v);
}
```

然后访问者有个抽象类，根据不同类型的访问处理，可以继承这个方法添加，这里的访问者也可以用接口实现笔者认为。
```
package Visitor;

public abstract class Visitor {
    public abstract void visit(File file);
    public abstract void visit(Directory directory);
}
```
访问者定义了两个抽象方法，分别是用来访问文件类和目录类的

接下来看看具体的访问者实现：
```
package Visitor;

import java.util.Iterator;

public class ListVisitor extends Visitor {
    private String currentdir = "";                      
    public void visit(File file) {                  
        System.out.println(currentdir + "/" + file);
    }
    public void visit(Directory directory) {  
        System.out.println(currentdir + "/" + directory);
        String savedir = currentdir;
        currentdir = currentdir + "/" + directory.getName();
        Iterator it = directory.iterator();
        while (it.hasNext()) {
            Entry entry = (Entry)it.next();
            entry.accept(this);
        }
        currentdir = savedir;
    }
}
```
这里就是具体的处理逻辑代码了。

file类和directory类需要实现element接口，并实现accpet方法就是接受一个访问者对象，并调用访问者对象访问自己
```
package Visitor;

public class File extends Entry {
    private String name;
    private int size;
    public File(String name, int size) {
        this.name = name;
        this.size = size;
    }
    public String getName() {
        return name;
    }
    public int getSize() {
        return size;
    }
    public void accept(Visitor v) {
        v.visit(this);
    }
}
```
```
package Visitor;

import java.util.Iterator;
import java.util.ArrayList;

public class Directory extends Entry {
    private String name;                    // 鏂囦欢澶瑰悕瀛�
    private ArrayList dir = new ArrayList();      // 鐩綍鏉＄洰闆嗗悎
    public Directory(String name) {         // 鏋勯�犲嚱鏁�
        this.name = name;
    }
    public String getName() {               // 鑾峰彇鍚嶅瓧
        return name;
    }
    public int getSize() {                  // 鑾峰彇澶у皬
        int size = 0;
        Iterator it = dir.iterator();
        while (it.hasNext()) {
            Entry entry = (Entry)it.next();
            size += entry.getSize();
        }
        return size;
    }
    public Entry add(Entry entry) {         // 澧炲姞鐩綍鏉＄洰
        dir.add(entry);
        return this;
    }
    public Iterator iterator() {      // 鐢熸垚Iterator
        return dir.iterator();
    }
    public void accept(Visitor v) {         // 鎺ュ彈璁块棶鑰呯殑璁块棶
        v.visit(this);
    }
}
```

```
package Visitor;


import java.util.Iterator;

public abstract class Entry implements Element {
    public abstract String getName();                                 
    public abstract int getSize();                                      
    public Entry add(Entry entry) throws FileTreatmentException {       
        throw new FileTreatmentException();
    }
    public Iterator iterator() throws FileTreatmentException {   
        throw new FileTreatmentException();
    }
    public String toString() {                                      
        return getName() + " (" + getSize() + ")";
    }
}
```

最后，测试：
```
package Visitor;

public class Main {
    public static void main(String[] args) {
        try {
            System.out.println("Making root entries...");
            Directory rootdir = new Directory("root");
            Directory bindir = new Directory("bin");
            Directory tmpdir = new Directory("tmp");
            Directory usrdir = new Directory("usr");
            rootdir.add(bindir);
            rootdir.add(tmpdir);
            rootdir.add(usrdir);
            bindir.add(new File("vi", 10000));
            bindir.add(new File("latex", 20000));
            rootdir.accept(new ListVisitor());              

            System.out.println("");
            System.out.println("Making user entries...");
            Directory yuki = new Directory("yuki");
            Directory hanako = new Directory("hanako");
            Directory tomura = new Directory("tomura");
            usrdir.add(yuki);
            usrdir.add(hanako);
            usrdir.add(tomura);
            yuki.add(new File("diary.html", 100));
            yuki.add(new File("Composite.java", 200));
            hanako.add(new File("memo.tex", 300));
            tomura.add(new File("game.doc", 400));
            tomura.add(new File("junk.mail", 500));
            rootdir.accept(new ListVisitor());              
        } catch (FileTreatmentException e) {
            e.printStackTrace();
        }
    }
}

```

测试结果：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-c28ceae0e9e69663.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# visitor模式分析

我们分析一下访问者模式示例程序的处理流程，假设一个文件夹下有两个文件

* 首先，main类生成了listVisitor实例。
* Main类调用directory类的方法，并将生成的listVistor实例传了进去
* directory的accept方法会调用访问者的处理方法，并将自身实例传了进去。
* 接下来，就来到了listVIsitor的处理流程中了，访问文件夹，然后找到文件夹里的第一个文件，对这个文件也调用accpet方法进行访问，传进去的对象是自身的listVIsitor实例
* 这是个递归的过程，listVisitor又会去访问处理file，然后访问完了就返回类似于堆栈
* 然后回到directory的处理中，又接着处理第二个文件，重复上述过程，直到没有可处理的对象了就会返回。

visitor模式中的角色：
* visitor（访问者）
访问者角色负责对数据结构中的每一个具体的元素声明一个对应的访问的visit方法，具体的实现则交给concretevisitor去实现

* ConcreteVisitor（具体的访问者）
负责继承实现visitor的访问处理的方法，实现具体的处理逻辑

* Element
表示访问者访问的对象，声明接受访问者的accept方法。

* ConcreteElemnet
负责实现具体的元素，实例中的file和directory类

* ObjectStructure
负责处理Elemnet元素的集合，每个Elemnet都实现了接收访问者的方法，所以为了访问到所有的元素，需要存储一个所有元素的集合结构，实例中的directory对应于这个。

类图：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-d0f70010d0cfd96b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
