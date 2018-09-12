---
layout: post
title: "Leetcode. 289 Game of Life"
description: "给定一个二维数组，代表当前细胞的状态图。同时，给出细胞状态变化的规则，求下一个状态的细胞状态图。"
categories: [算法]
tags: [位运算]
redirect_from:
  - /2018/07/14/
---

According to the Wikipedia's article: "The Game of Life, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

Given a board with m by n cells, each cell has an initial state live (1) or dead (0). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

1. Any live cell with fewer than two live neighbors dies, as if caused by under-population.
2. Any live cell with two or three live neighbors lives on to the next generation.
3. Any live cell with more than three live neighbors dies, as if by over-population.
4. Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

Write a function to compute the next state (after one update) of the board given its current state. The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously.

Example:

> Input: 
> [
>  [0,1,0],
>  [0,0,1],
>  [1,1,1],
>  [0,0,0]
> ]
> Output: 
> [
>  [0,0,0],
>  [1,0,1],
>  [0,1,1],
>  [0,1,0]
> ]

Follow up:

Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.

In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 因为状态用 0 和 1 表示，所以整型实际上只有一位被使用，其他位可由我们使用用来存储新的状态。

### 代码

```java
class Solution {
    public void gameOfLife(int[][] board) {
        if (board == null || board.length == 0 || board[0].length == 0) {
            return;
        }
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                board[i][j] = accessNeighbors(i, j, board);
            }
        }
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                board[i][j] = (board[i][j] & 2) >> 1;
            }
        }
    }
    
    private int accessNeighbors(int i, int j, int[][] board) {
        int liveCount = countLiveCell(i, j, board);
        if (board[i][j] == 1) {
            if (liveCount < 2) {
                return board[i][j];
            } else if (liveCount >= 2 && liveCount <= 3) {
                return board[i][j] | 2;
            } else {
                return board[i][j];
            }
        } else if (board[i][j] == 0) {
            if (liveCount == 3) {
                return board[i][j] | 2;
            } else {
                return board[i][j];
            }
        } else {
            return -1;
        }
    }
    
    private int countLiveCell(int i, int j, int[][] board) {
        int liveCount = 0;
        for (int x = i - 1; x <= i + 1; x++) {
            for (int y = j - 1; y <= j + 1; y++) {
                if (x >= 0 && x < board.length && y >= 0 && y < board[0].length) {
                    if (x == i && y == j) {
                        continue;
                    }
                    if ((board[x][y] & 1) == 1) {
                        liveCount++;
                    }
                } 
            }
        }
        return liveCount;
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 暂无

### 代码

```java
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture