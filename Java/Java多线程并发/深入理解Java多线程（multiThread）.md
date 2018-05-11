# 多线程的基本概念
一个java程序启动后，默认只有一个主线程（Main Thread）。如果我们要使用主线程同时执行某一件事，那么该怎么操作呢？
例如，在一个窗口中，同时画两排圆，一排在10像素的高度，一排在50像素的高度。
如果只能在一个主线程里写出同时执行的程序，那就只能先在高度10画个圆，再快速在高度50画一个圆，平移后再在高度10画一个圆，....依此循环下去：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-3bde40ec3f342406.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

假设，我们考虑，如果我们有两个线程，一个在高度10的地方画自己的圆，另一个在高度50的地方画自己的圆，互不干扰。那么程序写起来就很简单，我们只要考虑一个线程怎么在自己的高度上画圆就可以了。这就是java中多线程的简单引入。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-f7a8948e582fae15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

要在java中新建一个线程，可以继承实现Runnable接口，这个接口只有一个run方法需要实现，run方法就是一个可执行线程的进入点，run方法中的内容就是一个可执行的线程的执行内容。

```
public class CirclePainter implements Runnable {
    public CirclePainter(int x, int y, int r, int offset) {
        ....
    } 

    public void run() {
        while(...) {
           .. 在(x, y) 畫半徑 r 的 圓
           ... 平移 offset
        }
    } 
}
```

** JVM本身是个虚拟的系统，.class类就是JVM可执行的程序，一般假设，JVM只有一个CPU来执行可执行的.class程序，这个虚拟的CPU就是一个主线程，执行程序的程式入口就是main()方法。 **

** 如果你想在虚拟的系统中多使用几个CPU，那就可以建立执行程序，给每个执行程序写好执行代码，然后启动程序线程执行就行 **

```
Thread painterThread1 = new Thread(new CiclePainter(50, 10, 10));
Thread painterThread2 = new Thread(new CiclePainter(50, 50, 10));
painterThread1.start();
painterThread2.start();
```

新建的多线程Thread,执行的入口就是run方法，一旦执行完run方法之后，该线程就会被回收，如果执行完再反复调用就会发生错误。

Thread.start()方法会执行run方法中的代码，这是定义在在Thread的run()方法中，实际上Thread也继承实现了Runnable接口：
```
public class Thread implements Runnable {
    ...
    private Runnable target;
    ....
    public void run() {
        if (target != null) {
            target.run();
        }
    }
    ...
}
```

显然，我们也可以继承Thread类，实现它的run方法，通过这种方式来新建一个线程，但需要注意的是，一旦我们继承Thread，那么这个就一定一个Thread,这样就不能再继承其他类了，显然失去了灵活性，所以一般我们都是继承runnable接口。

# 线程同步
在执行多线程的时候，如果有两个或多个线程操作同样的共享代码或者数据时，就需要引起注意，这样可能引发线程同步的问题，导致不可预知的程序结果。出现这种问题原因是因为“（Race condition）”资源竞速产生的。

举个简单的例子来说，如果我们开发一个简单的stack类：
```
public class Stack {
    private int[] data;
    private int index;
    public Stack(int capacity) {
        data = new int[capacity];
    }
    public void put(int d) {
        data[index] = d;
        index++;
    }
    public int pop() {
        index--;
        return data[index];
    }
}
```
这个程序在单线程执行的时候，没有问题，但如果在多线程的执行的情况下，就可能出现race conditions的问题。假设在多线程的情况下，某个线程的run方法执行到put方法时，
```
public class Some implements Runnable {
    private Stack stack;
    ...
    public void run() {
        ....
        stack.put(d);
        ...
    }
}
```
假设index是2，执行完put的第一行代码时，那么下面应该接下来执行index++的操作，但假设此时另一个线程正在执行pop():
```
public class Other implements Runnable {
    private Stack stack;
    ...
    public void run() {
        ....
        int p = stack.pop();
        ...
    }
}
```
那么index--就变成了1，假设，执行完这句话后，又切换回执行put方法，执行index++,那么最后index变成了2.但其实执行完put方法本来应该是3的，所以这时候就因为race conditions出现了程序的错误，造成了难以预知的运行结果。

