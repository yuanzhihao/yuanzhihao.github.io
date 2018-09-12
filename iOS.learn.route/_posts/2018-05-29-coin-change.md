---
layout: post
title: "Leetcode. 322 Coin Change"
description: "给定一个硬币面额的数组和一个需要兑换成硬币整票，求解最少需要几个硬币。如果无法用给定的硬币破开，就返回 -1。假设每种硬币都无限量提供。"
categories: [算法]
tags: [动态规划]
redirect_from:
  - /2018/05/29/
---

> You are given coins of different denominations and a total amount of money amount. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.

Example 1:

> Input: coins = [1, 2, 5], amount = 11
>
> Output: 3 
>
> Explanation: 11 = 5 + 5 + 1

Example 2:

> Input: coins = [2], amount = 3
>
> Output: -1

Note:

You may assume that you have an infinite number of each kind of coin.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 因为不需要最终硬币的组成方案，所以排除 BFS，DFS，转而考虑采用动态规划。且每个数额的解依赖于该数额减去某个硬币面额的解。时间复杂度为 amount * coins.length()，空间复杂度为 amount。

### 代码

```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        if (amount < 0) {
            return -1;
        }
        int[] fewestCoins = new int[amount + 1];
        fewestCoins[0] = 0;
        for (int i = 1; i <= amount; i++) {
            fewestCoins[i] = -1;
        }
        for (int i = 1; i <= amount; i++) {
            int min = Integer.MAX_VALUE;
            for (int j = 0; j < coins.length; j++) {
                if (i - coins[j] >= 0 && fewestCoins[i - coins[j]] != -1) {
                    min = min < fewestCoins[i - coins[j]] + 1 ? min : fewestCoins[i - coins[j]] + 1;
                }
            }
            if (min != Integer.MAX_VALUE) {
                fewestCoins[i] = min;
            }
        }
        return fewestCoins[amount];
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 与我的思路相同。

### 代码

```java
public class Solution {
    public int coinChange(int[] coins, int amount) {
        int max = amount + 1;             
        int[] dp = new int[amount + 1];  
        Arrays.fill(dp, max);  
        dp[0] = 0;   
        for (int i = 1; i <= amount; i++) {
            for (int j = 0; j < coins.length; j++) {
                if (coins[j] <= i) {
                    dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
                }
            }
        }
        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture