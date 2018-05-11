# 题目
给出一个有n个整数的数组S，在S中找到三个整数a, b, c，找到所有使得a + b + c = 0的三元组。

 注意事项

在三元组(a, b, c)，要求a <= b <= c。

结果不能包含重复的三元组。

样例
如S = {-1 0 1 2 -1 -4}, 你需要返回的三元组集合的是：

(-1, 0, 1)

(-1, -1, 2)

# 分析
从第一个元素开始，找剩下两个元素，分别用两根指针一头一尾，慢慢靠近。
具体看代码里的注释

# 代码
```
public class Solution {
    /**
     * @param numbers : Give an array numbers of n integer
     * @return : Find all unique triplets in the array which gives the sum of zero.
     */
    public ArrayList<ArrayList<Integer>> threeSum(int[] nums) {
        //先将数组排序，由小到大，然后依次从第一个元素开始，两根指针，一头一尾遍历，如果找到等于target也就是0的就add进结果。
        
    	//判断边界情况，当nums为空或者null的时候
    	if(nums == null || nums.length == 0) {
    		return null;
    	}
    	
    	ArrayList<ArrayList<Integer>> res = new ArrayList<>();
    	//首先对数组进行排序
    	Arrays.sort(nums);
    	
    	//从第一个元素开始循环
    	for(int i=0;i<nums.length-2;i++) {
    		//优化情况，如果两个或多个元素相等判断一次就行了，相等的元素可以直接跳过
    		if(i != 0 && nums[i] == nums[i-1])
    			continue;
    		
    		//设置两个指针一头一尾开始找
    		int left = i+1;
    		int right = nums.length-1;
    		
    		//两根指针没相遇时寻找
    		while(left < right) {
    			int sum = nums[i] + nums[left] + nums[right];
    			
    			if(sum > 0)
    				right--;
    			else if(sum < 0)
    				left++;
    			else{
    				ArrayList<Integer> temp = new ArrayList<>();
    				temp.add(nums[i]);
    				temp.add(nums[left]);
    				temp.add(nums[right]);
    				res.add(temp);
    				left++;
        			right--;
    				//同样可以优化，如果有相等的元素判断一次就可以
    				while(left<right && nums[left] == nums[left-1])
    					left++;
    				while(left<right && nums[right] == nums[right+1])
    					right--;
    			}
    			
    		}
    	}
      return res;
    }
    
}
```
