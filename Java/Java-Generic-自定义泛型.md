# 如何自定义泛型
考虑我们要实现了一个节点对象，这个对象可以自定义类型，我们可以用泛型语法进行如下的定义：
```
package Generic;

public class Node<T> {
	private T value;
	Node<T> next;
	T getValue() {
		return value;
	}
	
	void setValue(T value) {
		this.value = value;
	}
	
	public static void main(String[] args) {
		Node<String> first = new Node<String>();
        first.setValue("Justin");
        first.next = new Node<String>();
        first.next.setValue("momor");
	}
}
```

同样，在定义接口的时候，也可以使用泛型，例如iterator接口就是泛型定义的
```
package java.util;

public interface Iterator<E> {
    boolean hasNext();
    E next();
    void remove();
}
```

# 自定义泛型的边界
在定义泛型的时候，可以定义泛型的边界，例如下面的例子
```
class Animal {}
class Human extends Animal {}
class Toy {}

class Duck<T extends Animal> {}

public class Main {
    public static void main(String[] args) {
        Duck<Animal> ad = new Duck<Animal>();
        Duck<Human> hd = new Duck<Human>();
        Duck<Toy> hd = new Duck<Toy>();  // 編譯錯誤
    }
}
```

在这个例子中，使用extends定义指定泛型的真正的形态的时候，必须是animal的子类，你可以使用animal与human来指定形态，但不可以使用toy来指定，因为toy不是animal的子类。

下面举一个快速排序的例子来说明：
```
class Sort {
    public void quick(int[] number) {
        sort(number, 0, number.length-1);
    }
    
    private void sort(int[] number, int left, int right) {
        if(left < right) { 
            int q = partition(number, left, right); 
            sort(number, left, q-1); 
            sort(number, q+1, right); 
        } 

    }

    private int partition(int number[], int left, int right) {  
        int i = left - 1; 
        for(int j = left; j < right; j++) { 
            if(number[j] <= number[right]) { 
                i++; 
                swap(number, i, j); 
            } 
        } 
        swap(number, i+1, right); 
        return i+1; 
    } 

    private void swap(int[] number, int i, int j) {
        int t = number[i]; 
        number[i] = number[j]; 
        number[j] = t;
    }
}
```

在这个例子中，使用的是int写死的类型，为了让这个排序算法更为通用，我们可以使用泛型，但要求是该形态必须具有可比较的对象大小的方法，一个方法就是要求排序的对象实例化[java.lang.Comparable<T>]
```
class Sort<T extends Comparable<T>> {
    void quick(T[] array) {
        sort(array, 0, array.length-1);
    }
    
    private void sort(T[] array, int left, int right) {
        if(left < right) { 
            int q = partition(array, left, right); 
            sort(array, left, q-1); 
            sort(array, q+1, right); 
        } 

    }

    private int partition(T[] array, int left, int right) {  
        int i = left - 1; 
        for(int j = left; j < right; j++) { 
            if(array[j].compareTo(array[right]) <= 0) {
                i++; 
                swap(array, i, j); 
            } 
        } 
        swap(array, i+1, right); 
        return i + 1; 
    } 

    private void swap(T[] array, int i, int j) {
        T t = array[i]; 
        array[i] = array[j]; 
        array[j] = t;
    }
}
```
若extends可以指定多个类和接口，想再指定其它接口，可以使用&连接。例如：
public class Some<T extends Iterable<T> & Comparable<T>> {
    ...
}

# 共变性，逆变性
假设我们定义了下列这种类别：
```
class Node<T> {
    T value;
    Node<T> next;
    
    Node(T value, Node<T> next) {
        this.value = value;
        this.next = next;
    }
}
```
在下面的例子中：
```
class Fruit {}
class Apple extends Fruit {
    @Override
    public String toString() {
        return "Apple";
    }
}

class Banana extends Fruit {
    @Override
    public String toString() {
        return "Banana";
    }
}


public class Main {
    public static void main(String[] args) {
        Node<Apple> apple = new Node<Apple>(new Apple(), null);
        Node<Fruit> fruit = apple;  // 編譯錯誤，incompatible types
    }
}
```

在上面的例子中，apple的形态是Node<Apple>，而fruit的类型是Node<fruit>,我们将apple所指向的对象给fruit，那么Node<Apple>是否应该是一种Node<Fruit>呢？编译器告诉我们不是的。

在泛型中，如果B是A的子类，而Node<B>被视为一种Node<A>类型，就称Node具有共变形（Covariance），反过来，如果Node<A>被视为一种Node<B>形态，则成为具有逆变性（Contravariance），如果不具有共变形或者逆变性，则称其是不可变的。

Java中的泛型不支持共变形和逆变性，不过可以使用通配字符？与extends或者super
来宣告达到类似的共变形和逆变性。如下面的例子：
```
public class Main {
    public static void main(String[] args) {
        Node<Apple> apple = new Node<Apple>(new Apple(), null);
        Node<? extends Fruit> fruit = apple; // 類似共變性效果
    }
}
```
一个实际应用的例子是：
```
public class Main {
    public static void main(String[] args) {
        Node<Apple> apple1 = new Node<Apple>(new Apple(), null);
        Node<Apple> apple2 = new Node<Apple>(new Apple(), apple1);
        Node<Apple> apple3 = new Node<Apple>(new Apple(), apple2);
        
        Node<Banana> banana1 = new Node<Banana>(new Banana(), null);
        Node<Banana> banana2 = new Node<Banana>(new Banana(), banana1);

        show(apple3);
        show(banana2);
    }
    
    static void show(Node<? extends Fruit> n) {
        Node<? extends Fruit> node = n;
        do {
            System.out.println(node.value);
            node = node.next;
        } while(node != null);
    }
}
```
你的目的是可以顯示所有的水果節點，由於show()方法使用型態通配字元宣告參數，使得n具備類似共變性的效果，因此show()方法就可以顯示Node<Apple>也可以顯示Node<Banana>。

