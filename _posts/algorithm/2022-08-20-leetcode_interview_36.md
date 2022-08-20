---
layout: post
title:  "[LeetCode] Top Interview Questions 36 - Valid Sudoku"
category: Algorithm
tags: [Algorithm, LeetCode, Valid Sudoku]
data: 2022-08-20 18:00:00+0900
comments: true  
---

### [LeetCode] Top interview Questions 36
#### [Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)

Determine if a `9 x 9` Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:

- Each row must contain the digits `1-9` without repetition.
- Each column must contain the digits `1-9` without repetition.
- Each of the nine `3 x 3` sub-boxes of the grid must contain the digits `1-9` without repetition.

**Note:**

- A Sudoku board (partially filled) could be valid but is not necessarily solvable.
- Only the filled cells need to be validated according to the mentioned rules.

 ***문제에 대한 자세한 설명은 출처를 참조***<br>

### **Example 1:**
```
Input: board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: true
```

### **Example 2:**
```
Input: board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
```

**Constraints:**

- `board.length == 9`
- `board[i].length == 9`
- `board[i][j]` is a digit `1-9` or `'.'`.

**My Solution:**
``` python
class Solution:
    def isValidSudoku(self, board: List[List[str]]) -> bool:
        
        def checkValid(array):
            length = len(array)
            check_array = [0 for _ in range(length)]
            for item in array:
                if item == ".":
                    continue
                if check_array[int(item)-1]:
                    return False
                else:
                    check_array[int(item)-1] = 1
            return True
        width = len(board[0])
        height = len(board)
        
        # ** Check row
        for w in range(width):
            valid = checkValid(board[w])
            if not valid:
                return False
        
        # ** Check column
        for h in range(height):
            tmp = list(zip(*board))[h]
            valid = checkValid(tmp)
            if not valid:
                return False
        
        # ** Check sub-box
        for w in range(0, width, 3):
            for h in range(0, height, 3):
                tmp = board[h][w:w+3] + board[h+1][w:w+3] + board[h+2][w:w+3]
                valid = checkValid(tmp)
                if not valid:
                    return False
        
        return True
```

**My Explanation:**

주어진 배열이 스도쿠의 규칙에 벗어나는지 확인하는 문제이다. 크게 고민할 것 없이 DP를 활용해 행, 열, 그리고 sub-box가 규칙에 어긋나는지 확인한다.

[**LeetCode Solution**](https://leetcode.com/problems/valid-sudoku/solution/)

##### 출처:
- [Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)