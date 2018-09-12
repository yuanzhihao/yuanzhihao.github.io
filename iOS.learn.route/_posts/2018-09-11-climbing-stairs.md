---
layout: post
title: "Leetcode. 70 Climbing Stairs"
description: "给定一个整数代表相应数值层数的阶梯，现在你每次可以上一层或两层阶梯，求有多少种方式可以爬完阶梯。"
categories: [算法]
tags: [动态规划]
redirect_from:
  - /2018/09/11/
---

You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

Note: Given n will be a positive integer.

Example 1:

> Input: 2
> Output: 2
> Explanation: There are two ways to climb to the top.
> 1. 1 step + 1 step
> 2. 2 steps

Example 2:

> Input: 3
> Output: 3
> Explanation: There are three ways to climb to the top.
> 1. 1 step + 1 step + 1 step
> 2. 1 step + 2 steps
> 3. 2 steps + 1 step

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 动态规划，已知一层阶梯和两层阶梯的有多少种方式，进而可以求得三层阶梯。依次类推，可得 n 层阶梯的解。

### 代码

```swift
class Solution {
    func climbStairs(_ n: Int) -> Int {
        if n == 1 || n == 2 {
            return n
        }
        var arr: [Int] = Array(repeating: 0, count: n)
        arr[0] = 1
        arr[1] = 2
        for i in 2..<n {
            arr[i] = arr[i - 1] + arr[i - 2]
        }
        return arr[n - 1]
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