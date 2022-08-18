---
layout: post
title:  "[LeetCode] Top Interview Questions 11 - Container With Most Water"
category: Algorithm
tags: [Algorithm, LeetCode, Container With Most Water]
data: 2022-08-18 17:15:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 11
#### [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)


You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the `ith` line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

**Notice** that you may not slant the container.

 ***문제에 대한 자세한 설명은 출처를 참조***<br>

### **Example 1:**
```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```

### **Example 2:**
```
Input: height = [1,1]
Output: 1
```

**Constraints:**

- n == height.length
- 2 <= n <= 105
- 0 <= height[i] <= 104

**My Solution:**
``` python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        start = 0
        end = len(height) - 1
        max_area = 0
        
        while start < end:
            w = end - start
            if height[start] < height[end]:
                area = w * height[start]
                start += 1
            else:
                area = w * height[end]
                end -= 1
            
            if area > max_area:
                max_area = area
    
        return max_area
```

**My Explanation:**

넓이를 구할 때 사용되는 포인터를 Start, End로 나누고, 그 둘 사이의 거리를 좁혀나가며 넓이를 구한다. 높이는 해당되는 포인터에서의 값 중 더 작은 값을 사용한다. 두 포인터에서의 값이 더 작은 포인터를 이동시킨다. O(n^2)로 문제를 풀면 Time Limit에 걸리게 된다.

##### 출처:
- [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)