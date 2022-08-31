---
layout: post
title:  "[LeetCode] Top Interview Questions 62 - Unique Paths"
category: Algorithm
tags: [Algorithm, LeetCode, Unique Paths]
data: 2022-08-24 18:20:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 62
#### [Unique Paths](https://leetcode.com/problems/unique-paths/)

There is a robot on an `m x n` grid. The robot is initially located at the **top-left corner** (i.e., `grid[0][0]`). The robot tries to move to the bottom-right corner (i.e., `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers `m` and `n`, return *the number of possible unique paths that the robot can take to reach the bottom-right corner*.

The test cases are generated so that the answer will be less than or equal to `2 * 10^9`.

 ***문제에 대한 자세한 설명은 출처를 참조***<br>

### **Example 1:**
```
Input: m = 3, n = 7
Output: 28
```

### **Example 2:**
```
Input: m = 3, n = 7
Output: 28
```

**Constraints:**

- `1 <= m, n <= 100`

**My Solution:**
``` python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp = [[1] * n for _ in range(m)]
        for h in range(1, m):
            for w in range(1, n):
                dp[h][w] = dp[h-1][w] + dp[h][w-1]
        return dp[m-1][n-1]
```

**My Explanation:**

고등학교 수학에서 배운 최단거리 찾기 문제이다. 우리는 이 문제를 공식으로 어떻게 푸는지 알고 있으나, 해당 방법 대신 dp를 이용하여 문제를 푼다.

<!-- [**LeetCode Solution**](https://leetcode.com/problems/jump-game/solution/) -->

##### 출처:
- [Unique Paths](https://leetcode.com/problems/unique-paths/)