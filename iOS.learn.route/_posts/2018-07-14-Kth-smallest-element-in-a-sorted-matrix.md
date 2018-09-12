---
layout: post
title: "Leetcode. 378 Kth Smallest Element in a Sorted Matrix"
description: "给定一个 n * n 的矩阵，其每一行，每一列都是增序的。求这个矩阵中第 k 小的数。"
categories: [算法]
tags: [堆]
redirect_from:
  - /2018/07/14/
---

Given a n x n matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

Example:

>     matrix = [
>       [ 1,  5,  9],
>       [10, 11, 13],
>       [12, 13, 15]
>     ],

> k = 8,

> return 13.

Note: 

You may assume k is always valid, 1 ≤ k ≤ n2.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 使用小顶堆。将最小的元素加到堆中，并记录已经出堆元素个数及最后一个出堆元素的值。然后开始循环直到出堆元素个数等于 k ，每次循环将堆顶元素出堆，同时将出堆元素的右边一个元素和下面一个元素加到堆中（前提是这两个元素之前没有被添加到堆中过）。循环结束后，最后一个出堆元素的值即是解。

### 代码

```java
class Solution {
    private class Node {
        int val;
        int i;
        int j;
        
        public Node(int val, int i, int j) {
            this.val = val;
            this.i = i;
            this.j = j;
        }
    }
    
    private class MinFirstComparator implements Comparator<Node> {
        public int compare(Node n1, Node n2) {
            return n1.val - n2.val;
        }
    }
    
    public int kthSmallest(int[][] matrix, int k) {
        PriorityQueue<Node> minTopHeap = new PriorityQueue<Node>(k, new MinFirstComparator());
        boolean[][] isAccessed = new boolean[matrix.length][matrix.length];
        int index = 0;
        int result = 0;
        int i = 0, j = 0;
        minTopHeap.add(new Node(matrix[0][0], 0, 0));
        isAccessed[0][0] = true;
        while (index != k) {
            Node temp = minTopHeap.poll();
            result = temp.val;
            if (temp.i + 1 < matrix.length && !isAccessed[temp.i + 1][temp.j]) {
                minTopHeap.add(new Node(matrix[temp.i + 1][temp.j], temp.i + 1, temp.j));
                isAccessed[temp.i + 1][temp.j] = true;
            }
            if (temp.j + 1 < matrix.length && !isAccessed[temp.i][temp.j + 1]) {
                minTopHeap.add(new Node(matrix[temp.i][temp.j + 1], temp.i, temp.j + 1));
                isAccessed[temp.i][temp.j + 1] = true;
            }
            index++;
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