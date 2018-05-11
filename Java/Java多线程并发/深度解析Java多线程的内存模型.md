> * 内部java内存模型
* 硬件层面的内存模型
*  Java内存模型和硬件内存模型的联系
* 共享对象的可见性
* 资源竞速

Java内存模型很好的说明了JVM是如何在内存里工作的，JVM可以理解为java执行的一个操作系统，作为一个操作系统就有内存模型，这就是我们常说的JAVA内存模型。

如果我们想正确的写多线程的并行程序。理解好java内存模型在多线程下的工作方式是及其重要的，这可以帮我们更好的理解底层的工作方式。

java内存模型说明了不同的线程怎样以及何时可以看到其他线程写入共享变量的值，以及同步程序怎么共享变量。最初的java内存模型不够好，存在很多的不足，所以在java1.5z中，java内存模型的版本的进行了一次重大的更新与改进，并且在java8中仍然被使用。

# 内部java内存模型
JVM的内部的内存模型分为了两部分，thread stack和heap，也就是线程栈和堆，我们将复杂的内存模型抽象成下图：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-0a8474641ef704d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


每一个在JVM中运行的线程在内存里都会有属于自己的线程栈。线程栈一般包含这个线程的方法执行到哪一个点了这些信息，也被称作“call stack”，当线程执行代码，调用栈就会随着执行的状态改变。

线程栈也包括了每个方法执行时的local 变量，所有的方法也都存储在线程栈上，一个线程可以只能访问自己的线程栈。每个线程自己创建的本地本地变量对其他线程是不可见的，也就是私有的，即使两个线程调用的是同一个方法，每个线程会分别保存一份本地变量，各自属于各自的线程栈。

所有基本类型的local变量（ boolean, byte, short, char, int, long, float, double）全都被存储在线程栈里，而且对其他线程是不可见的，一个线程可能会传递一份基本类型的变量值的一份拷贝给另一个线程，但是自己本身的变量是不能共享的，只能传递拷贝。

堆中存储着java程序中new出来的对象，不管是哪个线程new出来的对象，都存在一起，而且不区分是哪个线程的对象。这些对象里面也包括那些原始类型的对象版本(e.g. Byte, Integer, Long etc.). 不管这个对象是分配给本地变量还是成员变量，最终都是存在堆里。

下面这个图就说明了线程栈中存储了local变量，堆中存储着对象object。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-ea5d1f42c48880b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

一个原始数据类型的本地变量将完全被存储在线程栈中。

本地变量也可以是指向对象的引用，在这种情况下，本地变量存在线程栈上，但是对象本身是存在堆上。

一个对象可能包含方法这些方法同时也会包含本地变量，这些本地变量也是存储在线程栈上面，即使他们所属于的对象和方法是存在堆上的。

一个对象的成员变量是跟随着对象本身存储在堆上的，不管成员变量是原始数据类型还是指向对象的引用。

静态的类变量一般也存储在堆上，根据类的定义。

存储在堆上的对象可以被所有的线程通过引用来访问。当一个线程持有一个对象的引用时，他同时也就可以访问这个对象的成员变量了。如果两个线程同时调用同一个对象的一个方法，他们就会都拥有这个对象的成员变量，但是每一个线程会享有自己私有的本地变量。

下面这张图就说明以上的内容

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-8be0905d91a01264.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

两个线程有一系列的本地变量。其中一个本地变量(Local Variable 2)指向堆中的object3.这两个线程每个都有指向同一个对象object3的不同引用。他们的引用是本地变量，都存在各自的线程栈中，虽然这两个不同的引用是指向同一个对象的。

我们还可以发现，共有的对象object3有指向object2和object4的引用，这些引用是作为object3中的成员变量存在的。通过object3中的成员变量的引用，两个线程都可以访问到object2和object4.

这个图也说明了指向堆中不同对象的本地变量。例如图中的object1和object5，不是同一个对象。理论上，所有的线程都可以访问堆中的对象，只要这个线程持有堆中对象的引用。但是这个图中，每个线程只有这两个对象中的一个引用。

