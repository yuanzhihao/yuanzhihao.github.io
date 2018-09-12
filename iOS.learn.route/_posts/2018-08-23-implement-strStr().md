---
layout: post
title: "Leetcode. 28 Implement strStr()"
description: "给定字符串 A 、B，求在 A 中 B 第一次出现的位置，如果没有出现返回 -1，如果 B 为空或空字符串，返回 0."
categories: [算法]
tags: [Sunday算法]
redirect_from:
  - /2018/08/23/
---

Implement [!strStr()][http://www.cplusplus.com/reference/cstring/strstr/].

Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

Example 1:

> Input: haystack = "hello", needle = "ll"
> Output: 2

Example 2:

> Input: haystack = "aaaaa", needle = "bba"
> Output: -1

Clarification:

What should we return when `needle` is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when `needle` is an empty string. This is consistent to C's [!strStr()][http://www.cplusplus.com/reference/cstring/strstr/] and Java's [!indexOf()][https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)].

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 从头开始比较两个字符串，若不匹配，查看当前 A 字符串中参与比较的子串的后一个字符是否在 B 中出现。如果出现，则将 B 字符串最后一次出现的该字符与 A 字符串中的该字符对齐，再接着比较，否则，直接将 B 字符串的头与该字符的下一个字符对齐，再接着比较。若匹配，直接返回当前位置。

### 代码

```java
class Solution {
    public int strStr(String haystack, String needle) {
        if (needle == null || needle.isEmpty()) {
            return 0;
        }
        int i = 0;
        int limit = haystack.length() - needle.length();
        if (limit < 0) {
            return -1;
        } else if (limit == 0) {
            return haystack.equals(needle) ? 0 : -1;
        }
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        for (; i < needle.length(); i++) {
            map.put(needle.charAt(i), i);
        }
        i = 0;
        while (i <= limit) {
            int j = i;
            for (; j - i < needle.length(); j++) {
                if (haystack.charAt(j) != needle.charAt(j - i)) {
                    break;
                }
            }
            if (j - i >= needle.length()) {
                return i;
            }
            if (i + needle.length() >= haystack.length()) {
                break;
            }
            if (!map.containsKey(haystack.charAt(i + needle.length()))) {
                i += needle.length() + 1;
            } else {
                i += needle.length() - map.get(haystack.charAt(i + needle.length()));
            }
        }
        return -1;
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