---
layout: post
title:  "[LeetCode] Top Interview Questions 17 - Letter Combinations of a Phone Number"
category: Algorithm
tags: [Algorithm, LeetCode, Letter Combinations of a Phone Number]
data: 2022-08-19 12:15:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 17
#### [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)

Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent. Return the answer in **any order**.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

 ***문제에 대한 자세한 설명은 출처를 참조***<br>

### **Example 1:**
```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

### **Example 2:**
```
Input: digits = ""
Output: []
```

### **Example 3:**
```
Input: digits = "2"
Output: ["a","b","c"]
```
**Constraints:**

- `0 <= digits.length <= 4`
- `digits[i]` is a digit in the range `['2', '9']`.

**My Solution:**
``` python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        letters_dict = {2:"abc", 3:"def", 4:"ghi", 5:"jkl", 6:"mno", 7:"pqrs", 8:"tuv", 9:"wxyz"}
        combs_list = []
        for digit in digits:
            if not combs_list:
                combs_list.append("")
            new_combs_list = []
            for item in combs_list:
                for letter in letters_dict[int(digit)]:
                    new_combs_list.append(item+letter)
            combs_list = new_combs_list.copy()
        
        return combs_list
                     
```

**My Explanation:**

Digit이 만들어낼 수 있는 문자를 미리 선언한다. 입력되는 digit마다 만들어질 수 있는 문자열 조합을 리스트에 계속 추가한다.

[**LeetCode Solution**](https://leetcode.com/problems/letter-combinations-of-a-phone-number/solution/)

##### 출처:
- [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/)
