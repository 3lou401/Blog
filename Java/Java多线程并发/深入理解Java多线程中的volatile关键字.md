> * Java 的 volatile关键字对可见性的保证
* Java 的 volatile关键字在保证可见性之前的所做的事情
* 为什么volatile关键字有时候也不是足够的
* 什么时候volatile足够了
* volatile关键字对效率的影响

Java关键字用于将一个变量标记为“存储在内存中的变量”。更准确的说，意思就是每一次对volatile标记的变量进行读取的时候，都是直接从电脑的主内存进行的，而不是从cpu的cache中，而且每个对volatile变量的写入操作，都会被直接写入到主存里，而不是只写到cache里。

实际上，从java5开始，volatile关键字就不仅仅是保证volatile变量从主存读写，笔者会在后面详细讨论这个问题。

# Java 的 volatile关键字对可见性的保证

Java的volatile关键字可以保证变量的可见性。说起来很简单，但具体是什么意思呢？

在多线程的应用程序中，线程操作非volatile的变量，为了更快速的执行程序，每个线程都会将变量从主存复制到cpu的cache中。如果你的电脑有多个cpu，每个线程都在不同的cpu上运行，这就意味着，每个线程将变量的值复制到不同的cpu的cache上，就像下面这个图所表明：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-18efbeb44a38bd8e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果变量没有声明为volatile，那么就无法知道，变量什么时候从主存中读取到cpu的cache中，有什么时候从cache中写回到主存中。这就可能造成很多潜在的问题：

假设一种情况，多个线程同时持有一个共享对象的引用，这个对象包括一个counter变量：

```
public class SharedObject {

    public int counter = 0;

}
```

假设这种情况，只有线程1自增了这个counter变量，但是线程1和线程2可能随时读取这个counter变量。如果这个counter变量没有被声明为volatile，那么就无法确认，什么时候counter的变量的值会从cpu的cache中写回到主存中，这就意味着，counter变量的值在cpu的cache中的值可能和主存中不一样，如下图所示：


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-5f3768f54f0fc97e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个线程的问题无法及时的看到变量的最新的值，因为可能这个变量还没有被另一个线程写回到主存中。所以一个线程对一个变量的更新对其他的线程是不可见的。这就是我们最初提出的线程的可见性问题。

通过将一个变量声明为volatile，那么所有对这个变量写操作会被直接写回到主内存中，所以这对线程都是可见的。而且，所有对这个变量的读取操作，也会直接从主存中读取，下面说明了如何声明一个voaltile变量：
```
public class SharedObject {

    public volatile int counter = 0;

}
```

** 将一个变量声明为volatile就可以保证写操作，其他线程对这个变量的可见性 **

# Java 的 volatile关键字在保证可见性之前的所做的事情

从java5开始，volatile关键字不仅可以保证变量直接从主内存中读取，还有一下作用：
* 如果线程A对一个volatile变量进行写操作，线程B随后读取同一个volatile值，那么在线程将变量写操作完成之后的所有变量对线程A和B都是可见的。
* 那些操作volatile变量的读写指令的顺序无法被JVM改变（JVM有时候为了效率会改变变量读写顺序，只要JVM判断改变顺序对程序没有影响的话）。

上面两段话不是很理解，我们接下来进行一个更细致的说明：

当一个线程对一个volatile变量进行写操作的时候，不仅仅是这个变量自己被写入到主存中，同时，其他所有在这之前被改变值的变量也都会线程先写入到主存中。
当一个线程对一个volatile变量进行读取操作，他也会将所有跟着那个volatile变量一起写入到主存中的其他所有变量一起读出来。
看下面这个例子：
```
Thread A:
    sharedObject.nonVolatile = 123;
    sharedObject.counter     = sharedObject.counter + 1;

Thread B:
    int counter     = sharedObject.counter;
    int nonVolatile = sharedObject.nonVolatile;
```

因为线程A在对volatile的sharedObject.counter进行写操作之前，先对sharedObject.nonVolatile变量进行写操作，所以当线程A要将volatile的sharedObject.counter写回到主存时，这两个变量都会被写回到主存中。

同理，线程B在读取volatile变量到sharedObject.counter的时候，两个变量sharedObject.counter and sharedObject.nonVolatile所以线程读取变量sharedObject.nonVolatile就会看到他被线程A改变后的值。

开发者可以利用这个扩展的可见性去放大线程间的变量可见性，不需要将每一个变量都声明为volatile，只需要声明一两个变量为volatile就可以了。下面这个简单的例子，就来说明这个问题：

```
public class Exchanger {

    private Object   object       = null;
    private volatile hasNewObject = false;

    public void put(Object newObject) {
        while(hasNewObject) {
            //wait - do not overwrite existing new object
        }
        object = newObject;
        hasNewObject = true; //volatile write
    }

    public Object take(){
        while(!hasNewObject){ //volatile read
            //wait - don't take old object (or null)
        }
        Object obj = object;
        hasNewObject = false; //volatile write
        return obj;
    }
}
```

