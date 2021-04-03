---
author: "Nolan"
title: "Minesweeper"
date: "2021-04-01"
categories: ["alogorithm", "python"]
draft: false
description: "Minesweeper Algo"
tags: ["algo", "bfs"]
ShowToc: false
TocOpen: false
---

# Minesweeper

I came across this [challenge on leetcode](https://leetcode.com/problems/minesweeper/) that the goal is to generate a new board after a row, column is selected.
Here is an [online](https://minesweeperonline.com/) version of the game.
It's a very BFS like challenge, down below the code.


# Solution

```
class Solution:
    def is_valid_coord(self, board, row, col):
        if row >= 0 and row < len(board) and col >= 0 and col < len(board[0]):
            return True
        return False

    def adjacent_mine(self, board, direction, x, y):
        adjacent_mine = 0
        for nx, ny in direction:
            new_row =  nx + x
            new_col = ny + y
            if self.is_valid_coord(board, new_row, new_col) and board[new_row][new_col] == 'M':
                adjacent_mine += 1
        return adjacent_mine
    
    def updateBoard(self, board: List[List[str]], click: List[int]) -> List[List[str]]:
        pos_x, pos_y = click
        if board[pos_x][pos_y] == 'M':
            board[pos_x][pos_y] = "X"
            return board
        queue = [click]
        # left down right up
        direction = [[0,-1], [1,-1], [1,0], [1,1], [0,1], [-1,1], [-1,0], [-1,-1]]
        seen = set()
        while queue:
            x, y = queue.pop(0)
            if board[x][y] == "E" and self.adjacent_mine(board, direction, x, y) == 0:
                board[x][y] = 'B'
                seen.add((x, y))
                for nx, ny in direction:
                    if self.is_valid_coord(board, nx+x, ny+y) and (nx+x, ny+y) not in seen:
                        queue.append([nx+x, ny+y])
            elif board[x][y] == "E" and self.adjacent_mine(board, direction, x, y) >= 0:
                board[x][y] = str(self.adjacent_mine(board, direction, x, y))
        return board
            
        
```

Time Complexity: O(m*n)
Space Complexity: O(m+n)