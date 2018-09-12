---
layout: post
title: "Leetcode. 33 Search in Rotated Sorted Array"
description: "给定一个被旋转的有序增序数组，找到该数组中是否存给定整数 target。"
categories: [算法]
tags: [二分查找]
redirect_from:
  - /2018/08/25/
---

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).

You are given a target value to search. If found in the array return its index, otherwise return `-1`.

You may assume no duplicate exists in the array.

Your algorithm's runtime complexity must be in the order of O(log n).

Example 1:

> Input: nums = [4,5,6,7,0,1,2], target = 0
> Output: 4

Example 2:

> Input: nums = [4,5,6,7,0,1,2], target = 3
> Output: -1

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 使用二分查找的思想，对于给定首尾的一段数组，若其中间元素等于 target ，直接返回该下标。若中间元素小于 target ，如果首元素小于中间元素或所取中间元素就是首元素，则 target 若存在则一定在中间元素的后一个元素到尾元素之中（递归），如果首元素大于中间元素，则比较首元素与 target 的大小关系，若大于则 target 若存在则一定在中间元素的后一个元素到尾元素之中，否则 target 若存在则一定在首元素到中间元素的前一个元素之中。若中间元素大于 target ，如果首元素小于中间元素，比较首元素与 target 的大小关系，若大于则 target 若存在则一定在中间元素的后一个元素到尾元素之中，否则 target 若存在则一定在首元素到中间元素的前一个元素之中，如果所取中间元素就是首元素，则 target 若存在则一定在中间元素的后一个元素到尾元素之中，如果首元素大于中间元素，则 target 若存在则一定在首元素到中间元素的前一个元素之中。

### 代码

```swift
class Solution {
    func search(_ nums: [Int], _ target: Int) -> Int {
        return binarySearch(nums, target, 0, nums.count - 1)
    }
    
    func binarySearch(_ nums: [Int], _ target: Int, _ start: Int, _ end: Int) -> Int {
        while start <= end {
            let mid = start + (end - start) / 2
            if nums[mid] == target {
                return mid
            } else if nums[mid] < target {
                if nums[start] <= nums[mid] {
                    return binarySearch(nums, target, mid + 1, end)
                } else {
                    if nums[start] > target {
                        return binarySearch(nums, target, mid + 1, end)
                    } else {
                        return binarySearch(nums, target, start, mid - 1)
                    }
                }
            } else {
                if nums[start] < nums[mid] {
                    if nums[start] > target {
                        return binarySearch(nums, target, mid + 1, end)
                    } else {
                        return binarySearch(nums, target, start, mid - 1)
                    }
                } else if start == mid {
                    return binarySearch(nums, target, mid + 1, end)
                } else {
                    return binarySearch(nums, target, start, mid - 1)
                }
            }
        }
        return -1
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