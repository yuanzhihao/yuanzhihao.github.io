---
layout: post
title: "Leetcode. 34 Find First and Last Position of Element in Sorted Array"
description: "给定一个有序增序数组，找到给定元素第一次和最后一次出现的位置。"
categories: [算法]
tags: [二分查找]
redirect_from:
  - /2018/08/25/
---

Given an array of integers `nums` sorted in ascending order, find the starting and ending position of a given `target` value.

Your algorithm's runtime complexity must be in the order of O(log n).

If the target is not found in the array, return `[-1, -1]`.

Example 1:

> Input: nums = [5,7,7,8,8,10], target = 8
> Output: [3,4]

Example 2:

> Input: nums = [5,7,7,8,8,10], target = 6
> Output: [-1,-1]

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 使用二分查找的思想，排除不可能的部分，在可能的部分继续排除。现在到最左边的，再找最右边的。

### 代码

```swift
class Solution {
    func searchRange(_ nums: [Int], _ target: Int) -> [Int] {
        if nums.isEmpty {
            return [-1, -1]
        }
        return [binarySearch(nums, target, 0, nums.count - 1, true), binarySearch(nums, target, 0, nums.count - 1, false)]
    }

    func binarySearch(_ nums: [Int], _ target: Int, _ start: Int, _ end: Int, _ direction: Bool) -> Int {
        while start < end - 1 {
            let mid = start + (end - start) / 2
            if nums[mid] == target {
                if direction {
                    return binarySearch(nums, target, start, mid, direction)
                } else {
                    return binarySearch(nums, target, mid, end, direction)
                }
            } else if nums[mid] < target {
                return binarySearch(nums, target, mid + 1, end, direction)
            } else {
                return binarySearch(nums, target, start, mid - 1, direction)
            }
        }
        if direction {
            if nums[start] == target {
                return start
            }
            if nums[end] == target {
                return end
            }
            return -1
        } else {
            if nums[end] == target {
                return end
            }
            if nums[start] == target {
                return start
            }
            return -1
        }
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 和我的想法相似。

### 代码

```java
class Solution {
    // returns leftmost (or rightmost) index at which `target` should be
    // inserted in sorted array `nums` via binary search.
    private int extremeInsertionIndex(int[] nums, int target, boolean left) {
        int lo = 0;
        int hi = nums.length;

        while (lo < hi) {
            int mid = (lo + hi) / 2;
            if (nums[mid] > target || (left && target == nums[mid])) {
                hi = mid;
            }
            else {
                lo = mid+1;
            }
        }

        return lo;
    }

    public int[] searchRange(int[] nums, int target) {
        int[] targetRange = {-1, -1};

        int leftIdx = extremeInsertionIndex(nums, target, true);

        // assert that `leftIdx` is within the array bounds and that `target`
        // is actually in `nums`.
        if (leftIdx == nums.length || nums[leftIdx] != target) {
            return targetRange;
        }

        targetRange[0] = leftIdx;
        targetRange[1] = extremeInsertionIndex(nums, target, false)-1;

        return targetRange;
    }
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture