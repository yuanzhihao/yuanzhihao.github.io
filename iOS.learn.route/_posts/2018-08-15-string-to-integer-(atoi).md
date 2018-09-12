---
layout: post
title: "Leetcode. 8 String to Integer (atoi)"
description: "实现一个将字符串转化为整数的 atoi 函数。"
categories: [算法]
tags: [字符串]
redirect_from:
  - /2018/08/15/
---

Implement atoi which converts a string to an integer.

The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.

The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.

If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.

If no valid conversion could be performed, a zero value is returned.

Note:

Only the space character `' '` is considered as whitespace character.
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2<sup>31</sup>,  2<sup>31</sup> − 1]. If the numerical value is out of the range of representable values, INT_MAX (2<sup>31</sup> − 1) or INT_MIN (−2<sup>31</sup>) is returned.

Example 1:

> Input: "42"
> Output: 42

Example 2:

> Input: "   -42"
> Output: -42
> Explanation: The first non-whitespace character is '-', which is the minus sign.
             Then take as many numerical digits as possible, which gets 42.

Example 3:

> Input: "4193 with words"
> Output: 4193
> Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.

Example 4:

> Input: "words and 987"
> Output: 0
> Explanation: The first non-whitespace character is 'w', which is not a numerical 
             digit or a +/- sign. Therefore no valid conversion could be performed.

Example 5:

> Input: "-91283472332"
> Output: -2147483648
> Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.
             Thefore INT_MIN (−231) is returned.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 从头到尾按顺序遍历字符串，设置符号字符变量，用来表示是否已经访问到了第一个有效的字符（数字或者正负号），并存储数字的正负号。在符号字符为空时，碰到空格就跳过，碰到除空格外的非有效字符就停止遍历，返回 0 。当碰到第一个有效字符，根据字符类别，设置符号变量。符号变量设置完后，碰到任何非数字字符就返回当前结果；碰到数字则将当前结果乘 10 再加上 当前数字乘符号变量对应的正负 1。注意防止溢出。

### 代码

```java
class Solution {
    public int myAtoi(String str) {
        Character sign = null;
        int i = 0, result = 0;
        while (i < str.length()) {
            char currentChar = str.charAt(i);
            if (sign == null && currentChar == ' ') {
                i++;
                continue;
            } else if (sign != null && !Character.isDigit(currentChar)) {
                return result;
            } else if (sign == null && currentChar != ' ' && currentChar != '-' && currentChar != '+' && !Character.isDigit(currentChar)) {
                return result;
            } else {
                if (sign == null) {
                    if (currentChar == '+' || Character.isDigit(currentChar)) {
                        sign = '+';
                    } else {
                        sign = '-';
                    }
                }
                if (Character.isDigit(currentChar)) {
                    int digit = Character.digit(currentChar, 10);
                    if (sign.charValue() == '-') {
                        digit = 0 - digit;
                    }
                    int temp = result * 10 + digit;
                    if (temp / 10 != result) {
                        return sign.charValue() == '-' ? Integer.MIN_VALUE : Integer.MAX_VALUE;
                    }
                    result = temp;
                }
                i++;
            }
        }
        return result;
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