> * 线程死锁（Thread Deadlock）
* 数据库死锁（Database Deadlocks）
* 死锁避免 （Deadlock Prevention）
* Lock Ordering
* Lock Timeout
* Deadlock Detection

# 线程死锁（Thread Deadlock）

死锁就是当两个或者多个线程阻塞了 ，正在等到所需要的锁，但这些锁被其他也在等待的线程锁持有。死锁常常发生在多个线程在同一个时刻，需要同一些锁，但是他们获取锁的顺序有事交叉的，这样就会发生死锁的现象。

举个例子，如果线程1持有锁a,尝试去获取锁b，线程2已经拥有了锁b，尝试去获取锁a，这样就出现了死锁的情况。两个线程都在对方已经持有的锁，线程1得不到b，线程2得不到a，而且这两个线程并不知道他们目前的死锁情况，就会一直保持阻塞的等待状态。

如下面所示
```
Thread 1  locks A, waits for B
Thread 2  locks B, waits for A
```

我们看一个实际的例子：
```
public class TreeNode {
 
  TreeNode parent   = null;  
  List     children = new ArrayList();

  public synchronized void addChild(TreeNode child){
    if(!this.children.contains(child)) {
      this.children.add(child);
      child.setParentOnly(this);
    }
  }
  
  public synchronized void addChildOnly(TreeNode child){
    if(!this.children.contains(child){
      this.children.add(child);
    }
  }
  
  public synchronized void setParent(TreeNode parent){
    this.parent = parent;
    parent.addChildOnly(this);
  }

  public synchronized void setParentOnly(TreeNode parent){
    this.parent = parent;
  }
}
```

我们假设线程1调用parent.addChild(child)方法同时，另一个线程2调用child.setParent(parent)方法，在同一个parent对象和child对象上。这样就发生了死锁的情况。
```
Thread 1: parent.addChild(child); //locks parent
          --> child.setParentOnly(parent);

Thread 2: child.setParent(parent); //locks child
          --> parent.addChildOnly()
```

首先，线程1调用parent.addChild(child)方法，parent.addChild(child)方法是一个synchronize方法，会先取得parent对象的锁，这样其他线程就无法访问parent。

然后线程2调用child.setParent(parent)方法，因为child.setParent(parent)方法也是synchronize，线程2会取得child对象的锁，这样其他线程就无法访问child。

现在，child和parent对象都被两个不同的线程锁住了。下一步线程1执行内部的child.setParentOnly()方法，但是这时child已经被线程2获得了锁，所以线程1这个时候就进入阻塞等待。线程2也尝试去执行内部的parent.addChildOnly() 方法，但是parent对象被线程1锁住了，所以线程2也进入阻塞等待。现在两个线程都阻塞了，等待着对方的锁。

** 两个线程一定要同时调用parent.addChild(child) and child.setParent(parent)方法，而且必须是两个同样的实例parent和child，这样死锁才会发生。**

线程必须同时获得各自的锁才行。举个例子，如果线程1比线程2稍微快一点，以至于线程1一下获得了a和b两个锁。线程2慢一点，在获得b锁的时候就阻塞了，那么这个时候死锁就不会发生了。
由于线程的schedule是不可预测的，所以我们无法预测什么时候死锁会发生，只能做出判断，这种情况下可能会发生死锁。

# 更复杂的死锁情况
有时候会出现多个线程死锁的情况，这样情况比较复杂，很难探测到，比如下图
```
Thread 1  locks A, waits for B
Thread 2  locks B, waits for C
Thread 3  locks C, waits for D
Thread 4  locks D, waits for A
```
以上多个线程进入了循环等待的状态

# 数据库死锁

更复杂的死锁情况，是在数据库的事务中发生的。一个数据库的事务可能会包括很多sql的更新语句。当一条记录在一个事务期间被更新的时候，这个记录就被锁住了，以阻止其他事务也更新这条记录，直到这个事务完成才会释放这个锁。在一个事务期间，每一个更新请求丢能会锁住多条记录。

如果多个事务同时运行，而且需要更新相同的记录的时候，那么很可能就会发生死锁的情况。
```
Transaction 1, request 1, locks record 1 for update
Transaction 2, request 1, locks record 2 for update
Transaction 1, request 2, tries to lock record 2 for update.
Transaction 2, request 2, tries to lock record 1 for update.
```