下面，我们将写一段实际的代码，这段代码的内存模型就跟上图一样：
```
public class MyRunnable implements Runnable() {

    public void run() {
        methodOne();
    }

    public void methodOne() {
        int localVariable1 = 45;

        MySharedObject localVariable2 =
            MySharedObject.sharedInstance;

        //... do more with local variables.

        methodTwo();
    }

    public void methodTwo() {
        Integer localVariable1 = new Integer(99);

        //... do more with local variable.
    }
}
```

```
public class MySharedObject {

    //static variable pointing to instance of MySharedObject

    public static final MySharedObject sharedInstance =
        new MySharedObject();


    //member variables pointing to two objects on the heap

    public Integer object2 = new Integer(22);
    public Integer object4 = new Integer(44);

    public long member1 = 12345;
    public long member1 = 67890;
}
```

如果两个线程执行run方法那么，上图的内存模型就会是这段程序的执行结果。

methodOne()声明了一个原始数据类型的本地变量，int类型的localVariable1 ，和一个指向对象的引用的本地变量(localVariable2).

每个线程执行methodOne()的时候都会创建属于自己的一份本地变量的拷贝，也就是
 localVariable1 and localVariable2在他们各自的线程栈的空间中。localVariable1将会被完全对其他线程是不可见的，只存在与每个线程自己的线程栈空间中。一个线程不能看到其他线程对localVariable1所做的改变与操作，是不可见的。

每个线程执行methodOne()方法的时候也会创建localVariable2的拷贝，但是不同的localVariable2的拷贝最终却指向同一个堆上的对象。这段代码让localVariable2指向之前通过一个静态变量引用的对象。静态变量只会存在一份，不会有多余的拷贝，而且静态变量是存在堆中的。所以，localVariable2的两份拷贝同时指向同一个MySharedObject对象的实例，与此同时，还有一个堆中的静态变量也指向这个对象实例。这个对象就是对应上图中的object3. 

我们发现MySharedObject 包含这两个成员变量。这些成员变量跟对象一样存储在堆上。这两个成员变量指向两个integer对象。这两个对象分别对应上图中的object2和object4.

我们发现，methodTwo() 创建了一个本地变量叫做localVariable1。这个本地变量是一个对象的引用，他指向一个integer对象。这个方法将本地变量localVariable1指向一个新的值。在执行methodTwo()的时候，每个线程都会持有一份localVariable1的拷贝。这两个Integer对象将会被初始化在堆上，但是因为每次执行这个方法的时候，这个方法都会创建一个新的对象，所以两个线程会拥有独立的对象实例。这两个对象就对应上图中的object1和object5.

我们发现MySharedObject 中的成员变量是原始数据类型，但由于他们是成员变量，所以依旧存储在堆上。只有本地变量存储在线程栈中。

# 硬件层面的内存模型

硬件层面的内存内存结构与JVM中的内存结构是有不同的，对我们来说，正确理解掌握硬件层面的内存模型是很必要的，这可以帮助我们理解java多线程的底层机制，更要了解java内存模型如何在硬件内存结构上工作。这一章将讲述硬件层面内存模型，下一部分将讲述java如何结合硬件工作。

下图是一个简化的现代计算机硬件结构图：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-65520363c97a56e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

现代计算机通常会有两个甚至更多的cpu，这些cpu可能还会有多个核心，这个意义是，拥有多个cpu的计算机可能会有多个线程在同时执行，每个cpu都可以在任何给定的时间运行一个线程。这就意味着如果我们的java程序是多线程的，在内部就每个线程就会有一个cpu在同时执行。

每个cpu都会有一系列的寄存器registers在cpu的内存中，而且这些寄存器是很重要的。cpu在寄存器上进行计算操作比在主内存中进行计算要快的多。这是因为cpu访问寄存器的速度比访问内存要快得多。

每个cpu也会有一个cpu的cache内存。这是因为cpu访问cache比访问内存的速度要快得多，但是却比访问的寄存器要慢一些，所以cache的速度是介于寄存器和内存的。一些cpu还有多级cache，比如(Level 1 and Level 2)，但是这对于我们理解java内存模型关系不大，我们只需要cpu有三层内存结构，寄存器-cache-内存（RAM）.

