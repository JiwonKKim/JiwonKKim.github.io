---
layout: post
title:  "[LeetCode] Top Interview Questions 14 - Longest Common Prefix"
category: Algorithm
tags: [Algorithm, LeetCode, Longest Common Prefix]
data: 2022-08-18 20:00:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 14
#### [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

### **Example 1:**
```
Input: strs = ["flower","flow","flight"]
Output: "fl"
```

### **Example 2:**
```
Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

**Constraints:**

- `1 <= strs.length <= 200`
- `0 <= strs[i].length <= 200`
- `strs[i]` consists of only lowercase English letters.

**My Solution:**
``` python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        min_len = len(strs[0])
        for str in strs:
            if len(str) < min_len:
                min_len = len(str)
        
        prefix = ""
        for index in range(min_len):
            char = strs[0][index]
            for str in strs[1:]:
                if str[index] != char:
                    char = ""
                    break
            
            if char == "":
                break
            prefix += char
        
        return prefix
```

**My Explanation:**

Index 포인터를 사용하여 문자열이 가리키고 있는 리스트 내의 문자가 같은지 확인한다.

[**LeetCode Solution**](https://leetcode.com/problems/longest-common-prefix/solution/)

##### 출처:
- [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)