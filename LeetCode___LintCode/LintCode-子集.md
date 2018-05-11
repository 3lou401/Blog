# 题目
给出一个具有重复数字的列表，找出列表所有不同的排列。

样例
给出列表 [1,2,2]，不同的排列有：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-6ce18120a2f1d77b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 代码
```
class Solution {
    /**
     * @param nums: A list of integers.
     * @return: A list of unique permutations.
     */
    public List<List<Integer>> permuteUnique(int[] nums) {
	    
        ArrayList<List<Integer>> res = new ArrayList<>();
        
        if(nums == null)
        	return null;
        
        if(nums.length == 0)
        {	
        	res.add(new ArrayList<Integer>());
        	return res;
        }
        
        ArrayList<Integer> list = new ArrayList<>();
        
        //先将数组排序，这样相同元素将会出现在一起
        Arrays.sort(nums);
        
        int n = nums.length;
        int[] visited = new int[n];
        for(int i=0;i<n;i++)
        	visited[i] = 0;//0标识未访问
        
        helper(res, list, visited, nums);
        return res;
    }

	private void helper(ArrayList<List<Integer>> res, ArrayList<Integer> list, int[] visited, int[] nums) {
		
		if(nums.length == list.size()) {
			res.add( new ArrayList<Integer>(list));
		}
		
		for(int i=0;i<nums.length;i++) {
			
			if(visited[i] == 1 || i!= 0 && (visited[i-1] == 0 && nums[i] == nums[i-1]))
				continue;
			/*
            上面的判断主要是为了去除重复元素影响。
            比如，给出一个排好序的数组，[1,2,2]，那么第一个2和第二2如果在结果中互换位置，
            我们也认为是同一种方案，所以我们强制要求相同的数字，原来排在前面的，在结果
            当中也应该排在前面，这样就保证了唯一性。所以当前面的2还没有使用的时候，就
            不应该让后面的2使用。
            */
			list.add(nums[i]);
			visited[i] = 1;
			helper(res, list, visited, nums);
			list.remove(list.size()-1);
			visited[i] = 0;
			
		}
			
		
	}
}
```
