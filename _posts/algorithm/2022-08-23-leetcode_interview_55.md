---
layout: post
title:  "[LeetCode] Top Interview Questions 55 - Jump Game"
category: Algorithm
tags: [Algorithm, LeetCode, Jump Game]
data: 2022-08-23 22:15:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 55
#### [Jump Game](https://leetcode.com/problems/jump-game/)

You are given an integer array `nums`. You are initially positioned at the array's **first index**, and each element in the array represents your maximum jump length at that position.

Return `true` *if you can reach the last index, or* `false` *otherwise.*



An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

 <!-- ***문제에 대한 자세한 설명은 출처를 참조***<br> -->

### **Example 1:**
```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

### **Example 2:**
```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```

**Constraints:**

- `1 <= nums.length <= 10^4`
- `0 <= nums[i] <= 10^5`

**My Solution:**
``` python
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        curr_pos = 0
        length = len(nums)
        max_distance = nums[0]
        
        if max_distance + 1 >= length:
            return True
        
        index = 0
        while index < max_distance:
            index += 1
            max_distance = max(max_distance, index+nums[index])
            if max_distance >= length - 1:
                return True
        
        return False
```

**My Explanation:**

Index를 하나씩 증가시키며, index가 최대한으로 점프할 수 있는 거리를 계속 확인해간다. Index가 더이상 점프할 수 없는 상황이 되면 False를 반환하고, 배열의 마지막 요소에 index가 점프할 수 있으면 True를 반환한다.

[**LeetCode Solution**](https://leetcode.com/problems/jump-game/solution/)

##### 출처:
- [Jump Game](https://leetcode.com/problems/jump-game/)