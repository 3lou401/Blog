> * 引入Executor
* 创建Executor
* 创建固定大小的线程Executor

# 引入Executor
我们在开发Java多线程程序的时候，往往会创建很多个Runnable对象，然后创建对应的Thread对象来执行它们。但是，如果需要开发一个大量的并发任务，过多的任务就会导致下面这些问题：

* 必须给每个Runnable对象创建一个Thread，也就意味着要创建相关的线程创建，结束，取结果的代码，代码很冗余
* 过多的Thread对象对于降低了应用程序的效率，系统负荷过重

从Java 5之后，引入了一套API框架用来解决这个问题。这套新的框架就是执行器框架（Executor Framework）,围绕着Executor接口和它的自接口的ExecutorService，以及实现这两个接口的ThreadPoolExecutor类。

部分继承关系：

![image.png](http://upload-images.jianshu.io/upload_images/1234352-cd8df343470364cb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这套框架分离了任务的创建和执行。使用Executor，只要将Runnable对象，直接丢给执行器就可以了。Executor会自己创建线程，来负责这些Runnable对象任务的执行。Executor有一个好处就是利用线程池提高性能，当收到一个新任务时，会尝试使用线程池中的空闲线程来执行，避免了重复创建过多的线程而导致系统性能的下降。

# 创建Executor
使用Executor的第一步就是创建一个线程池对象，java提供了Executors的工厂类，可以帮我们创建不同的线程池对象


![image.png](http://upload-images.jianshu.io/upload_images/1234352-a98d3757ce1ccb75.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


然后调用Executor的execute方法执行相应的线程，并且要显示的结束线程池
server类：
```
package CreateExecutor;

import java.util.concurrent.Executors;
import java.util.concurrent.ThreadPoolExecutor;

public class Server {
	private ThreadPoolExecutor executor;

	public Server() {
		executor = (ThreadPoolExecutor)Executors.newCachedThreadPool();
	}
	
	public void executorTask(Task task) {
		System.out.println("a new task arrived");
		executor.execute(task);
		System.out.println("Server : Pool Size :" + executor.getPoolSize());
		System.out.println("Server : ActiveCount :" + executor.getActiveCount());
		System.out.println("Server : CompletedTaskCount :" + executor.getCompletedTaskCount());
		System.out.println();
	}
	
	public void endServer() {
		executor.shutdown();
	}
}
```

Task类用来创建多个runnable对象
```
package CreateExecutor;

import java.util.Date;
import java.util.concurrent.TimeUnit;

public class Task implements Runnable {

	private Date initDate;
	
	private String name;
	
	
	
	public Task(Date initDate, String name) {
		super();
		this.initDate = initDate;
		this.name = name;
	}



	@Override
	public void run() {
		
		System.out.printf("%s : Task %s : Created on : %s\n",
				Thread.currentThread().getName(),name,initDate);
		System.out.printf("%s : Task %s : Started on : %s\n",
				Thread.currentThread().getName(),name, new Date());
		
		//将任务休眠一段时间，模拟任务的执行
		try {
		Long duration = (long)(Math.random() * 10);
		System.out.printf("%s : Task %s : Doing a task during %d seconds\n",
				Thread.currentThread().getName(), name, duration);
		
		TimeUnit.SECONDS.sleep(duration);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		
		System.out.printf("%s : Task %s : Finished on : %s\n",
				Thread.currentThread().getName(), name, new Date());
	}
}

```

Main类测试：
```
package CreateExecutor;

import java.util.Date;

public class Main {

	public static void main(String[] args) {
		Server server = new Server();
		for(int i=0;i<10;i++) {
			Task task = new Task(new Date(), "Task " + i);
			server.executorTask(task);
		}
		server.endServer();
	}

}

```
运行结果

```
a new task arrived
Server : Pool Size :1
pool-1-thread-1 : Task Task 0 : Created on : Mon Jul 24 20:31:10 CST 2017
pool-1-thread-1 : Task Task 0 : Started on : Mon Jul 24 20:31:11 CST 2017
Server : ActiveCount :1
Server : CompletedTaskCount :0

a new task arrived
Server : Pool Size :2
Server : ActiveCount :2
Server : CompletedTaskCount :0

a new task arrived
Server : Pool Size :3
Server : ActiveCount :3
Server : CompletedTaskCount :0

a new task arrived
Server : Pool Size :4
Server : ActiveCount :4
Server : CompletedTaskCount :0

a new task arrived
Server : Pool Size :5
Server : ActiveCount :5
Server : CompletedTaskCount :0

pool-1-thread-2 : Task Task 1 : Created on : Mon Jul 24 20:31:11 CST 2017
pool-1-thread-2 : Task Task 1 : Started on : Mon Jul 24 20:31:11 CST 2017
pool-1-thread-5 : Task Task 4 : Created on : Mon Jul 24 20:31:11 CST 2017
pool-1-thread-4 : Task Task 3 : Created on : Mon Jul 24 20:31:11 CST 2017
pool-1-thread-3 : Task Task 2 : Created on : Mon Jul 24 20:31:11 CST 2017
pool-1-thread-1 : Task Task 0 : Doing a task during 3 seconds
pool-1-thread-3 : Task Task 2 : Started on : Mon Jul 24 20:31:11 CST 2017
pool-1-thread-3 : Task Task 2 : Doing a task during 8 seconds
pool-1-thread-4 : Task Task 3 : Started on : Mon Jul 24 20:31:11 CST 2017
pool-1-thread-4 : Task Task 3 : Doing a task during 7 seconds
pool-1-thread-5 : Task Task 4 : Started on : Mon Jul 24 20:31:11 CST 2017
pool-1-thread-2 : Task Task 1 : Doing a task during 9 seconds
pool-1-thread-5 : Task Task 4 : Doing a task during 1 seconds
pool-1-thread-5 : Task Task 4 : Finished on : Mon Jul 24 20:31:12 CST 2017
pool-1-thread-1 : Task Task 0 : Finished on : Mon Jul 24 20:31:14 CST 2017
pool-1-thread-4 : Task Task 3 : Finished on : Mon Jul 24 20:31:18 CST 2017
pool-1-thread-3 : Task Task 2 : Finished on : Mon Jul 24 20:31:19 CST 2017
pool-1-thread-2 : Task Task 1 : Finished on : Mon Jul 24 20:31:20 CST 2017
```

Executor的一个特性，必须通过显示的结束，如果不这么做，Executor会继续执行，线程也不会结束，程序也不会结束。如果Executor没有任务可执行了，它不会结束，会一直等待新的任务到来，而不会结束执行。

# 创建固定大小的线程Executor
上面的例子，对五个任务新生成了5个线程，为了重复利用线程，我们可以创建固定的线程数，Executors工厂类就提供了这么一个工厂方法。
这个Executor会有一个最大的线程最大数，如果发送超过这个任务数的任务给Executor，执行器不会再创建额外的线程，剩下的任务将被阻塞直到Executor有足够的空闲的线程可用。
```
package CreateFixedExecutor;

import java.util.concurrent.Executors;
import java.util.concurrent.ThreadPoolExecutor;

public class Server {
	private ThreadPoolExecutor executor;

	public Server() {
		executor = (ThreadPoolExecutor)Executors.newFixedThreadPool(5);
	}
	
	public void executorTask(Task task) {
		System.out.println("a new task arrived\n");
		executor.execute(task);
		System.out.println("Server : Pool Size :" + executor.getPoolSize());
		System.out.println("Server : ActiveCount :" + executor.getActiveCount());
		System.out.println("Server : TaskCount :" + executor.getTaskCount());
		System.out.println("Server : CompletedTaskCount :" + executor.getCompletedTaskCount());
	}
	
	public void endServer() {
		executor.shutdown();
	}
}

```

```
package CreateFixedExecutor;

import java.util.Date;
import java.util.concurrent.TimeUnit;

public class Task implements Runnable {

	private Date initDate;
	
	private String name;
	
	
	
	public Task(Date initDate, String name) {
		super();
		this.initDate = initDate;
		this.name = name;
	}



	@Override
	public void run() {
		
		System.out.printf("%s : Task %s : Created on : %s\n",
				Thread.currentThread().getName(),name,initDate);
		System.out.printf("%s : Task %s : Started on : %s\n",
				Thread.currentThread().getName(),name, new Date());
		
		//将任务休眠一段时间，模拟任务的执行
		try {
		Long duration = (long)(Math.random() * 10);
		System.out.printf("%s : Task %s : Doing a task during %d seconds\n",
				Thread.currentThread().getName(), name, duration);
		
		TimeUnit.SECONDS.sleep(duration);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		
		System.out.printf("%s : Task %s : Finished on : %s\n",
				Thread.currentThread().getName(), name, new Date());
	}
}

```

```
package CreateFixedExecutor;

import java.util.Date;

public class Main {

	public static void main(String[] args) {
		Server server = new Server();
		for(int i=0;i<10;i++) {
			Task task = new Task(new Date(), "Task " + i);
			server.executorTask(task);
		}
		server.endServer();
	}

}

```

运行结果：
```
a new task arrived

Server : Pool Size :1
pool-1-thread-1 : Task Task 0 : Created on : Mon Jul 24 20:40:07 CST 2017
Server : ActiveCount :1
Server : TaskCount :1
Server : CompletedTaskCount :0
a new task arrived

pool-1-thread-1 : Task Task 0 : Started on : Mon Jul 24 20:40:07 CST 2017
Server : Pool Size :2
Server : ActiveCount :2
Server : TaskCount :2
Server : CompletedTaskCount :0
a new task arrived

Server : Pool Size :3
Server : ActiveCount :3
Server : TaskCount :3
Server : CompletedTaskCount :0
a new task arrived

Server : Pool Size :4
Server : ActiveCount :4
Server : TaskCount :4
Server : CompletedTaskCount :0
a new task arrived

Server : Pool Size :5
Server : ActiveCount :5
Server : TaskCount :5
Server : CompletedTaskCount :0
a new task arrived

Server : Pool Size :5
Server : ActiveCount :5
Server : TaskCount :6
Server : CompletedTaskCount :0
a new task arrived

Server : Pool Size :5
pool-1-thread-2 : Task Task 1 : Created on : Mon Jul 24 20:40:07 CST 2017
pool-1-thread-2 : Task Task 1 : Started on : Mon Jul 24 20:40:07 CST 2017
Server : ActiveCount :5
pool-1-thread-2 : Task Task 1 : Doing a task during 7 seconds
pool-1-thread-4 : Task Task 3 : Created on : Mon Jul 24 20:40:07 CST 2017
pool-1-thread-4 : Task Task 3 : Started on : Mon Jul 24 20:40:07 CST 2017
pool-1-thread-1 : Task Task 0 : Doing a task during 9 seconds
pool-1-thread-3 : Task Task 2 : Created on : Mon Jul 24 20:40:07 CST 2017
pool-1-thread-3 : Task Task 2 : Started on : Mon Jul 24 20:40:07 CST 2017
pool-1-thread-4 : Task Task 3 : Doing a task during 2 seconds
pool-1-thread-5 : Task Task 4 : Created on : Mon Jul 24 20:40:07 CST 2017
pool-1-thread-5 : Task Task 4 : Started on : Mon Jul 24 20:40:07 CST 2017
pool-1-thread-5 : Task Task 4 : Doing a task during 5 seconds
Server : TaskCount :7
pool-1-thread-3 : Task Task 2 : Doing a task during 4 seconds
Server : CompletedTaskCount :0
a new task arrived

Server : Pool Size :5
Server : ActiveCount :5
Server : TaskCount :8
Server : CompletedTaskCount :0
a new task arrived

Server : Pool Size :5
Server : ActiveCount :5
Server : TaskCount :9
Server : CompletedTaskCount :0
a new task arrived

Server : Pool Size :5
Server : ActiveCount :5
Server : TaskCount :10
Server : CompletedTaskCount :0
pool-1-thread-4 : Task Task 3 : Finished on : Mon Jul 24 20:40:09 CST 2017
pool-1-thread-4 : Task Task 5 : Created on : Mon Jul 24 20:40:07 CST 2017
pool-1-thread-4 : Task Task 5 : Started on : Mon Jul 24 20:40:09 CST 2017
pool-1-thread-4 : Task Task 5 : Doing a task during 4 seconds
pool-1-thread-3 : Task Task 2 : Finished on : Mon Jul 24 20:40:11 CST 2017
pool-1-thread-3 : Task Task 6 : Created on : Mon Jul 24 20:40:07 CST 2017
pool-1-thread-3 : Task Task 6 : Started on : Mon Jul 24 20:40:11 CST 2017
pool-1-thread-3 : Task Task 6 : Doing a task during 4 seconds
pool-1-thread-5 : Task Task 4 : Finished on : Mon Jul 24 20:40:12 CST 2017
pool-1-thread-5 : Task Task 7 : Created on : Mon Jul 24 20:40:07 CST 2017
pool-1-thread-5 : Task Task 7 : Started on : Mon Jul 24 20:40:12 CST 2017
pool-1-thread-5 : Task Task 7 : Doing a task during 1 seconds
pool-1-thread-5 : Task Task 7 : Finished on : Mon Jul 24 20:40:13 CST 2017
pool-1-thread-5 : Task Task 8 : Created on : Mon Jul 24 20:40:07 CST 2017
pool-1-thread-5 : Task Task 8 : Started on : Mon Jul 24 20:40:13 CST 2017
pool-1-thread-5 : Task Task 8 : Doing a task during 4 seconds
pool-1-thread-4 : Task Task 5 : Finished on : Mon Jul 24 20:40:13 CST 2017
pool-1-thread-4 : Task Task 9 : Created on : Mon Jul 24 20:40:07 CST 2017
pool-1-thread-4 : Task Task 9 : Started on : Mon Jul 24 20:40:13 CST 2017
pool-1-thread-4 : Task Task 9 : Doing a task during 9 seconds
pool-1-thread-2 : Task Task 1 : Finished on : Mon Jul 24 20:40:14 CST 2017
pool-1-thread-3 : Task Task 6 : Finished on : Mon Jul 24 20:40:15 CST 2017
pool-1-thread-1 : Task Task 0 : Finished on : Mon Jul 24 20:40:16 CST 2017
pool-1-thread-5 : Task Task 8 : Finished on : Mon Jul 24 20:40:17 CST 2017
pool-1-thread-4 : Task Task 9 : Finished on : Mon Jul 24 20:40:22 CST 2017

```
