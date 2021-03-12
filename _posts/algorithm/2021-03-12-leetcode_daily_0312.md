---
layout: post
title:  "[LeetCode] Check If a String Contains All Binary Codes of Size K"
category: Algorithm
tags: [Algorithm, LeetCode]
comments: true  
---

#### [LeetCode] Daily 문제
##### [Check If a String Contains All Binary Codes of Size K](https://leetcode.com/problems/check-if-a-string-contains-all-binary-codes-of-size-k/)

Given a binary string s and an integer k.

Return True if every binary code of length k is a substring of s. Otherwise, return False.

 

Example 1:

Input: s = "00110110", k = 2
Output: true
Explanation: The binary codes of length 2 are "00", "01", "10" and "11". They can be all found as substrings at indicies 0, 1, 3 and 2 respectively.
Example 2:

Input: s = "00110", k = 2
Output: true
Example 3:

Input: s = "0110", k = 1
Output: true
Explanation: The binary codes of length 1 are "0" and "1", it is clear that both exist as a substring. 
Example 4:

Input: s = "0110", k = 2
Output: false
Explanation: The binary code "00" is of length 2 and doesn't exist in the array.
Example 5:

Input: s = "0000000001011100", k = 4
Output: false
 

Constraints:

- 1 <= s.length <= 5 * 10^5
- s consists of 0's and 1's only.
- 1 <= k <= 20


``` cpp
#define MAX_LENGTH 20

class Solution {
private:
    bool is_found;
public:
    Solution () {
        is_found = true;
    }
    bool hasAllCodes(string s, int k) {
        int length = s.size();
        int array_size = pow(2, k);
        bool *is_exist = new bool[array_size](); // ** if is_exist[i] == true, then i is in the subset of k-numbered binary number.
        string subset_s;
        bitset<MAX_LENGTH> binary;
        
        for (int i = 0; i <= length - k; i++) {
            subset_s = s.substr(i, k);
            binary = bitset<MAX_LENGTH>(subset_s);
            is_exist[binary.to_ulong()] = true;
        }
        
        for (int i = 0; i < array_size; i++ ) {
            if (!is_exist[i]) {
                is_found = false;
            }
        }
        
        return is_found;
    }
    
};
```

Binary 숫자로 구성된 String을 Integer로 만들어주는 STL이 있을까 찾아보았는데, 역시 있었다. bitset이라는 좋은 자료형을 배워간다. bitset은 boolean으로 구성된 배열같은 자료형이라고 보면 될거같다. 이 자료형에 Binary 숫자로 구성된 String을 입력으로 넣으니 이를 자동으로 진짜 Binary로 바꾸어준다. (ex. bitset<6>("011010") -> 011010)

##### 출처:
- [Check If a String Contains All Binary Codes of Size K](https://leetcode.com/problems/check-if-a-string-contains-all-binary-codes-of-size-k/)
