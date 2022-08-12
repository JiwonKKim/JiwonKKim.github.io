---
layout: post
title:  "[LeetCode] Top Interview Questions 5 - Longest Palindromic Substring"
category: Algorithm
tags: [Algorithm, LeetCode, Longest Palindromic Substring]
data: 2022-08-02 20:30:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 5
#### [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)


Given a string `s`, return *the longest palindromic substring* in `s`.

### **Example 1:**
```
Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.
```

### **Example 2:**
```
Input: s = "cbbd"
Output: "bb"
```

**Constraints:**

- `1 <= s.length <= 1000`
- `s` consist of only digits and English letters.


**My Solution:**
``` python
def is_palindrome(string):
    length = len(string)
    middle = length // 2
    string_left = string[:middle]
    string_right = string[middle:]
    if length % 2 == 1:
        string_right = string_right[1:]
    string_right = string_right[::-1]
    equal = string_left == string_right
    return equal

class Solution:
    def longestPalindrome(self, s: str) -> str:
        longest_palindrome = s[0]
        length = len(s)        
        for i in range(length):
            for j in reversed(range(i+1, length)):
                if s[i] == s[j]:
                    if is_palindrome(s[i:j+1]):
                        if len(longest_palindrome) < len(s[i:j+1]):
                            longest_palindrome = s[i:j+1]
                            break
            if len(longest_palindrome) > (length - i):
                break
        return longest_palindrome
                
```

**My Explanation:**

최대한 탐색횟수를 줄이기 위해 가장 긴 경우부터 Palindrome인지 탐색한다. Palindrome의 여부는 문자열 길이가 짝수/홀수인 경우를 생각한다.


[**LeetCode Solution**](https://leetcode.com/problems/longest-palindromic-substring/solution/)

##### 출처:
- [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)