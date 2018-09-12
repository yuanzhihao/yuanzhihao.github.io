---
layout: post
title: "Leetcode. 14 Longest Common Prefix"
description: "给定一个字符串数组，找出这些字符串的最长公共前缀。"
categories: [算法]
tags: [字符串]
redirect_from:
  - /2018/05/26/
---

> Write a function to find the longest common prefix string amongst an array of strings. If there is no common prefix, return an empty string "".

Example 1:

> Input: ["flower","flow","flight"]

> Output: "fl"

Example 2:

> Input: ["dog","racecar","car"]

> Output: ""

> Explanation: There is no common prefix among the input strings.

Note:

All given inputs are in lowercase letters a-z.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 依次取出数组中第一个字符串的字符，将其与之后的字符串对应位置的字符比较，找出最长公共前缀。时间复杂度为数组中的字符总数，空间复杂度为常数。

### 代码

#### 递归写法

```swift
class Solution {
    func longestCommonPrefix(_ strs: [String]) -> String {
        if strs.isEmpty || strs[0].isEmpty {
            return ""
        }
        var longestCommonPrefixArray: [Character] = []
        for i in 0..<strs[0].count {
            let currentIndex = strs[0].index(strs[0].startIndex, offsetBy:i)
            let currentChar = strs[0][currentIndex]
            for str in strs {
                if currentIndex == str.endIndex || str[currentIndex] != currentChar {
                    return String(longestCommonPrefixArray)
                }
            }
            longestCommonPrefixArray.append(currentChar)
        }
        return String(longestCommonPrefixArray)
    }
}
```

#### 迭代写法

待补充

## Leetcode 收录最优解决方案

### 思路

> 将字符串数组构建为 trie。通过一次构造 trie 的成本，提高查询速率。构造时间复杂度为字符串数组的字符数，查询时间复杂度为最长公共前缀长度，空间复杂度为字符串数组的字符数。

### 代码

```java
public String longestCommonPrefix(String q, String[] strs) {
    if (strs == null || strs.length == 0)
         return "";  
    if (strs.length == 1)
         return strs[0];
    Trie trie = new Trie();      
    for (int i = 1; i < strs.length ; i++) {
        trie.insert(strs[i]);
    }
    return trie.searchLongestPrefix(q);
}

class TrieNode {

    // R links to node children
    private TrieNode[] links;

    private final int R = 26;

    private boolean isEnd;

    // number of children non null links
    private int size;    
    public void put(char ch, TrieNode node) {
        links[ch -'a'] = node;
        size++;
    }

    public int getLinks() {
        return size;
    }
    //assume methods containsKey, isEnd, get, put are implemented as it is described
   //in  https://leetcode.com/articles/implement-trie-prefix-tree/)
}

public class Trie {

    private TrieNode root;

    public Trie() {
        root = new TrieNode();
    }

//assume methods insert, search, searchPrefix are implemented as it is described
//in  https://leetcode.com/articles/implement-trie-prefix-tree/)
    private String searchLongestPrefix(String word) {
        TrieNode node = root;
        StringBuilder prefix = new StringBuilder();
        for (int i = 0; i < word.length(); i++) {
            char curLetter = word.charAt(i);
            if (node.containsKey(curLetter) && (node.getLinks() == 1) && (!node.isEnd())) {
                prefix.append(curLetter);
                node = node.get(curLetter);
            }
            else
                return prefix.toString();

         }
         return prefix.toString();
    }
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture