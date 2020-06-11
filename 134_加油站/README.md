# [134. 加油站](https://leetcode-cn.com/problems/gas-station/)

在一条环路上有 N 个加油站，其中第 i 个加油站有汽油 gas[i] 升。

你有一辆油箱容量无限的的汽车，从第 i 个加油站开往第 i+1 个加油站需要消耗汽油 cost[i] 升。你从其中的一个加油站出发，开始时油箱为空。

如果你可以绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1。

说明: 

如果题目有解，该答案即为唯一答案。
输入数组均为非空数组，且长度相同。
输入数组中的元素均为非负数。

```
 1:

输入: 
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]

输出: 3

解释:
从 3 号加油站(索引为 3 处)出发，可获得 4 升汽油。此时油箱有 = 0 + 4 = 4 升汽油
开往 4 号加油站，此时油箱有 4 - 1 + 5 = 8 升汽油
开往 0 号加油站，此时油箱有 8 - 2 + 1 = 7 升汽油
开往 1 号加油站，此时油箱有 7 - 3 + 2 = 6 升汽油
开往 2 号加油站，此时油箱有 6 - 4 + 3 = 5 升汽油
开往 3 号加油站，你需要消耗 5 升汽油，正好足够你返回到 3 号加油站。
因此，3 可为起始索引。
示例 2:

输入: 
gas  = [2,3,4]
cost = [3,4,3]

输出: -1

解释:
你不能从 0 号或 1 号加油站出发，因为没有足够的汽油可以让你行驶到下一个加油站。
我们从 2 号加油站出发，可以获得 4 升汽油。 此时油箱有 = 0 + 4 = 4 升汽油
开往 0 号加油站，此时油箱有 4 - 3 + 2 = 3 升汽油
开往 1 号加油站，此时油箱有 3 - 3 + 3 = 3 升汽油
你无法返回 2 号加油站，因为返程需要消耗 4 升汽油，但是你的油箱只有 3 升汽油。
因此，无论怎样，你都不可能绕环路行驶一周。

```

**模拟**

```python
  Solution(object):
    def canCompleteCircuit(self, gas, cost):
        """
        :type gas: List[int]
        :type cost: List[int]
        :rtype: int
        """
        res = []
        # 首先使用gas-cost
        for i in range(len(gas)):
            res.append(gas[i]-cost[i])
        # 小于0说明到不了最后的地方
        if sum(res)<0:
            return -1
        # 开始找起点，起点为res值大于等于0的地点
        for i in range(0,len(res)):
            if res[i]>=0:
                # 比较从起点开始，左边和右边油量的情况
                l ,r = sum(res[0:i]),sum(res[i:])
                # 如果r>l,说明总体可到达，但是i右边不能出现负值
                if r >=abs(l):
                    flag = 1
                    num = 0
                    for j in range(i,len(res)):
                        # 有负值说明该点不能到达
                        if sum(res[i:j+1])<0:
                            flag=0
                            break
                    if flag:
                        return i
        return -1 
```

**[官 解](<https://leetcode-cn.com/problems/gas-station/solution/jia-you-zhan-by-leetcode/>)**

```python
class Solution(object):
    def canCompleteCircuit(self, gas, cost):
        """
        :type gas: List[int]
        :type cost: List[int]
        :rtype: int
        """
        total_gas = 0
        startindex = 0
        current_gas = 0
        for i in range(len(gas)):
            total_gas += gas[i]-cost[i] # 总油量
            current_gas += gas[i]-cost[i] # 当前油量
            # 当前油量小于0，则不能从startindex中开始走
            if current_gas < 0:
                current_gas = 0
                startindex = i+1
        # 总油量小于0则全程开不过去
        return -1 if total_gas<0 else startindex
```

