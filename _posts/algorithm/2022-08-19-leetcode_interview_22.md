---
layout: post
title:  "[LeetCode] Top Interview Questions 22 - Generate Parentheses"
category: Algorithm
tags: [Algorithm, LeetCode, Generate Parentheses]
data: 2022-08-19 15:25:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 22
#### [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)

Given `n` pairs of parentheses, write a function to *generate all combinations of well-formed parentheses*.

 <!-- ***문제에 대한 자세한 설명은 출처를 참조***<br> -->

### **Example 1:**
```
Input: n = 3
Output: ["((()))","(()())","(())()","()(())","()()()"]
```

### **Example 2:**
```
Input: n = 1
Output: ["()"]
```

**Constraints:**

- `1 <= n <= 8`


**My Solution:**
``` python
class Solution:
    def generateParenthesis(self, n: int) -> List[str]:
        results = []
        def backtracking(s, opened, closed):            
            if opened < 0 or closed < 0:
                return

            if opened <= closed:
                dfs(s+"(", opened-1, closed)
                dfs(s+")", opened, closed-1)
            
            if opened + closed == 0:
                results.append(s)
        
        backtracking("", n, n)
        
        return results
```

**My Explanation:**

문자열을 조합하는 방법은 기존 문자열에 열린 괄호(opened)를 추가하거나, 닫힌 괄호(closed)를 추가하는 두 가지 뿐이다. 하지만 열린 괄호가 사용되기 전에 닫힌 괄호를 사용할 수는 없다. 이러한 조건을 생각하여 Backtracking을 통해 문제를 푼다.

[**LeetCode Solution**](https://leetcode.com/problems/generate-parentheses/solution/)

##### 출처:
- [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/)