实际上，我们可以想见，put方法和pop方法的index操作和取元素操作应该是不可切分的，需要一口气执行完。但由于多线程的存在，就可能打破这个顺序

那么自然想到，如果要解决这个问题，就需要将这几步不能拆分的操作放在一个必须一次性执行完的代码区域，这就是线程同步的概念，线程同步的区域内，所有代码必须一次性执行完，当其在执行时，不会有其他线程插入进来。

使用synchronized关键字可以指定需要同步执行的代码范围，最基本的就是在方法前声明为synchronized，这样这个方法的代码就处在同步区域中。

```
public class Stack {
    private int[] data;
    private int index;
    public Stack(int capacity) {
        data = new int[capacity];
    }
    public synchronized void put(int d) {
        data[index] = d;
        index++;
    }
    public synchronized int pop() {
        index--;
        return data[index];
    }
}
```
实际上，每个对象里都会有一个lock对象，也叫做锁定，执行的线程要进入synchronized的区域中，必须取得这个对象的唯一的lock锁定。假设有一个线程正在synchronized中的代码块，那么另一个线程想要进入这个执行区域时，由于lock已经被取走了，所以只能等待另一个线程执行完代码，释放代码才行，所以这样就实现了线程的同步。
对于上面这个例子，显然执行put方法之前需要先取得stack的lock锁定。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-754dcd88be00200f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-b4e6dce5bb5a8ec3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以在这个例子中，如果在执行put方法，就无法执行pop方法，如果在执行pop方法，就无法执行put方法。就不会引发之前的错误。

进一步的，如果我们能清楚的知道，公用的存取范围是哪些代码块，那我门就没有必要将整个方法都声明synchronized，因为那样会降低效率，比如上个例子中，我们知道确切的共用代码块的范围：

```
public void put(int d) {
        ...
        synchronized(this) { 
            data[index] = d;
            index++;
        }
        ...
    }
    public int pop() {
        ...
        synchronized(this) { 
            index--;
            return data[index];
        }
        ...
    }
```

进一步的，synchronized语句还可以进行更精细的控制，提供不同的对象的锁定
如下面的例子：
```
public class Material {
    private int data1 = 0;
    private int data2 = 0;
    private Object lock1 = new Object();
    private Object lock2 = new Object();

    public void doSome() {
        ...
        synchronized(lock1) {
            ...
            data1++;
            ...
        }
        ...
    }

    public void doOther() {
        ...
        synchronized(lock2) {
            ...
            data2--;
            ...
        }
        ...
    }
}
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-c85e7d46df641e0c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在这个例子中，多个doSome方法无法同时执行，因为lock1锁定，同理，doOther方法无法同时执行，因为lock2锁定，但是doSome和doOther同时的执行是不干扰的，因为他们拥有不同的锁定，互不影响。

# wait，notify，notifyAll
wait和notify，notifyAll是由object所提供的方法，在定义自己的类的时候会被自动继承下来，由于在object中，wait，notify，notifyAll都被定义为final，所以我们无法修改重新定义他们，这三个方法的作用是通知参与竞争对象的锁定，或者是释放对象的锁定。

当执行程序进入synchronized区域时，会取得对象的锁定，在执行synchronized代码期间，如果使用对象的wait方法，就会释放对象的锁定，然后该执行程序就会被放入对象的等待集合中（wait set），这时候其他的线程就可以竞争锁定的目标，进入synchronized区域执行代码。

被放在wait set中的程序不会参加执行排版，而是一直等待notify方法或者interrupt方法调用才会参与排班，同时，wait方法可以指定wait的时间，那么就会在指定时间之后参与排班。

当调用被执行对象的notify方法时，会随机从对象的wait set里面取出一个线程参与排版执行，也就是恢复runnable状态，当你执行notifyAll方法时，就会从对象的wait set中取出所有的线程参与排班竞争。

举个简单的例子，这几个方法就好比你让一个做事，如果暂时不要他做事，就让他等一下wait，等到轮到他做事了，就调用notify方法，通知他做事。

说明这几个方法的最好例子就是生产者与消费者模式。生产者会生产商品交给店员，消费者会从店员处取走商品，店员只能持有一定数量的商品，超过商品限额，就会让生产者wait一下，待会再生产，如果没有商品了，就会让消费者wait一下。

下面来具体看程序的代码：
首先是生产者：
```
package Thread;

