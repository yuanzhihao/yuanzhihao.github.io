---
layout: post
title: "Leetcode. 101 Symmetric Tree"
description: "给定一颗二叉树，求其是否是关于根节点对称的。"
categories: [算法]
tags: [递归]
redirect_from:
  - /2018/07/08/
---

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:

>         1
>        / \
>       2   2
>      / \ / \
>     3  4 4  3

But the following [1,2,2,null,3,null,3] is not:

>         1
>        / \
>       2   2
>        \   \
>        3    3

Note:

Bonus points if you could solve it both recursively and iteratively.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 设计一个方法递归接受两棵子树，判断他们是否是对称的。将根节点的两棵子树传入该方法。

### 代码

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        return isMirrorOfEachOther(root.left, root.right);
    }
    
    private boolean isMirrorOfEachOther(TreeNode left, TreeNode right) {
        boolean temp = false;
        if (left == right) {
            return true;
        }
        if ((left == null && right != null) || (left != null && right == null)) {
            return false;
        }
        if (left.val == right.val) {
            temp = true;
        } else {
            return false;
        }
        boolean leftTemp = false;
        leftTemp = isMirrorOfEachOther(left.left, right.right);
        boolean rightTemp = false;
        rightTemp = isMirrorOfEachOther(left.right, right.left);
        return temp & leftTemp & rightTemp;
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 与我的思路相同。

### 代码

```java
public boolean isSymmetric(TreeNode root) {
    return isMirror(root, root);
}

public boolean isMirror(TreeNode t1, TreeNode t2) {
    if (t1 == null && t2 == null) return true;
    if (t1 == null || t2 == null) return false;
    return (t1.val == t2.val)
        && isMirror(t1.right, t2.left)
        && isMirror(t1.left, t2.right);
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture