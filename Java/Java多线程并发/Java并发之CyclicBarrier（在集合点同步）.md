> * CyclicBarrier引入
* 创建CyclicBarrier
* 遇到CyclicBarrier之后休眠
* CyclicBarrier的回调线程
* CyclicBarrier的简单例子
* CyclicBarrier进行分治编程的例子


# CyclicBarrier引入
CyclicBarrier类是一个同步辅助类，类似于CountDownLatch，但远比CountDownLatch要强大。CyclicBarrier 的字面意思是可循环使用（Cyclic）的屏障（Barrier）。它要做的事情是，让一组线程到达一个屏障（也可以叫同步点）时被阻塞，直到最后一个线程到达屏障时，屏障才会开门，所有被屏障拦截的线程才会继续干活。就如下面这个图所示

![image.png](http://upload-images.jianshu.io/upload_images/1234352-85a3de1256617ec1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

CyclicBarrier相当于一个屏障插在线程执行的过程中，取决于线程调用await方法的位置，直到指定线程数量的到达之后，这个屏障才可以取走。

# 创建CyclicBarrier
当你创建一个CyclicBarrier类的时候，需要指定需要等待的线程数
```
CyclicBarrier barrier = new CyclicBarrier(2);
```
# 遇到CyclicBarrier之后休眠
当在线程指定位置设置屏障的时候，只需要调用CyclicBarrier的await方法.
```
barrier.await();
```
await方法还可以指定等待的时间。当达到这个等待的时间，即使没有足够的线程到达，这个屏障也会被解除
```
barrier.await(10, TimeUnit.SECONDS);
```
终止线程遇到屏障之后的等待条件有下面这些：
* 足够的线程到达屏障处，自动解除屏障
* 线程等待屏幕指定的等待时间之后，超时，解除屏障
* 线程被中断，其他线程被中断，屏障会解除
* 外部线程调用了CyclicBarrier.reset()方法，屏障解除。

# CyclicBarrier的回调线程
CyclicBarrier初始化的时候，可以传入一个runnable对象作为初始化参数，当所有线程都到达屏障点后，屏障会先把这个指定的runnable对象作为线程来执行，执行完之后，就会移除屏障唤醒所有线程，这个特性很有作用，可以达到分治操作，fork/join。想象一下，我们让线程在屏障前计算好各自的结果，然后当所有线程都算完之后，我们在回调线程中执行统计所有计算结果，这样就相当于分治技术了，将一个大任务切分给其他线程分成小任务各自执行，执行完之后就将他们汇总。

```
Runnable      barrierAction = ... ;
CyclicBarrier barrier       = new CyclicBarrier(2, barrierAction);
```

# CyclicBarrier的简单例子
```
Runnable barrier1Action = new Runnable() {
    public void run() {
        System.out.println("BarrierAction 1 executed ");
    }
};
Runnable barrier2Action = new Runnable() {
    public void run() {
        System.out.println("BarrierAction 2 executed ");
    }
};

CyclicBarrier barrier1 = new CyclicBarrier(2, barrier1Action);
CyclicBarrier barrier2 = new CyclicBarrier(2, barrier2Action);

CyclicBarrierRunnable barrierRunnable1 =
        new CyclicBarrierRunnable(barrier1, barrier2);

CyclicBarrierRunnable barrierRunnable2 =
        new CyclicBarrierRunnable(barrier1, barrier2);

new Thread(barrierRunnable1).start();
new Thread(barrierRunnable2).start();
```

```
public class CyclicBarrierRunnable implements Runnable{

    CyclicBarrier barrier1 = null;
    CyclicBarrier barrier2 = null;

    public CyclicBarrierRunnable(
            CyclicBarrier barrier1,
            CyclicBarrier barrier2) {

        this.barrier1 = barrier1;
        this.barrier2 = barrier2;
    }

    public void run() {
        try {
            Thread.sleep(1000);
            System.out.println(Thread.currentThread().getName() +
                                " waiting at barrier 1");
            this.barrier1.await();

            Thread.sleep(1000);
            System.out.println(Thread.currentThread().getName() +
                                " waiting at barrier 2");
            this.barrier2.await();

            System.out.println(Thread.currentThread().getName() +
                                " done!");

        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (BrokenBarrierException e) {
            e.printStackTrace();
        }
    }
}
```
运行结果

![image.png](http://upload-images.jianshu.io/upload_images/1234352-41c018ee660d6a4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# CyclicBarrier进行分治编程的例子
我们实现一个CyclicBarrier分治编程的例子
我们假设现在一个数组中一个元素出现的次数，我们分出几个线程分别计算不同的行，让他们算完之后在屏障那里wait，然后等所有线程都算完了，我们就可以调用回调线程来计算总的结果

大数组类
```
package CyclicBarrier;

import java.util.Random;

public class MatrixMock {
	private int[][] data;
	
	public MatrixMock(int size, int length, int number) {
		int counter = 0;
		data = new int[size][length];
		Random random = new Random();
		
		for(int i=0;i<size;i++) {
			for(int j=0;j<length;j++) {
				data[i][j] = random.nextInt(10);
				if(data[i][j] == number)
					counter++;
			}
		}
		
		System.out.println("矩阵中有 " + counter + " 个要查找的数 " + number);
	}
	
	public int[] getRow(int row) {
		if((row >= 0) && (row < data.length))
			return data[row];
		return null;
	}
}

```

结果类：
```
package CyclicBarrier;

public class Results {
	private int[] data;
	
	public Results(int size) {
		data = new int[size];
	}
	
	public void setData(int position, int value) {
		data[position] = value;
	}
	
	public int[] getData() {
		return data;
	}
}

```

搜索线程
```
package CyclicBarrier;

import java.util.concurrent.BrokenBarrierException;
import java.util.concurrent.CyclicBarrier;

public class Searcher implements Runnable {

	private int firstRow;
	private int lastRow;
	
	private MatrixMock mock;
	
	private Results results;
	private int number;
	
	private final CyclicBarrier barrier;
	
	
	
	public Searcher(int firstRow, int lastRow, MatrixMock mock, Results results, int number, CyclicBarrier barrier) {
		super();
		this.firstRow = firstRow;
		this.lastRow = lastRow;
		this.mock = mock;
		this.results = results;
		this.number = number;
		this.barrier = barrier;
	}



	@Override
	public void run() {
		int counter;
		
		System.out.println(Thread.currentThread().getName() + "正在搜索数据" + firstRow + " " + lastRow);
		
		for(int i=firstRow;i<lastRow;i++) {
			int[] row = mock.getRow(i);
			counter = 0;
			for(int j=0;j<row.length;j++) {
				if(row[j] == number)
					counter++;
			}
			results.setData(i, counter);
		}
		
		System.out.println(Thread.currentThread().getName() + "查完了");
		
		try {
			barrier.await();
		} catch (InterruptedException | BrokenBarrierException e) {
			e.printStackTrace();
		}
		
		System.out.println(Thread.currentThread().getName() + "终于等到了");
	}

}

```

回调线程统计结果
```
package CyclicBarrier;

public class Grouper implements Runnable {

	private Results results;
	
	public Grouper(Results results) {
		this.results = results;
	}
	
	
	@Override
	public void run() {
		int finalResult = 0;
		
		System.out.println("正在统计结果。。。");
		
		int[] data = results.getData();
		
		for(int number : data) {
			finalResult += number;
		}
		
		System.out.println(finalResult);
	}

}

```

main类测试
```
package CyclicBarrier;

import java.util.concurrent.CyclicBarrier;

public class Main {

	public static void main(String[] args) {
		
		final int ROWS = 10000;
		final int NUMBERS = 10000;
		final int SEACHER = 5;
		final int PARTICIPANTS = 5;
		final int LINES_PARTICIPANTS = 2000;
		
		MatrixMock mock = new MatrixMock(ROWS, NUMBERS, SEACHER);
		
		Results results = new Results(ROWS);
		
		Grouper grouper = new Grouper(results);
		
		CyclicBarrier barrier = new CyclicBarrier(PARTICIPANTS, grouper);
		
		Searcher[] searchers = new Searcher[PARTICIPANTS];
		
		for(int i=0;i<PARTICIPANTS;i++) {
			searchers[i] = new Searcher(i*LINES_PARTICIPANTS, i*LINES_PARTICIPANTS + LINES_PARTICIPANTS, mock, results, 5, barrier);
			new Thread(searchers[i]).start();
		}
	}

}

```

运行结果

![image.png](http://upload-images.jianshu.io/upload_images/1234352-2bbea86404cd3cef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
