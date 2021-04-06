---
layout: post
title:  "[LeetCode] Minimum Operations to Make Array Equal"
category: Algorithm
tags: [Algorithm, LeetCode]
comments: true  
---

#### [LeetCode] Daily 문제
##### [Minimum Operations to Make Array Equal](https://leetcode.com/problems/minimum-operations-to-make-array-equal/)

You have an array arr of length n where arr[i] = (2 * i) + 1 for all valid values of i (i.e. 0 <= i < n).

In one operation, you can select two indices x and y where 0 <= x, y < n and subtract 1 from arr[x] and add 1 to arr[y] (i.e. perform arr[x] -=1 and arr[y] += 1). The goal is to make all the elements of the array equal. It is guaranteed that all the elements of the array can be made equal using some operations.

Given an integer n, the length of the array. Return the minimum number of operations needed to make all the elements of arr equal.

 

Example 1:

Input: n = 3
Output: 2
Explanation: arr = [1, 3, 5]
First operation choose x = 2 and y = 0, this leads arr to be [2, 3, 4]
In the second operation choose x = 2 and y = 0 again, thus arr = [3, 3, 3].

Example 2:

Input: n = 6
Output: 9


``` cpp
class Solution {
public:
    int minOperations(int n) {
        // n = 5 (odd) --> 1 3 5 7 9     --> 4 + 2     --> n-1 + n-3 + ... 
        // n = 6 (even)--> 1 3 5 7 9 11  --> 5 + 3 + 1 --> n-1 + n-3 + n-5 + ... + 1
        if (n % 2 == 1) {
            return ((n / 2)*(n + 1))/2;
        }
        else {
            return ((n / 2)*n)/2;
        }
    }
};
```

그냥.. 점화식 문제... 등차수열의 합 구하는 방법 까먹어서 조금 멘붕했다. 이 포스트를 올릴까 말까 고민할 정도로 코드가 짧지만 요즘 포스팅 하는것도 없어서 포스트 채우기용으로 올린다.

##### 출처:
- [Minimum Operations to Make Array Equal](https://leetcode.com/problems/minimum-operations-to-make-array-equal/)
