> * 利用共享对象实现通信
* 忙等（busy waiting）
* wait(), notify() and notifyAll()
* 信号丢失（Missed Signals）
* 虚假唤醒（Spurious Wakeups）
* 多个线程等待相同的信号
* 不要对String对象或者全局对象调用wait方法

线程通信的目的就是让线程间具有互相发送信号通信的能力。
而且，线程通信可以实现，一个线程可以等待来自其他线程的信号。举个例子，一个线程B可能正在等待来自线程A的信号，这个信号告诉线程B数据已经处理好了。

# 利用共享对象实现通信

一个实现线程通信的简单的方式就是通过在某些共享的对象变量中设置一个信号值。举个例子，线程A在一个synchronize的语句块中设置一个boolean的成员变量hasDataToProcess为true，线程B在一个synchronize语句块中读取hasDataToProcess，如果为true就执行代码，否则就等待。这样就实现了线程A对线程B的通知。看下面的代码实现：
```
public class MySignal{

  protected boolean hasDataToProcess = false;

  public synchronized boolean hasDataToProcess(){
    return this.hasDataToProcess;
  }

  public synchronized void setHasDataToProcess(boolean hasData){
    this.hasDataToProcess = hasData;  
  }
}
```

线程A和B都必须拥有同一个MySignal类的对象实例的引用。如果线程拥有的是不同的实例，那么他们就无法获取到对方的信号。

# 忙等（busy waiting）

线程B执行的条件是，等待线程A发出通知，也就是等到线程A将hasDataToProcess()设置为true，所以线程b一直在等待信号，在一个循环的检测条件中。这时候线程B就处于一个忙等的状态。，因为线程b在等待的过程中是忙碌的，因为线程B在不断的循环检测条件是否成功。
```
protected MySignal sharedSignal = ...

...

while(!sharedSignal.hasDataToProcess()){
  //do nothing... busy waiting
}
```

# wait(), notify() and notifyAll()

忙等对于cpu的利用不是一个有效率的选择，除非忙等的时间是非常短的。不然，与其让线程处于忙等的状态，不如直接让线程直接sleep，直到它收到信号再重新激活它。

Java有一个内置的方法，可以让线程在等待信号的变为inactive状态。所有类的超类 java.lang.Object 定义了三个方法， wait(), notify(), and notifyAll()

一个线程可以对任何一个对象调用wait方法，这样这个线程就会变成wait状态，inactive，等待其他线程在同一个对象上调用notify方法，来唤醒这个线程。值得注意的是，在调用wait和notify方法之前，必须要先获得这个对象的锁。换句话说，线程必须在synchronize的语句块中调用wait或者notify方法。看下面的代码实例：
```
public class MonitorObject{
}

public class MyWaitNotify{

  MonitorObject myMonitorObject = new MonitorObject();

  public void doWait(){
    synchronized(myMonitorObject){
      try{
        myMonitorObject.wait();
      } catch(InterruptedException e){...}
    }
  }

  public void doNotify(){
    synchronized(myMonitorObject){
      myMonitorObject.notify();
    }
  }
}
```

等待的线程可以调用dowait方法，notify线程可以调用donotify方法。当一个线程在一个对象上调用notify方法的时候，这个对象的等待线程队列中的一个线程会被唤醒，获得执行的权利。notifyAll方法则是会将给定对象的等待队列中的所有线程都唤醒。

我们可以看到我们调用wait或者notify方法的时候，都是在synchronize语句块中调用的。这是一个必要条件。一个线程如果没有取得相关对象的锁则无法调用wait和notify方法，会抛出IllegalMonitorStateException异常。

一旦一个线程调用wait方法，他就会释放锁，这就允许其他线程去继续调用wait方法或者notify方法，所以这些方法都必须出现在synchronize语句块中。

一个线程如果被唤醒了，不会立即离开wait方法，因为还没获得锁，要等到那个调用notify的线程离开他的synchronize的语句块，也就是等待他释放锁，才可以获得锁，离开wait。换句话说，换句话，线程要离开wait方法，必须重新获得锁相应对象的锁。如果多个线程被notifyall方法唤醒，那么在某一个时刻，只有一个被唤醒的线程可以离开wait方法，因为每个都必须重新获得锁才可以离开wait方法。

# 信号丢失（Missed Signals）

