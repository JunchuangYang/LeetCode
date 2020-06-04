# [4. 寻找两个正序数组的中位数](https://leetcode-cn.com/problems/median-of-two-sorted-arrays/)

给定两个大小为 m 和 n 的正序（从小到大）数组 nums1 和 nums2。

请你找出这两个正序数组的中位数，并且要求算法的时间**复杂度为 O(log(m + n))**。

你可以假设 nums1 和 nums2 不会同时为空。

```
示例 1:

nums1 = [1, 3]
nums2 = [2]

则中位数是 2.0

示例 2:

nums1 = [1, 2]
nums2 = [3, 4]

则中位数是 (2 + 3)/2 = 2.5
```

**1.暴力**

复杂度为 O(m + n)

```python
class Solution(object):
    def findMedianSortedArrays(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: float
        """
        len1 = len(nums1)
        len2 = len(nums2)
        lenn = len1+len2
        flag = 0
        if lenn%2==0:
            flag = 1
        l1 = 0
        l2 = 0
        num = 0
        res = []
        while l1<len1 and l2 < len2 :
            if nums1[l1]<nums2[l2]:
                res.append(nums1[l1])
                l1+=1
            else:
                res.append(nums2[l2])
                l2+=1
        res.extend(nums1[l1:])
        res.extend(nums2[l2:])
        if flag:
            return (res[lenn//2-1]+res[lenn//2])/2.0
        else:
            return res[lenn//2]
```

**2、折半删除，寻找第k小数**

<https://blog.csdn.net/qq_37369124/article/details/84923042>

知道思想，感觉下次遇见不看原来的代码应该也写不出来。

```python
def findMedianSortedArrays(nums1, nums2):
    n=len(nums1)+len(nums2)
    if n%2==1: 
        return find_k(nums1,nums2,n//2+1)
    else:
        return (find_k(nums1,nums2,n//2+1)+find_k(nums1,nums2,n//2))/2
def find_k(nums1,nums2,k):
 	# 使nums1的长度比nums2的小
    if(len(nums1)>len(nums2)):
        return find_k(nums2,nums1,k)
    if(len(nums1)==0):
        return nums2[k-1]
    if(k==1):
        return min(nums1[0],nums2[0])
    n1=min(len(nums1),k//2)
    n2=k-n1
  
    if(nums1[n1-1]<nums2[n2-1]):
        return find_k(nums1[n1:],nums2[:n2],k-n1)
    elif(nums1[n1-1]>nums2[n2-1]):
        return find_k(nums1[:n1],nums2[n2:],k-n2)
    else:
        return nums1[n1-1]
```











