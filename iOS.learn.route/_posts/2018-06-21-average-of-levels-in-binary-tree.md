---
layout: post
title: "Leetcode. 637 Average of Levels in Binary Tree"
description: "给定一个非空二叉树，求每一层节点的平均值。"
categories: [算法]
tags: [BFS,DFS]
redirect_from:
  - /2018/06/21/
---

> Given a non-empty binary tree, return the average value of the nodes on each level in the form of an array.

Example 1:

> Input:
>       3
>      / \
>     9  20
>       /  \
>      15   7
> Output: [3, 14.5, 11]
> Explanation:
> The average value of nodes on level 0 is 3,  on level 1 is 14.5, and on level 2 is 11. Hence return [3, 14.5, 11].

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 广度优先搜索遍历二叉树，计算每一层的平均值。

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
    public List<Double> averageOfLevels(TreeNode root) {
        LinkedList<Double> result = new LinkedList<Double>();
        LinkedList<TreeNode> queue = new LinkedList<TreeNode>();
        queue.add(root);
        int currentLevelNodeCount = 1, nextLevelNodeCount = 0, candidateNodeCount = currentLevelNodeCount;
        long sum = 0;
        while (!queue.isEmpty()) {
            TreeNode current = queue.poll();
            candidateNodeCount--;
            average += current.val;
            if (current.left != null) {
                queue.add(current.left);
                nextLevelNodeCount++;
            }
            if (current.right != null) {
                queue.add(current.right);
                nextLevelNodeCount++;
            }
            if (candidateNodeCount == 0) {
                result.add(sum / (double)currentLevelNodeCount);
                currentLevelNodeCount = nextLevelNodeCount;
                nextLevelNodeCount = 0;
                candidateNodeCount = currentLevelNodeCount;
                sum = 0;
            }
        }
        return result;
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 和我的思路相同。

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
public class Solution {
    public List < Double > averageOfLevels(TreeNode root) {
        List < Integer > count = new ArrayList < > ();
        List < Double > res = new ArrayList < > ();
        average(root, 0, res, count);
        for (int i = 0; i < res.size(); i++)
            res.set(i, res.get(i) / count.get(i));
        return res;
    }
    public void average(TreeNode t, int i, List < Double > sum, List < Integer > count) {
        if (t == null)
            return;
        if (i < sum.size()) {
            sum.set(i, sum.get(i) + t.val);
            count.set(i, count.get(i) + 1);
        } else {
            sum.add(1.0 * t.val);
            count.add(1);
        }
        average(t.left, i + 1, sum, count);
        average(t.right, i + 1, sum, count);
    }
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture