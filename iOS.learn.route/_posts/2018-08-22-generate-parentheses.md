---
layout: post
title: "Leetcode. 22 Generate Parentheses"
description: "给定 n 对小括号，求所有有效的排列。"
categories: [算法]
tags: [BFS,DFS]
redirect_from:
  - /2018/08/22/
---

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:

>     [
>       "((()))",
>       "(()())",
>       "(())()",
>       "()(())",
>       "()()()"
>     ]

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> BFS/DFS 遍历所有的给定对数的小括号排列，但在遍历过程中排除当前时刻左括号少于右括号的情况和左括号多于给定对数的情况。当深度 = n * 2 时，将当前排列添加到结果中。

### 代码

```swift
class Solution {
    class Node {
        var data: Character
        
        var leftParenthesis: UInt
        
        var rightParenthesis: UInt
        
        init(_ data: Character, _ leftParenthesis: UInt, _ rightParenthesis: UInt) {
            self.data = data
            self.leftParenthesis = leftParenthesis
            self.rightParenthesis = rightParenthesis
        }
    }
    
    var result:[String] = []
    
    func generateParenthesis(_ n: Int) -> [String] {
        if n == 0 {
            return result
        }
        let root = Node(Character("("), 1, 0)
        DFS(root, 1, n, "")
        return result
    }
    
    private func DFS(_ node: Node, _ level: Int, _ n: Int, _ s: String) {
        var temp = s
        temp.append(node.data)
        if level == n * 2 {
            result.append(temp)
            return
        }
        if node.leftParenthesis > node.rightParenthesis {
            DFS(Node(Character(")"), node.leftParenthesis, node.rightParenthesis + 1), level + 1, n, temp)
        }
        if node.leftParenthesis < n {
            DFS(Node(Character("("), node.leftParenthesis + 1, node.rightParenthesis), level + 1, n, temp)
        }
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 和我的思路相同。

### 代码

```java
class Solution {
    public List<String> generateParenthesis(int n) {
        List<String> ans = new ArrayList();
        backtrack(ans, "", 0, 0, n);
        return ans;
    }

    public void backtrack(List<String> ans, String cur, int open, int close, int max){
        if (str.length() == max * 2) {
            ans.add(cur);
            return;
        }

        if (open < max)
            backtrack(ans, cur+"(", open+1, close, max);
        if (close < open)
            backtrack(ans, cur+")", open, close+1, max);
    }
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture