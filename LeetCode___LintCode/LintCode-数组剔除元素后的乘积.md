# 题目
给定一个整数数组A。

定义B[i] = A[0] * ... * A[i-1] * A[i+1] * ... * A[n-1]， 计算B的时候请不要使用除法。

**样例**
给出A=**[1, 2, 3]**，返回 B为**[6, 3, 2]**

# 代码
```
public class Solution {
    /**
     * @param A: Given an integers array A
     * @return: A Long array B and B[i]= A[0] * ... * A[i-1] * A[i+1] * ... * A[n-1]
     */
    public ArrayList<Long> productExcludeItself(ArrayList<Integer> nums) {
        // write your code
        int n = nums.size();
        long[] left = new long[n];
        long[] right = new long[n];
        for(int i=0;i<n;i++)
        {
        	left[i] = 1;
        	right[i] = 1;
        }
        ArrayList<Long> res = new ArrayList<>();

        for (int i = 1; i < n; ++i)
            left[i] = left[i-1] * nums.get(i-1);
        for (int i = n-2; i > 0; --i)
            right[i] = right[i+1] * nums.get(i-1);

        for(int i = 0; i != nums.size(); ++i)
            res.add( left[i] * right[i] );
        return res;
    }
}

```
