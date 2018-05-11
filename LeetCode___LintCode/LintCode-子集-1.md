# 题目
给定一个含不同整数的集合，返回其所有的子集

 **注意事项**
子集中的元素排列必须是非降序的，解集必须不包含重复的子集

**样例**
如果 S = [1,2,3]，有如下的解：

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1234352-5b0b61a8b75ea8b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# 代码
```
class Solution {
    /**
     * @param S: A set of numbers.
     * @return: A list of lists. All valid subsets.
     */
    public ArrayList<ArrayList<Integer>> subsets(int[] num) {
        ArrayList<ArrayList<Integer>> res = new ArrayList<>();
        
        if(num == null || num.length == 0)
        	return res;
        
        ArrayList<Integer> list = new ArrayList<>();
        
        Arrays.sort(num);
        
        helper(res, list, num, 0);
        return res;
    }

	private void helper(ArrayList<ArrayList<Integer>> res, ArrayList<Integer> list, int[] num, int pos) {
		
		res.add(new ArrayList<>(list));
		
		for(int i=pos;i<num.length;i++) {
			list.add(num[i]);
			helper(res, list, num, i+1);
			list.remove(list.size() - 1);
		}
		
	}
}
```
