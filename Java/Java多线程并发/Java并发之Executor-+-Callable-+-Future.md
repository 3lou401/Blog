> * 引入 Callable + Future
* Callable + Future实例

# 引入 Callable + Future
Executor框架的优势之一就是，可以运行并发任务并且返回结果。
我们知道Runnable对象是没有返回值的，所以自然利用Runnable对象就无法返回结果，于是就定义了一个新的接口，可以理解为是“带有返回值的Runnable对象”。
这个接口就是
Callable接口：这个接口声明了一个call方法，类似于Runnable接口的run方法，是任务具体的逻辑，不同就在于可以有返回值，而且这个接口是一个泛型接口，这就意味着必须声明call方法的返回类型。

![image.png](http://upload-images.jianshu.io/upload_images/1234352-d3b43fab0acbf92f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们任务交给线程执行之后，什么时候执行结束，将结果返回这个时间往往是无法具体确定的，所以为了接受这个来自未来某个时刻执行完之后的返回结果，又新增了一个Future接口：
Future接口：这个接口声明了一些方法获取由callable对象产生的结果，并管理他们的状态。

![image.png](http://upload-images.jianshu.io/upload_images/1234352-7becf27f5c9fdb14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# Callable + Future实例
接下来，我们编写一个实例来接受任务的返回结果

实现一个Callable对象，由于返回值是Integer，所以定义为Callable<Integer>
```
package CreateExecutorCallableFuture;

import java.util.concurrent.Callable;
import java.util.concurrent.TimeUnit;

public class FactorialCaculator implements Callable<Integer> {
	
	private Integer number;
	
	

	public FactorialCaculator(Integer number) {
		super();
		this.number = number;
	}



	@Override
	public Integer call() throws Exception {
		int result = 1;
		if((number == 0) || (number == 1))
			return 1;
		else {
			for(int i=2;i<=number;i++) {
				result *= i;
				TimeUnit.MILLISECONDS.sleep(20);
			}
		}
		
		System.out.printf("%s : %d\n",Thread.currentThread().getName(),result);
		
		return result;
	}
	
}
```

Main类创建一个线程池来执行这些任务，并在任务执行完之后输出结果
```
package CreateExecutorCallableFuture;

import java.util.ArrayList;
import java.util.List;
import java.util.Random;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;

public class Main {

	public static void main(String[] args) throws InterruptedException {
		ThreadPoolExecutor executor = (ThreadPoolExecutor)Executors.newFixedThreadPool(2);
		List<Future<Integer>> resList = new ArrayList<>();
		
		Random random = new Random();
		
		for(int i=0;i<10;i++) {
			Integer number = random.nextInt(10);
			FactorialCaculator caculator = new FactorialCaculator(number);
			
			Future<Integer> res = executor.submit(caculator);
			resList.add(res);
		}
		
		do {
			System.out.println("Main : Number of CompleteTasks : " + executor.getCompletedTaskCount());
			for(int i=0;i<resList.size();i++) {
				Future<Integer> res = resList.get(i);
				System.out.printf("Main : task %d : %s\n", i, res.isDone());
			}
			
			TimeUnit.MILLISECONDS.sleep(50);
			
		}while(executor.getCompletedTaskCount() < resList.size());
		
		for(int i=0;i<resList.size();i++) {
			Future<Integer> res = resList.get(i);
			Integer number = null;
			try {
				number = res.get();
			} catch (ExecutionException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			System.out.printf("Main : Task %d : %s\n",i,number);
		}
	}

}
```

运行结果
```
Main : Number of CompleteTasks : 0
Main : task 0 : false
Main : task 1 : false
Main : task 2 : false
Main : task 3 : false
Main : task 4 : false
Main : task 5 : false
Main : task 6 : false
Main : task 7 : false
Main : task 8 : false
Main : task 9 : false
pool-1-thread-1 : 24
pool-1-thread-2 : 120
Main : Number of CompleteTasks : 2
Main : task 0 : true
Main : task 1 : true
Main : task 2 : false
Main : task 3 : false
Main : task 4 : false
Main : task 5 : false
Main : task 6 : false
Main : task 7 : false
Main : task 8 : false
Main : task 9 : false
pool-1-thread-2 : 2
pool-1-thread-2 : 2
Main : Number of CompleteTasks : 4
Main : task 0 : true
Main : task 1 : true
Main : task 2 : false
Main : task 3 : true
Main : task 4 : true
Main : task 5 : false
Main : task 6 : false
Main : task 7 : false
Main : task 8 : false
Main : task 9 : false
pool-1-thread-2 : 6
Main : Number of CompleteTasks : 6
Main : task 0 : true
Main : task 1 : true
Main : task 2 : false
Main : task 3 : true
Main : task 4 : true
Main : task 5 : true
Main : task 6 : true
Main : task 7 : false
Main : task 8 : false
Main : task 9 : false
pool-1-thread-1 : 40320
Main : Number of CompleteTasks : 7
Main : task 0 : true
Main : task 1 : true
Main : task 2 : true
Main : task 3 : true
Main : task 4 : true
Main : task 5 : true
Main : task 6 : true
Main : task 7 : false
Main : task 8 : false
Main : task 9 : false
pool-1-thread-2 : 720
Main : Number of CompleteTasks : 9
Main : task 0 : true
Main : task 1 : true
Main : task 2 : true
Main : task 3 : true
Main : task 4 : true
Main : task 5 : true
Main : task 6 : true
Main : task 7 : true
Main : task 8 : false
Main : task 9 : true
Main : Number of CompleteTasks : 9
Main : task 0 : true
Main : task 1 : true
Main : task 2 : true
Main : task 3 : true
Main : task 4 : true
Main : task 5 : true
Main : task 6 : true
Main : task 7 : true
Main : task 8 : false
Main : task 9 : true
pool-1-thread-1 : 362880
Main : Task 0 : 24
Main : Task 1 : 120
Main : Task 2 : 40320
Main : Task 3 : 2
Main : Task 4 : 2
Main : Task 5 : 6
Main : Task 6 : 1
Main : Task 7 : 720
Main : Task 8 : 362880
Main : Task 9 : 1
```

Future对象主要有一下两个目的：
* 控制任务的状态：可以取消和检查任务是否完成。可以使用isDone检查任务是否完成，检查isCancel检查任务是否暂停，也可以直接调用cancle方法暂停任务。

* 通过call方法获取返回结果，为了达到这个目的，可以使用get方法，这个方法会一直等到callable对象的call方法返回结果。如果此时任务尚未完成，get方法就会一直阻塞到线程完成。

** Future对象是用来管理Callavle任务的！**
