---
layout: post
title: "Leetcode. 3 Longest Substring Without Repeating Characters"
description: "给定义字符串，找出最长无重复子串。"
tags: [哈希表,滑动窗口]
redirect_from:
  - /2018/07/30/
---

Given a string, find the length of the longest substring without repeating characters.

Examples:

Given "abcabcbb", the answer is "abc", which the length is 3.

Given "bbbbb", the answer is "b", with the length of 1.

Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 维护一个哈希表 map ，key 是字符，value 是对应字符在字符串中最近一次出现的位置。遍历字符串，对每个字符检查其是否在 map 中出现。如果有，比较当前最大值和当前位置（不含当前位置）到 start 的距离，取较大者更新最大值，并检查当前字符在 map 中对应的下标是否小于当前的 start。如果小于代表从 start 后面到当前元素（含当前元素）有重复字符，需要更新 start 到当前字符在 map 中对应的下标的下一个元素。遍历完数组后，比较最大值和数组尾到 start 的距离。返回较大者。

### 代码

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int max = 0;
        int start = 0;
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        for (int i = 0; i < s.length(); i++) {
            if(map.containsKey(s.charAt(i))) {
                max = Math.max(max, i - start);
                start = (start < map.get(s.charAt(i)) + 1) ? map.get(s.charAt(i)) + 1 : start;
            }
            map.put(s.charAt(i), i);
        }
        max = Math.max(max, s.length() - start);
        return max;
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 与我的思路相同。

### 代码

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        Map<Character, Integer> map = new HashMap<>(); // current index of character
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            if (map.containsKey(s.charAt(j))) {
                i = Math.max(map.get(s.charAt(j)), i);
            }
            ans = Math.max(ans, j - i + 1);
            map.put(s.charAt(j), j + 1);
        }
        return ans;
    }
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture