---
layout: post
title:  "[LeetCode] Top Interview Questions 21 - Merge Two Sorted Lists"
category: Algorithm
tags: [Algorithm, LeetCode, Merge Two Sorted Lists]
data: 2022-08-19 14:50:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 21
#### [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

You are given the heads of two sorted linked lists `list1` and `list2`.

Merge the two lists in a one **sorted** list. The list should be made by splicing together the nodes of the first two lists.

Return *the head of the merged linked list*.

 ***문제에 대한 자세한 설명은 출처를 참조***<br>

### **Example 1:**
```
Input: list1 = [1,2,4], list2 = [1,3,4]
Output: [1,1,2,3,4,4]
```

### **Example 2:**
```
Input: list1 = [], list2 = []
Output: []
```

### **Example 3:**
```
Input: list1 = [], list2 = [0]
Output: [0]
```
**Constraints:**

- The number of nodes in both lists is in the range `[0, 50]`.
- `-100 <= Node.val <= 100`
- Both `list1` and `list2` are sorted in **non-decreasing** order.


**My Solution:**
``` python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        list1_node, list2_node = list1, list2
        sorted_head = ListNode()
        sorted_node = sorted_head
        while list1_node and list2_node:
            new_node = ListNode()
            if list1_node.val < list2_node.val:
                new_node.val = list1_node.val
                list1_node = list1_node.next
            else:
                new_node.val = list2_node.val
                list2_node = list2_node.next
            sorted_node.next = new_node
            sorted_node = new_node
        
        
        if list1_node:
            rest_node = list1_node
        else:
            rest_node = list2_node
        
        while rest_node:
            new_node = ListNode()
            new_node.val = rest_node.val
            rest_node = rest_node.next
            sorted_node.next = new_node
            sorted_node = new_node
            
        return sorted_head.next        
```

**My Explanation:**

이미 정렬된 두 개의 Linked List를 하나의 Sorted Linked List로 만든다. 새로운 Linked List를 만들고, 두 개의 Linked List에 포인터를 사용하여, 각각의 포인터가 가리키는 값을 비교하여 Sorted Linked List에 값을 넣는다.
<!-- [**LeetCode Solution**](https://leetcode.com/problems/letter-combinations-of-a-phone-number/solution/) -->

##### 출처:
- [Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

