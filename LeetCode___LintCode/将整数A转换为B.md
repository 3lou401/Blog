如果要将整数A转换为B，需要改变多少个bit位？

**样例**
如把31转换为14，需要改变2个bit位。(**31**)10=(**11111**)2
(**14**)10=(**01110**)2

```
class Solution {
    /**
     *@param a, b: Two integer
     *return: An integer
     */
    public static int bitSwapRequired(int a, int b) {
        // write your code here
        int c = a ^ b;
        return countOnes(c);
    }
    
        public static int countOnes(int num) {
        // write your code here
        return countOnes3(num);
    }
    
        public static int countOnes3(int num){
        int count = 0;
        while(num!=0){
            num = num & (num-1);
            count++;
        }
        return count;
    }
}

```
