---
layout: post
title: "Leetcode. 5 Longest Palindromic Substring"
description: "给定一个字符串，求最长回文子串。"
tags: [中心开花]
redirect_from:
  - /2018/07/31/
---

Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

Example 1:

> Input: "babad"
> Output: "bab"
> Note: "aba" is also a valid answer.

Example 2:

> Input: "cbbd"
> Output: "bb"

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 对字符串中的每个字符，将其视作包含该字符的回文子串的中心，向其两边扩张，直到不再满足回文性。遍历每个字符，即得到最长回文子串。

### 代码

```java
public class Solution {
    public String longestPalindrome(String s) {
        int length = 0;
        String result = "";
        for (int i = 0; i < s.length(); i++) {
            String temp = getLongestPalindromeWithCenter(s, i);
            if (temp.length() > length) {
                length = temp.length();
                result = temp;
            }
        }
        return result;
    }
    
    private String getLongestPalindromeWithCenter(String s, int center) {
        int length1 = 1;
        int length2 = 0;
        int i = center - 1,j = center + 1;
        while (i >= 0 && j < s.length() && s.charAt(i) == s.charAt(j)) {
            length1 += 2;
            i--;
            j++;
        }
        int x = center, y = center + 1;
        while (x >= 0 && y < s.length() && s.charAt(x) == s.charAt(y)) {
            length2 += 2;
            x--;
            y++;
        }
        if (length1 > length2)
            return s.substring(i + 1, j);
        else
            return s.substring(x + 1, y);
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 与我的思路相同。

### 代码

```java
public String longestPalindrome(String s) {
    if (s == null || s.length() < 1) return "";
    int start = 0, end = 0;
    for (int i = 0; i < s.length(); i++) {
        int len1 = expandAroundCenter(s, i, i);
        int len2 = expandAroundCenter(s, i, i + 1);
        int len = Math.max(len1, len2);
        if (len > end - start) {
            start = i - (len - 1) / 2;
            end = i + len / 2;
        }
    }
    return s.substring(start, end + 1);
}

private int expandAroundCenter(String s, int left, int right) {
    int L = left, R = right;
    while (L >= 0 && R < s.length() && s.charAt(L) == s.charAt(R)) {
        L--;
        R++;
    }
    return R - L - 1;
}
```

### 不普遍的解法-- Manacher's Algorithm

详见：[!Manacher's Algorithm][http://articles.leetcode.com/longest-palindromic-substring-part-ii/]

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture