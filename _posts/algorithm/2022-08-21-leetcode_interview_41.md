---
layout: post
title:  "[LeetCode] Top Interview Questions 41 - First Missing Positive"
category: Algorithm
tags: [Algorithm, LeetCode, First Missing Positive]
data: 2022-08-21 16:10:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 41
#### [First Missing Positive](https://leetcode.com/problems/first-missing-positive/)

Given an unsorted integer array `nums`, return the smallest missing positive integer.

You must implement an algorithm that runs in `O(n)` time and uses constant extra space.

 <!-- ***문제에 대한 자세한 설명은 출처를 참조***<br> -->

### **Example 1:**
```
Input: nums = [1,2,0]
Output: 3
Explanation: The numbers in the range [1,2] are all in the array.
```

### **Example 2:**
```
Input: nums = [3,4,-1,1]
Output: 2
Explanation: 1 is in the array but 2 is missing.
```

### **Example 3:**
```
Input: nums = [7,8,9,11,12]
Output: 1
Explanation: The smallest positive integer 1 is missing.
```

**Constraints:**

- `1 <= nums.length <= 105`
- `-231 <= nums[i] <= 231 - 1`

**My Solution:**
``` python
class Solution:
    def firstMissingPositive(self, nums: List[int]) -> int:
        nums = list(set(nums))
        nums.sort()
        length = len(nums)
        first_index = None
        for i in range(length):
            if nums[i] > 0:
                first_index = i
                break
        if first_index == None:
            return 1
        
    
        j = first_index
        for i in range(1, length+1):
            if j >= length:
                return nums[j-1]+1
            if i == nums[j]:
                j += 1
                continue
            else:
                return i
        
        return i+1
```

**My Explanation:**

주어진 배열에서 중복을 제거하고 정렬한 뒤, 가장 작은 positive integer가 등장하는 index를 저장한다. Index를 하나씩 증가시켜 존재하지 않는 positive integer를 찾는다.

[**LeetCode Solution**](https://leetcode.com/problems/first-missing-positive/solution/)

##### 출처:
- [First Missing Positive](https://leetcode.com/problems/first-missing-positive/)