public class Producer implements Runnable {
	
	private Clerk clerk; 
    
    public Producer(Clerk clerk) { 
        this.clerk = clerk; 
    } 
    
    public void run() { 
        System.out.println(
                "生產者開始生產整數......"); 

        // 生產1到10的整數
        for(int product = 1; product <= 10; product++) { 
            try { 
                // 暫停隨機時間
                Thread.sleep((int) (Math.random() * 3000)); 
            } 
            catch(InterruptedException e) { 
                e.printStackTrace(); 
            } 
            // 將產品交給店員
            clerk.setProduct(product); 
        }       
    } 

	public static void main(String[] args) {
		Clerk clerk = new Clerk(); 

        Thread producerThread = new Thread(new Producer(clerk)); 
        Thread consumerThread = new Thread(new Consumer(clerk)); 
 
        producerThread.start(); 
        consumerThread.start();

	}

}

```

消费者
```
package Thread;

public class Consumer implements Runnable {
private Clerk clerk; 
    
    public Consumer(Clerk clerk) { 
        this.clerk = clerk; 
    } 
    
    public void run() { 
        System.out.println(
                "消費者開始消耗整數......"); 

        // 消耗10個整數
        for(int i = 1; i <= 10; i++) { 
            try { 
                // 等待隨機時間
                Thread.sleep((int) (Math.random() * 3000)); 
            } 
            catch(InterruptedException e) { 
                e.printStackTrace(); 
            } 

            // 從店員處取走整數
            clerk.getProduct(); 
        } 
    } 
}

```
店员
```
package Thread;

public class Clerk {
	// -1 表示目前沒有產品
    private int product = -1; 
 
    // 這個方法由生產者呼叫
    public synchronized void setProduct(int product) { 
        while(this.product != -1) { 
            try { 
                // 目前店員沒有空間收產品，請稍候！
                wait(); 
            } 
            catch(InterruptedException e) { 
                e.printStackTrace(); 
            } 
        } 
 
        this.product = product; 
        System.out.printf("生產者設定 (%d)%n", this.product); 

        // 通知等待區中的一個消費者可以繼續工作了
        notify(); 
    } 
    
    // 這個方法由消費者呼叫
    public synchronized int getProduct() { 
        while(this.product == -1) { 
            try { 
                // 缺貨了，請稍候！
                wait(); 
            } 
            catch(InterruptedException e) { 
                e.printStackTrace(); 
            } 
        } 
 
        int p = this.product; 
        System.out.printf(
                  "消費者取走 (%d)%n", this.product); 
        this.product = -1; 
 
        // 通知等待區中的一個生產者可以繼續工作了
        notify(); 
       
        return p; 
    } 

}
```

程序执行的结果：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-f42ad40eef602867.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 线程的生命周期

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-7869fb3581ab8b63.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当你实例化一个thread对象的时候，你必须使用start方法调用他，start只能执行一次，如果重复执行thread方法就会产生异常，执行start方法，程序并非立即执行，而是进入runnable状态，进行执行排班的等待，等待分配cpu进行执行。

执行程序有优先级，你可以指定优先级setPriority进行优先级的设定。

执行程序一旦执行完就会进入dead状态，可以使用isAlive方法来判断程序是否仍存活，如果在程序死亡后，再次调用start方法就会抛出异常。

当执行程序由于IO等待或者因为执行thread.sleep方法之后，就会进入阻断状态，blocked,阻断条件消失，就会进入runnable状态，等待cpu排班执行

当执行程序进入synchronized区域时，必须先进入lock pool进行锁定的竞争，才能进入可执行状态进入cpu的排班。

执行中的对象取得了锁定正在执行，但是，由于调用了wait方法，就会释放锁定，并且进入等到池中，等待notify，或者notifyAll方法，再进入锁定池，进行锁定的竞争，取得锁定后，再进入可执行状态，得到cpu的排班执行。
