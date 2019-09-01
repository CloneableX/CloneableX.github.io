---
title: leetcode 题号976 Largest Perimeter Triangle
categories:
  - leetcode解题
  - sort
tags:
  - leetcode
p: leetcode\sort\easy\leetcode题号976_LargestPerimeterTriangle
date: 2019-08-31 14:10:40
mathjax: true
---

查看题目详情可[点击此处](https://leetcode.com/problems/largest-perimeter-triangle/)。

## 题目

Given an array A of positive lengths, return the largest perimeter of a triangle with non-zero area, formed from 3 of these lengths.

If it is impossible to form any triangle of non-zero area, return 0.

**Example 1:**

> Input: [2,1,2]
Output: 5

**Example 2:**

> Input: [1,2,1]
Output: 0

**Example 3:**

> Input: [3,2,3,4]
Output: 10

**Example 4:**

> Input: [3,6,2,3]
Output: 8
 

**Note:**
> 3 <= A.length <= 10000
1 <= A[i] <= 10^6

## 解题思路

题目需要在一组整数中找到三个能组成最大边长三角形的数字，所以最后找到的三个数字需要满足两个条件，首先是能够组成三角形，第二是能够组成的三角形中边长最大的。先讲一下第一点，三个数字是能够组成三角形必须满足两条最小边相加大于最大边，在此基础上比较组成三角形的大小就可以满足最大边长的条件。

如果通过遍历的方式求解，仅仅 100 个数字以 3 个一组组成就有 161700 种可能，也就是说需要三层嵌套循环，粗略估算时间复杂度为 $O(n^3)$，所以我们需要思考下其他的方法。既然是最大边长三角形，那三条边是肯定是尽量选取一组数据中最大的三个组成，那最大的三条边可能满足组成三角形的条件，这三条边组成的三角形便是最大边长的，如果这三个最大数据无法组成三角形，说明三个数据中的最大数不合适，我们需要舍弃最大的一个数据，从剩下的数据里取三个最大数据重复上述逻辑，当然为了方便地依次获取最大三个数，我们可以先将数据排序。

## 代码实现

```java
private static int TRIANGLE_ARRAY_LEN = 3;

// by quick sort
public static int largestPerimeterQsort(int[] A) {
    Arrays.sort(A);

    for (int i = A.length - 1; i > 1 ; i--) {
        if(A[i] < A[i - 1] + A[i - 2]) {
            return A[i] + A[i - 1] + A[i - 2];
        }
    }

    return 0;
}

// by choose sort
public static int largestPerimeterChooseSort(int[] A) {
    int[] T = new int[TRIANGLE_ARRAY_LEN];
    int k = 0;
    for (int i = 0; i < A.length; i++) {
        for (int j = i + 1; j < A.length; j++) {
            if(A[i] < A[j]) {
                int temp = A[i];
                A[i] = A[j];
                A[j] = temp;
            }
        }

        T[k++] = A[i];
        k %= TRIANGLE_ARRAY_LEN;
        if(i > 1 && T[(k + 1) % TRIANGLE_ARRAY_LEN] + T[(k + 2) % TRIANGLE_ARRAY_LEN] > T[k]) {
            return T[(k + 1) % TRIANGLE_ARRAY_LEN] + T[(k + 2) % TRIANGLE_ARRAY_LEN] + T[k];
        }
    }

    return 0;
}
```

****
> **代码详情可点击查看 [我的 GitHub 仓库](https://github.com/CloneableX/leetcode/)**