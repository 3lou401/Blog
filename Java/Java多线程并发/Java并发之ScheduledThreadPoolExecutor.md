> * 在Executor中延时执行任务
* 在Executor中周期的执行任务

ScheduledExecutorService类顾名思义，就是可以延迟执行的Executor。如果，对于某些任务，我们并不想马上执行，而是想让任务过一段时间后才执行，或者让任务进行周期性执行。我们就可以采用ScheduledExecutorService类。

# 在Executor中延时执行任务

Task类
```
package ScheduledThreadPoolExecutor;

import java.util.Date;
import java.util.concurrent.Callable;

public class Task implements Callable<String> {
	private String name;
	
	public Task(String name) {
		this.name = name;
	}

	@Override
	public String call() throws Exception {
		System.out.println(name + "starting at : " + new Date());
		return "Hello world!!!!";
	}
}
```

Main类：
```
package ScheduledThreadPoolExecutor;

import java.util.Date;
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.TimeUnit;

public class Main {

	public static void main(String[] args) {
		
		ScheduledExecutorService executor =  Executors.newScheduledThreadPool(1);
		
		System.out.println("Main : starting at " + new Date());
		
		for(int i=0;i<5;i++) {
			Task task = new Task("Task" + i);
			executor.schedule(task, (i+1), TimeUnit.SECONDS);
		}
		
		executor.shutdown();
		
		try {
			executor.awaitTermination(1, TimeUnit.DAYS);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		
		System.out.println("Main end at :" + new Date());
	}

}

```

运行结果
```
Main : starting at Tue Jul 25 09:25:38 CST 2017
Task0starting at : Tue Jul 25 09:25:39 CST 2017
Task1starting at : Tue Jul 25 09:25:40 CST 2017
Task2starting at : Tue Jul 25 09:25:41 CST 2017
Task3starting at : Tue Jul 25 09:25:42 CST 2017
Task4starting at : Tue Jul 25 09:25:43 CST 2017
Main end at :Tue Jul 25 09:25:43 CST 2017
```

# 在Executor中周期的执行任务
Executor框架通过并发任务而避免了线程的创建操作。当发送一个任务给Executor后，根据Executor的配置，它将尽快执行这个任务。当任务结束之后，这个任务就会从Executor中删除，如果想要再次执行这个任务，就需要再次将这个任务发送给Executor。
Executor框架中，提供了ScheduledThreadPoolExecutor来提供任务的周期性执行的功能

Task类：
```
package ScheduledThreadCycle;

import java.util.Date;
import java.util.concurrent.Callable;

public class Task implements Runnable {

	private String name;
	
	public Task(String name) {
		this.name = name;
	}

	@Override
	public void run() {
		
		System.out.println(name + " : Starting at : " + new Date());
		
	}

}

```

Main类：
```
package ScheduledThreadCycle;

import java.util.Date;
import java.util.concurrent.Executors;
import java.util.concurrent.ScheduledExecutorService;
import java.util.concurrent.ScheduledFuture;
import java.util.concurrent.TimeUnit;

public class Main {

	public static void main(String[] args) throws InterruptedException {
		
		ScheduledExecutorService executor = Executors.newScheduledThreadPool(1);
		
		System.out.println("Main start at :" + new Date());
		
		Task task = new Task("task");
		
		ScheduledFuture<?> result = executor.scheduleAtFixedRate(task, 1, 2, TimeUnit.SECONDS);
		
		for(int i=0;i<10;i++) {
			System.out.println("Main: Delay: " + result.getDelay(TimeUnit.MILLISECONDS));
			
			TimeUnit.MILLISECONDS.sleep(500);
		}
		
		//executor.shutdown();
		
		TimeUnit.SECONDS.sleep(5);
		
		System.out.println("Main : finished at : " + new Date());
		
		executor.shutdown();
	}

}

```

运行结果：
```
Main start at :Tue Jul 25 09:35:05 CST 2017
Main: Delay: 999
Main: Delay: 499
Main: Delay: -1
task : Starting at : Tue Jul 25 09:35:06 CST 2017
Main: Delay: 1498
Main: Delay: 998
Main: Delay: 497
task : Starting at : Tue Jul 25 09:35:08 CST 2017
Main: Delay: 1997
Main: Delay: 1497
Main: Delay: 996
Main: Delay: 453
task : Starting at : Tue Jul 25 09:35:10 CST 2017
task : Starting at : Tue Jul 25 09:35:12 CST 2017
task : Starting at : Tue Jul 25 09:35:14 CST 2017
Main : finished at : Tue Jul 25 09:35:15 CST 2017
```
想要通过Executor来执行宇哥周期性的任务时，需要一个ScheduledExecutorService类，可以利用Executors工厂类创建ScheduledExecutorService类。
要创建周期性任务的Executor，就需要像ScheduledExecutorService这个执行器发送周期性的任务，调用 scheduleAtFixedRate方法发送任务，值得注意的是这个方法，只接受runnable对象，不接收Callable对象。后面两个参数分别指定第一次执行的延迟时间，两次执行的时间周期。时间周期指的是两次执行开始的时间间隔。
scheduleAtFixedRate方法会返回宇哥ScheduledFuture对象，这个对象扩展自Future接口，这是一个参数化的类型接口，必须指定类型，由于任务是Runnable对象，没有返回值，所以需要指定为<?>。
