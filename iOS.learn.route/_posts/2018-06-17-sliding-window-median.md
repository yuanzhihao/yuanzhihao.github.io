---
layout: post
title: "Leetcode. 480 Sliding Window Median"
description: "给定一个数组和长度不定的窗口，从数组头滑动该窗口直到数组尾，求滑动过程中窗口对应的所有中位数。"
categories: [算法]
tags: [堆]
redirect_from:
  - /2018/06/17/
---

Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

Examples:

[2,3,4] , the median is 3

[2,3], the median is (2 + 3) / 2 = 2.5

Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Your job is to output the median array for each window in the original array.

For example,

Given nums = [1,3,-1,-3,5,3,6,7], and k = 3.

Example:

>     Window position                Median
>     ---------------               -----
>     [1  3  -1] -3  5  3  6  7       1
>      1 [3  -1  -3] 5  3  6  7       -1
>      1  3 [-1  -3  5] 3  6  7       -1
>      1  3  -1 [-3  5  3] 6  7       3
>      1  3  -1  -3 [5  3  6] 7       5
>      1  3  -1  -3  5 [3  6  7]      6

Therefore, return the median sliding window as [1,-1,-1,3,5,6].

Note: 

You may assume k is always valid, ie: k is always smaller than input array's size for non-empty array.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 当窗口滑动时，先将新元素加入到 MedianFinder 这个数据结构中，同时在 map 中记录本次应该删除的元素并更新应删除的次数。再根据当前中位数与应删除元素的大小关系（1. 当前中位数小于应删除元素，向 MedianFinder 中添加一个负无穷；2. 当前中位数大于等于应删除元素，向 MedianFinder 中添加一个正无穷）向 MedianFinder 添加元素。完成后，检查 MedianFinder 中两个堆得堆顶元素是否应被删除，从大顶堆开始检查（即优先删除这里的应删除元素）。最后记录此时的中位数。本质上是将不在堆顶的应删除元素暂缓删除，因为不在堆顶，所以这些元素只对区中位数的位置有影响而本身的值对中位数无影响。因此，在另一个堆上添加正负无穷以调整计算中位数的位置。当应删除元素来到堆顶时，删除该元素并添加正负无穷占位。(保证包含正负无穷的两个堆平衡，保证两个堆中的有效值平衡)

### 代码

#### 递归写法

```java
class Solution {
    private class MedianFinder {
        private class MaxFirstComparator implements Comparator<Integer> {
            public int compare(Integer o1, Integer o2) {
                return o2.compareTo(o1);
            }
        }
        // 存放小于中位数的数
        private PriorityQueue<Integer> maxRootHeap;
        // 存放大于中位数的数
        private PriorityQueue<Integer> minRootHeap;

        /** initialize your data structure here. */
        public MedianFinder() {
            maxRootHeap = new PriorityQueue<Integer>(11, new MaxFirstComparator());
            minRootHeap = new PriorityQueue<Integer>();
        }
    
        public void addNum(int num) {
            maxRootHeap.add(num);
            minRootHeap.add(maxRootHeap.poll());
            
            if (maxRootHeap.size() < minRootHeap.size()) {
                maxRootHeap.add(minRootHeap.poll());
            }
        }
    
        public double findMedian() {
            return maxRootHeap.size() > minRootHeap.size() ? maxRootHeap.peek() : maxRootHeap.peek() / 2.0 + minRootHeap.peek() / 2.0;
        }
        
        public void checkIfHeadShouldBeRemoved(HashMap<Integer, Integer> map) {
            while (map.containsKey(maxRootHeap.peek()) && map.get(maxRootHeap.peek()) > 0) {
                int temp = maxRootHeap.poll();
                map.put(temp, map.get(temp) - 1);
                maxRootHeap.add(Integer.MIN_VALUE);
            } 
            while (map.containsKey(minRootHeap.peek()) && map.get(minRootHeap.peek()) > 0) {
                int temp = minRootHeap.poll();
                map.put(temp, map.get(temp) - 1);
                minRootHeap.add(Integer.MAX_VALUE);
            }
        }
    }

    public double[] medianSlidingWindow(int[] nums, int k) {
        MedianFinder mf = new MedianFinder();
        for (int i = 0; i < k; i++) {
            mf.addNum(nums[i]);
        }
        int next = k;
        double[] result = new double[nums.length - k + 1];
        result[0] = mf.findMedian();
        HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
        while (next < nums.length) {
            mf.addNum(nums[next]);
            if (!map.containsKey(nums[next - k])) {
                map.put(nums[next - k], 0);
            }
            map.put(nums[next - k], map.get(nums[next- k]) + 1);
            if (mf.findMedian() >= nums[next - k]) {
                mf.addNum(Integer.MAX_VALUE);
            } else {
                mf.addNum(Integer.MIN_VALUE);
            }
            mf.checkIfHeadShouldBeRemoved(map);
            result[next - k + 1] = mf.findMedian();
            next++;
        }
        return result;
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 暂无

### 代码

```C++
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture