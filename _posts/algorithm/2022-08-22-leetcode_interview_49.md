---
layout: post
title:  "[LeetCode] Top Interview Questions 49 - Group Anagrams"
category: Algorithm
tags: [Algorithm, LeetCode, Group Anagrams]
data: 2022-08-22 17:15:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 49
#### [Group Anagrams](https://leetcode.com/problems/group-anagrams/)

Given an array of strings `strs`, group **the anagrams** together. You can return the answer in **any order**.

An **Anagram** is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.

 <!-- ***문제에 대한 자세한 설명은 출처를 참조***<br> -->

### **Example 1:**
```
Input: strs = ["eat","tea","tan","ate","nat","bat"]
Output: [["bat"],["nat","tan"],["ate","eat","tea"]]
```

### **Example 2:**
```
Input: strs = [""]
Output: [[""]]
```

### **Example 3:**
```
Input: strs = ["a"]
Output: [["a"]]
```

**Constraints:**

- `1 <= strs.length <= 10^4`
- `0 <= strs[i].length <= 100`
- `strs[i]` consists of lowercase English letters.

**My Solution:**
``` python
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        sorted_list = []
        for item in strs:
            sorted_list.append("".join(sorted(item)))
        
        anagram_dict = dict()
        for i, item in enumerate(sorted_list):
            if item in anagram_dict:
                anagram_dict[item].append(strs[i])
            else:
                anagram_dict[item] = [strs[i]]
                
        return anagram_dict.values()
```

**My Explanation:**

먼저 주어진 리스트 내의 문자열을 정렬한다. 정렬된 문자열 리스트에서 문자열이 같은 index를 찾고, strs[index]를 같은 dict형 자료에 배치하여 반환한다.

<!-- [**LeetCode Solution**](https://leetcode.com/problems/permutations/solution/) -->

##### 출처:
- [Group Anagrams](https://leetcode.com/problems/group-anagrams/)