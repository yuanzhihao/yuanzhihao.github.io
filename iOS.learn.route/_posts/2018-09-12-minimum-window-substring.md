---
layout: post
title: "Leetcode. 76 Minimum Window Substring"
description: "给定两个字符串 S, T，在 S 中找出包含 T 所有字符的最短子串。"
categories: [算法]
tags: [滑动窗口]
redirect_from:
  - /2018/09/12/
---

Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).

Example:

> Input: S = "ADOBECODEBANC", T = "ABC"
> Output: "BANC"

Note:

* If there is no such window in S that covers all characters in T, return the empty string `""`.
* If there is such window, you are guaranteed that there will always be only one unique minimum window in S.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 滑动窗口，需要两个指针。初始化时，左右指针都位于字符串头，先移动右指针直到左指针到右指针这一段包含了 T 中的所有字符，再将左指针向右移动直到两个指针之间不再全部包含 T 中的字符，记下当前的长度和字符串。并再次开始移动右指针，重复上述过程，每次只保存长度最小的字符串。当右指针访问到字符串尾时，上述过程结束，当前保存的字符串就是最终结果。

### 代码

```java
public class Solution {
    public String minWindow(String s, String t) {
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        for (int i = 0; i < t.length(); i++) {
            if (map.containsKey(t.charAt(i))) {
                map.put(t.charAt(i), map.get(t.charAt(i)) + 1);
            } else {
                map.put(t.charAt(i), 1);
            }
        }
        int length = Integer.MAX_VALUE;
        int start = 0, end = 0, counter = t.length(), head = 0;
        while (end < s.length()) {
            if (map.containsKey(s.charAt(end))) {
                if (map.get(s.charAt(end)) > 0) {
                    counter--;
                }
                map.put(s.charAt(end), map.get(s.charAt(end)) - 1);
            }
            end++;
            while (counter == 0) {
                int temp = end - start;
                if (temp < length) {
                    length = temp;
                    head = start;
                }
                if (map.containsKey(s.charAt(start))) {
                    map.put(s.charAt(start), map.get(s.charAt(start)) + 1);
                    if (map.get(s.charAt(start)) > 0) {
                        counter++;
                    }
                }
                start++;
            }
        }
        return length == Integer.MAX_VALUE ? "" : s.substring(head, head + length);
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 与我的思路相似，但是会对 S 进行预处理，过滤掉所有没有在 T 中出现的字符，只保留出现在 T 中的字符和这些字符在 S 中对应的下标。在之后沿用我的思路。

### 代码

```java
import javafx.util.Pair;

class Solution {
    public String minWindow(String s, String t) {

        if (s.length() == 0 || t.length() == 0) {
            return "";
        }

        Map<Character, Integer> dictT = new HashMap<Character, Integer>();

        for (int i = 0; i < t.length(); i++) {
            int count = dictT.getOrDefault(t.charAt(i), 0);
            dictT.put(t.charAt(i), count + 1);
        }

        int required = dictT.size();

        // Filter all the characters from s into a new list along with their index.
        // The filtering criteria is that the character should be present in t.
        List<Pair<Integer, Character>> filteredS = new ArrayList<Pair<Integer, Character>>();
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (dictT.containsKey(c)) {
                filteredS.add(new Pair<Integer, Character>(i, c));
            }
        }

        int l = 0, r = 0, formed = 0;
        Map<Character, Integer> windowCounts = new HashMap<Character, Integer>();  
        int[] ans = {-1, 0, 0};

        // Look for the characters only in the filtered list instead of entire s.
        // This helps to reduce our search.
        // Hence, we follow the sliding window approach on as small list.
        while (r < filteredS.size()) {
            char c = filteredS.get(r).getValue();
            int count = windowCounts.getOrDefault(c, 0);
            windowCounts.put(c, count + 1);

            if (dictT.containsKey(c) && windowCounts.get(c).intValue() == dictT.get(c).intValue()) {
                formed++;
            }

            // Try and contract the window till the point where it ceases to be 'desirable'.
            while (l <= r && formed == required) {
                c = filteredS.get(l).getValue();

                // Save the smallest window until now.
                int end = filteredS.get(r).getKey();
                int start = filteredS.get(l).getKey();
                if (ans[0] == -1 || end - start + 1 < ans[0]) {
                    ans[0] = end - start + 1;
                    ans[1] = start;
                    ans[2] = end;
                }

                windowCounts.put(c, windowCounts.get(c) - 1);
                if (dictT.containsKey(c) && windowCounts.get(c).intValue() < dictT.get(c).intValue()) {
                    formed--;
                }
                l++;
            }
            r++;   
        }
        return ans[0] == -1 ? "" : s.substring(ans[1], ans[2] + 1);
    }
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture