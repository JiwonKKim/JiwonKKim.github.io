---
layout: post
title:  "[LeetCode] Top Interview Questions 26 - Remove Duplicates from Sorted Array"
category: Algorithm
tags: [Algorithm, LeetCode, Remove Duplicates from Sorted Array]
data: 2022-08-19 17:00:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 26
#### [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)

Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates in-place such that each unique element appears only **once**. The relative order of the elements should be kept the **same**.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the **first part** of the array `nums`. More formally, if there are `k` elements after removing the duplicates, then the first `k` elements of `nums` should hold the final result. It does not matter what you leave beyond the first `k` elements.

Return `k` *after placing the final result in the first `k` slots of `nums`*.

Do **not** allocate extra space for another array. You must do this by **modifying the input array in-place** with O(1) extra memory.

**Custom Judge:**

The judge will test your solution with the following code:

``` cpp
int[] nums = [...]; // Input array
int[] expectedNums = [...]; // The expected answer with correct length

int k = removeDuplicates(nums); // Calls your implementation

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}
```
If all assertions pass, then your solution will be **accepted**.


 <!-- ***문제에 대한 자세한 설명은 출처를 참조***<br> -->

### **Example 1:**
```
Input: nums = [1,1,2]
Output: 2, nums = [1,2,_]
Explanation: Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

### **Example 2:**
```
Input: nums = [0,0,1,1,1,2,2,3,3,4]
Output: 5, nums = [0,1,2,3,4,_,_,_,_,_]
Explanation: Your function should return k = 5, with the first five elements of nums being 0, 1, 2, 3, and 4 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).
```

**Constraints:**

- `1 <= nums.length <= 3 * 104`
- `-100 <= nums[i] <= 100`
- `nums` is sorted in **non-decreasing** order.

**My Solution:**
``` python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        i = 0
        length = len(nums)
        for j in range(i+1, length):
            if nums[i] != nums[j]:
                i += 1
                nums[i] = nums[j]
                
        return i+1
```

**My Explanation:**

인자로 주어진 nums를 In-place로 수정하라는 조건이 달려있다. 주어진 nums는 이미 non-decreasing order로 정렬되어 있으므로, 두 개의 포인터를 사용해서 중복되지 않는 숫자를 파악하여 기존의 배열에 덮어쓴다.

<!-- [**LeetCode Solution**](https://leetcode.com/problems/merge-k-sorted-lists/solution/) -->

##### 출처:
- [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)