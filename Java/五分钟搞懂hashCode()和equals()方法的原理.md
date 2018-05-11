Java中的超类java.lang.Object 有两个非常重要的方法：
```
public boolean equals(Object obj)
public int hashCode()
```

这两个方法最开发者来说是十分重要的，必须清楚的理解，但实际上，甚至很多经验丰富的Java开发者有时候也没有真正搞清楚这两个方法的使用和原理。当我们自定义了对象，并且想要将自定义的对象加到Map中时，我们就必须对自定义的对象重写这两个方法，才能正确使用Map。我们接下来将用这篇文章指出在使用hashcode和equals方法时，经常范的错误，并指出如何正确的使用这两个方法，以及这两个方法工作的原理。

# 常见的误区
看下面这段代码：
```
import java.util.HashMap;

public class HashCodeEqual {
	public static void main(String[] args) {
		Apple a1 = new Apple("Blue");
		Apple a2 = new Apple("Green");
		
		HashMap<Apple, Integer> map = new HashMap<>();
		map.put(a1, 10);
		map.put(a2, 20);
		
		System.out.println(map.get(new Apple("Green")));
	}
}

class Apple {
	public String color;
	
	public Apple(String color) {
		this.color = color;
	}
	
	@Override
	public boolean equals(Object obj) {
		if(! (obj instanceof Apple))
			return false;
		if(obj == this)
			return true;
		return this.color.equals(((Apple)obj).color);
	}
}
```
我们执行上面这段代码

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-49af8c431017ea23.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

却发现与我们预想的结果并不一样，我们想取出map中颜色为Green的apple，最后却得到一个null值，这说明map没有我们需要的颜色为green的apple对象，但实际上，我们明明向其中添加了一个颜色为green的apple对象，也重写了equals方法，为什么最后却取不出这个对象呢？

![Upload Paste_Image.png failed. Please try again.]

# 错误出现的原因

这个问题引起的原因是因为我们没有重写“hashCode”方法，这就需要我们深入理解equals方法和hashCode方法的原理：
* 如果两个对象是相等的，那么他们必须拥有一样的hashcode，这是第一个前提
* 如果两个对象有一样的hashcode，但仍不一定相等，因为还需要第二个要求，也就是equals方法的判断。
其实，map判断对象的方法就是先判断hashcode是否相等，如果相等再判断equals方法是否返回true，只有同时满足两个条件，最后才会被认为是相等的。
Map查找元素比线性搜索更快，这是因为map利用hashkey去定位元素，这个定位查找的过程分成两步，内部原理中，map将对象存储在类似数组的数组的区域，所以要经过两个查找，先找到hashcode相等的，然后在再在其中按线性搜索使用equals方法，通过这两部来查找一个对象。


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-ea968ed118a4f8b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

就像上图这个结构，每个hashcode对应一个桶，每个tongli桶里还有多个对象，确定桶的方法是hashCode，在桶中遍历线性查找的方法是equals。

在Object中的默认的hashCode方法的实现是为不同的对象返回不同的hashcode,因此如果我们不重写hashcode方法，那么没有任何两个对象会是相等的，因为object类中的hashcode实现是为不同的对象返回不同的hashcode。

所以，我们就搞清楚了上一段代码出错的原因，由于没有重写hashcode方法，所有的对象都是不一样的，所以我们需要重写hashcode方法，让颜色的对象的hashcode是一样的，比较直接的写法就是直接用color的length作为hashcode。

```
public int hashCode(){
return this.color.length();
}
```


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-d92879e7c1a03d4b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

** 切记，一定要同时重写hashCode和equals方法 **