由于不同的请求中会重复持有这些锁，而且不是所有事务的所需要的锁都事前知道，所以很难检测或者预测数据库事务中的死锁。

# 死锁避免（Deadlock Prevention）
在某些情况，我们可以利用一些方法阻止死锁的发生。主要有一下三种方法
* lock ordering 
* lock timeout
* Deadlock Detection

# lock ordering

死锁的发生通常是以为多个线程需要相同的锁，但是获取这些锁的顺序却不一样。

如果我们保证所有的线程都是以一个相同的顺序获得锁的话，那么就可以避免死锁的发生了。
看下面这个例子：
```
Thread 1:

  lock A 
  lock B


Thread 2:

   wait for A
   lock C (when A locked)


Thread 3:

   wait for A
   wait for B
   wait for C
```

像上个例子中的线程3，需要三个锁，那我们就要保证他的锁必须按顺序获得，它不能先获得c再获得a，如果没有获得a，那么b和c即使有，也不能获取，这样就保证了lock ordering

举个例子，线程2和3都不可以获得c锁，除非他们已经获得了a锁。当线程1获得a锁的时候，线程2和3不会获得b和c，而是会等到线程1释放a锁后，他们先获得a锁才会再获得b或c锁。

lock ordering是一个简单有效的避免死锁的方法。但是它要求必须知道所有线程需要的锁，这通常是不好获得的

# Lock Timeout

lock timeout就是当线程去获取一个锁，在给定的timeout的时间内没有得到话，就放弃，而且将它目前所持有的锁都释放，然后等待一段随机的时间之后，重新去获取锁。这个等待的随机时间就给了其他线程去获取锁的时间。
Here is an example of two threads trying to take the same two locks in different order, where the threads back up and retry:
看下面这个例子：
```
Thread 1 locks A
Thread 2 locks B

Thread 1 attempts to lock B but is blocked
Thread 2 attempts to lock A but is blocked

Thread 1's lock attempt on B times out
Thread 1 backs up and releases A as well
Thread 1 waits randomly (e.g. 257 millis) before retrying.

Thread 2's lock attempt on A times out
Thread 2 backs up and releases B as well
Thread 2 waits randomly (e.g. 43 millis) before retrying.
```

在上面那个例子中，线程2会比线程1快大概200ms结束随机等待的时间。所以线程2结束等待的时候，就可以成功获取两个锁。然后当线程2 结束释放锁，线程1也可以获得锁，这样就很好的避免了死锁的发生。

需要谨记注意的是，线程发生timeout的时候，并不说明线程发生了死锁的情况。只能说明线程持有了部分锁很长时间但又没法执行。

而且，如果线程足够多的话，那么一次timeout可能依然无法解决死锁的情况，需要重复尝试多次。

lock timeout方法的一个问题就是我们不可能给synchronize语句块设置一个timeout时间。所以我们可能创建一个自定义的锁去解决这个问题。这个问题我们将在后面介绍。

# Deadlock Detection死锁探测

死锁探测是一个效率很低消耗比较大的避免死锁的方法。通常在lock ordering或者lock timeout不可用的时候可以使用死锁探测。

我们设计一个数据结构，他会记录线程获得锁的情况和请求锁的情况。

当一个线程请求一个锁，但是被拒绝的时候，线程就会去检查那个记录数据结构，探测是否发生了死锁的情况。举个例子，如果线程a请求lock 7，但是lock 7正在被线程b持有。那么线程a就会去检查，线程b请求的锁里有没有线程a已经持有的。如果有，那么死锁就发生了，如果没有，就没有检测到死锁。

Below is a graph of locks taken and requested by 4 threads (A, B, C and D). A data structure like this that can be used to detect deadlocks.
下图就是一个死锁检测的例子：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-74c61e005dae1514.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

So what do the threads do if a deadlock is detected?
那么当探测到死锁发生的时候我们接下来应该怎么做呢？

一个可能的方法就是释放所有持有的锁，等待一段随机的时间，类似于lock timeout的情况。

一个更好的方法是给线程分配不同的优先级，所以只有部分线程释放锁。其他线程继续持有锁，等待其他线程释放的锁。
