---
layout: post
title: "Leetcode. 295 Find Median from Data Stream"
description: "给定一段整型数据流，设计一个数据结构，返回当前已加载数据的中位数。"
categories: [算法]
tags: [递归]
redirect_from:
  - /2018/06/17/
---

> Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

For example,

[2,3,4], the median is 3

[2,3], the median is (2 + 3) / 2 = 2.5

Design a data structure that supports the following two operations:

* void addNum(int num) - Add a integer number from the data stream to the data structure.
* double findMedian() - Return the median of all elements so far.

Example:

> addNum(1)
> addNum(2)
> findMedian() -> 1.5
> addNum(3) 
> findMedian() -> 2

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 使用两个堆，一个存放大于等于中位数的数（类型为小顶堆），另一个存放小于等于中位数的数（类型为大顶堆），在读取数据流时，保证两个堆大小相差小于2。当需要获取中位数时，若两个堆大小相等，则返回两个堆堆顶元素的平均值，否则，返回大小较大堆的堆顶元素。 

### 代码

#### 递归写法

```java
class MedianFinder {
    
    private class MaxFirstComparator implements Comparator<Integer> {
        public int compare(Integer o1, Integer o2) {
            return o2 - o1;
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
        if (isEmpty()) {
            maxRootHeap.add(num);
            return;
        }
        double median = findMedian();
        if (median > num) {
            maxRootHeap.add(num);
        } else {
            minRootHeap.add(num);
        }
        if (!isbalanced()) {
            balance();
        }
    }
    
    public double findMedian() {
        if (isEqualSize()) {
            return (maxRootHeap.peek() + minRootHeap.peek()) / 2.0;
        }
        return maxRootHeap.size() > minRootHeap.size() ? maxRootHeap.peek() : minRootHeap.peek();
    }
    
    private boolean isEqualSize() {
        return maxRootHeap.size() == minRootHeap.size();
    }   
    
    private boolean isEmpty() {
        return maxRootHeap.isEmpty() && minRootHeap.isEmpty();
    }
    
    private boolean isbalanced() {
        return Math.abs(maxRootHeap.size() - minRootHeap.size()) < 2;
    }
    
    private void balance() {
        if (maxRootHeap.size() > minRootHeap.size()) {
            minRootHeap.add(maxRootHeap.poll());
        } else {
            maxRootHeap.add(minRootHeap.poll());
        }
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder obj = new MedianFinder();
 * obj.addNum(num);
 * double param_2 = obj.findMedian();
 */
```

## Leetcode 收录最优解决方案

### 思路

> 与我的思路相似，但是通过维护小顶堆的大小永远不小于大顶堆，简化获取中位数的判断，通过每次添加数字时，直接放入小顶堆，再将小顶堆堆顶元素放入大顶堆，简化堆的平衡。

### 代码

```C++
class MedianFinder {
    priority_queue<int> lo;                              // max heap
    priority_queue<int, vector<int>, greater<int>> hi;   // min heap

public:
    // Adds a number into the data structure.
    void addNum(int num)
    {
        lo.push(num);                                    // Add to max heap

        hi.push(lo.top());                               // balancing step
        lo.pop();

        if (lo.size() < hi.size()) {                     // maintain size property
            lo.push(hi.top());
            hi.pop();
        }
    }

    // Returns the median of current data stream
    double findMedian()
    {
        return lo.size() > hi.size() ? (double) lo.top() : (lo.top() + hi.top()) * 0.5;
    }
};
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture