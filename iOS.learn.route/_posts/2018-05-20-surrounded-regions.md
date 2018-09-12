---
layout: post
title: "Leetcode. 130 Surrounded Regions"
description: "给定一个由 O 和 X 组成的二维数组，将所有没有被 X 包围的 O 替换为 X。"
categories: [算法]
tags: [BFS, DFS]
redirect_from:
  - /2018/05/20/
---

> Given a 2D board containing 'X' and 'O' (the letter O), capture all regions surrounded by 'X'. A region is captured by flipping all 'O's into 'X's in that surrounded region.

Example:

Input: 

> X X X X
>
> X O O X
>
> X X O X
>
> X O X X

After running your function, the board should be:

Output: 

> X X X X
>
> X X X X
>
> X X X X
>
> X O X X

Explanation:

Surrounded regions shouldn’t be on the border, which means that any 'O' on the border of the board are not flipped to 'X'. Any 'O' that is not on the border and it is not connected to an 'O' on the border will be flipped to 'X'. Two cells are connected if they are adjacent cells connected horizontally or vertically.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 只有和数组最外围的 O 连通的 O 不会被替换为 X 。使用 BFS 将所有和最外围 O 连通的 O 标记，再将所有未被标记的字符替换为 X 。

### 代码

> 此处省略队列实现。

```swift
class Solution {
    func solve(_ board: inout [[Character]]) {
        if board.isEmpty || board[0].isEmpty {
            return
        }
        var isAccessed = [[Int]](repeating:[Int](repeating:0, count:board[0].count), count:board.count) 
        let node1 = Node()
        let node2 = Node()
        node1.column = 0
        node2.column = board[0].count - 1
        for i in 0..<board.count {
            node1.row = i
            node2.row = i
            if isAccessed[node1.row][node1.column] == 0 && board[node1.row][node1.column] == Character("O") {
                exploreAdjacentZero(&board, &isAccessed, node1)
            }
            if board[0].count - 1 >= 0 && isAccessed[node2.row][node2.column] == 0 && board[node2.row][node2.column] == Character("O") {
                exploreAdjacentZero(&board, &isAccessed, node2)
            }
        }
        node1.row = 0
        node2.row = board.count - 1
        for j in 0..<board[0].count {
            node1.column = j
            node2.column = j
            if isAccessed[node1.row][node1.column] == 0 && board[node1.row][node1.column] == Character("O") {
                exploreAdjacentZero(&board, &isAccessed, node1)
            }
            if board.count - 1 >= 0 && isAccessed[node2.row][node2.column] == 0 && board[node2.row][node2.column] == Character("O") {
                exploreAdjacentZero(&board, &isAccessed, node2)
            }
        }
        for i in 0..<board.count {
            for j in 0..<board[0].count {
                if isAccessed[i][j] != 1 {
                    board[i][j] = Character("X")
                }
            }
        }
    }
    
    func exploreAdjacentZero(_ board: inout [[Character]], _ isAccessed: inout [[Int]], _ node: Node) {
        var queue = Queue()
        queue.push(node)
        while !queue.isEmpty() {
            let cur = queue.pop()
            if let node = cur {
                if isAccessed[node.row][node.column] == 0 && board[node.row][node.column] == Character("O") {
                    isAccessed[node.row][node.column] = 1;
                    if node.row - 1 >= 0 {
                        var n = Node()
                        n.row = node.row - 1
                        n.column = node.column
                        queue.push(n)
                    }
                    if node.row + 1 < board.count {
                        var n = Node()
                        n.row = node.row + 1
                        n.column = node.column
                        queue.push(n)
                    }
                    if node.column - 1 >= 0 {
                        var n = Node()
                        n.row = node.row
                        n.column = node.column - 1
                        queue.push(n)
                    }
                    if node.column + 1 < board[0].count {
                        var n = Node()
                        n.row = node.row
                        n.column = node.column + 1
                        queue.push(n)
                    }
                }
            }
        }
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