线程A可能会调用put方法将objects put进去，线程B可能会调用take方法将object拿出来。这个类可以正常工作，只要我们使用一个volatile变量即可（不使用同步语句），只要只有线程A调用put，只有线程B调用take。

然后，JVM有时候为了提高效率，可能会改变指令执行的顺序，只要JVM判断这样做不改变指令的语义，那么就有可能改变指令的顺序。那么如果JVM改变了指令的执行顺序会发生什么呢？put方法可能会像下面这样执行：
```
while(hasNewObject) {
    //wait - do not overwrite existing new object
}
hasNewObject = true; //volatile write
object = newObject;
```

我们观察到，现在对于volatile的hasNewObject 操作在object = newObject;之前执行，这说明，object还没有真正被赋值新对象，但是hasNewObject 已经先变为true了。对于JVM来说，这种交换是完全有可能的。因为这两个write的指令彼此不是互相依赖的。

但是这样交换顺序之后可能会对object变量的可见性产生不好的影响。首先，线程B可能会在线程A真正给object写入一个新值之前，就看到hasNewObject 变为true。
另一方面，我们无法确保object什么时候会被真正写入到主内存中。

为了防止上面这种情况的发生，volatile关键字就提出了一种“happens before guarantee”，这可以保证volatile的变量的读写指令不会被重新排序。指令前面的和后面的可以随意排序，但是volatile变量的读写指令的相对顺序是不能改变的。

看下面这个例子就能理解了：
```
sharedObject.nonVolatile1 = 123;
sharedObject.nonVolatile2 = 456;
sharedObject.nonVolatile3 = 789;

sharedObject.volatile     = true; //a volatile variable

int someValue1 = sharedObject.nonVolatile4;
int someValue2 = sharedObject.nonVolatile5;
int someValue3 = sharedObject.nonVolatile6;
```

JVM可能会改变前三个指令的顺序，只要他们在volatile的写指令之前发生（就是说他们必须在volatile的写指令之前发生）。
同理，JVM也可能改变后三个指令的顺序，只要他们在volatile的写指令之后发生。

这就是对于Java的 volatile happens before guarantee.的最基本的理解

# Volatile有时候也是不够的

虽然volatile可以保证读取操作直接从主内存中的读取，写操作直接写到内存中，但仍然存在一些情况下，光使用volatile关键字是不够的。

在之前的举例的程序中，只有一个线程在向共享变量写入数据的时候，声明为volatile，另一个线程就可以一直看到最新被写入的值。

实际上，只要新值不依赖旧值的情况下，多个线程同时向共享的volatile变量里写入数据时，仍然能在主内存中得到正确的值。换句话说，就是这个volatile变量值更新的时候，不需要先读取出他以前的值才能得到下一个值。

只要一个线程需要先读取一个voaltile变量，然后必须基于他的值才能产生新的值，那么volatile关键字就不再能保证变量的可见性了。在读取变量和写入变量的时候，存在一个短的时间间隙，这就会造成，多个线程可能会在这个间隙读取同一个值，产生一个新值，然后写入到主内存中，将其他线程对值的改变给覆盖了。

所以常见的情况就是如果一个volatile变量在进行自增或者自减操作，那么这时候使用volatile就可能出问题。
接下来我们更深入的讨论这个问题，假设线程1读取一个共享的counter变量到cpu的cache中，此时他的值是0，然后给它自增加一，但是还没有写到主存中，所以主存中还是1，线程2也能够读取同一个counter变量，而这个变量读取的时候还是0，在他自己的cpucache中，这样就出现问题了：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-44288ccd45592290.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

线程1和线程2实际上是不同步的。共享变量counter的真实值实际上应该为2，因为被加了两次，但是每个线程在自己的cache上存的值是1，而且在主存中这个值仍然是0，这就变得很混乱。即使线程最后将值写回到主存中，但最后的值也是不正确的。

# 什么时候volatile足够了

前文中提到，如果两个线程都在对volatile变量进行读写操作，那么仅仅使用volatile关键字是远远不够的。你需要使用synchronize关键字，来保证读写操作的原子性。
但如果是只有一个线程在读写volatile变量，另外的多个线程仅仅是读取这个变量的话，那么这就可以保证，其他读线程所看到的变量值都是最新的。volatile关键字可以使用在32位或者64位的变量上。

# volatile关键字对效率的影响

读写一个volatile变量的时候，会导致变量直接在主存中读写，显然，直接从主存中读写速度要比从cache中来得慢。另一方面，操作volatile变量的时候不能改变指令的执行顺序，这一定程度上也会影响读写的效率。所以，只有我们需要确保变量可见性的时候，才会使用volatile关键字。
