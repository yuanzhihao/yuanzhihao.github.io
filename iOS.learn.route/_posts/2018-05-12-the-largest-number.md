---
layout: post
title: "Leetcode. 179 Largest Number"
description: "给定一个非负整数数组，求这些整数能排列出的最大数。"
categories: [算法]
tags: [快速排序]
redirect_from:
  - /2018/05/12/
---

> Given a list of non negative integers, arrange them such that they form the largest number.

Example 1:

> Input: [10,2]

> Output: "210"

Example 2:

> Input: [3,30,34,5,9]

> Output: "9534330"

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 将数组按“递减”顺序排序。数字比较的规则为：将两个数字变为字符串，同时从头到尾进行遍历，若一方先到字符串尾，则从头再次开始遍历，直到双方同时到字符串尾。对于遍历的每对数字，比较它们的大小，一旦分出大小，就根据该结果决定两个字符串（即原数字的大小)，如果同时到字符串尾后也未能区分大小，则判定两者相等。排序完成后拼接所有数字，即为最终解。时间复杂度：排序本身要进行 nlog(n) 次比较，每次比较需进行两个字符串最小公倍数次的比较，假定为 m，所以时间复杂度为 mnlog(n)。

### 代码

#### 递归写法

```swift
class Solution {
    public String largestNumber(int[] nums) {
        StringBuilder sb = new StringBuilder();
        decreaseSort(nums);
        for (int num : nums) {
            sb.append(num);
        }
        return sb.toString().charAt(0) == '0' ? "0" : sb.toString();
    }
    
    private void decreaseSort(int[] nums) {
        decreaseSort(nums, 0, nums.length - 1);
    }
    
    private void decreaseSort(int[] nums, int left, int right) {
        if (left < right) {
            int pivot = nums[left];
            int l = left, r = right;
            while (left < right) {
                while (left < right && compare(nums[right], pivot) <= 0) {
                    right--;
                }
                if (left < right) {
                    nums[left++] = nums[right];
                }
                while (left < right && compare(nums[left], pivot) > 0) {
                    left++;
                }
                if (left < right) {
                    nums[right--] = nums[left];
                }
            }
            nums[left] = pivot;
            decreaseSort(nums, l, left - 1);
            decreaseSort(nums, left + 1, r);
        }
    }
    
    private int compare(int firstNum, int secondNum) {
        String firstStr = String.valueOf(firstNum);
        String secondStr = String.valueOf(secondNum);
        int i = 0, j = 0;
        while (i % firstStr.length() != 0 || j % secondStr.length() != 0 || i == 0) {
            char firstChar = firstStr.charAt(i % firstStr.length());
            char secondChar = secondStr.charAt(j % secondStr.length());
            if (firstChar > secondChar) {
                return 1;
            }
            if (firstChar < secondChar) {
                return -1;
            }
            i++;
            j++;
        }
        return 0;
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 原理同我的解法基本一致。比较数字时，直接比较两者按照不同顺序拼接后的字符串的大小，本质相同。

### 代码

```java
class Solution {
    private class LargerNumberComparator implements Comparator<String> {
        @Override
        public int compare(String a, String b) {
            String order1 = a + b;
            String order2 = b + a;
           return order2.compareTo(order1);
        }
    }

    public String largestNumber(int[] nums) {
        // Get input integers as strings.
        String[] asStrs = new String[nums.length];
        for (int i = 0; i < nums.length; i++) {
            asStrs[i] = String.valueOf(nums[i]);
        }

        // Sort strings according to custom comparator.
        Arrays.sort(asStrs, new LargerNumberComparator());

        // If, after being sorted, the largest number is `0`, the entire number
        // is zero.
        if (asStrs[0].equals("0")) {
            return "0";
        }

        // Build largest number from sorted array.
        String largestNumberStr = new String();
        for (String numAsStr : asStrs) {
            largestNumberStr += numAsStr;
        }

        return largestNumberStr;
    }
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture