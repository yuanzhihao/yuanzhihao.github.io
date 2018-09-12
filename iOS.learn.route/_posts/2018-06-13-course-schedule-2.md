---
layout: post
title: "Leetcode. 210 Course Schedule II"
description: "给定一组课程，有的课程之间存在依赖关系，求一种上课顺序。"
categories: [算法]
tags: [拓扑排序]
redirect_from:
  - /2018/06/05/
---

> There are a total of n courses you have to take, labeled from 0 to n-1.
> 
> Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]
>
> Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.
>
> There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

Example 1:

> Input: 2, [[1,0]] 
>
> Output: [0,1]
>
> Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1] .

Example 2:

> Input: 4, [[1,0],[2,0],[3,1],[3,2]]
>
> Output: [0,1,2,3] or [0,2,1,3]
>
> Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0. So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3] .

Note:

1. The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.

2. You may assume that there are no duplicate edges in the input prerequisites.

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 先上所有无依赖的课程，再将只依赖刚刚上的课程的课程视作无依赖的课程，重复该步骤。直到没有剩下的课程，即最终有效解，或者找不到无依赖的课程且还有课程未上，即无有效解。

### 代码

```java
class Solution {
    public int[] findOrder(int numCourses, int[][] prerequisites) {
        HashSet<Integer> involvedCourese = new HashSet<Integer>();
        HashMap<Integer, Set<Integer>> dependMap = new HashMap<Integer, Set<Integer>>();
        HashMap<Integer, List<Integer>> dependedMap = new HashMap<Integer, List<Integer>>();
        for (int i = 0; i < prerequisites.length; i++) {
            involvedCourese.add(prerequisites[i][0]);
            involvedCourese.add(prerequisites[i][1]);
            if (!dependMap.containsKey(prerequisites[i][0])) {
                dependMap.put(prerequisites[i][0], new HashSet<Integer>());
            }
            dependMap.get(prerequisites[i][0]).add(prerequisites[i][1]);
            if (!dependedMap.containsKey(prerequisites[i][1])) {
                dependedMap.put(prerequisites[i][1], new LinkedList<Integer>());
            }
            dependedMap.get(prerequisites[i][1]).add(prerequisites[i][0]);
        }
        LinkedList<Integer> list = new LinkedList<Integer>();
        Boolean isExistCycle = topologicalSort(transfer(involvedCourese.toArray()), new HashSet<Integer>(), dependMap, dependedMap, list);
        if (!isExistCycle) {
            return new int[0];
        }
        int i = 0;
        int[] result = new int[numCourses];
        Object[] temp = list.toArray();
        for (int j = 0; j < temp.length; j++) {
            result[i++] = (int)temp[j];
        }
        for (int j = 0; j < numCourses; j++) {
            if (!involvedCourese.contains(j)) {
                result[i++] = j;
            }
        }
        return result;
    }
    
    private Boolean topologicalSort(int[] prerequisites, HashSet<Integer> takenCourse, HashMap<Integer, Set<Integer>> dependMap, HashMap<Integer, List<Integer>> dependedMap, LinkedList<Integer> list) {
        for (int i = 0; i < prerequisites.length; i++) {
            if (!takenCourse.contains(prerequisites[i]) && (!dependMap.containsKey(prerequisites[i]) || dependMap.get(prerequisites[i]).isEmpty())) {
                takenCourse.add(prerequisites[i]);
                list.add(prerequisites[i]);
                List<Integer> l = dependedMap.get(prerequisites[i]);
                if (l != null) {
                    for (Integer j : l) {
                        dependMap.get(j).remove(prerequisites[i]);
                    }
                }
                return topologicalSort(prerequisites, takenCourse, dependMap, dependedMap, list);
            }
        }
        if (takenCourse.size() == prerequisites.length) {
            return true;
        }
        return false;
    }
    
    private int[] transfer(Object[] array) {
        int[] r = new int[array.length];
        for (int i = 0; i < array.length; i++) {
            r[i] = (int)array[i];
        }
        return r;
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