Write a program that outputs the string representation of numbers from 1 to n.

But for multiples of three it should output “Fizz” instead of the number and for the multiples of five output “Buzz”. For numbers which are multiples of both three and five output “FizzBuzz”.

Example:

n = 15,

Return:
[
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
]
# 题目
给你一个整数*n*. 从 *1* 到 *n* 按照下面的规则打印每个数：
* 如果这个数被3整除，打印fizz.
* 如果这个数被5整除，打印buzz.
* 如果这个数能同时被3和5整除，打印fizz buzz.

#分析
较为简单，就是一个分支问题

# 代码
```
public class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> res = new ArrayList<String>();
        for(int i=1;i<=n;i++) {
        	if(i % 15 == 0)
        		res.add("FizzBuzz");
        	else if(i % 3 == 0)
        		res.add("Fizz");
        	else if(i % 5 == 0)
        		res.add("Buzz");
        	else
        		res.add(String.valueOf(i));
        }
        return res;
    }
}
```


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-83bf722713093595.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
