## [18. 4Sum](https://leetcode.com/problems/4sum/)

**Medium**

Given an array `nums` of *n* integers and an integer `target`, are there elements *a*, *b*, *c*, and *d* in `nums` such that *a* + *b* + *c* + *d* = `target`? Find all unique quadruplets in the array which gives the sum of `target`.

**Note:**

The solution set must not contain duplicate quadruplets.

**Example:**

```
Given array nums = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

解题思想：同[015_3Sum](https://github.com/JunchuangYang/LeetCode/tree/master/015_3Sum)一样，本题要求用四个数加起来等于目标值。固定两个数，其余两个数用双指针法进行遍历，注意去重。

C++版：

```c++
//Runtime: 40 ms, faster than 35.01% of C++ online submissions for 4Sum.
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        sort(nums.begin(),nums.end());
        vector<vector<int>>vec;
        int length = nums.size();
        for(int i=0;i<length-1;i++)
        {
            //去重
            if(i>0)
            {
                while(i<length&&nums[i-1]==nums[i])
                    i++;
            }
            for(int j=i+1;j<length-1;j++)
            {
                //去重
                while(j>i+1&&j<length-1&&nums[j-1]==nums[j])
                    j++;
                int k=j+1,l=length-1;
                while(k<l)
                {
                    int tar = nums[i]+nums[j]+nums[k]+nums[l];
                    if(tar == target)
                    {
                        vector<int>vec1{nums[i],nums[j],nums[k],nums[l]};
                        vec.push_back(vec1);
                        k++;
                        //去重
                        while(k<l&&nums[k-1]==nums[k])
                            k++;
                        l--;
                        while(k<l&&nums[l]==nums[l+1])
                            l--;
                    }
                    else if(tar>target)
                    {
                        l--;
                        while(k<l&&nums[l]==nums[l+1])
                            l--;
                    }
                    else
                    {
                        k++;
                        while(k<l&&nums[k-1]==nums[k])
                            k++;
                    }
                }
            }
        }
        return vec;
    }
};
```



Python版：

使用上面的方法超时

```python
# Time Limit Exceeded
class Solution:
    def fourSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        nums.sort()
        res = []
        length = len(nums)
        if length<4:
            return res
        for i in range(length-3):
            if i>0:
                while (i<length-3) and (nums[i-1]==nums[i]):
                    i+=1
            j=i+1;
            while j<length-2:
                while (j>i+1) and (j<length-2) and (nums[j]==nums[j-1]):
                    j+=1
                k=j+1
                l=length-1
                while k<l:
                    sum1 = nums[i] + nums[j] + nums[k] + nums[l]
                    if sum1==target:
                        res.append((nums[i],nums[j],nums[k],nums[l]))
                        k+=1
                        while (k<l) and (nums[k-1]==nums[k]):
                            k+=1
                        l-=1
                        while (k<l) and (nums[l]==nums[l+1]):
                            l-=1
                    elif sum1>target:
                        l-=1
                        while (k<l) and (nums[l]==nums[l+1]):
                            l-=1
                    elif sum1<target:
                        k+=1
                        while (k<l) and (nums[k-1]==nums[k]):
                            k+=1
                j+=1
            
        return list(set(res))
```



参考：[4Sum](https://blog.csdn.net/alicelmx/article/details/81026573)

思路

可以和之前3Sum的思路一样，采用快排，时间复杂度为O(N^3)，但是很容易超时，所以这里采用另外一种方法，哈希表用空间换时间，以增加空间复杂度的代价来降低时间复杂度。首先建立一个字典dict，字典的key值为数组中每两个元素的和，每个key对应的value为这两个元素的下标组成的元组，元组不一定是唯一的。 
如对于num=[1,2,3,2]来说，dict={3:[(0,1),(0,3)], 4:[(0,2),(1,3)], 5:[(1,2),(2,3)]}。 

这样就可以检查target-key这个值在不在dict的key值中，如果target-key在dict中并且下标符合要求，那么就找到了这样的一组解。由于需要去重，这里选用set()类型的数据结构，即无序无重复元素集。最后将每个找出来的解(set()类型)转换成list类型输出即可。

```python
#Runtime: 248 ms, faster than 64.43% of Python3 online submissions for 4Sum.
class Solution:
    def fourSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        length,res,adict = len(nums),set(),{}
        
        nums.sort()
         # 如果输入数字不够直接返回空列表
        if(length<4):
            return []
        
        # 遍历所有两个数字的和，记录下来索引到字典中
        # 构建哈希表，如对于num=[1,2,3,2]来说，dict={3:[(0,1),(0,3)], 4:[(0,2),(1,3)], 5:[(1,2),(2,3)]}
        for i in range(length):
            for j in range(i+1,length):
                if nums[i]+nums[j] not in adict:
                    adict[nums[i]+nums[j]] = [(i,j)]
                else:
                    adict[nums[i]+nums[j]].append((i,j))
                    
        for i in range(length):
            for j in range(i+1,length-2):
                digit = target - nums[i] - nums[j]
                if digit in adict:
                    for item in adict[digit]:
                        if item[0]>j:
                            res.add((nums[i],nums[j],nums[item[0]],nums[item[1]]))
        
        return [list(k) for k in res]

```