Java的泛型亦不支援逆變性，不過可以使用型態通配字元?與super來宣告變數，使其達到類似逆變性，例如：

我们的目的是可以显示所有水果的节点，由于show方法使用通配字符宣告共变形，所以show方法既可以显示apple，也能显示banana。
java的泛型不支持逆变性，不过可以使用通配字符super来宣告逆变性，如下面的例子：
```
class Fruit {
    int price;
    int weight;
    Fruit(int price, int weight) {
        this.price = price;
        this.weight = weight;
    }
}

class Apple extends Fruit {
     Apple(int price, int weight) {
         super(price, weight);
     }
}

class Banana extends Fruit {
     Banana(int price, int weight) {
         super(price, weight);
     }
}

interface Comparator<T> {
    int compare(T t1, T t2);
}

class Basket<T> {
    private T[] things;
    Basket(T... things) {
        this.things = things;
    }
    void sort(Comparator<? super T> comparator) {
        // 作一些排序
    }
}
```
```
public class Main {
    public static void main(String[] args) {
        Comparator<Fruit> comparator = new Comparator<Fruit>() {
            public int compare(Fruit f1, Fruit f2) {
                return f1.price - f2.price;
            }
        };
        Basket<Apple> b1 = new Basket<Apple>(
                                 new Apple(20, 100), new Apple(25, 150));
        Basket<Banana> b2 = new Basket<Banana>(
                                 new Banana(30, 200), new Banana(25, 250));
        b1.sort(comparator);
        b2.sort(comparator);
    }
}
```

# 泛型对象的比较
如果我们需要重写泛型对象的equal方法，我们可能会这么写：
```
import java.util.*;

class Basket<T> {
    T[] things;
    Basket(T... things) {
        this.things = things;
    }
    
    @Override
    public boolean equals(Object o) {
        if(o instanceof Basket<T>) {  // 編譯錯誤
            Basket that = (Basket) o;
            return Arrays.deepEquals(this.things, that.things);
        }
        return false;
    }
}
```
但如果我们编译这个程序，我们会发现如下的错误：
illegal generic type for instanceof
        if(o instanceof Basket<T>) {
在 程式中instanceof對Basket<T>的型態判斷是不合法的，因為Java的泛型所採用的是型態抹除，也就是說，程式中泛型語法的 型態指定，僅提供編譯器使用，執行時期無法獲型態資訊，因而instanceof在執行時期比對時，僅能針對Basket型態比對，無法針對當中的泛型實 際型態進行比對。

如果想要通過編譯，可以使用型態通配字元?：

在程序中对Basket<T>的类型的判断是不合法的，因为java泛型采用的是类型擦除，也就是说，在程序中泛型语法的类型指定，仅给编译器使用，执行时无法获取类型的信息，因而instanceOf在执行器对比时，仅能根据basket类型进行对比，无法针对当众的泛型实际的类型进行对比

如果想要通过编译，就要使用通配符？：
```
import java.util.*;

class Basket<T> {
    T[] things;
    Basket(T... things) {
        this.things = things;
    }
    
    @Override
    public boolean equals(Object o) {
        if(o instanceof Basket<?>) {
            Basket that = (Basket) o;
            return Arrays.deepEquals(this.things, that.things);
        }
        return false;
    }
}
```
我们可以来测试一下，这样写的效果：
```
public class Main {
    public static void main(String[] args) {
        Basket<Integer> b1 = new Basket<Integer>(1, 2);
        Basket<Integer> b2 = new Basket<Integer>(1, 2);
        Basket<Integer> b3 = new Basket<Integer>(2, 2);
        Basket<String> b4 = new Basket<String>("1", "2");
        System.out.println(b1.equals(b2));       // true
        System.out.println(b1.equals(b3));       // false
        System.out.println(b1.equals(b4));       // false
    }
}
```
好像不错，可以正确的比较，但我们看下面的例子：
```
public class Main {
    public static void main(String[] args) {
        Basket<String> b1 = new Basket<String>();
        Basket<Integer> b2 = new Basket<Integer>();
        System.out.println(b1.equals(b2));    // true
    }
}
```
Basket<Integer>與Basket<String>本来应该是被看作为不同的类型的，显然比较的结果应该为不相等，但实际上，由于java采用类型擦除的方式，结果就是在这种情况下，空对象的相等的，因为还没有塞值进去。
```
public class Main {
    public static void main(String[] args) {
        List<Integer> l1 = new ArrayList<Integer>();
        List<String> l2 = new ArrayList<String>();
        System.out.println(l1.equals(l2));       // true
    }
}
```
java中是这么理解的，l1,l2都是空串，那么他们不就是相等的么，这就是采取类型擦除的结果。
