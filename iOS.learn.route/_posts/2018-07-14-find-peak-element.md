---
layout: post
title: "Leetcode. 162 Find Peak Element"
description: "给定一个数组，可将其视作波形图，求其中一个波峰对应的下标。注：数组的两端可被视作负无穷。"
categories: [算法]
tags: [二分查找]
redirect_from:
  - /2018/07/14/
---

A peak element is an element that is greater than its neighbors.

Given an input array nums, where nums[i] ≠ nums[i+1], find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that nums[-1] = nums[n] = -∞.

Example 1:

> Input: nums = [1,2,3,1]
> Output: 2
> Explanation: 3 is a peak element and your function should return the index number 2.
> Example 2:

> Input: nums = [1,2,1,3,5,6,4]
> Output: 1 or 5 
> Explanation: Your function can return either index number 1 where the peak element is 2, 
             or index number 5 where the peak element is 6.
Note:

Your solution should be in logarithmic complexity.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 设计一个递归方法判断数组的某一段是否存在波峰。递归方法：取该段的中间点，判断其是否是波峰，如果是返回其下标。否则，判断左右两边的元素，哪一边的元素大于中间点对应的元素，波峰应该出现在这个区间中。

### 代码

```java
class Solution {
    public int findPeakElement(int[] nums) {
        return findPeakElement(nums, 0, nums.length - 1);
    }
    
    private int findPeakElement(int[] nums, int start, int end) {
        int mid = (end - start) / 2 + start;
        boolean isGreaterThanPrev = mid - 1 >= 0 ? nums[mid] > nums[mid - 1] : true;
        boolean isGreaterThanNext = mid + 1 < nums.length ? nums[mid] > nums[mid + 1] : true;
        if (isGreaterThanPrev && isGreaterThanNext) {
            return mid;
        } else if (!isGreaterThanPrev) {
            return findPeakElement(nums, start, mid - 1);
        } else  {
            return findPeakElement(nums, mid + 1, end);
        }
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 与我的思路类似。

### 代码

```java
public class Solution {
    public int findPeakElement(int[] nums) {
        int l = 0, r = nums.length - 1;
        while (l < r) {
            int mid = (l + r) / 2;
            if (nums[mid] > nums[mid + 1])
                r = mid;
            else
                l = mid + 1;
        }
        return l;
    }
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture