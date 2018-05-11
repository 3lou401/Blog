# 题目
wiki链接: Cosine Similarity
这里给出公式:
![/media/problem/cosine-similarity.png](http://upload-images.jianshu.io/upload_images/1234352-2208a8fdeeea850e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
给你两个相同大小的向量 *A* *B*，求出他们的余弦相似度
返回2.0000
如果余弦相似不合法 (比如 A = [0] B = [0]).

**样例**
给出 *A* =[1, 2, 3], *B* =[2, 3 ,4]
返回 0.9926.
给出 *A* =[0], *B* =[0]
返回 2.0000

# 分析
这道题较为简单，直接计算就可以了

# 代码
```
class Solution {
    /**
     * @param A: An integer array.
     * @param B: An integer array.
     * @return: Cosine similarity.
     */
    public double cosineSimilarity(int[] A, int[] B) {
        // write your code here
            	int ab=0;
    	if(sumArray(A) == 0 || sumArray(B) == 0)
    	{
    		return 2.00000;
    	}
    	for(int i=0;i<A.length;i++)
    	{
    		ab += A[i] * B[i];
    	}
    	return ab/Math.sqrt(sumArray(A))/Math.sqrt(sumArray(B));
    }
    
        public double sumArray(int[] A)
    {
    	int sum = 0;
    	for(int i=0;i<A.length;i++)
    	{
    		sum += A[i] * A[i];
    	}
    	return sum;
    }
}
```
