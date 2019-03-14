##  [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)

Hard

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

**Example:**

```
Input: [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2.
    Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Note:**

You can assume that you can always reach the last index.

解题参考:[leetcode 45:Jump Game II](https://blog.csdn.net/onlyou2030/article/details/49700397)

基于上篇博客的思想，此题采用贪心的思想。我没看他的代码，看了他的思想自己写了一下。

我的代码大致的思路是：每走一步step，把这一步中所有可能的情况都计算出来，如果大于数组长度，则直接返回结果；如果不满足，则step+1，继续下一循环。

```c++
class Solution {
public:
    int jump(vector<int>& nums) {
        int len = nums.size();
        if(len == 1)
            return 0;
        int maxpos=nums[0],currentpos=nums[0],step=1;
        for(int i=1 ; i<len;i++)
        {
            if(maxpos>=len-1)
                return step;
            for(int j=i;j<=maxpos&&j<len;j++)
            {
               currentpos=max(nums[j]+j,currentpos);
            }
            maxpos = currentpos;//maxpos为在这一步中最大能到达的位置
            step+=1;         
        }
        return len;
    }
};
```

我AC之后去看了上篇博客的代码，只用了一层for循环，代码简洁，理解完之后着实要比我的代码强。

如下：

```C++
class Solution{
public:
	int jump(vector<int> &nums) {
		int size=nums.size();
		if(size<2) return 0;
		int maxReach = nums[0];
		int curMaxReach = nums[0];
		int curStep = 1;
		for(int i = 1; i <= min(size-1, maxReach); i++)
        {
			curMaxReach = max(curMaxReach, i + nums[i]);//当前步中能达到的最远距离
			if(i == size - 1)
            {
				return curStep;
			}
			if(i == maxReach)//如果走到了在这一步中能达到的最远距离，则更新最远距离，step+1
            {
				maxReach = curMaxReach;
				curStep++;
			}
		}
		return 0;
	}
};

```

Python3版：用的是上述博客的思想

```py
class Solution:
    def jump(self, nums: List[int]) -> int:
        length = len(nums)
        if length < 2:
            return 0
        maxpos = nums[0]
        curpos = nums[0]
        step = 1
        i=1
        while i<(min(length,maxpos+1)):
            curpos = max(curpos , nums[i]+i)
            if i == length-1:
                return step
            if i == maxpos:
                maxpos = curpos
                step+=1
            i+=1
        
```

