---
layout: post
title:  "[LeetCode] Top Interview Questions 56 - Merge Intervals"
category: Algorithm
tags: [Algorithm, LeetCode, Merge Intervals]
data: 2022-08-24 16:20:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 56
#### [Merge Intervals](https://leetcode.com/problems/merge-intervals/)

Given an array of `intervals` where `intervals[i] = [start_i, end_i]`, merge all overlapping intervals, and return *an array of the non-overlapping intervals that cover all the intervals in the input*.

 <!-- ***문제에 대한 자세한 설명은 출처를 참조***<br> -->

### **Example 1:**
```
Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlap, merge them into [1,6].
```

### **Example 2:**
```
Input: intervals = [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```

**Constraints:**

- `1 <= intervals.length <= 104`
- `intervals[i].length == 2`
- `0 <= start_i <= end_i <= 104`

**My Solution:**
``` python
class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        intervals.sort(key=lambda x: x[0])
        answer = []
        for item in intervals:
            if answer and answer[-1][1] >= item[0]:
                answer[-1][1] = max(item[1], answer[-1][1])
            else:
                answer.append(item)
        
        return answer
```

**My Explanation:**

새삼 LeetCode 문제가 거지같다고 느꼈다. 예를들어, [[1, 4], [0, 0]]를 입력으로 넣으면 [[0, 0], [1, 4]]로 답이 출력되어야 한다. 즉, [0, 0]도 non-overlapping interval로 간주하여야 한다. DP로 문제를 풀다가 이놈 때문에 코드를 다시 짰다. 도대체 왜..?

intervals의 start를 기준으로 intervals를 정렬한다. interval을 하나씩 answer에 append하면서, answer에 마지막으로 append한 interval의 end 값이 지금 append하고자 하는 interval의 start값과 비교하여, overlapping될 수 있는지 확인한다.

계속 이런식으로 혼란을 주는 문제가 나온다면 프로그래머스로 넘어갈 예정이다.

<!-- [**LeetCode Solution**](https://leetcode.com/problems/jump-game/solution/) -->

##### 출처:
- [Merge Intervals](https://leetcode.com/problems/merge-intervals/)