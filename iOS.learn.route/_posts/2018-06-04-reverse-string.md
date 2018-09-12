---
layout: post
title: "Leetcode. 344 Reverse String"
description: "给定一个字符串，反转该字符串。"
categories: [算法]
tags: [字符串]
redirect_from:
  - /2018/06/04/
---

> YWrite a function that takes a string as input and returns the string reversed.

Example:

> Given s = "hello", return "olleh".

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 使用 StringBuilder 从后往前依次添加字符串中的字符。

### 代码

```java
class Solution {
    public String reverseString(String s) {
        if (s == null) {
            return null;
        }
        StringBuilder sb = new StringBuilder();
        for (int i = s.length() - 1; i > -1; i--) {
            sb.append(s.charAt(i));
        }
        return sb.toString();
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