> # 题目
There are *N* gas stations along a circular route, where the amount of gas at station *i* is gas[i].
You have a car with an unlimited gas tank and it costs cost[i]
 of gas to travel from station *i* to its next station (*i*+1). You begin the journey with an empty tank at one of the gas stations.
Return the starting gas station's index if you can travel around the circuit once, otherwise return -1.
**Note:**The solution is guaranteed to be unique.
[Subscribe](https://leetcode.com/subscribe/) to see which companies asked this question.
在一条环路上有 N 个加油站，其中第 i 个加油站有汽油gas[i]，并且从第_i_个加油站前往第_i_+1个加油站需要消耗汽油cost[i]。
你有一辆油箱容量无限大的汽车，现在要从某一个加油站出发绕环路一周，一开始油箱为空。
求可环绕环路一周时出发的加油站的编号，若不存在环绕一周的方案，则返回-1。
 注意事项
数据保证答案唯一。
样例
现在有4个加油站，汽油量gas[i]=[1, 1, 3, 1]，环路旅行时消耗的汽油量cost[i]=[2, 2, 1, 1]。则出发的加油站的编号为2。

# 分析

最简单的思路就是每个车站尝试求解一遍：
```
public class Solution {
    /**
     * @param gas: an array of integers
     * @param cost: an array of integers
     * @return: an integer
     */
    public int canCompleteCircuit(int[] gas, int[] cost) {
        //从每个车站为起点尝试能不能走通
        for(int i=0;i<gas.length;i++) {
        	int j=i;
        	int curgas = gas[j]; //初始的汽油大小
        	
        	//如果油量足够到下一站
        	while(curgas >= cost[j]) {
        		curgas -= cost[j];//减去所需要的油量
        		
        		j = (j+1) % gas.length; //走到下一站；
        		//如果回到起点，说明走了一圈，可以返回结果了
        		if(j==i)
        			return i;
        		
        		//加上本站的油
        		curgas += gas[j];
        		
        	}
        	
        }
        return -1;
    }
}
```

更好的方法，更有技巧：

** 对于一个循环数组，如果这个数组整体和 SUM >= 0，那么必然可以在数组中找到这么一个元素：从这个数组元素出发，绕数组一圈，能保证累加和一直是出于非负状态。**

因此，当我们发现到达k 站点邮箱见底儿后，i 到 k 这些站点都不用作为出发点来试验了，肯定不满足条件，只需要从k+1站点尝试即可！

```
public class Solution {
    /**
     * @param gas: an array of integers
     * @param cost: an array of integers
     * @return: an integer
     */
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int sum = 0;  
        int total = 0;  
        int j = -1;  
        for (int i = 0; i < gas.length; i++) {  
            sum += gas[i] - cost[i];  
            total += gas[i] - cost[i];  
            if(sum < 0) {   //之前的油量不够到达当前加油站  
                j = i;  
                sum = 0;  
            }  
        }  
        if (total < 0) return -1;    //所有加油站的油量都不够整个路程的消耗  
        else return j + 1;  
    }
}
```
