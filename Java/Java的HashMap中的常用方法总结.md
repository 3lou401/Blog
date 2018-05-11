HashMap在编程中是一个非常有用的工具，使用的频率很高，所以本文简单总结一下hashmap的常用方法

# 遍历HashMap
可以通过entryset取得iter，然后逐个遍历
```
Iterator it = mp.entrySet().iterator();
while (it.hasNext()) {
Map.Entry pairs = (Map.Entry)it.next();
System.out.println(pairs.getKey() + " = " + pairs.getValue());
}
```
也可以直接简单的for循环遍历
```
Map<Integer, Integer> map = new HashMap<Integer, Integer>();
for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
System.out.println("Key = " + entry.getKey() + ", Value = " +
entry.getValue());
}
```

# 打印HashMap
```
public static void printMap(Map mp) {
Iterator it = mp.entrySet().iterator();
while (it.hasNext()) {
Map.Entry pairs = (Map.Entry)it.next();
System.out.println(pairs.getKey() + " = " + pairs.getValue());
it.remove(); // avoids a ConcurrentModificationException
}
}
```

# 根据HashMap的value进行排序
```
class ValueComparator implements Comparator<String> {
Map<String, Integer> base;
public ValueComparator(Map<String, Integer> base) {
this.base = base;
}
public int compare(String a, String b) {
if (base.get(a) >= base.get(b)) {
return -1;
} else {
return 1;
} // returning 0 would merge keys
}
}
```
```
HashMap<String, Integer> countMap = new HashMap<String, Integer>();
//add a lot of entries
countMap.put("a", 10);
countMap.put("b", 20);
ValueComparator vc = new ValueComparator(countMap);
TreeMap<String,Integer> sortedMap = new TreeMap<String,Integer>(vc);
sortedMap.putAll(countMap);
printMap(sortedMap);
```
这种方法是在stackoverflow上被voted最多的，借用treeMap的构造函数
