# 题目
给出一个不含重复数字的排列，求这些数字的所有排列按字典序排序后该排列的编号。其中，编号从1开始。
**样例**
例如，排列 [1,2,4]是第 1个排列。

# 分析
1.对于四位数：4213 = 4*100+2*100+1*10+3
2.4个数的排列有4！种。当我们知道第一位数的时候，还有3！种方式，当知道第二位数时候还有2！种方式，当知道第三位数的时候还有1！种方式，前面三位数都确定的时候，最后一位也确定了。<这里是按照高位到地位的顺序>
3.对4个数的排列，各位的权值为：3！，2！，1！，0！。第一位之后的数小于第一位的个数是x，第二位之后的数小于第二位的个数是y，第三位之后的数小于第三的个数是z，第四位之后的数小于第四位的个数是w，则abcd排列所在的序列号:index = x*3！+y*2!+z*1!,<0!=0>
在数的排列中，小数在前面，大数在后面，所以考虑该位数之后的数小于该为的数的个数，这里我自己理解的也不是很透，就这样。
4.例如 4213；x= 3，y = 1，z=0，index = 18+2=20
123；x = 0，y=0，index = 0
321；x= 2，y=1，index = 2*2！+1*1！ = 5
这里的下标是从0开始的。

# 代码
```
public class Solution {
    /**
     * @param A an integer array
     * @return a long integer
     */
    public long permutationIndex(int[] permutation) {
        // Write your code here
        long index = 0;
  long position = 2;// position 1 is paired with factor 0 and so is skipped
  long factor = 1;
  for (int p = permutation.length - 2; p >= 0; p--) {
    long successors = 0;
    for (int q = p + 1; q < permutation.length; q++) {
      if (permutation[p] > permutation[q]) {
        successors++;
      }
    }
    index += (successors * factor);
    factor *= position;
    position++;
  }
  return index+1;
    }
}
```
