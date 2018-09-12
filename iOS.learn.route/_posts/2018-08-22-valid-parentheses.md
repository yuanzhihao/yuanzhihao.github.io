---
layout: post
title: "Leetcode. 20 Valid Parentheses"
description: "给定一个由括号构成的字符串，判断这组括号是否有效。"
categories: [算法]
tags: [栈]
redirect_from:
  - /2018/08/22/
---

Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Note that an empty string is also considered valid.

Example 1:

> Input: "()"
> Output: true

Example 2:

> Input: "()[]{}"
> Output: true

Example 3:

> Input: "(]"
> Output: false

Example 4:

> Input: "([)]"
> Output: false

Example 5:

> Input: "{[]}"
> Output: true

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 用栈存储访问过的括号，如果栈为空时，访问到了反括号，直接返回假；如果栈不为空且栈顶元素和访问到的括号为一对，则栈顶元素出栈；如果栈不为空，栈顶元素和访问到的括号不为一对且访问到的是反括号，直接返回假；其他情况，将访问到的括号入栈。所有括号访问完后，栈为空则这组括号有效，否则无效。

### 代码

```java
public class Solution {
    public boolean isValid(String s) {
        Deque<Integer> stack = new ArrayDeque();
        int id = 0;
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(')
                id = 1;
            else if (s.charAt(i) == ')')
                id = -1;
            else if (s.charAt(i) == '[')
                id = 2;
            else if (s.charAt(i) == ']')
                id = -2;
            else if (s.charAt(i) == '{')
                id = 3;
            else if (s.charAt(i) == '}')
                id = -3;
            else
                return false;
            Integer top = stack.peekFirst();
            if (top == null && id < 0) {
                return false;
            } else if (top != null && top + id == 0) {
                stack.pollFirst();
            } else if (top != null && top + id != 0 && (top + id < top || top + id < id)) {
                return false;
            } else {
                stack.addFirst(id);
            }
        }
        return stack.isEmpty();
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