我们都知道Map是一种键-值对的数据结构，每个键都是唯一的！本文讨论了关于Java中Map使用的最常见的8个问题。为了叙述的简单，所有的例子都会使用泛型。并且本文出现的泛型符号 K,V,都是继承实现Comparable接口的！

# 1 将Map转换成一个List

Java中，Map接口提供了三个集合表现：
* key set
* value set
* key-value
这三个都可以被转换为List通过使用构造函数初始化或者addAll方法。下面这段简单的代码段向我们展示了如何从Map中构造一个ArrayList。
```
// key list
List keyList = new ArrayList(map.keySet());
// value list
List valueList = new ArrayList(map.valueSet());
// key-value list
List entryList = new ArrayList(map.entrySet());
```

# 2 遍历map中的键值对

遍历一个map中的键值对是最基本的操作。为此，在java中，所有这些键值对都存储在Map.Entry的实例中，我们调用Map.entrySet() 就会返回一个存储着所有键值对的对象，然后遍历循环就可以得到了。
```
for(Entry entry: map.entrySet()) {
// get key
K key = entry.getKey();
// get value
V value = entry.getValue();
}
```
在jDK1.5之前我们也可以使用Iterator
```
Iterator itr = map.entrySet().iterator();
while(itr.hasNext()) {
Entry entry = itr.next();
// get key
K key = entry.getKey();
// get value
V value = entry.getValue();
}
```

# 3 根据Map的key值排序

根据map的key值将map进行排序是一个很常用的操作。
一个方法就是将Map.Entry转换到一个list里去，然后list排序就可以了。
如下面的例子：
```
List list = new ArrayList(map.entrySet());
Collections.sort(list, new Comparator() {
@Override
public int compare(Entry e1, Entry e2) {
return e1.getKey().compareTo(e2.getKey());
}
});
```
另一个方法就是使用SortedMap,这种collection会将所有的key按照给定的排序comparator进行排序。所以sortedMap中的key必须实现了comparable接口，或者实现comparator。
sortedMap的一个实例类就是TreeMap,他的构造函数可以接受一个comparator参数，下面的代码说明了怎样将一个普通的Map转换成sortedmap。
```
SortedMap sortedMap = new TreeMap(new Comparator() {
@Override
public int compare(K k1, K k2) {
return k1.compareTo(k2);
}
});
sortedMap.putAll(map);
```

# 4 根据Map的value值排序

第一种方法也是将map转换成一个list，然后根据value排序，方法与key的排序是一样的。
```
List list = new ArrayList(map.entrySet());
Collections.sort(list, new Comparator() {
@Override
public int compare(Entry e1, Entry e2) {
return e1.getValue().compareTo(e2.getValue());
}
});
```

显然key的第二种方法也是可以适用的，但必须要求值是唯一的，我们也可以将key和value进行反转，但是并不推荐这样做。

# 5 初始化一个静态的不可变的Map

如果你需要一个map像静态常量那样保持不变，那么我们将它复制到一个immutable的map中，也就是不可变Map。这样做不仅可以帮我们保证使用时的不变性，同时还可以起到线程安全的作用。

初始化一个static/immutable的map的时候，我们可以使用一个static修饰符。
问题在于，虽然我们将map声明为static，但是这个map仍然可以被操作，比如
map.put(3,"three");因此，这个map还不是真正意义上不可变的。为了创建一个不可变的map，我们需要static修饰符，同时需要一个额外的匿名类，并且在最后一步将其复制到一个不可以操作的map中。
```
public class Test {
private static final Map map;
static {
map = new HashMap();
map.put(1, "one");
map.put(2, "two");
}
}
public class Test {
private static final Map map;
static {
Map aMap = new HashMap();
aMap.put(1, "one");
aMap.put(2, "two");
map = Collections.unmodifiableMap(aMap);
}
}
```

# 6 HashMap TreeMap HashTable的对比
java中的Map的实现主要有三种，分别是HashMap TreeMap HashTable，最重要的差别有以下几方面：
* 迭代的顺序。hashMap和HashTable迭代是，是无序的，无法预测会以特定的顺序进行迭代。但是treemap迭代的时候，是有序的，会按照key的comparator给定的排序规则进行排序。

* key-value的范围。hashmap允许key为null和value为null，而且只允许一个一个key为null，因为map不可以有两个相同的键值啊！。hashtable不允许有key为null或者value为null。TreeMap同样也不允许有key为null或者value为null

* 同步。只有Hashtable是同步，也就是线程安全的，其他两个都不是。因此，如果我们需要一个线程安全的map，那我们需要使用hashtable，如果不需要线程安全，那更推荐使用hashmap和hashtable。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-3fb99fb1afd6594c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
