collection是java中用来收集对象的。java提供了collection的Api，为了避免出现死记api的情况，为了更好的使用collection，首先我们需要对collection的继承架构有一个清晰的认识。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-3dc0a14c0075e369.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们可以从这个架构图中得出很多信息
* iterable在这个架构中处于顶端，说明所有collection都是可iterable的，意思就是所有collection都是可迭代的，所有继承自collection的list,set等等都实现了iterable接口，并且可以产生iterator进行迭代。
* 操作对象的行为例如，add，remove等方法都定义在collection中，所以所有collection都可以添加或者移除对象，这是显而易见。
* 收集对象的行为都定义在collection中，然而不同的对象不同的情境下，我们对对象会有不同的操作，如果想收集时记录每个对象的顺序，并可以按照索引取回每个物件，这样的行为定义在list中，如果想让对象拥有类似集合的那种性质，不重复，无序的，则这样的行为定义在set中，如果想和队列一样，这样的行为就定义在Queue中。

我们举个例子，例如我们最常使用的arraylist,它的架构是这样的

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-cef70022d0a097d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

abstractCollection实现了collection接口，实现了里面定义的行为
abstractList实现了list接口，实现了里面定义的行为
Arraylist则继承自abstractList类，并且实现了List接口。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-32fba1060d9bcc34.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面这个架构图就很简洁的介绍了collection中的各个接口类之间的继承关系。

# 总结
* 在Java中，必須了解collection的基本架构，如此就不会繁杂的API所迷惑。Collection继承自Iterable接口
* Iterable的iterator()规定返回一个Iterator的对象，这是因为，不同的对象会导致要一个个取出对象需要不同的方法，为了有个统一的方法进行迭代，循环访问所有的对象，而不用管是使用的哪种collection，所以可以使用 [Iterator 模式]
* 由于Collection的父类是Iterable，這代表所有继承自Collection的对象，都可以使用iterator()方法返回Iterator对象，然後使用Iterator对象来循环访问所有的容器中的对象。

* List对象，在收集对象时，会带有索引的信息，最基本的就是使用数组作为List的內部的存储结构，因为矩阵本身就是具有索引结构的，这其实就是ArrayList的实现。由於使用数组作为List的內部的存储结构，所以指定索引随机取出对象时非常快速，但插入对象则较慢。LinkedList 則在內部使用链表的结构实现，适合插入不适合随机存取。

* Iterator基本上只能往后一个一个访问对象，也就是可以使用 hasNext()測試是否有下一对象，使用next()取得下一对象，而其子接口ListIterator，則提供了往前走訪对象的行為，也就是可以使 用hasPrevious()測試是否有上一对象，使用previous()取得上一物件。ListIterator可由List物件的 listIterator()返回，List具有索引特性，ListIterator則也提供了nextIndex()與previousIndex() 可以取得下一個或上一個索引。

* Set中所收集的对象是不重复的，是否重复的判断则涉及到所收集对象的equals()及hashCode()的方法来判断，事实上，** 所有要被Collection收集的对象，建议都重新定義其equals()與hashCode()方法，以在必要的使用供 Collection使用。**

有了这些基本的架构认识，面对各个实例对象上复杂的参数，就不会再迷惑。
