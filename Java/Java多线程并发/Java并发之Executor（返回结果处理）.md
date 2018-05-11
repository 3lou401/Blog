> * 运行多个任务并处理第一个结果
* 运行多个任务并处理所有结果

# 运行多个任务并处理第一个结果
并发编程常见的问题，就是当采用多个并发任务来解决一个问题，我们往往只对第一个返回的结果有兴趣。比如，对一个数组有多种排序算法，可以并发启动所有算法，但是对于一个给定的数组，第一个得到排序结果的算法就是最快的排序算法。

我们通过一个实例，这个实例会发起两种验证任务，只要有一个任务验证通过，就通过。


实现验证过程的类，逻辑很简单，不管是什么用户名，都是随机验证的，随机返回一个boolean。
```
package CreateExcutorInvokeAny;

import java.util.Random;
import java.util.concurrent.TimeUnit;

public class UserValidator {
	private String name;
	
	public UserValidator (String name) {
		this.name = name;
	}
	
	public boolean validate(String name,String password) {
		Random random = new Random();
		
		Long duration = (long)Math.random()*10;
		System.out.printf("Validator %s : Validator a user during %d seconds\n", 
				this.name, duration);
		try {
			TimeUnit.SECONDS.sleep(duration);
		} catch (InterruptedException e) {
			e.printStackTrace();
			return false;
		}
		
		return random.nextBoolean();
	}
	
	public String getName() {
		return this.name;
	}
}
```

Callable对象,他的逻辑是如果验证通过，就返回结果，如果验证不通过，就抛出异常。
```
package CreateExcutorInvokeAny;

import java.util.concurrent.Callable;
import java.util.concurrent.Future;

public class TaskValidator implements Callable<String> {
	
	private UserValidator validator;
	
	private String user;
	private String password;
	
	

	public TaskValidator(UserValidator validator, String user, String password) {
		super();
		this.validator = validator;
		this.user = user;
		this.password = password;
	}



	@Override
	public String call() throws Exception {
		if(!validator.validate(user, password)) {
			System.out.println(validator.getName() + "the user has not been found");
			throw new Exception("Error validating user");
		}
		System.out.println(validator.getName() + "has found");
		return validator.getName();
	}
	
}

```

Main类
```
package CreateExcutorInvokeAny;

import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class Main {

	public static void main(String[] args) {
		String username = "test";
		String password = "test";
		UserValidator oneValidator = new UserValidator("one");
		UserValidator twoValidator = new UserValidator("two");
		TaskValidator oneTask = new TaskValidator(oneValidator, username, password);
		TaskValidator twoTask = new TaskValidator(twoValidator, username, password);
		List<TaskValidator> taskList = new ArrayList<>();
		taskList.add(oneTask);
		taskList.add(twoTask);
		
		ExecutorService executor = Executors.newCachedThreadPool();
		String res;
		
		try {
			res = executor.invokeAny(taskList);
			System.out.println("Main : res : " + res);
		} catch (InterruptedException | ExecutionException e) {
			e.printStackTrace();
		}
		
		executor.shutdown();
		System.out.println("Main : end of the execution");
	}

}

```

这里的关键步骤就是invokeAny这个方法，会返回第一个执行结束的任务的结果，也就是说，如果验证没通过，任务无法执行完成，自然就不会完成，就不会返回，如果验证通过了，就会返回结果。

我们分析程序，会有四种可能性：
* 如果两个任务都返回true，也就是都验证通过，那么invokeany会返回第一个通过的结果
* 如果第一个任务验证返回true，第二个任务抛出exception，那么invokeAny方法的结果就是第一个任务的名称
* 如果第一个任务抛出异常，第二个任务返回true，那么第二个任务的结果就是返回结果
* 最后就是，两个任务都抛出异常，那么invokeAny方法也会抛出异常

![image.png](http://upload-images.jianshu.io/upload_images/1234352-a09332f77e7c704b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-276e77fe8bb4fa86.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-bfaaea5b46122c51.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-d9f4ea2fedb04752.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


# 运行多个任务并处理所有结果

Executor允许执行并发的任务而不需要去考虑线程创建和执行
如果想要等待线程结束，有以下两种方法：
* 如果任务执行结束，那么Future接口的isDone方法将返回true
* 在调用shutdown方法之后，ThreadPoolExecutor类的awaitTermination方法会将线程休眠，直到所有任务执行结束

使用invokeall方法就可以执行所有任务，这个方法会等到所有任务执行完成之后，再返回。

我们看一个实例：

```
package CreateExecutorInvokeAll;

import java.util.concurrent.Callable;
import java.util.concurrent.TimeUnit;

public class Task implements Callable<Result> {
	
	private String name;
	
	public Task(String name) {
		this.name = name;
	}
	
	@Override
	public Result call() throws Exception {
		System.out.println(this.name + " Starting\n");
		
		Long duration = (long)Math.random()*10;
		System.out.printf("Validator %s : Validator a user during %d seconds\n", 
				this.name, duration);
		try {
			TimeUnit.SECONDS.sleep(duration);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		
		int value = 0;
		for(int i=0;i<5;i++) {
			value += (int)Math.random()*100;
		}
		
		Result res = new Result();
		res.setName(this.name);
		res.setValue(value);
		
		System.out.println(this.name + " end");
		return res;
	}
	
}

```

```
package CreateExecutorInvokeAll;

public class Result {
	
	private String name;
	private int value;
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getValue() {
		return value;
	}
	public void setValue(int value) {
		this.value = value;
	}
	
	
}

```

```
package CreateExecutorInvokeAll;

import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class Main {

	public static void main(String[] args) {
		
		ExecutorService executor = Executors.newCachedThreadPool();
		
		List<Task> tasklist = new ArrayList<>();
		
		for(int i=0;i<3;i++) {
			Task task = new Task(String.valueOf(i));
			tasklist.add(task);
		}

		List<Future<Result>> reslist = new ArrayList<>();
		
		try {
			reslist = executor.invokeAll(tasklist);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		
		executor.shutdown();
		
		System.out.println("Main : res");
		
		for(int i=0;i<reslist.size();i++) {
			Future<Result> future = reslist.get(i);
			
			try {
				Result res = future.get();
				System.out.println("result : " + res.getName() + "||" + res.getValue());
			} catch (InterruptedException | ExecutionException e) {
				e.printStackTrace();
			}
		}
	}

}

```
运行结果

![image.png](http://upload-images.jianshu.io/upload_images/1234352-1c57ac84a5f97eb4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
