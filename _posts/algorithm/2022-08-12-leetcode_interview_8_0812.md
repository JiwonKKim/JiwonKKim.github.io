---
layout: post
title:  "[LeetCode] Top Interview Questions 8 - String to Integer (atoi)"
category: Algorithm
tags: [Algorithm, LeetCode, String to Integer (atoi)]
data: 2022-08-12 23:00:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 8
#### [String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)


Implement the `myAtoi(string s)` function, which converts a string to a 32-bit signed integer (similar to C/C++'s `atoi` function).

The algorithm for `myAtoi(string s)` is as follows:

1. Read in and ignore any leading whitespace.
2. Check if the next character (if not already at the end of the string) is `'-'` or `'+'`. Read this character in if it is either. This determines if the final result is negative or positive respectively. Assume the result is positive if neither is present.
3. Read in next the characters until the next non-digit character or the end of the input is reached. The rest of the string is ignored.
4. Convert these digits into an integer (i.e. `"123" -> 123`, `"0032" -> 32`). If no digits were read, then the integer is `0`. Change the sign as necessary (from step 2).
5. If the integer is out of the 32-bit signed integer range `[-2^31, 2^31 - 1]`, then clamp the integer so that it remains in the range. Specifically, integers less than `-2^31` should be clamped to `-2^31`, and integers greater than `2^31 - 1` should be clamped to `2^31 - 1`.
Return the integer as the final result.

**Note:**
- Only the space character `' '` is considered a whitespace character.
- **Do not ignore** any characters other than the leading whitespace or the rest of the string after the digits.

### **Example 1:**
```
Input: s = "42"
Output: 42
Explanation: The underlined characters are what is read in, the caret is the current reader position.
Step 1: "42" (no characters read because there is no leading whitespace)
         ^
Step 2: "42" (no characters read because there is neither a '-' nor '+')
         ^
Step 3: "42" ("42" is read in)
           ^
The parsed integer is 42.
Since 42 is in the range [-231, 231 - 1], the final result is 42.
```

### **Example 2:**
```
Input: s = "   -42"
Output: -42
Explanation:
Step 1: "   -42" (leading whitespace is read and ignored)
            ^
Step 2: "   -42" ('-' is read, so the result should be negative)
             ^
Step 3: "   -42" ("42" is read in)
               ^
The parsed integer is -42.
Since -42 is in the range [-231, 231 - 1], the final result is -42.
```

### **Example 3:**
```
Input: s = "4193 with words"
Output: 4193
Explanation:
Step 1: "4193 with words" (no characters read because there is no leading whitespace)
         ^
Step 2: "4193 with words" (no characters read because there is neither a '-' nor '+')
         ^
Step 3: "4193 with words" ("4193" is read in; reading stops because the next character is a non-digit)
             ^
The parsed integer is 4193.
Since 4193 is in the range [-231, 231 - 1], the final result is 4193.
```

**Constraints:**

- `0 <= s.length <= 200`
- `s` consists of English letters (lower-case and upper-case), digits (`0-9`), `' '`, `'+'`, `'-'`, and `'.'`.

**My Solution:**
``` python
class Solution:
    def myAtoi(self, s: str) -> int:
        s = s.strip()
        sign = ["+", "-"]
        start, end = -1, -1
        for index, char in enumerate(s):
            if start >= 0:
                if not char.isdigit():
                    end = index
                    break
            else:
                if char in sign or char.isdigit():
                    start = index
                else:
                    return 0
        if end >= 0:
            s = s[start:end]
        else:
            s = s[start:]
        
        try:
            if int(s) > (2**31 - 1):
                return 2**31-1
            elif int(s) < -2**31:
                return -2**31

            return int(s)
        except:
            return 0          
```

**My Explanation:**

거지발싸개같은 문제였다. 문제에 제대로 나와있지 않는 예외처리같은 것들 처리하느라 한 세월이었다.

먼저 string 내에 sign과 digit이 있는지 확인하고, 그 이후로 쭉 digit과 관련된 무자열이 나오는지 확인한다. 찾아낸 문자열을 integer형으로 변환할 있으면 잘 찾은 것이므로 integer형으로 반환하고, 변환할 수 없다면 문제에서 요구하지 않는 문자열이므로 0을 반환한다.

[**LeetCode Solution**](https://leetcode.com/problems/string-to-integer-atoi/solution/)

##### 출처:
- [String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)
