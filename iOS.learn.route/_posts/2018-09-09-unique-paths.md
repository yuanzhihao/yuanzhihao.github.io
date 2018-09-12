---
layout: post
title: "Leetcode. 62 Unique Paths"
description: "给定一个 m * n 大小的网盘，在左上角放置一个机器人，机器人只能向下或向右行动，求有多少条路径可以到达右下角。"
categories: [算法]
tags: [动态规划]
redirect_from:
  - /2018/09/09/
---

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there?

[!Above is a 7 x 3 grid. How many possible unique paths are there?][https://leetcode.com/static/images/problemset/robot_maze.png]

Above is a 7 x 3 grid. How many possible unique paths are there?

Note: m and n will be at most 100.

Example 1:

> Input: m = 3, n = 2
> Output: 3
> Explanation:
> From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
> 1. Right -> Right -> Down
> 2. Right -> Down -> Right
> 3. Down -> Right -> Right

Example 2:

> Input: m = 7, n = 3
> Output: 28

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 建立一个和网盘大小相同的二维数组，存储到达每个对应网盘位置的路径条数。初始起点对应位置的路径数为 1 ，从初始位置开始遍历数组，对于还未计算路径数的位置，只需将其上方位置和其左方位置的路径数相加（假如存在），即得当前位置的路径数。

### 代码

```swift
class Solution {
    func uniquePaths(_ m: Int, _ n: Int) -> Int {
        var arr: [[Int]] = Array<[Int]>(repeating: Array<Int>(repeating: 0, count: n), count: m)
        arr[0][0] = 1
        for i in 0..<m {
            for j in 0..<n {
                if i - 1 > -1 && arr[i - 1][j] > 0 {
                    arr[i][j] += arr[i - 1][j]
                }
                if j - 1 > -1 && arr[i][j - 1] > 0 {
                    arr[i][j] += arr[i][j - 1]
                }
            }
        }
        return arr[m - 1][n - 1]
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 暂无。

### 代码

```java
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture