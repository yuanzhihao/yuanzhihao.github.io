---
layout: post
title: "Leetcode. 395 Longest Substring with At Least K Repeating Characters"
description: "给定一个字符串和一个整数 k，找出一个每个字符都出现 k 次的最长子串。"
categories: [算法]
tags: [字符串]
redirect_from:
  - /2018/05/29/
---

> Find the length of the longest substring T of a given string (consists of lowercase letters only) such that every character in T appears no less than k times.

Example 1:

> Input:
>
> s = "aaabb", k = 3
> 
> Output:
>
> 3
>
> The longest substring is "aaa", as 'a' is repeated 3 times.

Example 2:

> Input:
>
> s = "ababbc", k = 2
>
> Output:
>
> 5
>
> The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 对于满足条件的子串，一定不会包含出现次数少于 k 的字符。用这些出现次数少于 k 的字符，切割字符串。最后不能切割且长度大于 k 的子串即为最终结果。

### 代码

```java
class Solution {
    public int longestSubstring(String s, int k) {
        if (k <= 0) {
            return s.length();
        }
        char[] charArray = s.toCharArray();
        return longestSubstringForSpecificSection(charArray, 0, s.length(), k);
    }
    
    private int longestSubstringForSpecificSection(char[] array, int left, int right, int k) {
        if (right - left < k) {
            return 0;
        }
        int[] count = new int[26];
        HashSet<Character> remove = new HashSet<Character>();
        for (int i = left; i < right; i++) {
            count[array[i] - 'a']++;
        }
        for (int i = 0; i < count.length; i++) {
            if (count[i] < k && count[i] > 0) {
                remove.add((char)('a' + i));
            }
        }
        if (remove.isEmpty()) {
            return right - left;
        }
        int max = Integer.MIN_VALUE;
        int start = left;
        for (int i = left; i < right; i++) {
            if (remove.contains(array[i])) {
                int temp = longestSubstringForSpecificSection(array, start, i, k);
                max = max > temp ? max : temp;
                start = i + 1;
            }
        }
        int temp = longestSubstringForSpecificSection(array, start, right, k);
        max = max > temp ? max : temp;
        return max;
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