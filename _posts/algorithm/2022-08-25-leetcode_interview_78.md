---
layout: post
title:  "[LeetCode] Top Interview Questions 78 - Subsets"
category: Algorithm
tags: [Algorithm, LeetCode, Subsets]
data: 2022-08-25 16:50:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 78
#### [Subsets](https://leetcode.com/problems/subsets/)

Given an integer array `nums` of **unique** elements, return *all possible subsets (the power set)*.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

 <!-- ***문제에 대한 자세한 설명은 출처를 참조***<br> -->

### **Example 1:**
```
Input: nums = [1,2,3]
Output: [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

### **Example 2:**
```
Input: nums = [0]
Output: [[],[0]]
```

**Constraints:**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- All the numbers of `nums` are unique.

**My Solution:**
``` python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        subsets_list = [[]]
        for num in nums:
            subset_list = subsets_list.copy()
            for subset in subset_list:
                subsets_list.append(subset+[num])
        return subsets_list
```

**My Explanation:**

고등학교 수학에서 배운 부분집합 구하기이다. 공집합이 포함된 subsets_list에서부터 차근차근 element를 추가해가며 부분집합을 구한다.

입력이 [1, 2, 3]인 Example 1을 예로 든다. 최초의 subsets_list는 공집합 []이다. 이 각각의 subsets_list의 요소들에 nums내의 원소를 하나씩 추가한다. 공집합인 subsets_list의 요소들에 원소 1을 추가하면 subsets_list는 [[], []+1], 즉 [[], [1]]이 된다. 다시, 이 각각의 subsets_list의 요소들에 원소 2를 추가하면 [[], [1], []+2, [1]+2], 즉 [[], [1], [2], [1, 2]]가 된다. 이러한 동작을 nums 내의 모든 요소에 대해 적용하면 부분집합을 구할 수 있다.
[**LeetCode Solution**](https://leetcode.com/problems/subsets/solution/)

##### 출처:
- [Subsets](https://leetcode.com/problems/subsets/)