---
layout: post
title:  "[LeetCode] Top Interview Questions 4 - Median of Two Sorted Arrays"
category: Algorithm
tags: [Algorithm, LeetCode, Longest Substring Without Repeating Characters]
data: 2022-08-02 23:00:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 4
#### [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)


Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

### **Example 1:**
```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```

### **Example 2:**
```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```

**Constraints:**

- `nums1.length == m`
- `nums2.length == n`
- `0 <= m <= 1000`
- `0 <= n <= 1000`
- `1 <= m + n <= 2000`
- `-106 <= nums1[i], nums2[i] <= 106`


**My Solution:**
``` python
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:
        nums = nums1 + nums2
        nums = sorted(nums)
        if len(nums) % 2:
            return nums[len(nums)//2]
        else:
            index = len(nums) // 2
            median = (nums[index-1] + nums[index])/2
            return median
```

**My Explanation:**

난이도는 Hard로 나와 있는데... 뭔가 좀 싱겁다. 처리 속도를 보는 문제로 보인다. 인자로 주어진 두 리스트를 합쳐서 sorting하고 median을 찾아낸다.

[**LeetCode Solution**](https://leetcode.com/problems/median-of-two-sorted-arrays/solution/)

##### 출처:
- [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)