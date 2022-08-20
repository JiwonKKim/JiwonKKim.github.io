---
layout: post
title:  "[LeetCode] Top Interview Questions 15 - 3Sum"
category: Algorithm
tags: [Algorithm, LeetCode, 3Sum]
data: 2022-08-18 20:45:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 15
#### [3Sum](https://leetcode.com/problems/3sum/)

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

### **Example 1:**
```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation: 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.
```

### **Example 2:**
```
Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.
```

### **Example 3:**
```
Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums up to 0.
```
**Constraints:**

- `3 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`

**My Solution:**
``` python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        length = len(nums)
        answer = []
        for i in range(length-2):
            
            left, right = i+1, length - 1
            val = nums[i]
            
            if i > 0 and val == nums[i-1]:
                continue

            while left < right:
                sums = val + nums[left] + nums[right]
                if sums > 0:
                    right -= 1
                elif sums < 0:
                    left += 1
                else:
                    answer.append([val, nums[left], nums[right]])
                    while left < right and nums[left] == nums[left+1]:
                        left += 1
                    while left < right and nums[right] == nums[right-1]:
                        right -= 1 
                    left, right = left+1, right-1                    
        
        return answer            
```

**My Explanation:**

for문을 통해 리스트에서 값 하나를 선정하여, 두 개의 포인터를 통해 참조된 값의 합이 선정된 값과 같은지 확인한다. 이때, 중복되는 값을 잘 걸러내는 것이 이번 문제의 핵심으로 보인다.

[**LeetCode Solution**](https://leetcode.com/problems/3sum/solution/)

##### 출처:
- [3Sum](https://leetcode.com/problems/3sum/)
