---
layout: post
title:  "[LeetCode] Top Interview Questions 23 - Merge k Sorted Lists"
category: Algorithm
tags: [Algorithm, LeetCode, Merge k Sorted Lists]
data: 2022-08-19 15:50:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 23
#### [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

*Merge all the linked-lists into one sorted linked-list and return it*.

 <!-- ***문제에 대한 자세한 설명은 출처를 참조***<br> -->

### **Example 1:**
```
Input: lists = [[1,4,5],[1,3,4],[2,6]]
Output: [1,1,2,3,4,4,5,6]
Explanation: The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```

### **Example 2:**
```
Input: lists = []
Output: []
```

### **Example 3:**
```
Input: lists = [[]]
Output: []
```

**Constraints:**

- `k == lists.length`
- `0 <= k <= 104`
- `0 <= lists[i].length <= 500`
- `-104 <= lists[i][j] <= 104`
- `lists[i]` is sorted in **ascending order**.
- The sum of `lists[i].length` will not exceed `10^4`.

**My Solution:**
``` python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeKLists(self, lists: List[Optional[ListNode]]) -> Optional[ListNode]:
        sorted_head = ListNode()
        sorted_node = sorted_head
        
        values = []
        for node_head in lists:
            node = node_head
            while node:
                values.append(node.val)
                node = node.next
        
        values.sort()
        
        for item in values:
            new_node = ListNode()
            new_node.val = item
            sorted_node.next = new_node
            sorted_node = sorted_node.next
        
        return sorted_head.next
```

**My Explanation:**

문제 출제자의 의도가 이거였을까.. 싶었지만 Solution에도 이 방법이 적혀있었다. 그냥 노드 내의 val을 모두 탐색해서 리스트에 꾸겨넣고 정렬하여 반환했다.

[**LeetCode Solution**](https://leetcode.com/problems/merge-k-sorted-lists/solution/)

##### 출처:
- [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)
