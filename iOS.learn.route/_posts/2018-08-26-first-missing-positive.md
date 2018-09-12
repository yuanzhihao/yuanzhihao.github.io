---
layout: post
title: "Leetcode. 41 First Missing Positive"
description: "给定一个未排序的整数数组，找到最小的未出现的正整数。"
categories: [算法]
tags: [数组]
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

> 因为本方法使用数组元素的负号表示对应下标加一的正整数是否出现，所以先将数组中所有的负数和零都换成任意大于数组长度的数，避免这些无用元素的干扰。再遍历数组，对遍历到的元素取绝对值，如果该绝对值小于等于数组长度且该绝对值减一对应的数组元素为正数，则将对应的数组元素取负，如果已经是负数则不变。本轮遍历完后，接着从头遍历数组，如果数组中某个元素不为负数，则其对应下标加一的正整数是最小的未出现正整数。如果数组元素都为负，则数组长度加一的正整数是最小的未出现正整数，因为最小未出现正整数的取值范围是一到数组长度加一。

### 代码

```swift
class Solution {
    func firstMissingPositive(_ nums: [Int]) -> Int {
        if nums.isEmpty {
            return 1
        }
        if nums.count == 1 {
            if nums[0] == 1 {
                return 2
            } else {
                return 1
            }
        }
        var n = nums
        for index in 0..<n.count {
            if n[index] <= 0 {
                n[index] = n.count + 1
            }
        }
        for index in 0..<n.count {
            let temp = Swift.abs(n[index])
            if temp > 0 && temp < n.count + 1 {
                if n[temp - 1] > 0 {
                    n[temp - 1] = -n[temp - 1]
                }
            }
        }
        for index in 0..<n.count {
            if n[index] > 0 {
                return index + 1
            }
        }
        return n.count + 1
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