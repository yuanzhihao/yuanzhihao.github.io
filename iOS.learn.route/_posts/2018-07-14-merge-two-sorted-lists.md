---
layout: post
title: "Leetcode. 21 Merge Two Sorted Lists"
description: "合并两个增序链表"
categories: [算法]
tags: [两根指针]
redirect_from:
  - /2018/07/08/
---

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

Example:

> Input: 1->2->4, 1->3->4
> Output: 1->1->2->3->4->4

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 两个指针分别指向两个链表。判断所指向元素的大小关系，按大小关系依次添加到结果链表，同时将指向被添加到结果链表的节点的指针向后移动直到某个指针移动到队尾。最后将没有移动到队尾的指针所指向的节点添加到结果链表。

### 代码

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        ListNode head = new ListNode(-1);
        ListNode tail = head;
        ListNode i = l1, j = l2;
        while (i != null && j != null) {
            if (i.val <= j.val) {
                tail.next = i;
                i = i.next;
            } else {
                tail.next = j;
                j = j.next;
            }
            tail = tail.next;
        }
        if (i != null) {
            tail.next = i;
        }
        if (j != null) {
            tail.next = j;
        }
        return head.next;
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