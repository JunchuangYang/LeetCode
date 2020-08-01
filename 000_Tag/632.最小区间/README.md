# [最小区间](https://leetcode-cn.com/problems/smallest-range-covering-elements-from-k-lists/)

你有 k 个升序排列的整数数组。找到一个最小区间，使得 k 个列表中的每个列表至少有一个数包含在其中。

我们定义如果 b-a < d-c 或者在 b-a == d-c 时 a < c，则区间 [a,b] 比 [c,d] 小。

示例 1:

输入:[[4,10,15,24,26], [0,9,12,20], [5,18,22,30]]
输出: [20,24]
解释: 
列表 1：[4, 10, 15, 24, 26]，24 在区间 [20,24] 中。
列表 2：[0, 9, 12, 20]，20 在区间 [20,24] 中。
列表 3：[5, 18, 22, 30]，22 在区间 [20,24] 中。
注意:

给定的列表可能包含重复元素，所以在这里升序表示 >= 。
1 <= k <= 3500
-10^5 <= 元素的值 <= 10^5
对于使用Java的用户，请注意传入类型已修改为List<List<Integer>>。重置代码模板后可以看到这项改动。

[官解：堆](<https://leetcode-cn.com/problems/smallest-range-covering-elements-from-k-lists/solution/zui-xiao-qu-jian-by-leetcode-solution/>)

**给定 k 个列表，需要找到最小区间，使得每个列表都至少有一个数在该区间中。该问题可以转化为，从 k 个列表中各取一个数，使得这 k 个数中的最大值与最小值的差最小。**

由于 k 个列表都是升序排列的，因此对每个列表维护一个指针，通过指针得到列表中的元素，指针右移之后指向的元素一定大于或等于之前的元素。

使用最小堆维护 k 个指针指向的元素中的最小值，同时维护堆中元素的最大值。初始时，k 个指针都指向下标 0，最大元素即为所有列表的下标 0 位置的元素中的最大值。每次从堆中取出最小值，根据最大值和最小值计算当前区间，如果当前区间小于最小区间则用当前区间更新最小区间，然后将对应列表的指针右移，将新元素加入堆中，并更新堆中元素的最大值。

如果一个列表的指针超出该列表的下标范围，则说明该列表中的所有元素都被遍历过，堆中不会再有该列表中的元素，因此退出循环。

**Python3:**

```python
class Solution:
    def smallestRange(self, nums: List[List[int]]) -> List[int]:
        index = [0]*len(nums)
        heap = []
        maxx = float('-inf')
        for i in range(len(nums)):
            maxx = max(nums[i][0],maxx)
            heapq.heappush(heap,[nums[i][0],i,0])
        start,right = float('-inf'),float('-inf')
        while True:
            item = heapq.heappop(heap)
            if start==float('-inf'):
                start,end = item[0],maxx
            else:
                if  maxx-item[0]<end-start:
                    start,end = item[0],maxx
            if item[2]+1<len(nums[item[1]]):
                maxx = max(nums[item[1]][item[2]+1],maxx)
                heapq.heappush(heap,[nums[item[1]][item[2]+1],item[1],item[2]+1])
            else:
                break
        return [start,end]
```

**C++:**

```c++
class Solution {
public:
    vector<int> smallestRange(vector<vector<int>>& nums) {
        priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>> pq;
        int maxx=INT_MIN;
        for(int i=0;i<nums.size();i++)
        {
            maxx = max(maxx,nums[i][0]);
            pq.push(make_pair(nums[i][0],i));
        }
        vector<int> index(nums.size());
        vector<int> ans;
        ans.push_back(INT_MIN);
        ans.push_back(INT_MIN);
        while (1)
        {
            pair<int,int> temp = pq.top();
            pq.pop();
            if(ans[0]==INT_MIN)
            {
                ans[0]=temp.first;
                ans[1]=maxx;
            }
            else
            {
                if (maxx-temp.first<ans[1]-ans[0])
                {
                    ans[0]=temp.first,ans[1]=maxx;
                }
            }
            if (index[temp.second]+1<nums[temp.second].size())
            {
                index[temp.second]+=1;
                maxx = max(nums[temp.second][index[temp.second]],maxx);
                pq.push(make_pair(nums[temp.second][index[temp.second]],temp.second));
            }
            else
                break;
        }
        return ans;
    }
};
```

