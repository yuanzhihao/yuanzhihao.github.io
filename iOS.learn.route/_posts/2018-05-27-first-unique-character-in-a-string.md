---
layout: post
title: "Leetcode. 387 First Unique Character in a String"
description: "给定一个字符串，找出第一个没有重复出现的字符的位置。如果不存在，则返回 -1。"
categories: [算法]
tags: [字符串,哈希表]
redirect_from:
  - /2018/05/27/
---

> Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

Example:

> s = "leetcode"
> return 0.
> 
> s = "loveleetcode",
> return 2.

Note: You may assume the string contain only lowercase letters.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 使用两个 HashSet。Set 1 用于存放只出现过一次的字符，Set 2 用于存放重复出现过的字符。当得到最终的 Set 1 后，遍历字符串找出第一个存在于 Set 1 的字符并返回当前位置。

### 代码

```java
class Solution {
    public int firstUniqChar(String s) {
        HashSet<Character> set = new HashSet<Character>();
        HashSet<Character> repeatingSet = new HashSet<Character>();
        for (int i = 0; i < s.length(); i++) {
            Character c = s.charAt(i);
            if (repeatingSet.contains(c)) {
                continue;
            }
            if (set.contains(c)) {
                set.remove(c);
                repeatingSet.add(c);
                continue;
            }
            set.add(c);
        }
        for (int i = 0; i < s.length(); i++) {
            if (set.contains(s.charAt(i))) {
                return i;
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