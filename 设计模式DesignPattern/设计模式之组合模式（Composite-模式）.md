> * 引入composite模式
* composite模式的实例
* composite模式的分析
* 小结

# 引入composite模式
在计算机文件系统中，有**文件夹**的概念，文件夹里面既可以放入文件也可以放入文件夹，但是文件中却不能放入任何东西。文件夹和文件构成了一种递归结构和容器结构。
虽然文件夹和文件是不同的对象，但是他们都可以被放入到文件夹里，所以一定意义上，文件夹和文件又可以看作是同一种类型的对象，所以我们可以把文件夹和文件统称为目录条目，（directory entry）.在这个视角下，文件和文件夹是同一种对象。
所以，我们可以将文件夹和文件都看作是目录的条目，将容器和内容作为同一种东西看待，可以方便我们递归的处理问题，在容器中既可以放入容器，又可以放入内容，然后在小容器中，又可以继续放入容器和内容，这样就构成了容器结构和递归结构。
这就引出了我们本文所要讨论的composite模式，也就是组合模式，组合模式就是用于创造出这样的容器结构的。是容器和内容具有一致性，可以进行递归操作。

# composite模式的具体实例
我们实现一个实例程序，可以列出文件和文件夹的信息。
自然，根据前文的讨论，我们需要建立三个类，一个文件类，一个文件夹类，同时还要抽象出两种类的共性，新建一个entry类，也就是目录条目类，这个类是实现文件类和文件夹类的一致性的。

我们先简单看一下类图：


![image.png](http://upload-images.jianshu.io/upload_images/1234352-1601bae235fc0ccb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


首先我们实现Entry类，这个类表示目录条目的抽象类
```
package Composite;

public abstract class Entry {
	public abstract String getName();
	public abstract int getSize();
	
	public Entry add(Entry entry) throws FileTreatMentException {
		throw new FileTreatMentException();
	}
	
	public void printList() {
		printList("");
	}
	
	protected abstract void printList(String prefix);
	
	public String toString() {
		return getName() + "(" + getSize() + ")";
	}
}
```

File类是文件类
```
package Composite;

public class File extends Entry {
	
	private String name;
	
	private int size;
	
	public File(String name, int size) {
		this.name = name;
		this.size = size;
	}

	@Override
	public String getName() {
		return name;
	}

	@Override
	public int getSize() {
		return size;
	}

	@Override
	protected void printList(String prefix) {
		System.out.println(prefix + '/' + this);
	}
	
}
```
Directory是目录类，它持有一个目录的集合
```
package Composite;

import java.util.ArrayList;
import java.util.Iterator;

public class Directory extends Entry {
	
	private String name;
	
	private ArrayList directory = new ArrayList();
	
	public Directory(String name) {
		this.name = name;
	}

	@Override
	public String getName() {
		return name;
	}

	@Override
	public int getSize() {
		int size = 0;
		Iterator it = directory.iterator();
		while(it.hasNext()) {
			Entry entry = (Entry)it.next();
			size += entry.getSize();
		}
		return size;
	}
	
	public Entry add(Entry entry) {
		directory.add(entry);
		return this;
	}
	
	@Override
	protected void printList(String prefix) {
		System.out.println(prefix + "/" + this);
		Iterator it = directory.iterator();
		while(it.hasNext()) {
			Entry entry = (Entry)it.next();
			entry.printList(prefix + "/" + name);
		}
	}
	
}

```
我们新建一个测试类
```
package Composite;

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
            rootdir.printList();

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
            rootdir.printList();
        } catch (FileTreatMentException e) {
            e.printStackTrace();
        }

	}

}

```
输出结果：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-8b318dd4a93394d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# composite模式
composite模式主要有一下几类角色

* leaf 树叶
表示内容的角色，该角色中不能放入其他对象，对应我们实例程序中的file

* Composite 复合物
表示容器的角色，可以放入小容器和内容，也就是leaf和composite，此实例中，由directory类代表composite

* component
是leaf和composite角色具有一致性的角色，一般作为leaf和composite的父类，定义一些共有的行为和属性，此例中的entry类代表

类图如下：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-205a607fddccdde8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

典型的composite结构：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-28cfe01a00eb3935.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

一个小问题，add方法应该放在哪里？
因为add方法只是容器可以使用的，内容无法使用add，所以add方法的位置可以有所选择，我们实例中是将add放在entry中，同时抛出异常，当文件类调用的时候就抛出异常

* 定义在entry类中，报错
就是我们实例中的做法，让其报错

* 定义在entr类，但什么都不做
交给要做的子类去做

* 声明在entry中，但不实现
子类需要实现，优点是所有子类都要实现add，但是file本不需要add，却也要实现

* 只定义在directory中
就是在使用的时候需要进行类型转换。

# 小结
在实例程序中，我们以文件夹的结构实现了composite模式，实际上现实世界中，到处都存在composite模式，例如，视窗系统中，窗口可以含有子窗口也可以含有button类似的控件。通常来说，树结构的数据结构都适合composite模式
