---
layout: post
title: "Leetcode. 19 Remove Nth Node From End of List"
description: "给定一个链表，移除到链表尾为 n 的节点。"
categories: [算法]
tags: [快慢指针]
redirect_from:
  - /2018/08/21/
---

Given a linked list, remove the n-th node from the end of list and return its head.

Example:

> Given linked list: 1->2->3->4->5, and n = 2.
> 
> After removing the second node from the end, the linked list becomes 1->2->3->5.

Note:

Given n will always be valid.

Follow up:

Could you do this in one pass?

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 令快指针指向第 n 个节点，慢指针指向伪头，同时向尾部移动两个指针，移动幅度为 1 ，当快指针遍历当链表尾时，

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode fakeHead = new ListNode(-1);
        fakeHead.next = head;
        ListNode slow = fakeHead;
        ListNode fast = head;
        while (n != 0) {
            fast = fast.next;
            n--;
        }
        while (fast != null) {
            slow = slow.next;
            fast = fast.next;
        }
        slow.next = slow.next.next;
        return fakeHead.next;
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 与我的思路相同。

### 代码

```java
public ListNode removeNthFromEnd(ListNode head, int n) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode first = dummy;
    ListNode second = dummy;
    // Advances first pointer so that the gap between first and second is n nodes apart
    for (int i = 1; i <= n + 1; i++) {
        first = first.next;
    }
    // Move first to the end, maintaining the gap
    while (first != null) {
        first = first.next;
        second = second.next;
    }
    second.next = second.next.next;
    return dummy.next;
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture