# 题目
给定一个数字列表，返回其所有可能的排列。

**注意事项**
你可以假设没有重复数字。

**样例**
给出一个列表[1,2,3]，其全排列为：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-4cca035ced46c5f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 分析

可以用递归和非递归解决

首先递归法，也是利用了回溯法和深度优先搜索。

我们考虑一个一个将数组元素加入到排列中，递归求解，就好像下面的解答树：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-b54091100c813c0c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

添加的时候排除掉相同的元素即可，回溯法我们经常会设置一个已访问标识数组，来表示数组被访问过，但这里不用这样，因为如果list里面已经包含就说明已经访问过了，所以只要判断，跳过已有的元素即可。
再考虑递归的结束条件，当元素都添加足够就结束了，添加足够的意思就是，元素个数等于数组的长度。

```
class Solution {
    /**
     * @param nums: A list of integers.
     * @return: A list of permutations.
     */
    public List<List<Integer>> permute(int[] nums) {
		List<List<Integer>> res = new ArrayList<>();
		if(nums == null)
			return res;
		if(nums.length == 0)
		{	
			res.add(new ArrayList<Integer>());
			return res;
		}
			
		ArrayList<Integer> list = new ArrayList<>();
		dfs(res, list, nums);
		return res;
   }

	private void dfs(List<List<Integer>> res, ArrayList<Integer> list, int[] nums) {
		
		int n = nums.length;
		if(list.size() == n)
		{	
			res.add(new ArrayList<Integer>(list));
			return;
		}
		
		for(int i = 0;i < n;i++) {
			if(list.contains(nums[i]))
				continue;
			list.add(nums[i]);
			dfs(res, list, nums);
			list.remove(list.size() - 1);
		}
	}
}
```

非递归实现
思路是这样的，就是高中的排列组合知识，运用插入法即可，假设有i个元素的排列组合，那么对于i+1个元素，可以考虑就是将i+1的元素插入到上述的排列的每一个位置即可。

```
class Solution {
    /**
     * @param nums: A list of integers.
     * @return: A list of permutations.
     */
    public List<List<Integer>> permute(int[] nums) {
		List<List<Integer>> res = new ArrayList<List<Integer>>();  
        if ( nums == null)  
            return res;
        if( nums.length == 0)
        {    
            res.add(new ArrayList<Integer>());
            return res;
        }
        List<Integer> list = new ArrayList<>();
        list.add(nums[0]);
        res.add(new ArrayList<Integer>(list));
        
        for(int i=1;i<nums.length;i++) {
        	int size1 = res.size();
        	for(int j=0;j<size1;j++) {
        		int size2 = res.get(0).size();
        		for(int k=0;k<=size2;k++) {
        			ArrayList<Integer> temp = new ArrayList<>(res.get(0));
        			temp.add(k,nums[i]);
        			res.add(temp);
        		}
        		res.remove(0);
        	}
        }
        return res;
   }
}
```
