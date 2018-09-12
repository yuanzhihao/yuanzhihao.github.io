---
layout: post
title: "Leetcode. 190 Reverse Bits"
description: "按 bit 翻转一个 32 bit 的无符号整数。"
categories: [算法]
tags: [位操作]
redirect_from:
  - /2018/05/28/
---

> Reverse bits of a given 32 bits unsigned integer.

Example:

>     Input: 43261596
>     Output: 964176192
> 
>     Explanation: 43261596 represented in binary as 00000010100101000001111010011100,
>    
>                  return 964176192 represented in binary as 00111001011110000010100101000000.

Follow up:

If this function is called many times, how would you optimize it?

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 可以将输入看做一个元素只有0和1且长度为32的数组。从最高位开始依次判断出每位是0或1，并将结果从最低位存入结果数组。此思路反映到本题上，即是通过将1左移 i 位获得只有一位为1的32位整数，用此整数与输入进行 & 运算，判断出输入的第 i 位上是0或1，再通过无符号数右移 i 位和左移 31 - i 位，并将此结果加到最终结果上。等到对输入的每一位都完成上述操作时，即得到最终结果。 
> 
> 关于 Follow up 的思考，可以将用于和输入 & 运算的探针，保存起来，避免每次都要重新计算。


### 代码

```java
public class Solution {
    // you need treat n as an unsigned value
    public int reverseBits(int n) {
        int result = 0;
        for (int i = 0; i < 32; i++) {
            int flag = 1 << i;
            int current = flag & n;
            if (current == flag) {
                result += current >>> i << (31 - i);
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