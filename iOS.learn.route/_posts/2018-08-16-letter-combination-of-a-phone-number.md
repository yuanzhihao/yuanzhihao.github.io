---
layout: post
title: "Leetcode. 17 Letter Combinations of a Phone Number"
description: "给定一个个位数数组表示九宫格键盘点击顺序，求可能产生的字符组合。"
categories: [算法]
tags: [BFS,DFS]
redirect_from:
  - /2018/08/16/
---

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

[!Keyboard][http://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png]

Example:

> Input: "23"
> Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].

Note:

Although the above answer is in lexicographical order, your answer could be in any order you want.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> DFS 生成所有可能结果。

### 代码

```swift
class Solution {
    
    var dic: [Character: String] = [:]
    
    func letterCombinations(_ digits: String) -> [String] {
        var result: [String] = []
        if digits.isEmpty {
            return result
        }
        dic[Character("1")] = "1"
        dic[Character("2")] = "abc"
        dic[Character("3")] = "def"
        dic[Character("4")] = "ghi"
        dic[Character("5")] = "jkl"
        dic[Character("6")] = "mno"
        dic[Character("7")] = "pqrs"
        dic[Character("8")] = "tuv"
        dic[Character("9")] = "wxyz"
        DFS(digits, 0, "", &result)
        return result
    }
    
    func DFS(_ s:String, _ index: Int, _ pre: String, _ result: inout [String]) {
        if index == s.characters.count {
            result.append(pre)
            return
        }
        let candidate = dic[s[s.index(s.startIndex, offsetBy: index)]]!
        for i in 0..<candidate.characters.count {
            var temp = pre
            temp.append(candidate[candidate.index(candidate.startIndex, offsetBy: i)])
            DFS(s, index + 1, temp, &result)
        }
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