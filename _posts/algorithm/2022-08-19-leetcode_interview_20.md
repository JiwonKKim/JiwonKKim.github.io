---
layout: post
title:  "[LeetCode] Top Interview Questions 20 - Valid Parentheses"
category: Algorithm
tags: [Algorithm, LeetCode, Valid Parentheses]
data: 2022-08-19 13:45:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 20
#### [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

Given a string s containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
 

### **Example 1:**
```
Input: s = "()"
Output: true
```

### **Example 2:**
```
Input: s = "()[]{}"
Output: true
```

### **Example 3:**
```
Input: s = "(]"
Output: false
```
**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of parentheses only `'()[]{}'`.


**My Solution:**
``` python
class Solution:
    def isValid(self, s: str) -> bool:
        bracket_dict = {")":"(", "}": "{", "]": "["}
        bracket_list = []
        for char in s:
            if char in bracket_dict:
                if not bracket_list:
                    return False
                poped = bracket_list.pop()
                if poped != bracket_dict[char]:
                    return False
            else:
                bracket_list.append(char)
        if bracket_list:
            return False
        
        return True             
```

**My Explanation:**

Stack을 활용하여, Bracket이 적절하게 배치되었는지 확인한다. 문자열을 탐색하면서, 포인터가 가리키고 있는 괄호가 여는 괄호이면 Stack, 즉 리스트에 Push하고, 닫는 괄호이면 Pop한다. 이때, Pop한 문자와 현재 포인터가 가리키고 있는 닫는 괄호가 같은 종류라면 정상적이고, 그렇지 않다면 오류이다.
<!-- [**LeetCode Solution**](https://leetcode.com/problems/letter-combinations-of-a-phone-number/solution/) -->

##### 출처:
- [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
