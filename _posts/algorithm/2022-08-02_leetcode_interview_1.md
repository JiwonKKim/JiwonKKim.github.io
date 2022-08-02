---
layout: post
title:  "[LeetCode] Top Interview Questions 1 - Two Sum"
category: Algorithm
tags: [Algorithm, LeetCode, Two Sum]
comments: true  
---

### [LeetCode] Top interview Questions 1
#### [Two Sum](https://leetcode.com/problems/two-sum/)

오랜만의 포스팅. 이번에 졸업을 하게 되어 일자리를 찾고 있으므로, 다시 코딩 테스트 준비를 해보고자 한다. 다만 이전과 달리 C++ 대신 Python을 사용하며, Daily LeetCode 대신 Top Interview Questions를 풀어보고자 한다.

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to target.

You may assume that each input would have **_exactly one solution_**, and you may not use the same element twice.

You can return the answer in any order.
 

### **Example 1:**
```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```
### **Example 2:**
```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```

``` python
import numpy as np

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        nums_list = nums
        sorted_args = np.argsort(nums_list)
        length = len(sorted_args)
        answer = None
        for start in range(length):
            end = -1
            minimum = nums_list[sorted_args[start]]
            for end in reversed(range(length)):
                maximum = nums_list[sorted_args[end]]
                if minimum + maximum < target:
                    break
                elif minimum + maximum == target:
                    answer = [sorted_args[start], sorted_args[end]]
                    break
            if answer:
                break
        return answer
```

Constraints:

- 2 <= nums.length <= 104
- -109 <= nums[i] <= 109
- -109 <= target <= 109
- Only one valid answer exists.
 

Follow-up: Can you come up with an algorithm that is less than O(n2) time complexity?

Input을 미리 sorting 해놓음으로써 O(n2) time complexity를 피해보았다. Example 3와 같이 입력 데이터에 같은 숫자가 포함된 경우도 있어서, sorting하고 풀어도 괜찮을까 싶었지만 다행히 잘 되었다.

##### 출처:
- [Two Sum](https://leetcode.com/problems/two-sum/)
