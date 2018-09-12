---
layout: post
title: "Leetcode. 28 Implement strStr()"
description: "给定一个目标串和匹配串，找出目标串中第一次出现匹配串的位置，如果不存在，返回 -1。"
categories: [算法]
tags: [Sunday 算法]
redirect_from:
  - /2018/07/08/
---

Implement strStr().

Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

Example 1:

> Input: haystack = "hello", needle = "ll"
> Output: 2

Example 2:

> Input: haystack = "aaaaa", needle = "bba"
> Output: -1

Clarification:

What should we return when needle is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when needle is an empty string. This is consistent to C's strstr() and Java's indexOf().

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 从目标串头到尾进行字符串匹配。如果匹配成功，直接返回位置。如果失败，关注每次目标串中参与匹配的子串的后一个字符，如果该字符没有在匹配串中出现，就意味着任何包含该字符的子串都不能和匹配串成功匹配，直接从该字符的后一个字符开始新的匹配。如果该字符在匹配串中出现，则将匹配串向右移动直到第一次匹配串出现和该字符相同的字符，然后开始新的比较。

### 代码

```java
class Solution {
    public int strStr(String haystack, String needle) {
        if (needle == null || needle.isEmpty()) {
            return 0;
        }
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        for (int i = 0; i < needle.length(); i++) {
            map.put(needle.charAt(i), i);
        }
        int i = 0;
        int limit = haystack.length() - needle.length();
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