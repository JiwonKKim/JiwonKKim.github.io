---
layout: post
title:  "[LeetCode] Top Interview Questions 38 - Count and Say"
category: Algorithm
tags: [Algorithm, LeetCode, Count and Say]
data: 2022-08-20 18:50:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 38
#### [Count and Say](https://leetcode.com/problems/count-and-say/)

The **count-and-say** sequence is a sequence of digit strings defined by the recursive formula:

- `countAndSay(1) = "1"`
- `countAndSay(n)` is the way you would "say" the digit string from `countAndSay(n-1)`, which is then converted into a different digit string.
To determine how you "say" a digit string, split it into the **minimal** number of substrings such that each substring contains exactly **one** unique digit. Then for each substring, say the number of digits, then say the digit. Finally, concatenate every said digit.

For example, the saying and conversion for digit string `"3322251"`:

`"23321511"`

Given a positive integer `n`, return *the `n^th` term of the **count-and-say** sequence*.

 ***문제에 대한 자세한 설명은 출처를 참조***<br>

### **Example 1:**
```
Input: n = 1
Output: "1"
Explanation: This is the base case.
```

### **Example 2:**
```
Input: n = 4
Output: "1211"
Explanation:
countAndSay(1) = "1"
countAndSay(2) = say "1" = one 1 = "11"
countAndSay(3) = say "11" = two 1's = "21"
countAndSay(4) = say "21" = one 2 + one 1 = "12" + "11" = "1211"
```

**Constraints:**

- `1 <= n <= 30`

**My Solution:**
``` python
class Solution:
    def countAndSay(self, n: int) -> str:
        stacked = "1"
        
        for _ in range(2, n+1):
            index = 0
            s = ""
            while index < len(stacked):
                val = stacked[index]
                nums = 1
                while index + 1 < len(stacked) and stacked[index] == stacked[index+1]:
                    nums += 1
                    index += 1
                s += str(nums) + val
                index += 1
            stacked = s
            
        return stacked
```

**My Explanation:**

피보나치 수열과 같이 이전 값이 현재 값에 영향을 준다. 연속된 같은 숫자가 있는지 몇 개 있는지 파악하여, 그것을 문자열로 표현해서 쌓아나간다.

<!-- [**LeetCode Solution**](https://leetcode.com/problems/valid-sudoku/solution/) -->

##### 출처:
- [Count and Say](https://leetcode.com/problems/count-and-say/)