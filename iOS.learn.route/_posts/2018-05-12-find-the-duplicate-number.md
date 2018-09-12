---
layout: post
title: "Leetcode. 287 Find the Duplicate Number"
description: "给定一个出现且仅出现一个重复整数的数组。假定数组长度为 n + 1，所有整数取值范围为 1 到 n（包含上下界），找出重复的整数。要求：不能修改原数组，使用常数空间，时间复杂度小于 n 平方。"
categories: [算法]
tags: [二分查找, 链表]
redirect_from:
  - /2018/05/12/
---

> Given an array nums containing n + 1 integers where each integer is between 1 and n (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

Example 1:

> Input: [1,3,4,2,2]

> Output: 2

Example 2:

> Input: [3,1,3,4,2]

> Output: 3

Note:

* You must not modify the array (assume the array is read only).
* You must use only constant, O(1) extra space.
* Your runtime complexity should be less than O(n2).
* There is only one duplicate number in the array, but it could be repeated more than once.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 将整个解的取值范围看做一个有序的数组，应用二分查找算法。对于每次二分的 mid 值，遍历数组分别统计小于它的数和等于它的数。当等于它的数大于 1 时，即是本问题的解。否则，若小于它的数大于 mid - 1 时，则解在 1 到 mid - 1 中，反之亦然。二分 log(n) 次，每次需遍历 n 个元素。所以时间复杂度为 nlog(n)。空间复杂度，迭代写法为常数级，递归写法为 log(n)。

### 代码

#### 递归写法

```swift
class Solution {
    func findDuplicate(_ nums: [Int]) -> Int {
        return findDuplicate(nums, 1, nums.count - 1)
    }
    
    func findDuplicate(_ nums: [Int], _ lower: Int, _ upper: Int) -> Int {
        if lower >= upper {
            return lower
        }
        let target = (upper - lower) / 2 + lower
        let counts = countNumber(nums, target)
        if counts.1 > 1 {
            return target
        }
        if counts.0 > target - 1 {
            return findDuplicate(nums, lower, target - 1)
        } 
        return findDuplicate(nums, target + 1, upper)
    }
    
    func countNumber(_ nums: [Int], _ target: Int) -> (Int, Int) {
        var countOfNumberLessThanTarget = 0
        var countOfTarget = 0;
        for num in nums {
            if num < target {
                countOfNumberLessThanTarget += 1
            } else if num == target {
                countOfTarget += 1
            } 
        }
        return (countOfNumberLessThanTarget, countOfTarget)
    }
}
```

#### 迭代写法

待补充

## Leetcode 收录最优解决方案

### 思路

> 将数组看作是链表，对于每个元素（链表），它的下一个元素是 array[当前元素的值]，这样问题就被转化为求链表中环开始的点。我们将 array[0] 看做链表的头，使用快慢指针。时间复杂度为 n ，空间复杂度为 1 。

### 代码

```java
class Solution {
    public int findDuplicate(int[] nums) {
        // Find the intersection point of the two runners.
        int tortoise = nums[0];
        int hare = nums[0];
        do {
            tortoise = nums[tortoise];
            hare = nums[nums[hare]];
        } while (tortoise != hare);

        // Find the "entrance" to the cycle.
        int ptr1 = nums[0];
        int ptr2 = tortoise;
        while (ptr1 != ptr2) {
            ptr1 = nums[ptr1];
            ptr2 = nums[ptr2];
        }

        return ptr1;
    }
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture