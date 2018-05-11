> Reverse digits of an integer.
Example1: x = 123, return 321
Example2: x = -123, return -321
Note:
The input is assumed to be a 32-bit signed integer. Your function should return 0 when the reversed integer overflows.
将一个整数中的数字进行颠倒，当颠倒后的整数溢出时，返回 0 (标记为 32 位整数)。

# 分析
通过取余得到最后一位，循环，对于溢出进行判断即可

# 代码
```
public int reverse(int x) {
        int result = 0;

	    while(x != 0) {
	    	int tail = x % 10;
	    	int newResult = result * 10 + tail;
	    	if((newResult-tail)/10 != result)
	    		return 0;
	    	result = newResult;
	    	x = x/10;
	    }
	    return result;
    }
```


![image.png](http://upload-images.jianshu.io/upload_images/1234352-00215c266efd4eab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
