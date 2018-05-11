# 题目
给一个包含n个数的整数数组S，在S中找到所有使得和为给定整数target的四元组(a, b, c, d)。

 注意事项

四元组(a, b, c, d)中，需要满足a <= b <= c <= d

答案中不可以包含重复的四元组。

样例
例如，对于给定的整数数组S=[1, 0, -1, 0, -2, 2] 和 target=0. 满足要求的四元组集合为：

(-1, 0, 0, 1)

(-2, -1, 1, 2)

(-2, 0, 0, 2)

# 分析
跟三数之和思路一样，多一层循环而已

# 代码
```
public class Solution {
    /**
     * @param numbers : Give an array numbersbers of n integer
     * @param target : you need to find four elements that's sum of target
     * @return : Find all unique quadruplets in the array which gives the sum of
     *           zero.
     */
    public ArrayList<ArrayList<Integer>> fourSum(int[] num, int target) {
        if(num == null || num.length <4)
			return new ArrayList<>();
			
			
		ArrayList<ArrayList<Integer>> rst = new ArrayList<ArrayList<Integer>>();
		Arrays.sort(num);
		
		for(int i=0;i<num.length-3;i++) {
			if(i!=0 && num[i] ==num[i-1])
				continue;
			
			for(int j=i+1;j<num.length-2;j++) {
				if(j!=i+1 && num[j] ==num[j-1])
					continue;
				
				int left = j+1;
				int right = num.length-1;
				
				while(left < right) {
					int sum = num[i] + num[j] + num[left] + num[right];
					if(sum == target) {
						ArrayList<Integer> temp = new ArrayList<>();
						temp.add(num[i]);
						temp.add(num[j]);
						temp.add(num[left]);
						temp.add(num[right]);
						rst.add(temp);
						left++;
						right--;
						while(left<right && num[left] == num[left-1])
							left++;
						while(left<right && num[right] == num[right+1])
							right--;
					} else if(sum < target)
						left++;
					else
						right--;
					
				}
			}
		}
		return rst;
				
    }
}
```
