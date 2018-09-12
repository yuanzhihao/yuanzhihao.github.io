---
layout: post
title: "Leetcode. 46 Permutations"
description: "给定一组互不相同的整数，求所有的排列。"
categories: [算法]
tags: [BFS,DFS]
redirect_from:
  - /2018/08/26/
---

Given an unsorted integer array, find the smallest missing positive integer.

Example 1:

> Input: [1,2,0]
> Output: 3

Example 2:

> Input: [3,4,-1,1]
> Output: 2

Example 3:

> Input: [7,8,9,11,12]
> Output: 1

Note:

Your algorithm should run in O(n) time and uses constant extra space.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> BFS/DFS 遍历给定的数组，即可得到所有排列。

### 代码

```swift
class Solution {
    func permute(_ nums: [Int]) -> [[Int]] {
        var results: [[Int]] = []
        if nums.isEmpty {
            return results
        }
        for i in 0..<nums.count {
            var candidate = nums
            let num = nums[i]
            candidate.remove(at: i)
            generatePermutations(num, &results, [], candidate)
        }
        return results
    }
    
    func generatePermutations(_ num: Int, _ results: inout [[Int]], _ pre:[Int], _ candidate:[Int]) {
        var tempPre = pre
        tempPre.append(num)
        if candidate.isEmpty {
            results.append(tempPre)
            return
        }
        for i in 0..<candidate.count {
            var temp = candidate
            let n = candidate[i]
            temp.remove(at: i)
            generatePermutations(n, &results, tempPre, temp)
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