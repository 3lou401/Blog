> * 临界区
* 临界区的资源竞速
* 避免资源竞速
* 临界区的吞吐量


critical section是每个线程中访问[临界资源](http://baike.baidu.com/item/%E4%B8%B4%E7%95%8C%E8%B5%84%E6%BA%90)的那段代码，不论是硬件临界资源，还是软件临界资源，多个[线程](http://baike.baidu.com/item/%E7%BA%BF%E7%A8%8B)必须互斥地对它进行访问。
资源竞速就是可能在由于在访问临界区时没有互斥的访问而导致的特殊情况。

如果多个线程在临界区的执行结果可能因为代码的执行顺序不同而出现不同的结果，我们就说这时候在临界区出现了资源竞速的情况。

我们接下来会详细的介绍资源竞速和临界区的概念

# 临界区

当多个线程访问相同资源的时候，就会出现问题。比如说访问相同的内存区域（变量，数组或者对象），系统资源（数据库，文件等）
而且，一般来说，只在多个线程对这个资源进行写操作的时候才会出现问题，如果是简单的读操作，不改变资源的话，显然是不会出现问题的。
Here is a critical section Java code example that may fail if executed by multiple threads simultaneously:
下面这段临界区的代码，如果被多个线程执行就可能出现问题：
```
 public class Counter {

     protected long count = 0;

     public void add(long value){
         this.count = this.count + value;
     }
  }

```

我们假设有两个线程A和B同时执行add方法在相同的实例上，我们无法获知什么时候操作系统进行线程的切换，而且代码中的add操作对jvm来说不是原子操作，而是由一下类似的几步指令：
* load，从内存中将this.count的值load到寄存器上
* 在寄存器上进行加法运算
* 将寄存器的值写回到内存中

我们看看如果按下面的执行顺序，结果会是什么样？
```
   A:  Reads this.count into a register (0)
   B:  Reads this.count into a register (0)
   B:  Adds value 2 to register
   B:  Writes register value (2) back to memory. this.count now equals 2
   A:  Adds value 3 to register
   A:  Writes register value (3) back to memory. this.count now equals 3
```

我们如果按照上述的执行顺序，counter最后结果会是3，但我们都知道最后的值应该为5才对，但由于上述指令是交叉执行的，所以最后会导致不一样的结果。

# 临界区的资源竞速

add方法中包括了一个临界区，当多个线程访问临界区时，就会出现资源竞速的问题。
更标准的说，当两个或多个线程竞争同一个资源时，对资源的访问顺序就变的很关键了，这就是资源竞速的问题，一段代码块导致资源竞速的问题就是临界区代码。

# 避免资源竞速

为了避免资源竞速的问题，我们必须保证临界区的代码必须原子性。这就意味着当一个线程进入临界区执行时，其他线程不能进入这个线程执行，除非那个线程离开了临界区，也就是说只有一个线程能在临界区执行在某个时刻。

为了避免资源竞速的问题，我们可以使用synchronized关键字同步代码块，或者使用lock对象，或者使用atomic variables。我们将在后续文章中讲述。

# 临界区的吞吐量

对于小的临界区，我们直接将整个代码块标为synchronized就可以避免资源竞速了。但是对于较大的临界区，我们为了执行效率，最好将代码分为小的临界区，并分别同步的不同的临界区，因为我们知道synchronized的关键字的影响是比较大的。如果我们直接同步整个临界区，很可能会影响临界区的吞吐量。
我们通过下面这个例子来具体讲述：
```
public class TwoSums {
    
    private int sum1 = 0;
    private int sum2 = 0;
    
    public void add(int val1, int val2){
        synchronized(this){
            this.sum1 += val1;   
            this.sum2 += val2;
        }
    }
}
```

为了避免资源竞速，我们将两个加法操作同步了，这样的话，在某一个时刻，只有一个时刻可以执行加法操作。

然而，因为两个sum变量都是互相独立的，所以我们可以将两个变量分别分成不同的同步块中：
```
public class TwoSums {
    
    private int sum1 = 0;
    private int sum2 = 0;

    private Integer sum1Lock = new Integer(1);
    private Integer sum2Lock = new Integer(2);

    public void add(int val1, int val2){
        synchronized(this.sum1Lock){
            this.sum1 += val1;   
        }
        synchronized(this.sum2Lock){
            this.sum2 += val2;
        }
    }
}
```