如果在调用notify或者notifyAll的时候，线程等待队列中，没有线程在等待，那么这个唤醒的信号并不会被保存。而是会丢失。所以，如果一个线程在另一个线程调用wait方法等待之前，就调用了notify方法，那么这个notify的信号就被丢失了，这就可能导致那个等待的线程将一直不会被唤醒，因为notify的唤醒信号丢失了。

To avoid losing signals they should be stored inside the signal class. In the MyWaitNotify example the notify signal should be stored in a member variable inside the MyWaitNotify instance. Here is a modified version of MyWaitNotify that does this:
为了避免信号的丢失，我们可以想办法将信号存起来，利用一个变量。如下面这个例子：
```
public class MyWaitNotify2{

  MonitorObject myMonitorObject = new MonitorObject();
  boolean wasSignalled = false;

  public void doWait(){
    synchronized(myMonitorObject){
      if(!wasSignalled){
        try{
          myMonitorObject.wait();
         } catch(InterruptedException e){...}
      }
      //clear signal and continue running.
      wasSignalled = false;
    }
  }

  public void doNotify(){
    synchronized(myMonitorObject){
      wasSignalled = true;
      myMonitorObject.notify();
    }
  }
}
```

我们可以看到，上面的方法在调用notidy之前先将wasSignalled设置为true。dowait方法会先检查wasSignalled变量，如果为true，就直接跳过wait方法，因为已经有notify信号发出了。如果为false，则说明还没有信号发出，就进入wait方法，进行等待。所以，我们利用一个boolean变量就可以解决通知过早的问题。

# 虚假唤醒（Spurious Wakeups）

有时候因为某些原因，线程可能会在没有调用notify或者notifyAll的情况下被唤醒，这也叫做虚假唤醒（Spurious Wakeups）。如果一个线程被虚假唤醒就会产生很多意想不到的问题，所以必须重视这个问题。

我们使用一个自旋锁机制，也就是用while循环替代if循环，循环检查这样就可以避免虚假唤醒的情况。
```
public class MyWaitNotify3{

  MonitorObject myMonitorObject = new MonitorObject();
  boolean wasSignalled = false;

  public void doWait(){
    synchronized(myMonitorObject){
      while(!wasSignalled){
        try{
          myMonitorObject.wait();
         } catch(InterruptedException e){...}
      }
      //clear signal and continue running.
      wasSignalled = false;
    }
  }

  public void doNotify(){
    synchronized(myMonitorObject){
      wasSignalled = true;
      myMonitorObject.notify();
    }
  }
}
```

wait方法现在放在了一个while循环里，如果一个线程被唤醒，但是没有获得信号，那么wasSignalled 仍是false，while循环会进行多次判断，重新将线程变为wait。

我们更好的理解，我们举一个具体的例子：
假设有两个类负责加减：
```
package Thread;

public class Add {
	private String lock;
	
	public Add(String lock) {
		super();
		this.lock = lock;
	}
	
	public void add() {
		synchronized (lock) {
			ValueObject.list.add("anything");
			lock.notifyAll();
		}
	}
}

```
```
package Thread;

public class Subtract {
	private String lock;
	public Subtract(String lock) {
		super();
		this.lock = lock;
	}
	
	public void subtract() {
		try {
			synchronized (lock) {
				if(ValueObject.list.size() == 0) {
					System.out.println("Wait begin ThreadName:" + Thread.currentThread().getName());
					lock.wait();
					System.out.println("Wait end ThreadName:" + Thread.currentThread().getName());
				}
				ValueObject.list.remove(0);
				System.out.println("list size : " + ValueObject.list.size());
			}
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
}

```

```
package Thread;

import java.util.ArrayList;
import java.util.List;

public class ValueObject {
	public static List<String> list = new ArrayList<>();
}

```

我们建立两个线程
```
package Thread;

public class ThreadAdd extends Thread {
	
	private Add p;
	
	public ThreadAdd(Add p) {
		this.p = p;
	}
	
	@Override
	public void run() {
		p.add();
	}
}

```

```
package Thread;

public class ThreadSubtract extends Thread {
	
	private Subtract p;
	
	public ThreadSubtract(Subtract p) {
		this.p = p;
	}
	
	
	@Override
	public void run() {
		p.subtract();
	}
}

```

