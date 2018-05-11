> Determine whether an integer is a palindrome. Do this without extra space.
判断一个正整数是不是回文数。
回文数的定义是，将这个数反转之后，得到的数仍然是同一个数。

# 分析
最常规的思路是直接将数反转再对比，但实际上，我们只需要反转后半部分跟前半部分对比就可以了。两种情况，一是位数为偶数，直接对比，位数为奇数，大数除以10对比。

# 代码
```
public boolean isPalindrome(int x) {
        if(x < 0 || x%10 ==0 && x!=0)
        	return false;
        int rev = 0;
        while(x > rev) {
        	rev = rev*10 + x%10;
        	x = x/10;
        }
        return (x ==rev || x==rev/10);
    }
```



![image.png](http://upload-images.jianshu.io/upload_images/1234352-24d3ac163e3f12e1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
