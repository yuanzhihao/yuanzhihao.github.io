---
layout: post
title: "Leetcode. 239 Sliding Window Maximum"
description: "给定一个数组和滑动窗口的长度，将滑动窗口从头到尾滑动，求整个滑动过程中滑动窗口对应区域的所有最大值。"
categories: [算法]
tags: [堆]
redirect_from:
  - /2018/07/08/
---

Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.

Example:

> Input: nums = [1,3,-1,-3,5,3,6,7], and k = 3
> Output: [3,3,5,5,6,7] 
> Explanation: 

> Window position                Max
> ---------------               -----
> [1  3  -1] -3  5  3  6  7       3
> 1 [3  -1  -3] 5  3  6  7       3
> 1  3 [-1  -3  5] 3  6  7       5
> 1  3  -1 [-3  5  3] 6  7       5
> 1  3  -1  -3 [5  3  6] 7       6
> 1  3  -1  -3  5 [3  6  7]      7

Note: 

You may assume k is always valid, 1 ≤ k ≤ input array's size for non-empty array.

Follow up:

Could you solve it in linear time?

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 使用大顶堆。将滑动窗口所包含的元素添加到堆中，同时记录已经离开滑动窗口的元素，如果这种元素出现在堆顶，则移除。这样就可以保证堆顶元素永远是滑动窗口的最大值。

### 代码

```java
class Solution {
    private class MaxFirstComparator implements Comparator<Integer> {
        public int compare(Integer o1, Integer o2) {
            return o2.compareTo(o1);
        }
    }
    
    public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums == null || nums.length == 0) {
            return new int[0];
        }
        int[] result = new int[nums.length - k + 1];
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        PriorityQueue<Integer> heap = new PriorityQueue<Integer>(k, new MaxFirstComparator());
        for (int i = 0; i < k; i++) {
            heap.add(nums[i]);
        }
        for (int i = 0; i < result.length; i++) {
            result[i] = heap.peek();
            if (i + k < nums.length) {
                heap.add(nums[i + k]);
                if (!map.containsKey(nums[i])) {
                    map.put(nums[i], 0);
                }
                map.put(nums[i], map.get(nums[i]) + 1);
            }
            while (map.get(heap.peek()) != null && map.get(heap.peek()) > 0) {
                int temp = heap.poll();
                map.put(temp, map.get(temp) - 1);
            }
        }
        return result;
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