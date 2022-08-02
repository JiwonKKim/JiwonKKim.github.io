---
layout: post
title:  "[LeetCode] Top Interview Questions 3 - Longest Substring Without Repeating Characters"
category: Algorithm
tags: [Algorithm, LeetCode, Longest Substring Without Repeating Characters]
data: 2022-08-02 22:53:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 3
#### [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)


Given a string `s`, find the length of the **longest substring** without repeating characters.

### **Example 1:**
```
Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

### **Example 2:**
```
Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

### **Example 3:**
```
Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

**Constraints:**

- `0 <= s.length <= 5 * 104`
- `s` consists of English letters, digits, symbols and spaces.

**My Solution:**
``` python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        longest_list = []
        tmp_list = []
        for item in s:
            # print(item)
            if item not in tmp_list:
                tmp_list.append(item)
            else:
                if len(longest_list) < len(tmp_list):
                    longest_list = tmp_list
                index = tmp_list.index(item)
                tmp_list = tmp_list[index+1:]
                tmp_list.append(item)
        if len(longest_list) < len(tmp_list):
            longest_list = tmp_list
            
        return len(longest_list)
```

**My Explanation:**

먼저, 문자열을 순차적으로 탐색한다. 현재 탐색중인 문자와 `tmp_list` 내에 저장되어 있는 문자가 겹치는지 확인한다. 겹치지 않는 경우, `tmp_list`에 현재 탐색중인 문자를 추가한다. 겹치는 부분이 있는 경우, `tmp_list`를 재설정한다. 가장 긴 `tmp_list`를 찾는 것이 최종 목표.

##### 출처:
- [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
