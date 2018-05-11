在收集对象之后，对象的排序是经常需要用到的操作。但我们不需要亲自实现各种排序算法，java.util.Collections提供了sort方法，List作为一种collections,而且存在索引，所以可以调用sort方法进行排序。显然，必须具有索引，才能进行排序。

```
package Collection;

import java.util.*;

public class Sort {

	public static void main(String[] args) {
		List numbers = Arrays.asList(1,2,3,4,5,6,8,7,12,1);
		Collections.sort(numbers);
		System.out.println(numbers);
	}

}

```
执行结果是将数据进行由小到大的排序：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-f7e1535930946af6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


但如果是下面这个例子呢？
```
package Collection;

import java.util.Arrays;
import java.util.Collections;
import java.util.List;

public class Sort2 {
	
	

	public static void main(String[] args) {
		List accounts = Arrays.asList(
                new Account("Justin", "X1234", 1000),
                new Account("Monica", "X5678", 500),
                new Account("Irene", "X2468", 200)
        );
        Collections.sort(accounts);
        System.out.println(accounts);

	}

}

class Account {
	private String name;
	private String numbers;
	private double balance;
	
	Account(String name,String numbers,double balance) {
		this.name = name;
		this.numbers = numbers;
		this.balance = balance;
	}
	
	@Override
	public String toString() {
		return String.format("Account(%s,%s,%d)", name,numbers,balance);
	}
}

```
执行之后，我们发现程序报错了：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-6fc390bf02d31b5f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

错误信息是，我们定义的Account无法转换为Comparable类型。
显然，这就是说明collection的sort方法必须室友comparable类的对象进行调用，因为我们既然要对对象进行排序，显然就要告诉程序如何对对象进行排序，是按什么排？这就需要去实现comparable接口，这是一个形容词，也就是表明，对象是可比较大小的，那自然就可以排序了。这个接口有一个comparaTo的方法，必须返回大于0，小于0或者等于0的数，这就是sort的排序方式。

所以接下来我们就改写上面的例子，使其更具账户余额的大小进行排序。
```
package Collection;

import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Sort3 {
	
	

	public static void main(String[] args) {
		List accounts = Arrays.asList(
                new Account2("Justin", "X1234", 1000),
                new Account2("Monica", "X5678", 500),
                new Account2("Irene", "X2468", 200)
        );
        Collections.sort(accounts);
        System.out.println(accounts);

	}

}

class Account2 implements Comparable<Account2> {
	private String name;
	private String numbers;
	private double balance;
	
	Account2(String name,String numbers,double balance) {
		this.name = name;
		this.numbers = numbers;
		this.balance = balance;
	}
	
	@Override
	public String toString() {
		return String.format("Account(%s,%s,%f)", name,numbers,balance);
	}

	

	@Override
	public int compareTo(Account2 o) {
		// TODO Auto-generated method stub
		
		return (int) (this.balance - o.balance);
	}
	
	
}

```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-c09cf89b2c29ef47.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这就根据银行余额的值返回了正确的结果，所以我们知道，要对对象进行排序，首先一定要是可比较大小的，不然怎么排序，要可比较大小，就需要继承实现comparable接口，实现里面的comparaTo方法，定义排序的规则。

那么疑问来了，为什么我们的第一个例子，可以正确的对integer进行排序呢？
那是因为jdk已经默认帮integer实现了comparable接口。
但问题还是来了，如果我们希望integer的排序方法由大到小呢？那怎么办，难道我们需要重新去实现integer的comparable接口的comparaTo方法？显然这是不现实的，因为这些都是在jdk默认提供的api里，这时候，我们发现，还有一个comparator接口。

Collections的sort()方法有另一个重载的版本，可接受java.util.Comparator接口的的实例对象，如果你使用这个版本，排序方式就将根据Comparator的compare()定义來決定。

```
package Collection;

import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Sort5 {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		List<String> words = Arrays.asList("B", "X", "A", "M", "F", "W", "O");
		Collections.sort(words, new Comparator<String>() {

			@Override
			public int compare(String o1, String o2) {
				// TODO Auto-generated method stub
				return -o1.compareTo(o2);
			}
			
		});
		
		System.out.println(words);
	}

}
```

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-43ed1992e1003a5a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在Java中，根据顺序有关的行为要么是实现了comparable接口，要么就是实现了comparator接口类型。
