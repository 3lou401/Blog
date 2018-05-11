set是用来存储没有重复的元素的。set在java中有三种比较常用实现：HashSet, TreeSet and LinkedHashSet。所以，不同的时候我们自然需要考虑如何选择使用不同的set。这就要我们对于这三种set的特点和实现有一定的了解。一般来说，如果我们需要一个存取效率比较高的set，我们可以选择hashset，如果我们需要一个可以自动给元素排序的set，我们就需要使用treeset，如果我们想要元素按插入的样子保持顺序，那么我们就可以使用LinkedHashSet。

![image.png](http://upload-images.jianshu.io/upload_images/1234352-ea93533e89614cbe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

hashset通过哈希表实现，元素是不排序的，所以输出set的时候元素的顺序是随机的，add,remove, and contains这三个方法的时间复杂度都是常数 O(1)。
treeset通过红黑树实现，元素是排好序的，但是相应的操作时间复杂度就增加了，add,remove, and contains这三个方法的时间复杂度都是 O(log (n))
LinkedHashSet is between HashSet and TreeSet. It is implemented as a hash table
with a linked list running through it, so it provides the order of insertion. The time
complexity of basic methods is O(1)
LinkedHashSet是介于HashSet and TreeSet.之间。通过一个带链表的哈希表实现，所以它是按插入顺序排序的，保持插入的顺序，基本操作的时间复杂度和hashset一样都是常数时间。

** 注意的是，treeset由于需要对元素排序，所以添加的元素需要实现comparable或者comparator，不然就会报错 **像下面这个例子
```
import java.util.Iterator;
import java.util.TreeSet;

public class SetTest {

	public static void main(String[] args) {
		
		TreeSet<Dog> tree = new TreeSet<>();
		tree.add(new Dog(1));
		tree.add(new Dog(5));
		tree.add(new Dog(3));
		
		Iterator<Dog> iter = tree.iterator();
		while(iter.hasNext()) {
			System.out.println(iter.next());
		}
	}

}

class Dog {
	private int size;
	
	public Dog(int size) {
		this.size = size;
	}
	
}
```

![image.png](http://upload-images.jianshu.io/upload_images/1234352-f748328a7ce535ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们对dog实现comparable接口就可以
```
class Dog implements Comparable<Dog>{
	private int size;
	
	public Dog(int size) {
		this.size = size;
	}

	@Override
	public int compareTo(Dog o) {
		return this.size - o.size;
	}
	
	public String toString() {
		return size + " ";
	}
	
}
```
下面我们简单的写一个小程序测试三个set的效率：
```
public static void main(String[] args) {
Random r = new Random();
HashSet<Dog> hashSet = new HashSet<Dog>();
TreeSet<Dog> treeSet = new TreeSet<Dog>();
LinkedHashSet<Dog> linkedSet = new LinkedHashSet<Dog>();
// start time
long startTime = System.nanoTime();
for (int i = 0; i < 1000; i++) {
int x = r.nextInt(1000 - 10) + 10;
hashSet.add(new Dog(x));
}
// end time
long endTime = System.nanoTime();
long duration = endTime - startTime;
System.out.println("HashSet: " + duration);
// start time
startTime = System.nanoTime();
for (int i = 0; i < 1000; i++) {
int x = r.nextInt(1000 - 10) + 10;
treeSet.add(new Dog(x));
}
// end time
endTime = System.nanoTime();
duration = endTime - startTime;
System.out.println("TreeSet: " + duration);
// start time
startTime = System.nanoTime();
for (int i = 0; i < 1000; i++) {
int x = r.nextInt(1000 - 10) + 10;
linkedSet.add(new Dog(x));
}
// end time
endTime = System.nanoTime();
duration = endTime - startTime;
System.out.println("LinkedHashSet: " + duration);
}
```

![image.png](http://upload-images.jianshu.io/upload_images/1234352-1658e3e5a7b48772.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![image.png](http://upload-images.jianshu.io/upload_images/1234352-ef8c2e7af6b60656.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
