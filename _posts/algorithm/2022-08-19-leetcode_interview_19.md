---
layout: post
title:  "[LeetCode] Top Interview Questions 19 - Remove Nth Node From End of List"
category: Algorithm
tags: [Algorithm, LeetCode, Remove Nth Node From End of List]
data: 2022-08-19 13:15:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 19
#### [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

Given the head of a linked list, remove the `nth` node from the end of the list and return its head.

 ***문제에 대한 자세한 설명은 출처를 참조***<br>

### **Example 1:**
```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```

### **Example 2:**
```
Input: head = [1], n = 1
Output: []
```

### **Example 3:**
```
Input: head = [1,2], n = 1
Output: [1]
```
**Constraints:**

- The number of nodes in the list is `sz`.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`


**My Solution:**
``` python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        tmp = head
        length = 1
        while tmp.next:
            tmp = tmp.next
            length += 1
        
        dummy = ListNode()
        dummy.next = head
        
        index = 1
        prev_target_node = dummy
        while index < (length - n + 1):
            prev_target_node = prev_target_node.next
            index += 1
        
        prev_target_node.next = prev_target_node.next.next
        
        return dummy.next                     
```

**My Explanation:**

Linked List의 전체 길이를 파악하여, 뒤에서 n번째에 위치한 노드를 찾아 제거한다.

<!-- [**LeetCode Solution**](https://leetcode.com/problems/letter-combinations-of-a-phone-number/solution/) -->

##### 출처:
- [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
