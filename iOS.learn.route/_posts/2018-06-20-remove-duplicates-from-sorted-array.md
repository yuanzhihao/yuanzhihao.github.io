---
layout: post
title: "Leetcode. 26 Remove Duplicates from Sorted Array"
description: "给定一个有序数组，在位的移除所有重复元素并返回有效元素长度。"
categories: [算法]
tags: [两根指针]
redirect_from:
  - /2018/06/20/
---

> Given a sorted array nums, remove the duplicates in-place such that each element appear only once and return the new length. Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory.

Example 1:

> Given nums = [1,1,2],
> 
> Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.
>
> It doesn't matter what you leave beyond the returned length.

Example 2:

> Given nums = [0,0,1,1,1,2,2,3,3,4],
>
> Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.
>
> It doesn't matter what values are set beyond the returned length.

Clarification:

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by reference, which means modification to the input array will be known to the caller as well.

Internally you can think of this:

```java
// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 两根指针 i，j。i 的左边代表最终结果，j 右边（含 j ）代表未探索区域，j 本身指向当前要探索的元素。将 j 指向的元素与前一个放入最终结果的元素比较，若不同则将其加入最终结果。否则忽略该元素。

### 代码

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        // i 左边代表最终结果，j 右边（含 j ）代表未探索区域，j 本身指向当前要探索的元素
        int i = 0, j = 0;
        Integer currentLastInteger = null;
        while (j != nums.length) {
            if (currentLastInteger == null || currentLastInteger != nums[j]) {
                exchangeElement(i, j, nums);
                currentLastInteger = nums[i++];
            }
            j++;
        }
        return i;
    }
    
    private void exchangeElement(int i, int j, int[] nums) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = nums[i];
    }
}
```

## Leetcode 收录最优解决方案

### 思路

>  与我的思路相同。

### 代码

```java
public int removeDuplicates(int[] nums) {
    if (nums.length == 0) return 0;
    int i = 0;
    for (int j = 1; j < nums.length; j++) {
        if (nums[j] != nums[i]) {
            i++;
            nums[i] = nums[j];
        }
    }
    return i + 1;
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture