## [3Sum](https://leetcode.com/problems/3sum/)

**Medium**

Given an array `nums` of *n* integers, are there elements *a*, *b*, *c* in `nums` such that *a* + *b* + *c* = 0? Find all unique triplets in the array which gives the sum of zero.

**Note:**

The solution set must not contain duplicate triplets.

**Example:**

```
Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

题意：从所给的数字中找出三个数字和为0的，不能重复。

C++超时版：暴力循环

```c++
#Time Limit Exceeded

class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>>vec2;
        sort(nums.begin(),nums.end());
        int len=nums.size(); 
        for(int i=0;i<len;i++)
        {
            for(int j=i+1;j<len;j++)
            {
                int k = 0-(nums[i]+nums[j]);
                vector<int>::iterator it = find(nums.begin()+j+1, nums.end(), k);
                if(it!=nums.end())
                {
                    vector<int>vec1;
                    vec1.push_back(nums[i]);
                    vec1.push_back(nums[j]);
                    vec1.push_back(k);
                    vector<vector<int>>::iterator itt = find(vec2.begin(),vec2.end(),vec1);
                    if(itt==vec2.end())
                        vec2.push_back(vec1);
                }
            }
        }
        return vec2; 
    }
};


```

超时之后修改了一下提交之后还是超时，无奈上网搜了一下题解,算法如下：

**先升序排序，然后用第一重for循环确定第一个数字。**

**然后在第二重循环里，第二、第三个数字分别从两端往中间扫。**

**如果三个数的sum等于0，得到一组解。**

**如果三个数的sum小于0，说明需要增大，所以第二个数往右移。**

**如果三个数的sum大于0，说明需要减小，所以第三个数往左移。**

**时间复杂度：O(n2)**

参考：https://www.cnblogs.com/ganganloveu/p/3832180.html

然后根据上述算法写了一下，提交后还是超时，代码如下：

```c++

class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>>vec2;
        sort(nums.begin(),nums.end());
        int len=nums.size();
        for(int i=0;i<len;i++)
        {

            int l,r;
            l=i+1;
            r=len-1;
            while(l<r)
            {
                int sum=nums[i]+nums[l]+nums[r];
                if(sum==0)
                {
                    vector<int>vec1{nums[i],nums[l],nums[r]};
                    vector<vector<int>>::iterator it = find(vec2.begin(),vec2.end(),vec1);
                    if(it==vec2.end())
                    vec2.push_back(vec1);
                    l++;
                    r--;
                }
                else if(sum<0)
                {
                    l++;
                }
                else if(sum>0)
                {
                    r--;
                }
                
            }
        }
        return vec2; 
    }
};


```

**看来是不能用stl的find函数查找是否有重复值**

删掉find函数，那就是去重的问题。因为事先排了序，保证刚验证过的数不在重复验证。

```c++
#Runtime: 116 ms, faster than 48.05% of C++ online submissions for 3Sum.
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>>vec2;
        sort(nums.begin(),nums.end());
        int len=nums.size();
        for(int i=0;i<len;i++)
        {
            while(i<len&&i>0&&nums[i]==nums[i-1])//skip the same key
                i++;
            int l,r;
            l=i+1;
            r=len-1;
            while(l<r)
            {
                int sum=nums[i]+nums[l]+nums[r];
                if(sum==0)
                {
                    vector<int>vec1{nums[i],nums[l],nums[r]};
                    vec2.push_back(vec1);
                    l++;
                    while(l<r&&nums[l]==nums[l-1])//skip the same key
                        l++;
                    r--;
                    while(l<r&&nums[r]==nums[r+1])//skip the same key
                        r--;
                }
                else if(sum<0)
                {
                    l++;
                    while(l<r&&nums[l]==nums[l-1])//skip the same key
                        l++;

                }
                else if(sum>0)
                {
                    r--;
                    while(l<r&&nums[r]==nums[r+1])//skip the same key
                        r--;
                }
                
            }
        }
        return vec2; 
    }
};


```

Python3版：

算法思想同上。

下面这个代码运行的时候，一会儿**Time Limit Exceeded**,一会**Accepted**，

```python
# 运行时间对比
# Runtime: 1136 ms, faster than 57.88% of Python3 online submissions for 3Sum.
# Runtime: 1628 ms, faster than 14.60% of Python3 online submissions for 3Sum.
# Runtime: 1492 ms, faster than 24.64% of Python3 online submissions for 3Sum.
# 处在超时的边缘
class Solution:
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        nums.sort()
        res=[]
        leng = len(nums)
        # 不加超时
        if leng<3:
            return res
        for i in range(leng):
            # 不加超时
            if nums[i]>0:
                return res
            if i>0 and i<leng and nums[i]==nums[i-1]:
                continue
            l = i + 1
            r = leng -1
            while l<r:
                sum1 = nums[i] + nums[l] + nums[r]
                if(sum1==0):
                    res.append((nums[i],nums[l],nums[r]))
                    l+=1
                    r-=1
                    while l<r and  nums[l]==nums[l-1] :
                        l+=1
                    while l<r and nums[r] == nums[r+1]:
                        r-=1
                elif(sum1<0):
                    l+=1
                    while l<r and nums[l]==nums[l-1]:
                        l+=1
                elif(sum1>0):
                    r-=1
                    while l<r and nums[r]==nums[r+1]:
                        r-=1
        return res
```

