# 题目
给一个包含 n 个整数的数组 S, 找到和与给定整数 target 最接近的三元组，返回这三个数的和。

 注意事项

只需要返回三元组之和，无需返回三元组本身

样例
例如 S = [-1, 2, 1, -4] and target = 1. 和最接近 1 的三元组是 -1 + 2 + 1 = 2

# 分析
与前一道题的三数之和思路类似，甚至更为简单，首先对数组进行排序，然后用两根指针，一头一尾，向中间靠近，并不停的更新最佳的三数和

# 代码
```
public class Solution {
    /**
     * @param numbers: Give an array numbers of n integer
     * @param target : An integer
     * @return : return the sum of the three integers, the sum closest target.
     */
    public int threeSumClosest(int[] numbers, int target) {
        // write your code here
        if(numbers ==  null || numbers.length < 3)
        	return -1;
        
        Arrays.sort(numbers);
        
        int bestSum = numbers[0] + numbers[1] + numbers[2];
        
        for(int i=0;i<numbers.length-2;i++) {
        	int left = i+1;
        	int right = numbers.length-1;
        	while(left < right) {
        		int sum = numbers[i] + numbers[left] + numbers[right];
        		if(Math.abs(bestSum-target) > Math.abs(sum-target))
        			bestSum = sum;
        		if(sum < target)
        			left++;
        		else
        			right--;
        	}
        }
        return bestSum;
    }
}
```
