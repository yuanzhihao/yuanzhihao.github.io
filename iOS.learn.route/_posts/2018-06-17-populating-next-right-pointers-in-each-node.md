---
layout: post
title: "Leetcode. 116 Populating Next Right Pointers in Each Node"
description: "给定一个完美二叉树，每个节点除了 left，right 指针外还有一个 next 指针，令所有节点的 next 指针指向同层相邻右侧节点。"
categories: [算法]
tags: [递归]
redirect_from:
  - /2018/06/17/
---

Given a binary tree

>           struct TreeLinkNode {
>            TreeLinkNode \*left;
>            TreeLinkNode \*right;
>            TreeLinkNode \*next;
>           }


Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

Note:

* You may only use constant extra space.
* Recursive approach is fine, implicit stack space does not count as extra space for this problem.
* You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).

Example:

Given the following perfect binary tree,

>          1
>        /  \
>       2    3
>      / \  / \
>     4  5  6  7

After calling your function, the tree should look like:

>          1 -> NULL
>        /  \
>       2 -> 3 -> NULL
>      / \  / \
>     4->5->6->7 -> NULL

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 采用递归的思路，将 connect 方法的作用定义为将 root 节点的左右子树之间的跨子树 next 指针赋值。 

### 代码

#### 递归写法

```java
/**
 * Definition for binary tree with next pointer.
 * public class TreeLinkNode {
 *     int val;
 *     TreeLinkNode left, right, next;
 *     TreeLinkNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void connect(TreeLinkNode root) {
        if (root == null || root.left == null) {
            return;
        }
        connect(root.left);
        connect(root.right);
        root.left.next = root.right;
        TreeLinkNode tempLeft = root.left.right;
        TreeLinkNode tempRight = root.right.left;
        while (tempLeft != null) {
            tempLeft.next = tempRight;
            tempLeft = tempLeft.right;
            tempRight = tempRight.left;
        }
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