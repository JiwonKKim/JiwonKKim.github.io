---
layout: post
title:  "[LeetCode] Top Interview Questions 13 - Roman to Integer"
category: Algorithm
tags: [Algorithm, LeetCode, Roman to Integer]
data: 2022-08-18 18:00:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 13
#### [Roman to Integer](https://leetcode.com/problems/roman-to-integer/)

풀었던 문제였는데.. 까먹었기도 하고 C++로 풀었던거라 Python 버전 포스팅을 따로 한다.

Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

| Symbol | Value |
| ------ | ----- |
| I      | 1     |
| C      | 5     |
| X      | 10    |
| L      | 50    |
| C      | 100   |
| D      | 500   |
| M      | 1000  |

For example, `2` is written as `II` in Roman numeral, just two ones added together. `12` is written as `XII`, which is simply `X + II`. The number `27` is written as `XXVII`, which is `XX + V + II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

- `I` can be placed before `V (5)` and `X (10)` to make `4` and `9`. 
- `X` can be placed before `L (50)` and `C (100)` to make `40` and `90`. 
- `C` can be placed before `D (500)` and `M (1000)` to make `400` and `900`.

Given a roman numeral, convert it to an integer.

### **Example 1:**
```
Input: s = "III"
Output: 3
Explanation: III = 3.
```

### **Example 2:**
```
Input: s = "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```

### **Example 3:**
```
Input: s = "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

**Constraints:**

- 1 <= s.length <= 15
- s contains only the characters ('I', 'V', 'X', 'L', 'C', 'D', 'M').
- It is guaranteed that s is a valid roman numeral in the range [1, 3999].

**My Solution:**
``` python
class Solution:
    def romanToInt(self, s: str) -> int:
        length = len(s)
        one_symbols = dict(I=1, V=5, X=10, L=50, C=100, D=500, M=1000)
        double_symbols = dict(IV=4, IX=9, XL=40, XC=90, CD=400, CM=900)
        val = 0
        index = 0
        while length > index:
            if length > index+1 and s[index:index+2] in double_symbols:
                val += double_symbols[s[index:index+2]]
                index += 2
            else:
                val += one_symbols[s[index]]
                index += 1
        
        return val
```

**My Explanation:**

문자열 내의 문자를 두 개씩 확인하며 Subtract가 필요한 문자, 즉 두 개의 문자를 사용하여 표현되는 숫자인지 확인한다. 문자열이 표현한 값을 더해주며 index 포인터를 옮겨나간다.

[**LeetCode Solution**](https://leetcode.com/problems/roman-to-integer/solution/)

##### 출처:
- [Roman to Integer](https://leetcode.com/problems/roman-to-integer/)