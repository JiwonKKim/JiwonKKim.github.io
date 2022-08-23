---
layout: post
title:  "[LeetCode] Top Interview Questions 42 - Trapping Rain Water"
category: Algorithm
tags: [Algorithm, LeetCode, Trapping Rain Water]
data: 2022-08-22 14:00:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 42
#### [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

 ***문제에 대한 자세한 설명은 출처를 참조***<br>

### **Example 1:**
```
Input: height = [0,1,0,2,1,0,1,3,2,1,2,1]
Output: 6
Explanation: The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.
```

### **Example 2:**
```
Input: height = [4,2,0,3,2,5]
Output: 9
```

**Constraints:**

- `n == height.length`
- `1 <= n <= 2 * 10^4`
- `0 <= height[i] <= 10^5`

**My Solution:**
``` python
class Solution:
    def trap(self, height: List[int]) -> int:
        left, right = 0, len(height) - 1
        left_val, right_val = height[left], height[right]
        area = 0
        while left < right:
            left_val = max(left_val, height[left])
            right_val = max(right_val, height[right])
            if left_val < right_val:
                area += left_val - height[left]
                left += 1
            else:
                area += right_val - height[right]
                right -= 1
        
        return area
```

**My Explanation:**

좌측과 우측에서 시작되는 두 개의 포인터를 사용한다. 우측 포인터에서의 값이 더 크면 좌측 포인터를 우측으로, 좌측 포인터에서의 값이 더 크면 우측 포인터를 좌측으로 옮겨나간다. 옮겨나가면서 물이 찰 수 있는 공간이 있는지 확인한다.

[**LeetCode Solution**](https://leetcode.com/problems/trapping-rain-water/solution/)

##### 출처:
- [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)