---
layout: post
title: "Leetcode. 49 Group Anagrams"
description: "给定一个字符串数组，按同字异序词分组。"
categories: [算法]
tags: [哈希表,排序]
redirect_from:
  - /2018/08/27/
---

Given an array of strings, group anagrams together.

Example:

> Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
> Output:
>     [
>      ["ate","eat","tea"],
>      ["nat","tan"],
>      ["bat"]
>     ]

Note:

* All inputs will be in lowercase.
* The order of your output does not matter.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 将所有字符串排序，再用排序好的字符串作为 key ，使用哈希表对这些字符串分组。

### 代码

```swift
class Solution {
    func groupAnagrams(_ strs: [String]) -> [[String]] {
        var result = [[String]]()
        var copy = strs
        var map: [String : [String]] = [:]
        for index in 0..<copy.count {
            copy[index] = String(strs[index].characters.sorted())
            if map[copy[index]] == nil {
                map[copy[index]] = []
            }
            map[copy[index]]!.append(strs[index])
        }
        for (_, value) in map {
            result.append(value)
        }
        return result
    }
}
```

### 衍生

如果字符的取值范围有限，可以用数组统计字符串出现次数，将统计数组转化的字符串用作哈希表的 key，使用哈希表对这些字符串分组。

## Leetcode 收录最优解决方案

### 思路

> 与我的思路相同。

### 代码

```swift
class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
        if (strs.length == 0) return new ArrayList();
        Map<String, List> ans = new HashMap<String, List>();
        for (String s : strs) {
            char[] ca = s.toCharArray();
            Arrays.sort(ca);
            String key = String.valueOf(ca);
            if (!ans.containsKey(key)) ans.put(key, new ArrayList());
            ans.get(key).add(s);
        }
        return new ArrayList(ans.values());
    }
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture