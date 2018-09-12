---
layout: post
title: "Leetcode. 48 Rotate Image"
description: "给定一个 n 维矩阵代表图片，将这个图片顺时针旋转 90 度。"
categories: [算法]
tags: [数组,递归]
redirect_from:
  - /2018/08/27/
---

You are given an n x n 2D matrix representing an image.

Rotate the image by 90 degrees (clockwise).

Note:

You have to rotate the image [!in-place][https://en.wikipedia.org/wiki/In-place_algorithm], which means you have to modify the input 2D matrix directly. DO NOT allocate another 2D matrix and do the rotation.

Example 1:

> Given input matrix = 
>     [
>      [1,2,3],
>      [4,5,6],
>      [7,8,9]
>     ],
> 
> rotate the input matrix in-place such that it becomes:
>     [
>      [7,4,1],
>      [8,5,2],
>      [9,6,3]
>     ]

Example 2:

> Given input matrix =
>     [
>      [ 5, 1, 9,11],
>      [ 2, 4, 8,10],
>      [13, 3, 6, 7],
>      [15,14,12,16]
>     ], 
>
> rotate the input matrix in-place such that it becomes:
>     [
>      [15,13, 2, 5],
>      [14, 3, 4, 1],
>      [12, 6, 8, 9],
>      [16, 7,10,11]
>     ]

* Kramdown table of contents
{:toc .toc}

# 解决方案

## 我的解决方案

### 思路

> 一层一层旋转 90 度。

### 代码

```swift
class Solution {
    func rotate(_ matrix: inout [[Int]]) {
        var layer = 0
        if matrix.count == 0 {
            return
        }
        if matrix.count == 1 {
            return
        }
        if matrix.count == 2 {
            rotateLayer(&matrix, layer)
            return
        }
        while matrix.count / (layer + 1) >= 2 {
            rotateLayer(&matrix, layer)
            layer += 1
        }
    }
    
    func rotateLayer(_ matrix: inout [[Int]], _ layer: Int) {
        let topLeft = matrix[layer][layer]
        let topRight = matrix[layer][matrix.count - 1 - layer]
        let bottomLeft = matrix[matrix.count - 1 - layer][layer]
        let bottomRight = matrix[matrix.count - 1 - layer][matrix.count - 1 - layer]
        if layer + 1 < matrix.count - 1 - layer {
            for index in layer + 1..<matrix.count - 1 - layer {
                matrix[layer][index] ^= matrix[index][matrix.count - 1 - layer]
                matrix[index][matrix.count - 1 - layer] ^= matrix[layer][index]
                matrix[layer][index] ^= matrix[index][matrix.count - 1 - layer]
                matrix[layer][index] ^= matrix[matrix.count - 1 - layer][matrix.count - 1 - index]
                matrix[matrix.count - 1 - layer][matrix.count - 1 - index] ^= matrix[layer][index]
                matrix[layer][index] ^= matrix[matrix.count - 1 - layer][matrix.count - 1 - index]
                matrix[layer][index] ^= matrix[matrix.count - 1 - index][layer]
                matrix[matrix.count - 1 - index][layer] ^= matrix[layer][index]
                matrix[layer][index] ^= matrix[matrix.count - 1 - index][layer]
            }
        }
        matrix[layer][matrix.count - 1 - layer] = topLeft
        matrix[matrix.count - 1 - layer][matrix.count - 1 - layer] = topRight
        matrix[matrix.count - 1 - layer][layer] = bottomRight
        matrix[layer][layer] = bottomLeft
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