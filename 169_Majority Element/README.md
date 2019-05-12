[169. Majority Element](https://leetcode.com/problems/majority-element/)

Easy

Given an array of size *n*, find the majority element. The majority element is the element that appears **more than** `⌊ n/2 ⌋` times.

You may assume that the array is non-empty and the majority element always exist in the array.

**Example 1:**

```
Input: [3,2,3]
Output: 3
```

**Example 2:**

```
Input: [2,2,1,1,1,2,2]
Output: 2
```

找出序列中超过一半的元素。

**思路：**

每找出两个不同的element，则成对删除。最终剩下的一定就是所求的。即每两个数不一样就抵消掉，一样就记为出现了两次，这样抵消之后的数一定能是出现次数超过n/2次的元素。[参考：https://blog.csdn.net/u012243115/article/details/42076523]

C++

```c++
/*
Runtime: 20 ms, faster than 97.78% of C++ online submissions for Majority Element.
Memory Usage: 11.1 MB, less than 94.91% of C++ online submissions for Majority Element.
*/
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = nums.size();
        int count = 0;
        int num;
        for ( int i=0 ; i<n;i++)
        {
            if (count == 0)
            {
                num = nums[i];
                count++ ;
            }
            else
            {
                if(num == nums[i])
                    count++;
                else
                    count--;
            }
        }
        return num;
    }
};
```

[229. Majority Element II](https://leetcode.com/problems/majority-element-ii/)

Medium

Given an integer array of size *n*, find all elements that appear more than `⌊ n/3 ⌋`times.

**Note:** The algorithm should run in linear time and in O(1) space.

**Example 1:**

```
Input: [3,2,3]
Output: [3]
```

**Example 2:**

```
Input: [1,1,1,3,3,2,2,2]
Output: [1,2]
```

找出序列中元素多余三分之一的元素，最多有两个。

与上题类似，两个标记为，同时删除三个元素，剩下的两个判断出现的次数。

C++：

```c++
/*
Runtime: 16 ms, faster than 86.70% of C++ online submissions for Majority Element II.
Memory Usage: 10.8 MB, less than 13.39% of C++ online submissions for Majority Element II.
*/
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        int n = nums.size();
        int num1,num2,count1=0,count2=0;
        for(int i=0;i<n;i++)
        {
            if((count1==0||num1==nums[i]))
            {
                num1=nums[i];
                count1++;
            }
            else
            {
                if(count2 == 0||num2==nums[i])
                {
                    num2=nums[i];
                    count2++;
                }
                else
                    count1--,count2--;
                if(count1==0&&count2!=0)//交换1,2，排除num1=num2的情况
                {
                    count1=count2;
                    num1=num2;
                    count2=0;
                }
            }

        }
        count1=0,count2=0;
        for(int i=0 ;i<n;i++)
        {
            if(nums[i]==num1)
                count1++;
            else if (nums[i]==num2)
                count2++;
        }
        vector<int>s;
        if(count1>n/3) s.push_back(num1);
        if(count2>n/3) s.push_back(num2);
        return s;
    }
};
```

Python3：

```python
'''
Runtime: 48 ms, faster than 75.41% of Python3 online submissions for Majority Element II.
Memory Usage: 14 MB, less than 7.61% of Python3 online submissions for Majority Element II.
'''
class Solution:
    def majorityElement(self, nums: List[int]) -> List[int]:
        count1,count2,num1,num2 = 0,0,0,1
        for i in nums:
            if i == num1:
                count1+=1
            elif i == num2:
                count2+=1
            elif count1==0:
                num1=i
                count1+=1
            elif count2==0:
                num2=i
                count2+=1
            else:
                count1-=1
                count2-=1
        ans = [n for n in (num1,num2) if nums.count(n)>len(nums)//3]
        return ans
```

