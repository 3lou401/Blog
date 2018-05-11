> * 引入CountDownLatch类
* CountDownLatch类的具体实例
* CountDownLatch小结

# 引入CountDownLatch类
Java在JDK1.5之后引入了CountDownLatch类。这个类是一个同步辅助类。用于一个线程等待多个操作完成之后再执行，也就是这个当前线程会一直阻塞，直到它所等待的多个操作已经完成。首先CountDownLatch类会初始化，设置它需要等待完成的操作的数量。然后每当一个操作完成之后，就会调用countDown方法，这个方法会将CountDownLatch内部的计数器减一。当减为0的时候，CountDownLatch类会唤醒所有调用await方法而进入休眠的线程。


# CountDownLatch类的具体实例
多说无意，我们具体看一个实例就可以理解CountDownLatch类的使用了。

我们举一个最直观的例子，比如我们需要开一个视频会议，这个会议需要等待一定的人数到达之后，才开始会议。这种情况就非常适合使用CountDownLatch类来进行同步，也就是等待多个并发事件的发生，因为每个参会人员的到达是并发的。


首先我们实现会议类，这个类持有一个CountDownLatch类的对象，并且定义了一个arrive方法，每当会议人员到达之后，就会调用这个方法，告诉会议已经到达了，这个方法，会调用CountDown方法，将计数器减一。

在会议类的run方法中，在宣布会议开始之前，会调用CountDownLatch类的await方法休眠，直到countDown减为0，也就是计数器减为0，说明所有的人都到了，才唤醒继续这个线程的代码，宣布会议开始
```
package CountDown;

import java.util.concurrent.CountDownLatch;
import java.util.concurrent.TimeUnit;

public class VideoConference implements Runnable  {
	
	private final CountDownLatch controller;
	
	public VideoConference(int number) {
		controller = new CountDownLatch(10);
	}
	
	public synchronized void arrive(String name) {
		
		
		System.out.println(name + " has arrived.");
		
		System.out.println("VideoConference : waiting for " + (controller.getCount() - 1) + " partivipants");
		
		
		controller.countDown();
	}
	
	@Override
	public void run() {
		System.out.println("VideoConference : Innitiaization : " + controller.getCount() + " participants");
		
		try {
			controller.await();
			System.out.println("All people has come");
			System.out.println("Let's start...");
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}

}
```

接着我们实现参会人员的类，这个类也实现了runnable接口，首先它持有一个会议对象，为了在执行的时候调用arrive方法，告诉会议人员到了。

```
package CountDown;

import java.util.concurrent.TimeUnit;

public class Participant implements Runnable {

	private VideoConference conference;
	
	private String name;
	
	public Participant(VideoConference conference, String name) {
		this.conference = conference;
		this.name = name;
	}
	
	
	@Override
	public void run() {
		long duration = (long)Math.random() * 10;
		
		try {
			TimeUnit.SECONDS.sleep(duration + 10);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		
		conference.arrive(name);
	}

}

```
最后我们实现测试类
```
package CountDown;

public class Main {

	public static void main(String[] args) {
		
		VideoConference conference = new VideoConference(10);
		
		new Thread(conference).start();
		
		for(int i=0;i<10;i++) {
			Participant p = new Participant(conference, "Participant " + i);
			new Thread(p).start();
		}
	}
}

```

运行结果：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-7538f9703d6d29f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


await方法还可以指定休眠的时间，当休眠时间到了或者计数器减为0，就会唤醒所有被CountDownLatch休眠的线程，那我们在这里就可以使用这个休眠时间来设置，我们只等10s中，如果10s中，还有人没到，我们也不管了，先开始会议。

只需对run方法进行简单的修改：
```
@Override
	public void run() {
		System.out.println("VideoConference : Innitiaization : " + controller.getCount() + " participants");
		
		try {
			controller.await(10, TimeUnit.SECONDS);
			if(controller.getCount() == 0) {
				System.out.println("All people has come");
				System.out.println("Let's start...");
			}
			else {
				System.out.println("不等了");
				System.out.println("Let's start...");
			}
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
```

运行结果

![image.png](http://upload-images.jianshu.io/upload_images/1234352-d8d3572fb435ca1b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# CountDownLatch小结

CountDownLatch有三个基本的要素：
* 一个初始值，定义必须等待多少个并发线程完成的数目
* await方法，需要等到其他操作先完成的那个线程调用的，先将线程休眠，直到其他操作完成，计数器减为0，才会唤醒因此休眠的线程
* countDown方法，每个被等待的事件在完成之后调用，会将计数器减一

* CountDownLatch不是用来保护临界区和共享资源的，是用来同步执行线程和操作的

* CountDownLatch是一次性的，当计数器减为0 之后，这个类就相当于没用，我们之后对它的操作都不起作用，需要新建一个countDownLatch类