一台计算机一般都会有主内存也就是RAM,所有cpu都可以访问主内存，主内存的容量一般远比cache大得多。

一般的，当cpu需要访问内存的时候，他会先读取一部分主内存到cache中，甚至，会读取一部分cache到内部的寄存器中，然后再在寄存器进行计算操作。当cpu将计算结果写回内存中时，他会flush寄存器和cache中的数据，然后将值写回至内存中。

当cpu要求cache去存储其他内容时，也会将cache中的内容flush到内存中。cpu的cache可以边写入一部分数据到内存，边写入一部分到自己cache中，所以在更新数据，不必要全部清空cache，可以边读边写。一般的，cache真正更新数据是在更小的内存块上，叫做“cache lines”。多个“cache lines”可能正在读取数据到cache中，而另一部分可能正在将数据写回到内存中。

# Java内存模型和硬件内存模型的联系

上文已经提到，java内存模型和硬件内存模型是不同的。硬件内存模型不区分堆和栈。在硬件层面，所有的线程栈和堆都被存储在主内存中，一部分线程栈和堆可能有时候会出现在cpu cache中和cpu寄存器中。下图可以说明这个问题：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-6a9e26f10ccaa42e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当对象和变量被存储在不同的内存区域的时候，很多问题就可能发生，主要有以下两类问题：
* 当线程对一些共享数据进行更新或者写操作时，可见性的问题
* 当读写共享数据产生资源竞速的问题
接下来的部分就会讨论这两个问题

## 共享对象的可见性

如果多个线程在共享一个对象，没有正确使用volatile或者synchronize声明，更新共享对象的时候就可能出现其他线程不可见的问题。

我们假设共享对象初始化主内存中。一个在cpu中运行的线程读取共享对象到cache中。这时候，随着程序的执行，可能导致共享对象发生一些变化。只要cpu的cache还没有被写回到主内存中，这个共享对象的变化就对其他在cpu上运行的线程不可见。这种情况下，每个线程都会有持有一份自己对于共享对象的拷贝，这份拷贝存储在各自的cpu的cache中，而且对于其他线程是不可见的。

下图说明了大致的情况，在左边cpu执行的线程将共享对象读取到cache中，并且将他的值改变为2.这个变化对右边的cpu的其他线程是不可见的，因为对于变量count的更新还没有被写回到主内存中。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-9f992a05d55d5ccf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

想要解决这个共享对象可见性的问题，可以使用java的volatile关键字（参见笔者的另一篇volatile的博文），这个关键字可以保证所给定的变量都是直接从主内存中读取，而且每当更新时就立即写回到内存中，所以可以保证变化是及时可见的。

## 资源竞速

如果多个线程共享一个对象，而且多个线程需要更新共享对象中的变量，那么就可能造成资源竞速的发生。

假设线程A读取读取一个共享对象的变量count到cpu的cache中，同时，线程B也执行同样的步骤，但是是读取到一个不同的CPU的cache中，现在线程A给count加一，线程B也做同样的事情，现在这个变量被加了两次，分别在不同的cpu的cache中。

如果这两次递增操作是被按顺序先后执行的，这个变量count就会被加两次而且比最初的值加了2，写回到主内存中。

然而，如果这两个递增操作是并发执行的，且没有正确的进行同步操作，写回内存的时候，更新后的值只会被加一，虽然实际上是进行了两次递增操作。
下图就说明了程序并发执行的时候，产生的资源竞速的问题：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-db30344a33ca84dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

想要解决这个问题，我们可以使用java中的synchronize关键字。synchronize可以保证只有一个线程能进入那些被声明为synchronize的代码段中。同步的线程可以保证所有同步代码段中的变量都会从内存中读取，而且当线程离开代码块的时候，所有更新后的值都会被写回主内存中，不管这个变量有没有被声明volatile。

# 小结
本文详细的剖析了java内存模型和硬件层面的内存模型，并且分析了硬件和java是怎么在内存模型上合作联系的。这对于我们接下来理解java多线程的概念是及其重要的，打下了牢固的基础。
