---
title: leetcode 题号1030 Matrix Cells in Distance Order
categories:
  - leetcode解题
  - sort
tags:
  - leetcode
p: leetcode\sort\easy\leetcode题号1030_MatrixCellsinDistanceOrder
date: 2019-08-31 14:48:45
---

查看题目详情可[点击此处](https://leetcode.com/problems/matrix-cells-in-distance-order/)。

## 题目

We are given a matrix with R rows and C columns has cells with integer coordinates (r, c), where 0 <= r < R and 0 <= c < C.

Additionally, we are given a cell in that matrix with coordinates (r0, c0).

Return the coordinates of all cells in the matrix, sorted by their distance from (r0, c0) from smallest distance to largest distance.  Here, the distance between two cells (r1, c1) and (r2, c2) is the Manhattan distance, |r1 - r2| + |c1 - c2|.  (You may return the answer in any order that satisfies this condition.)

**Example 1:**

> Input: R = 1, C = 2, r0 = 0, c0 = 0
Output: [[0,0],[0,1]]
Explanation: The distances from (r0, c0) to other cells are: [0,1]

**Example 2:**

> Input: R = 2, C = 2, r0 = 0, c0 = 1
Output: [[0,1],[0,0],[1,1],[1,0]]
Explanation: The distances from (r0, c0) to other cells are: [0,1,1,2]
The answer [[0,1],[1,1],[0,0],[1,0]] would also be accepted as correct.

**Example 3:**

> Input: R = 2, C = 3, r0 = 1, c0 = 2
Output: [[1,2],[0,2],[1,1],[0,1],[1,0],[0,0]]
Explanation: The distances from (r0, c0) to other cells are: [0,1,1,2,2,3]
There are other answers that would also be accepted as correct, such as [[1,2],[1,1],[0,2],[1,0],[0,1],[0,0]].

**Note:**

> 1 <= R <= 100
1 <= C <= 100
0 <= r0 < R
0 <= c0 < C

## 解题思路

题目希望将矩阵中的点按与矩阵中某个点（以参数传入方式确认）的距离进行从小到大排序输出，两个点的距离如何计算不需要担心，题目中明确指出可以通过 `|r1 - r2| + |c1 - c2|` 计算得到。其实简单点解决，我们只需要将经典的排序算法，如快排的比较方式修改一下就可以解决问题。但我使用另外一种方式解决，因为点之间的距离会出现相同的情况，所以我将点之间的距离作为散列值，此处的散列冲突以链表法解决，遍历完矩阵点后，将散列表按散列值先后顺序拼接为数组输出即可。

## 代码实现

代码实现中主要关注 `orderMatrixByDistance` 方法。

```java
public int getDistance(int[] pointA, int[] pointB) {
    return getPointAbs(0, pointA, pointB) + getPointAbs(1, pointA, pointB);
}

private int getPointAbs(int i, int[] pointA, int[] pointB) {
    return Math.abs(pointA[i] - pointB[i]);
}

public int[][] orderMatrixByDistance(int row, int col, int x, int y) {
    int[] basePoint = new int[]{x, y};
    LinkedList<Point>[] distances = new LinkedList[row * col];
    for (int i = 0; i < distances.length; i++) {
        distances[i] = new LinkedList();
    }

    for (int i = 0; i < row; i++) {
        for (int j = 0; j < col; j++) {
            int distance = getDistance(new int[]{i, j}, basePoint);
            distances[distance].add(new Point(i, j));
        }
    }

    Point[] sortedPoint = transLinkedToArray(row * col, distances);
    int[][] sortedIntPoint = new int[row * col][2];
    for (int i = 0; i < sortedPoint.length; i++) {
        sortedIntPoint[i] = sortedPoint[i].transInt();
    }

    return sortedIntPoint;
}

private Point[] transLinkedToArray(int size, LinkedList<Point>[] distances) {
    Point[] sortedDistance = new Point[size];
    int count = 0;
    for (int i = 0; i < distances.length; i++) {
        LinkedList<Point> distanceLinked = distances[i];
        if (distanceLinked.isEmpty()) {
            continue;
        }
        Point[] distanceArray = new Point[distanceLinked.size()];
        distanceLinked.toArray(distanceArray);
        System.arraycopy(distanceArray, 0, sortedDistance, count, distanceLinked.size());
        count += distanceLinked.size();
    }
    return sortedDistance;
}

private class Point {
    private final int x;
    private final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int[] transInt() {
        return new int[]{x, y};
    }
}
```

****
> **代码详情可点击查看 [我的 GitHub 仓库](https://github.com/CloneableX/leetcode/)**