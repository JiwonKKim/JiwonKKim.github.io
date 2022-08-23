---
layout: post
title:  "[LeetCode] Top Interview Questions 46 - Permutations"
category: Algorithm
tags: [Algorithm, LeetCode, Permutations]
data: 2022-08-22 16:15:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 46
#### [Permutations](https://leetcode.com/problems/permutations/)

Given an array `nums` of distinct integers, return *all the possible permutations.* You can return the answer in **any order**.

 <!-- ***문제에 대한 자세한 설명은 출처를 참조***<br> -->

### **Example 1:**
```
Input: nums = [1,2,3]
Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

### **Example 2:**
```
Input: nums = [0,1]
Output: [[0,1],[1,0]]
```

### **Example 3:**
```
Input: nums = [1]
Output: [[1]]
```

**Constraints:**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- All the integers of `nums` are **unique**.

**My Solution:**
``` python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        # ** [1] -> [1, 2], [2, 1] -> [1, 2, 3], [1, 3, 2], [3, 1, 2], [2, 1, 3], [2, 3, 1], [2, 1, 3] ...
        num = nums.pop()
        answer = [[num]]
        while nums:
            tmp = []
            num = nums.pop()
            for item in answer:
                base = item.copy()
                length = len(base)
                for i in range(length):
                    tmp.append(base[:i] + [num] + base[i:])
                tmp.append(base+[num])
            answer = tmp
        
        return answer
```

**My Explanation:**

주어진 리스트에서 적당한 element를 pop한다. pop한 element가 배치될 수 있는 모든 위치에 element를 삽입하여 새로운 리스트를 만든다.

[**LeetCode Solution**](https://leetcode.com/problems/permutations/solution/)

##### 출처:
- [Permutations](https://leetcode.com/problems/permutations/)