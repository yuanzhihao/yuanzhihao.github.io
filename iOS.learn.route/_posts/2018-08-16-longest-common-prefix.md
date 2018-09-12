---
layout: post
title: "Leetcode. 14 Longest Common Prefix"
description: "给定一个字符串数组，求这些字符串的最长公共前缀。"
categories: [算法]
tags: [字符串]
redirect_from:
  - /2018/08/16/
---

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string `""`.

Example 1:

> Input: ["flower","flow","flight"]
> Output: "fl"

Example 2:

> Input: ["dog","racecar","car"]
> Output: ""
> Explanation: There is no common prefix among the input strings.

Note:

All given inputs are in lowercase letters `a-z`.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 从下标 0 开始取出第一个字符串的对应的字符，检查其他字符串该位置是否是该元素。若都是，则检查下一个位置，否则，返回当前前缀。

### 代码

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

## Leetcode 收录最优解决方案

### 变种

> Given a set of keys S = [S<sup>1</sup> , S<sup>2</sup>…S<sup>n</sup>], find the longest common prefix among a string q and S. This LCP query will be called frequently.

### 思路

> 构建 trie。

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