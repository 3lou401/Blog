# 题目

Given an integer array, sort it in ascending order. Use selection sort, bubble sort, insertion sort or any O(n2) algorithm.

**Example** Given[3, 2, 1, 4, 5] , return[1, 2, 3, 4, 5]
.
# 分析
显然这道题我们可以用冒泡，选择，插入等排序方法实现。
就此机会正好复习一下这三种复杂度为 O(n2)的排序算法

# 解答
## 冒泡排序
```
public class Bubble {

	public static void sort(int a[])
	{
		int N = a.length;
		
		for(int i=0;i<N;i++)
		{
			for(int j=0;j<N-i-1;j++)
			{
				if(a[j+1]<a[j])
					exch(a,j,j+1);
			}
		}
	}
}
```

#选择排序
```
public class Selection {

	public static void sort(int a[])
	{
		int N = a.length;
		
		for(int i=0;i<N;i++)
		{
			int min = i;
			for(int j=i+1;j<N;j++)
			{
				if(a[j]<a[min])
					min = j;
			}
			exch(a,i,min);
		}
	}
}
```
# 插入排序
```
public class Insert {

	public static void sort(int a[])
	{
		int N = a.length;
		
		for(int i=1;i<N;i++)
		{
			for(int j=i;j>0 && a[j]<a[j-1];j--)
			{
				exch(a,j,j-1);
			}			
		}
	}
}
```

# 小结
三种排序都用了两个for循环实现。实现这三种排序的代码很多，也可以用while循环实现。重要的是三种排序方法的思想以及将算法转换为代码的能力
