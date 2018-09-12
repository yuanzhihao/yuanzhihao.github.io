---
layout: post
title: "Leetcode. 36 Valid Sudoku"
description: "给定一个 9 ✖ 9 数组，检查其是否满足特定规则。"
categories: [算法]
tags: [hash]
redirect_from:
  - /2018/07/08/
---

Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated according to the following rules:

1. Each row must contain the digits 1-9 without repetition.
2. Each column must contain the digits 1-9 without repetition.
3. Each of the 9 3x3 sub-boxes of the grid must contain the digits 1-9 without repetition.

[!A partially filled sudoku which is valid.][https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png]

The Sudoku board could be partially filled, where empty cells are filled with the character '.'.

Note:

* A Sudoku board (partially filled) could be valid but is not necessarily solvable.
* Only the filled cells need to be validated according to the mentioned rules.
* The given board contain only digits 1-9 and the character '.'.
* The given board size is always 9x9.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 使用 HashSet 按照三条规则分别检查。

### 代码

```java
class Solution {
    public boolean isValidSudoku(char[][] board) {
        return isValidForRow(board) && isValidForColumn(board) && isVaildForSubBox(board);
    }
    
    private boolean isValidForRow(char[][] board) {
        HashSet<Character> set = new HashSet<Character>();
        for (int i = 0; i < board.length; i++) {
            set.clear();
            for (int j = 0; j < board[0].length; j++) {
                if (Character.isDigit(board[i][j])) {
                    if (set.contains(board[i][j])) {
                        return false;
                    }
                    set.add(board[i][j]);
                }
            }
        }
        return true;
    }
    
    private boolean isValidForColumn(char[][] board) {
        HashSet<Character> set = new HashSet<Character>();
        for (int j = 0; j < board.length; j++) {
            set.clear();
            for (int i = 0; i < board[0].length; i++) {
                if (Character.isDigit(board[i][j])) {
                    if (set.contains(board[i][j])) {
                        return false;
                    }
                    set.add(board[i][j]);
                }
            }
        }
        return true;
    }
    
    private boolean isVaildForSubBox(char[][] board) {
        for (int i = 0; i < board.length; i += 3) {
            for (int j = 0; j < board[0].length; j += 3) {
                if (!isVaildForSubBox(i, j, board)) {
                    return false;
                }
            }
        }
        return true;
    }
    
    private boolean isVaildForSubBox(int x, int y, char[][] board) {
        HashSet<Character> set = new HashSet<Character>();
        for (int j = 0; j < 3; j++) {
            for (int i = 0; i < 3; i++) {
                if (Character.isDigit(board[x + i][y + j])) {
                    if (set.contains(board[x + i][y + j])) {
                        return false;
                    }
                    set.add(board[x + i][y + j]);
                }
            }
        }
        return true;
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