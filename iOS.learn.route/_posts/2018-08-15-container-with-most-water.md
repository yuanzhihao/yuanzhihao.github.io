---
layout: post
title: "Leetcode. 11 Container With Most Water"
description: "给定一个数组，数组中的每个值可以视作该下标对应的高，以 0 为底，取任意两条高，求这组数组中所能形成的最大面积。"
categories: [算法]
tags: [两根指针]
redirect_from:
  - /2018/08/15/
---

Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.

[!The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.][https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg]

The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 暴力法将所有两条高的情况都进行计算取最大值，但事实上，没有必要将所有的情况都进行计算，当底的宽度减小时，如果两条高中最小的高不能变大，那么结果一定小于之前宽度较大时计算出的结果。所以，将两只分别放置在数组头尾，并计算当前面积。再将两条高中较小的那条向另一侧移动，直到新移动到的下标对应的高大于原本高，并计算面积，将其与先前计算的面积比较。循环上述过程，直到两根指针相遇。

### 代码

```java
public class Solution {
    public int maxArea(int[] height) {
        int[] volumes=new int[height.length-1];
        for(int i=0;i<height.length-1;i++)
            volumes[i]=compute(i,i+1,height[i],height[i+1]);
        int max=0;
        for(int x=0;x<volumes.length;x++) {
            int i=x;
            int j=x;
            while(i>=1&&volumes[i-1]>=volumes[x]) 
                i--;
            while(j<volumes.length-2&&volumes[j+1]>=volumes[x])
                j++;
            max=max>(j-i+1)*volumes[x]?max:(j-i+1)*volumes[x];
        }
        return max;
    }
    
    private int compute(int i,int j,int ai,int aj) {
        int volume=Math.min(ai,aj);
        return volume;
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 与我的思路相同。

### 代码

```java
public class Solution {
    public int maxArea(int[] height) {
        int maxarea = 0, l = 0, r = height.length - 1;
        while (l < r) {
            maxarea = Math.max(maxarea, Math.min(height[l], height[r]) * (r - l));
            if (height[l] < height[r])
                l++;
            else
                r--;
        }
        return maxarea;
    }
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture