---
layout: post
title: "Leetcode. 103 Binary Tree Zigzag Level Order Traversal"
description: "给定一个二叉树，按照 zigzag 的方式按层遍历这棵二叉树。（zigzag： 从左到右遍历一层，下一层从右到左遍历，不断交换）"
categories: [算法]
tags: [BFS]
redirect_from:
  - /2018/05/12/
---

> Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example:

Given binary tree [3,9,20,null,null,15,7],

 >         3
 >        / \\
 >       9  20
 >         /  \\
 >        15   7

return its zigzag level order traversal as:

>      [
>       [3],
>       [20,9],
>       [15,7]
>      ]

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

>  大体按照广度优先搜索的思路，但是不采用队列存储节点，而是使用两个栈来分别存储奇数层（1，3，5...)和偶数层（2,4,6...）的节点。并且单数层的节点将子节点入栈时，按照先左节点后右节点的顺序，偶数层则相反。时间复杂度为节点数，空间复杂度为节点数。

### 代码

#### 递归写法

```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        if (root == null) {
            return new LinkedList<List<Integer>>();
        }
        LinkedList<List<Integer>> result = new LinkedList<List<Integer>>();
        int currentLevel = 0;
        int currentLevelNodeCount = 1;
        int nextLevelNodeCount = 0;
        ArrayDeque<TreeNode> deque = new ArrayDeque<TreeNode>();
        deque.add(root);
        LinkedList<Integer> list = new LinkedList<Integer>();
        while (!deque.isEmpty()) {
            TreeNode node;
            if (currentLevel % 2 == 0) {
                node = deque.pollLast();
                if (node.left != null) {
                    deque.addFirst(node.left);
                    nextLevelNodeCount++;
                }
                if (node.right != null) {
                    deque.addFirst(node.right);
                    nextLevelNodeCount++;
                }
            } else {
                node = deque.poll();
                if (node.right != null) {
                    deque.add(node.right);
                    nextLevelNodeCount++;
                }
                if (node.left != null) {
                    deque.add(node.left);
                    nextLevelNodeCount++;
                }
            }
            currentLevelNodeCount--;
            list.add(node.val);
            if (currentLevelNodeCount == 0) {
                result.add(new LinkedList<Integer>(list));
                list.clear();
                currentLevel++;
                currentLevelNodeCount = nextLevelNodeCount;
                nextLevelNodeCount = 0;
            }
        }
        return result;
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 未收录

### 代码

```java
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture