---
layout: post
title: "Leetcode. 234 Palindrome Linked List"
description: "给定一个链表，判断它是否是回文的。"
categories: [算法]
tags: [两根指针]
redirect_from:
  - /2018/07/14/
---

Given a singly linked list, determine if it is a palindrome.

Example 1:

> Input: 1->2
> Output: false
> Example 2:

> Input: 1->2->2->1
> Output: true

Follow up:

Could you do it in O(n) time and O(1) space?

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 先遍历一遍链表获得长度，得到需要判断对称的区域，再将其中一个区域翻转。然后使用两个指针分别指向两个区域的头，判断两个区域是否完全相同。

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
    public boolean isPalindrome(ListNode head) {
        int length = 0;
        ListNode cur = head;
        while (cur != null) {
            length++;
            cur = cur.next;
        }
        if (length == 0 || length == 1) {
            return true;
        }
        int temp = (length + 1) / 2;
        cur = head; 
        while (temp != 1) {
            cur = cur.next;
            temp--;
        }
        cur.next = reverseList(cur.next);
        ListNode pre = head, nex = cur.next;
        while (nex != null) {
            if (pre.val != nex.val) {
                return false;
            }
            pre = pre.next;
            nex = nex.next;
        }
        return true;
    }
    
    private ListNode reverseList(ListNode head) {
        ListNode fakeHead = new ListNode(-1);
        ListNode cur = head;
        ListNode pre = null;
        while (cur != null) {
            pre = cur;
            cur = cur.next;
            pre.next = fakeHead.next;
            fakeHead.next = pre;
        }
        return fakeHead.next;
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