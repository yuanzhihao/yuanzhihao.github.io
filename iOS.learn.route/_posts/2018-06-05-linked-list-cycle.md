---
layout: post
title: "Leetcode. 141 Linked List Cycle"
description: "给定一个单向链表，判断是否有环。"
categories: [算法]
tags: [链表,快慢指针]
redirect_from:
  - /2018/06/05/
---

> Given a linked list, determine if it has a cycle in it.

Follow up:
Can you solve it without using extra space?

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 一个快指针一个慢指针，快指针每次走两步，慢指针每次走一步。如果有环，则两个指针必然相遇。否则，快指针必然会先到链表尾。

### 代码

```java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode fast = head;
        ListNode slow = head;
        while (slow != null && fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                return true;
            }
        }
        return false;
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 与我的思路相同。

### 代码

```java
public boolean hasCycle(ListNode head) {
    if (head == null || head.next == null) {
        return false;
    }
    ListNode slow = head;
    ListNode fast = head.next;
    while (slow != fast) {
        if (fast == null || fast.next == null) {
            return false;
        }
        slow = slow.next;
        fast = fast.next.next;
    }
    return true;
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture