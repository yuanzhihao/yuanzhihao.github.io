---
layout: post
title: "Leetcode. 334 Increasing Triplet Subsequence"
description: "给定一个数组，判断其中是否存在 i，j，k。使得 arr[i] < arr[j] < arr[k]。"
categories: [算法]
tags: [两根指针]
redirect_from:
  - /2018/06/20/
---

Given an unsorted array return whether an increasing subsequence of length 3 exists or not in the array.

Formally the function should:

> Return true if there exists i, j, k 
> such that arr[i] < arr[j] < arr[k] given 0 ≤ i < j < k ≤ n-1 else return false.

Your algorithm should run in O(n) time complexity and O(1) space complexity.

Example:

> Given [1, 2, 3, 4, 5],
> return true.
>
> Given [5, 4, 3, 2, 1],
> return false.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 将题目转化为找到一个元素，在其之前有一个元素小于它，并且对于之前的那个元素，同样存在一个更前面的元素小于它。

### 代码

```swift
class Solution {
    func increasingTriplet(_ nums: [Int]) -> Bool {
        var potentialMin = Int.max
        // 不为 max 就代表这个值前面有一个小于它的数
        var currentMid = Int.max
        for num in nums {
            if num <= potentialMin {
                potentialMin = num
            } else if num <= currentMid {
                currentMid = num
            } else {
                return true
            }
        }
        return false
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