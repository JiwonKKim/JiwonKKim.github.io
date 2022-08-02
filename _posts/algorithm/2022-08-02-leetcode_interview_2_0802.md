---
layout: post
title:  "[LeetCode] Top Interview Questions 2 - Add Tow Numbers"
category: Algorithm
tags: [Algorithm, LeetCode, Add Two Numbers]
data: 2022-08-02 22:35:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 2
#### [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)


You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order**, and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.
 
 ***문제에 대한 자세한 설명은 출처를 참조***<br>

### **Example 1:**
```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```

### **Example 2:**
```
Input: l1 = [0], l2 = [0]
Output: [0]
Example 3:
```

### **Example 3:**
```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

**Constraints:**

- The number of nodes in each linked list is in the range `[1, 100]`.
- `0 <= Node.val <= 9`
- It is guaranteed that the list represents a number that does not have leading zeros.
  
**My Solution:**
``` python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
def list_to_int(node):
    val = 0
    for i, item in enumerate(node):
        val += item * (10**(i))
    
    return val

def int_to_node_list(val):
    tmp_val = str(val)
    first_node = ListNode()
    node = first_node
    for i, item in enumerate(reversed(tmp_val)):
        node.val = int(item)
        if i != len(tmp_val) - 1:
            node.next = ListNode()
            node = node.next
    
    return first_node
        
def node_to_list(node):
    val_list = []
    tmp_node = node
    val_list.append(tmp_node.val)
    while tmp_node.next:
        tmp_node = tmp_node.next
        val_list.append(tmp_node.val)        
    
    return val_list
    
class Solution:
    def addTwoNumbers(self, l1: Optional[ListNode], l2: Optional[ListNode]) -> Optional[ListNode]:
        v1, v2 = node_to_list(l1), node_to_list(l2)        
        v1_int, v2_int = list_to_int(v1), list_to_int(v2)        
        answer_int = v1_int + v2_int
        answer = int_to_node_list(answer_int)
        return answer
```

**My Explanation:**

각각의 노드를 탐색하여 노드가 가리키고 있던 값을 리스트화하고, 이를 정수형으로 변환시킨다. 변환된 정수형끼리 더하고, 더한 값을 다시 노드화하여 반환한다.

[**LeetCode Solution**](https://leetcode.com/problems/add-two-numbers/solution/)

##### 출처:
- [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)
