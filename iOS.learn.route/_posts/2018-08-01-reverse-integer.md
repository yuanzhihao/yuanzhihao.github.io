---
layout: post
title: "Leetcode. 7 Reverse Integer"
description: "给定一个 32-bit 有符号整数，翻转该整数。"
tags: [溢出]
redirect_from:
  - /2018/08/01/
---

Given a 32-bit signed integer, reverse digits of an integer.

Example 1:

> Input: 123
> Output: 321

Example 2:

> Input: -123
> Output: -321

Example 3:

> Input: 120
> Output: 21

Note:

Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 从整数个位开始依次取出每一位上的数字，将之前计算的结果（起始为 0）乘 10 再加上刚刚取出的数字。将得到的结果减去刚刚取出的数字并除以 10 ，再将这个值与之前计算的结果比较，如果相等则没有溢出可以继续上述操作。否则返回 0。

### 代码

```java
class Solution {
    public int reverse(int x) {
        int result = 0;
        while (x != 0) {
            int point = x % 10;
            int remain = x / 10;
            int temp = result * 10 + point;
            if ((temp - point) / 10 != result) {
                return 0;
            }
            result = temp;
            x = remain;
        }
        return result;
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 与我的思路类似，但检查溢出的手段变为：如果输入为正数，检查当前翻转的结果是否大于 Integer.MAX_VALUE / 10 或者是否等于 Integer.MAX_VALUE / 10 且 当前位上的数字大于 7。负数同理。

### 代码

```java
class Solution {
    public int reverse(int x) {
        int rev = 0;
        while (x != 0) {
            int pop = x % 10;
            x /= 10;
            if (rev > Integer.MAX_VALUE/10 || (rev == Integer.MAX_VALUE / 10 && pop > 7)) return 0;
            if (rev < Integer.MIN_VALUE/10 || (rev == Integer.MIN_VALUE / 10 && pop < -8)) return 0;
            rev = rev * 10 + pop;
        }
        return rev;
    }
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture