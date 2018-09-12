---
layout: post
title: "Leetcode. 169 Majority Element"
description: "给定一个长度为 n 的数组，找到出现次数超过一半的元素。可以假设数组非空且一定存在这么一个元素。"
categories: [算法]
tags: [街头打架]
redirect_from:
  - /2018/05/12/
---

> Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times. You may assume that the array is non-empty and the majority element always exist in the array.

Example 1:

> Input: [3,2,3]

> Output: 3

Example 2:

> Input: [2,2,1,1,1,2,2]

> Output: 2

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 将每个不同数字视作可以进行反应消耗的化学物质，且消耗比例为 1 : 1。相同数字只叠加不消耗。将所有数字放到一起进行反应，最后剩下的数字一定是解。时间复杂度为 n ，空间复杂度为 1 。

### 代码

#### 递归写法

```swift
class Solution {
    func majorityElement(_ nums: [Int]) -> Int {
        var count = 1
        var currentNum = nums[0]
        var i = 1
        while i < nums.count {
            if nums[i] != currentNum {
                count -= 1
                if count < 0 {
                    currentNum = nums[i]
                    count = 1
                }
            } else {
                count += 1
            }
            i += 1
        }
        return currentNum
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 思路同我的解法一致。

### 代码

```java
class Solution {
    public int majorityElement(int[] nums) {
        int count = 0;
        Integer candidate = null;

        for (int num : nums) {
            if (count == 0) {
                candidate = num;
            }
            count += (num == candidate) ? 1 : -1;
        }

        return candidate;
    }
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture