---
layout: post
title: "Leetcode. 56 Merge Intervals"
description: "给定一组区间，合并所有有交集的区间。"
categories: [算法]
tags: [数组,排序]
redirect_from:
  - /2018/09/08/
---

Given a collection of intervals, merge all overlapping intervals.

Example 1:

> Input: [[1,3],[2,6],[8,10],[15,18]]
> Output: [[1,6],[8,10],[15,18]]
> Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].

Example 2:

> Input: [[1,4],[4,5]]
> Output: [[1,5]]
> Explanation: Intervals [1,4] and [4,5] are considerred overlapping.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 拓展区间的数据类型，实现可比较和可相等的接口。如果区间首尾都相同，则区间相等。如果区间首相同，则按区间尾的大小决定区间的大小。如果区间首不同，则按区间首的大小决定区间的大小。按这样的大小规则，排序给定的区间数组。从头开始遍历已排序的区间数组，所有相邻的区间都存在合并的可能，遍历时进行检查，将合并后的区间加入结果数组。

### 代码

```swift
/**
 * Definition for an interval.
 * public class Interval {
 *   public var start: Int
 *   public var end: Int
 *   public init(_ start: Int, _ end: Int) {
 *     self.start = start
 *     self.end = end
 *   }
 * }
 */
extension Interval: Comparable, Equatable {
    public static func < (l: Interval, r: Interval) -> Bool {
        if l.start == r.start && l.end != r.end {
            return l.end < r.end
        } else {
            return l.start < r.start
        }
    }
    
    public static func == (l: Interval, r: Interval) -> Bool {
        return l.start == r.start && l.end == r.end
    }
}

class Solution {
    func merge(_ intervals: [Interval]) -> [Interval] {
        let arr = intervals.sorted()
        var results: [Interval] = []
        var maxEnd = Int.min
        var i = 0
        while i < arr.count {
            maxEnd = arr[i].end
            var j = i + 1
            while j < arr.count && maxEnd >= arr[j].start {
                maxEnd = maxEnd >= arr[j].end ? maxEnd : arr[j].end
                j += 1
            }
            results.append(Interval(arr[i].start, maxEnd))
            if j == arr.count {
                return results
            }
            i = j
        }
        return results
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 与我的思路类似，但区间的大小只由区间首决定。

### 代码

```java
class Solution {
    private class IntervalComparator implements Comparator<Interval> {
        @Override
        public int compare(Interval a, Interval b) {
            return a.start < b.start ? -1 : a.start == b.start ? 0 : 1;
        }
    }

    public List<Interval> merge(List<Interval> intervals) {
        Collections.sort(intervals, new IntervalComparator());

        LinkedList<Interval> merged = new LinkedList<Interval>();
        for (Interval interval : intervals) {
            // if the list of merged intervals is empty or if the current
            // interval does not overlap with the previous, simply append it.
            if (merged.isEmpty() || merged.getLast().end < interval.start) {
                merged.add(interval);
            }
            // otherwise, there is overlap, so we merge the current and previous
            // intervals.
            else {
                merged.getLast().end = Math.max(merged.getLast().end, interval.end);
            }
        }

        return merged;
    }
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture