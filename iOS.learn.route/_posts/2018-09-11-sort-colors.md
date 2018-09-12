---
layout: post
title: "Leetcode. 75 Sort Colors"
description: "给定一个包含红白蓝三种对象的数组，按红白蓝的顺序对数组排序。"
categories: [算法]
tags: [两根指针,排序]
redirect_from:
  - /2018/09/11/
---

Given an array with n objects colored red, white or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

Note: You are not suppose to use the library's sort function for this problem.

Example:

> Input: [2,0,2,1,1,0]
> Output: [0,0,1,1,2,2]

Follow up:

* A rather straight forward solution is a two-pass algorithm using counting sort.
First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
* Could you come up with a one-pass algorithm using only constant space?

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 彩虹排序。使用两根指针，左指针的左边存放红色对象，右指针的右边存放蓝色对象。初始化时，左指针在数组头，右指针在数组尾，从头遍历数组直到穿过右指针。遍历时，将红色对象和左指针指向的对象交换，并将左指针加一（维持左指针的物理含义）；将蓝色对象和右指针指向的对象交换，并将右指针减一（维持右指针的物理含义）。遍历完成时，排序完成。

### 代码

```swift
class Solution {
    func sortColors(_ nums: inout [Int]) {
        var leftTap = 0
        var rightTap = nums.count - 1
        var fast = 0
        while fast <= rightTap {
            if nums[fast] == 0 {
                swap(&nums, fast, leftTap)
                leftTap += 1
                fast += 1
            } else if nums[fast] == 2 {
                swap(&nums, fast, rightTap)
                rightTap -= 1
            } else {
                fast += 1
            }
        }
    }
    
    func swap(_ nums: inout [Int], _ i: Int, _ j: Int) {
        let temp = nums[i]
        nums[i] = nums[j]
        nums[j] = temp
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