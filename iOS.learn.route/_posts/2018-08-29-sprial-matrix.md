---
layout: post
title: "Leetcode. 54 Spiral Matrix"
description: "给定一个 m * n 的矩阵，按照螺旋顺序打印该矩阵。"
categories: [算法]
tags: [递归,数组]
redirect_from:
  - /2018/08/29/
---

Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.

Example 1:

> Input:
>     [
>      [ 1, 2, 3 ],
>      [ 4, 5, 6 ],
>      [ 7, 8, 9 ]
>     ]
> Output: [1,2,3,6,9,8,7,4,5]

Example 2:

Input:
>     [
>      [1, 2, 3, 4],
>      [5, 6, 7, 8],
>      [9,10,11,12]
>     ]
> Output: [1,2,3,4,8,12,11,10,9,5,6,7]

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 按题意直接迭代螺旋遍历矩阵。

### 代码

```swift
class Solution {
    func spiralOrder(_ matrix: [[Int]]) -> [Int] {
        if matrix.count == 0 {
            return []
        }
        if matrix.count == 1 {
            return matrix[0]
        }
        var result: [Int] = []
        if matrix[0].count == 1 {
            var i = 0
            while i < matrix.count {
                result.append(matrix[i][0])
                i += 1
            }
            return result
        }
        var isAccessed = Array<[Bool]>(repeating: Array<Bool>(repeating: false, count: matrix[0].count), count: matrix.count)
        var count = matrix.count * matrix[0].count
        // 0 means top right, 1 means bottom right, 2 means bottom left, 3 means top left
        var i = 0, j = 0, direction = 0
        isAccessed[i][j] = true
        count -= 1
        result.append(matrix[i][j])
        while count != 0 {
            if direction == 0 {
                if j + 1 < matrix[0].count && !isAccessed[i][j + 1] {
                    j += 1
                    result.append(matrix[i][j])
                    isAccessed[i][j] = true
                }
                else {
                    i += 1
                    result.append(matrix[i][j])
                    isAccessed[i][j] = true
                    direction = 1
                }
            }
            else if direction == 1 {
                if i + 1 < matrix.count && !isAccessed[i + 1][j] {
                    i += 1
                    result.append(matrix[i][j])
                    isAccessed[i][j] = true
                }
                else {
                    j -= 1
                    result.append(matrix[i][j])
                    isAccessed[i][j] = true
                    direction = 2
                }
            }
            else if direction == 2 {
                if j - 1 > -1 && !isAccessed[i][j - 1] {
                    j -= 1
                    result.append(matrix[i][j])
                    isAccessed[i][j] = true
                }
                else {
                    i -= 1
                    result.append(matrix[i][j])
                    isAccessed[i][j] = true
                    direction = 3
                }
            }
            else if direction == 3 {
                if i - 1 > -1 && !isAccessed[i - 1][j] {
                    i -= 1
                    result.append(matrix[i][j])
                    isAccessed[i][j] = true
                }
                else {
                    j += 1
                    result.append(matrix[i][j])
                    isAccessed[i][j] = true
                    direction = 0
                }
            }
            count -= 1
        }
        return result
    }
}
```

## Leetcode 收录最优解决方案

### 思路

> 两种思路，一种与我的思路相同，另一种只考虑按层遍历。此处代码是第二种，优点是空间少用一半相比于第一种。

### 代码

```java
class Solution {
    public List < Integer > spiralOrder(int[][] matrix) {
        List ans = new ArrayList();
        if (matrix.length == 0)
            return ans;
        int r1 = 0, r2 = matrix.length - 1;
        int c1 = 0, c2 = matrix[0].length - 1;
        while (r1 <= r2 && c1 <= c2) {
            for (int c = c1; c <= c2; c++) ans.add(matrix[r1][c]);
            for (int r = r1 + 1; r <= r2; r++) ans.add(matrix[r][c2]);
            if (r1 < r2 && c1 < c2) {
                for (int c = c2 - 1; c > c1; c--) ans.add(matrix[r2][c]);
                for (int r = r2; r > r1; r--) ans.add(matrix[r][c1]);
            }
            r1++;
            r2--;
            c1++;
            c2--;
        }
        return ans;
    }
}
```

[^1]: This is a footnote.

[kramdown]: https://kramdown.gettalong.org/
[Simple Texture]: https://github.com/yizeng/jekyll-theme-simple-texture