> You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.
Suppose you have n
 versions [1, 2, ..., n]
 and you want to find out the first bad one, which causes all the following ones to be bad.
You are given an API bool isBadVersion(version)
 which will return whether version
 is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.
**Credits:**Special thanks to [@jianchao.li.fighter](https://leetcode.com/discuss/user/jianchao.li.fighter) for adding this problem and creating all test cases.
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
# 题目
代码库的版本号是从 1 到 n 的整数。某一天，有人提交了错误版本的代码，因此造成自身及之后版本的代码在单元测试中均出错。请找出第一个错误的版本号。

你可以通过 isBadVersion 的接口来判断版本号 version 是否在单元测试中出错，具体接口详情和调用方法请见代码的注释部分。

 注意事项

请阅读上述代码，对于不同的语言获取正确的调用 isBadVersion 的方法，比如java的调用方式是SVNRepo.isBadVersion(v)

样例
给出 n=5

调用isBadVersion(3)，得到false

调用isBadVersion(5)，得到true

调用isBadVersion(4)，得到true

此时我们可以断定4是第一个错误的版本号

# 分析
二分法

# 代码
```
/**
 * public class SVNRepo {
 *     public static boolean isBadVersion(int k);
 * }
 * you can use SVNRepo.isBadVersion(k) to judge whether 
 * the kth code version is bad or not.
*/
class Solution {
    /**
     * @param n: An integers.
     * @return: An integer which is the first bad version.
     */
    public int findFirstBadVersion(int n) {
        // write your code here
        int start = 1, end = n;
        while (start + 1 < end) {
            int mid = start + (end - start) / 2;
            if(SVNRepo.isBadVersion(mid)){
                end = mid;
            } else {
            	start = mid;
            }
        }
            
        if (SVNRepo.isBadVersion(start)) {
            return start;
        }
        return end;
    }
}
```
