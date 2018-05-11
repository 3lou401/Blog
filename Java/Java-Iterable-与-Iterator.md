如果你想写一个foreach方法，访问List里面的所有对象，你可能会这么写
```
public static void forEach(List list) {
        int size = list.size();
        for(int i = 0; i < size; i++) {
            out.println(list.get(i));
        }
    }
```
但实际中，我们不需要这么麻烦，因为所有collection都有一个iterator()方法，在JDK1.4之前这个方法定义在collection接口中的，因此所有collection都有这个方法。

iterator()方法會傳回java.util.Iterator介面的實作物件，這個物件包括了Collection收集的所有物件，你可以使用Iterator的hasNext()看看有無下一個物件，若有的話，再使用next()取得下一個物件。因此，無論是List、Set、Queue或任何Collection，都可以使用以下的forEach()來顯示所收集之物件：

iterator方法会传回java.util.Iterator接口的实例对象，你可以使用hasNext方法看看有没有下一个对象，如果有的话，在使用next取得下一个对象，所以无论通过多态，无论是List,Set,Queue或者任何collection,都可以使用下面这个foreach方法收集对象：
```
public static void forEach(Collection collection) {
        Iterator iterator = collection.iterator();
        while(iterator.hasNext()) {
            out.println(iterator.next());
        }
    }
```
在JDK5之後，原先定義在Collection中的iterator()方法，提昇至新的java.util.Iterable父介面，因此在JDK5之後，你可以使用以下的forEach()方法顯示收集的所有物件：
在JDK5之后，原先定义在collection中的iterator方法，提升到新的java.util.Iterable接口中，这样做的好处是，所有实现了iterable接口的对象，都是可迭代的，而之前只有collection是可迭代的。所以在JDK5之后，可以使用下面的foreach方法收集所有的对象：
```
public static void forEach(Iterable iterable) {
        Iterator iterator = iterable.iterator();
        while(iterator.hasNext()) {
            out.println(iterator.next());
        }
    }
```
同时由于JDk5之后还引入了增强式的for循环，foreach可以进一步的简化
```
import java.util.*;

public class ForEach {
    public static void main(String[] args) {
        List names = Arrays.asList("Justin", "Monica", "Irene");
        forEach(names);
        forEach(new HashSet(names)); 
        forEach(new ArrayDeque(names)); 
    }

    public static void forEach(Iterable iterable) {
        for(Object o : iterable) {
            System.out.println(o);
        }
    }
}
```
实际上，增强式for循环是编译器的蜜糖，实际代码会展开为：
```
public static void forEach(Iterable iterable) {
    Object o;
    for(Iterator i\$ = iterable.iterator(); i\$.hasNext(); System.out.println(o)) {
        o = i\$.next();
    }
}
```
可以看到实际还是调用iterator方法。

由于iterable接口的引进，导致所有实现了这个接口的方法，都可以iterator，看下面这个例子：
```
package Collection;

import java.util.Iterator;

public class IterableString implements Iterable<Character> {
	
	private String original;

    public IterableString(String original) {
        this.original = original;
    }

	public static void main(String[] args) {
		IterableString str = new IterableString("Justin");
    	for(Character c : str) {
            System.out.println(c);
        }
		// TODO Auto-generated method stub

	}

	@Override
	public Iterator<Character> iterator() {
		// TODO Auto-generated method stub
		return new InnerIterator();
	}
	
	private class InnerIterator implements Iterator<Character> {
        private int index;
        public boolean hasNext() {
            return index < original.length();
        }

        public Character next() {
            Character c = original.charAt(index);
            index++;
            return c;
        }

        public void remove() {}
    }

}

```
查看编译器，我们发现编译器做了这些事：
```
Character c;
for(Iterator iterator = str.iterator(); iterator.hasNext();
  System.out.println(c))
    c = (Character)iterator.next();
```
