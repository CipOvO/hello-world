# Array

力扣数组部分的AC代码汇总。

## 目录

点击跳转

* [1 Two Sum](#1)

* [2 Median of Two Sorted Arrays](#2)
* [3 Container With Most Water](#3)
* [4 3Sum](#4)

## 题目

### <span id="1">1 [Two Sum](https://leetcode.com/problems/two-sum/)</span>

**思路：哈希**

快速找到数组中是否存在两数和等于target。思路是用哈希表“反”存数组。这里的“反”的意思是，用数组的值当键，数组Index当值。遍历哈希表，这样便可以在单位时间内找到target的补数。

**Code**

```c++
class Solution {
public:
	vector<int> twoSum(vector<int>& nums, int target) 
	{
		unordered_map <int,int> m; //hash table
		vector <int> ret;
		int size = nums.size();
    
		for(int i=0; i<size; i++)
		{
			if(m.find(target-nums[i])!=m.end())
			{
				ret.push_back(m[target-nums[i]]);
				ret.push_back(i);
				break;
			}
			m[nums[i]] = i;
		}
		return ret;
	}
};
```

Python solution

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        hash_table = dict()
        ans = []
        for i in range(len(nums)):
            if (target - nums[i]) in hash_table:
                ans.append(hash_table[target - nums[i]]);
                ans.append(i)
                break
            hash_table[nums[i]] = i
        return ans
```

---

### <span id="2">2 [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)</span>

**思路：二分**

难题。在两个有序数组中找到中位数。利用中位数的定义将题目变成了一个二分搜索问题。左边部分的长度等于右边部分的长度；左边的数都要小于右边的数。

```
          left_part          |        right_part
    A[0], A[1], ..., A[i-1]  |  A[i], A[i+1], ..., A[m-1]
    B[0], B[1], ..., B[j-1]  |  B[j], B[j+1], ..., B[n-1]
```

**Code**

```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size();
        int n = nums2.size();
        if(n < m){
            swap(nums1, nums2);
            swap(m, n);
        }
        
        int iMin = 0, iMax = m, i, j;
        int len = m + n;
        while(iMin <= iMax){
            i = (iMin + iMax) / 2;
            j = (len + 1) / 2 - i;
            if(i > 0 && j < n && nums1[i-1] > nums2[j])
                iMax = i - 1;
            else if(i < m && j > 0 && nums2[j-1] > nums1[i])
                iMin = i + 1;
            else{  //i is perfect, we can stop searching
                int maxL = 0;
                if( i == 0)
                    maxL = nums2[j-1];
                else if(j == 0)
                    maxL = nums1[i-1];
                else
                    maxL = max(nums1[i-1], nums2[j-1]);
                if(len % 2 == 1)
                    return maxL;
                
                int minR = 0;
                if(i == m)
                    minR = nums2[j];
                else if(j == n)
                    minR = nums1[i];
                else
                    minR = min(nums1[i], nums2[j]);
                return (minR + maxL) / 2.0; 
            }
        }
        return 0.0;
    }
};

```

---

### <span id="3">3 [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)</span>

**思路：双指针**

双指针（double points）也是数组问题中一个十分好用的方法。

**Code**

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        n = len(height)
        l, r = 0, n-1
        water = min(height[r], height[l]) * (r - l)
        while(l < r):
            water = max(water, min(height[r], height[l]) * (r - l))
            if height[l] < height[r]:
                l += 1
            else:
                r -= 1
        return water 
```

---

### <span id="3">3 [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)</span>

**思路：双指针**

双指针（double points）也是数组问题中一个十分好用的方法。

**Code**

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        n = len(height)
        l, r = 0, n-1
        water = min(height[r], height[l]) * (r - l)
        while(l < r):
            water = max(water, min(height[r], height[l]) * (r - l))
            if height[l] < height[r]:
                l += 1
            else:
                r -= 1
        return water 
```

---

### <span id="4">4 [3Sum](https://leetcode.com/problems/3sum/)</span>

**思路：排序+双指针**

排序预处理也是解决数组问题的一种方法。Two Sum的升级版，延续Two Sum的思路，用哈希也可以。这次尝试用双指针的方法去找第二个数和第三个数。这里要注意去除答案集合的重复元素。

**Code**

```python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        n = len(nums)
        res = []
        for first in range(n):
            if first > 0 and nums[first-1] == nums[first]:
                continue
            if nums[first] > 0:
                break
            sum = 0 - nums[first]
            j = first + 1
            k = n - 1
            while j < k:
                tmp = nums[j] + nums[k]
                if tmp > sum:
                    k -= 1
                elif tmp < sum:
                    j += 1
                else:
                    res.append([nums[first], nums[j], nums[k]])
                    l, r = nums[j], nums[k]
                    while j < k and nums[j] <= l:
                        j += 1
                    while j < k and nums[k] >= r:
                        k -= 1
                    
        return res
```

------

