---
layout: post
title:  "[LeetCode] Integer to Roman"
category: Algorithm
tags: [Algorithm, LeetCode]
comments: true  
---

#### [LeetCode] Daily 문제
##### [Integer to Roman](https://leetcode.com/problems/integer-to-roman/)

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

|Symbol | Value |
|:-----:|:-----:|
|   I   | 1     |
|   V   | 5     |
|   X   | 10    |
|   L   | 50    |
|   C   | 100   |
|   D   | 500   |
|   M   | 1000  |

For example, 2 is written as II in Roman numeral, just two one's added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given an integer, convert it to a roman numeral.


Example 1:

Input: num = 3
Output: "III"
Example 2:

Input: num = 4
Output: "IV"
Example 3:

Input: num = 9
Output: "IX"
Example 4:

Input: num = 58
Output: "LVIII"
Explanation: L = 50, V = 5, III = 3.
Example 5:

Input: num = 1994
Output: "MCMXCIV"
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
 

Constraints:

1 <= num <= 3999

``` cpp
enum Roman {
    M  = 1000   ,
    CM = 900    ,
    D  = 500    ,
    CD = 400    ,
    C  = 100    ,
    XC = 90     ,
    L  = 50     ,
    XL = 40     ,
    X  = 10     ,
    IX = 9      ,
    V  = 5      ,
    IV = 4      ,
    I  = 1
};

#define ROMAN_NUM 13

class Solution {
public:
    string intToRoman(int num) {
        string roman = "";
        int roman_i[ROMAN_NUM] = {M, CM, D, CD, C, XC, L, XL, X, IX, V, IV, I};
        string roman_s[ROMAN_NUM] = {"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"};
        while (num > 0) {
            for (int i = 0; i < ROMAN_NUM; i++) {
                if (num >= roman_i[i]) {
                    num -= roman_i[i];
                    roman += roman_s[i];
                    break;
                }
            }
        }
        return roman;
    }
};
```

더 쉽게 짤 수 있는 방법이 있을거 같았는데 시간이 없어서 이렇게 짰다.. if문 다 때려박으려다가 보기 흉할거같아서 for 문을 사용했다

##### 출처:
- [Integer to Roman](https://leetcode.com/problems/integer-to-roman/)