我们测试
```
package Thread;

public class Run {

	public static void main(String[] args) throws InterruptedException {
		
		String lock = new String("");
		Add add = new Add(lock);
		Subtract sub = new Subtract(lock);
		
		ThreadAdd addthread = new ThreadAdd(add);
		
		ThreadSubtract sub1 = new ThreadSubtract(sub);
		sub1.start();
		
		ThreadSubtract sub2 = new ThreadSubtract(sub);
		sub2.start();
		
		Thread.sleep(1000);
		addthread.start();

	}

}
```

![image.png](http://upload-images.jianshu.io/upload_images/1234352-9346a3470d9b6fee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们发现发生了异常，这是为什么呢？因为notifyAll同时唤醒了两个减的线程，然后第二个减的线程获得了锁，将size减为0，随后第一个减线程获得锁，再去减就抛异常了，因为它没有继续判断是否为0的条件，所以我们需要在获得锁之后依然去判断条件，也就是将if改为while

```
package Thread;

public class Subtract {
	private String lock;
	public Subtract(String lock) {
		super();
		this.lock = lock;
	}
	
	public void subtract() {
		try {
			synchronized (lock) {
				while(ValueObject.list.size() == 0) {
					System.out.println("Wait begin ThreadName:" + Thread.currentThread().getName());
					lock.wait();
					System.out.println("Wait end ThreadName:" + Thread.currentThread().getName());
				}
				ValueObject.list.remove(0);
				System.out.println("list size : " + ValueObject.list.size());
			}
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
}

```


![image.png](http://upload-images.jianshu.io/upload_images/1234352-86ee8724dbf989d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这样就可以正确运行了。

# 多个线程等待相同的信号

如果你有多个线程在等待队列中，然后你又要调用notifyAll方法，那么使用while来替代if，是一个很好的解决虚假唤醒的方法。只有一个线程在一个时刻会被唤醒，然后可以获得锁，离开wait方法，并清楚wasSignalled 的标识，一旦这个线程离开了synchronize的语句块，其他线程可以获得锁并且离开wait方法。但是，由于wasSignalled 被第一个线程清除了，其他等待的线程因为while的存在会继续回到wait的状态，知道下一个信号来了

# 不要对String对象或者全局对象调用wait方法
如果我们对一个String对象调用wait方法
```
public class MyWaitNotify{

  String myMonitorObject = "";
  boolean wasSignalled = false;

  public void doWait(){
    synchronized(myMonitorObject){
      while(!wasSignalled){
        try{
          myMonitorObject.wait();
         } catch(InterruptedException e){...}
      }
      //clear signal and continue running.
      wasSignalled = false;
    }
  }

  public void doNotify(){
    synchronized(myMonitorObject){
      wasSignalled = true;
      myMonitorObject.notify();
    }
  }
}
```

如果我们在一个空emptyString或者其他的常量String对象上调用wait方法会产生问题。JVM/Compiler 在内部将常量的String变成相同的对象。这就意味着，即使我们有两个不同的MyWaitNotify实例，他们确实引用着同一个对象。这就意味着本来不相关的两个实例，最后通信的结果可能发生不可预测的交叉结果。
如下图所示：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-c5497f3e076dd4ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要注意的是，即使四个线程调用wait和notify都是在同一个对象上的，但是信号都是存储在各自的实例中的，也就是wasSignal是存储在各自实例中的，这就会引起很大的问题。一个来自MyWaitNotify 1的信号可能会唤醒MyWaitNotify 2中的等待线程，但是wasSignal确实存在MyWaitNotify 1中的。

如果notify作用在第二个实例上MyWaitNotify 2，那就可能发生线程A和B被唤醒的情况，但是线程A和B会在while循环中检查wasSignal信号，结果发现依然是false，就会继续等待，所以notify并没有起到作用，这就类似虚假唤醒的情况。

这样发生的情况就是，如果我们调用notify方法，然后notify的又不是自己这个实例的线程，结果就没有线程会被唤醒，这就类似于信号丢失的情况。

但如果我们调用的notifyAll方法就不会出现信号丢失的情况，因为wasSignal会被正确的设置，相应的线程会被唤醒，其他对象的线程会因为while循环继续回到wait状态。

那你也许会说，我们直接调用notifyAll不就可以避免String带来的问题么？确实是这样，但是我们如果在全部情况都调用notifyAll的话，就会出现性能的问题，我们完全没有必要在只有一个线程的情况下，调用notifyAll。

所以，我们不要使用全局的对象或者String变量调用wait。
