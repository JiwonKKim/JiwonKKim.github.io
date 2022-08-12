---
layout: post
title:  "[LeetCode] Top Interview Questions 7 - Reverse Integer"
category: Algorithm
tags: [Algorithm, LeetCode, Reverse Integer]
data: 2022-08-02 21:15:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 7
#### [Reverse Integer](https://leetcode.com/problems/reverse-integer/)


Given a signed 32-bit integer `x`, return `x` *with its digits reversed*. If reversing `x` causes the value to go outside the signed 32-bit integer range `[-2^31, 2^31 - 1]`, then return `0`.

**Assume the environment does not allow you to store 64-bit integers (signed or unsigned).**

### **Example 1:**
```
Input: x = 123
Output: 321
```

### **Example 2:**
```
Input: x = -123
Output: -321
```

### **Example 3:**
```
Input: x = 120
Output: 21
```

**Constraints:**

- `-2^31 <= x <= 2^31 - 1`


**My Solution:**
``` python
class Solution:
    def reverse(self, x: int) -> int:
        string_x = str(x)
        if string_x[0] == "-":
            sign = -1
            string_x = string_x[1:]
        else:
            sign = 1
    
        string_x = string_x[::-1]
        reversed_x = sign * int(string_x)
        
        if reversed_x not in range(-2**31, 2**31):
            reversed_x = 0
        
        return reversed_x                
```

**My Explanation:**

입력된 integer형 x를 string으로 변환하여 reverse한 후, 다시 integer로 변환한다. 32-bit signed integer인지 범위를 확인한다.

[**LeetCode Solution**](https://leetcode.com/problems/reverse-integer/solution/)

##### 출처:
- [Reverse Integer](https://leetcode.com/problems/reverse-integer/)
