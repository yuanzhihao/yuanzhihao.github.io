---
layout: post
title: "Leetcode. 29 Divide Two Integers"
description: "给定两个有符号 32 位整数，求两数商。"
categories: [算法]
tags: [位运算,二分查找]
redirect_from:
  - /2018/08/25/
---

Given two integers dividend and divisor, divide two integers without using multiplication, division and mod operator.

Return the quotient after dividing dividend by divisor.

The integer division should truncate toward zero.

Example 1:

> Input: dividend = 10, divisor = 3
> Output: 3

Example 2:

> Input: dividend = 7, divisor = -3
> Output: -2

Note:

* Both dividend and divisor will be 32-bit signed integers.
* The divisor will never be 0.
* Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 231 − 1 when the division result overflows.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 因为最小负整数除以负一会溢出，所以判断该情况直接返回最大正整数；对于除数为一或负一时，因为实际运算时，会将除数、被除数都转化为负数，而负一在右移一位的运算中恒等于负一（一般认为右移运算等同于除以二），会造成死循环，所以判断该情况并返回相应数值。对于其它一般情况，将除数、被除数都转化成负数后，将除数不断左移一位直到除数小于等于被除数或者除数小于二的三十次幂（因为任何大于二的三十次幂的数乘二都会溢出）并统计该数值相当于原始负数化的除数 \* count，然后开始判断当前被除数是否小于等于除数，如果是，将被除数减除数并赋给被除数，同时将当前对应的 count 加到结果商中（初始值为 0），否则不做任何事，再接着将除数右移一位，count 同样右移一位，直到当前除数大于原始负数化的除数。

### 代码

```java
class Solution {
    public int divide(int dividend, int divisor) {
        if (dividend == Integer.MIN_VALUE && divisor == -1) {
            return Integer.MAX_VALUE;
        } else if (divisor == 1) {
            return dividend;
        } else if (divisor == -1) {
            return -dividend;
        }
        return bitOperatorDivide(dividend, divisor);
    }
    
    private int bitOperatorDivide(int dividend, int divisor) {
        int d1 = dividend > 0 ? -dividend : dividend;
        int d2 = divisor > 0 ? -divisor : divisor;
        int copy = d2;
        int limit = (int)-Math.pow(2, 30);
        int count = 1, result = 0;;
        while (d2 > d1 && d2 >= limit) {
            d2 = d2 << 1;
            count = count << 1;
        }
        while (d2 <= copy) {
            if (d1 <= d2) {
                d1 -= d2;
                result += count;
            }
            d2 = d2 >> 1;
            count = count >> 1;
        }
        return (dividend > 0 && divisor < 0) || (dividend < 0 && divisor > 0) ? -result : result;
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