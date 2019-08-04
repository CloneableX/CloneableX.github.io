---
title: leetcode 题号922 Sort Array By Parity II
categories:
  - leetcode解题
tags:
  - leetcode
p: leetcode\sort\easy\leetcode题号922_SortArrayByParityII
date: 2019-08-04 09:40:55
mathjax: true
---

查看题目详情可[点击此处](https://leetcode.com/problems/sort-array-by-parity-ii/)。

## 题目

Given an array A of non-negative integers, half of the integers in A are odd, and half of the integers are even.

Sort the array so that whenever A[i] is odd, i is odd; and whenever A[i] is even, i is even.

You may return any answer array that satisfies this condition.

 

**Example 1:**

> Input: [4,2,5,7]
Output: [4,5,2,7]
Explanation: [4,7,2,5], [2,5,4,7], [2,7,4,5] would also have been accepted.
 

**Note:**

1. 2 <= A.length <= 20000
2. A.length % 2 == 0
3. 0 <= A[i] <= 1000


## 解题思路

题目说来就是奇数需要放置在奇数下标位置，偶数需要放置在偶数下标位置，接下来有两种解题思路。

一种是可以创造一个新数组用于放置整理好的数据，这个新数组使用两个指针，一个奇数指针「**oddIndex**」和一个偶数指针「**evenIndex**」，分别指向新数组中奇数下标和偶数下标可存储数据的第一个空位，将原数组中的所有数据按奇偶分别存储到相应空位，并且 oddIndex 和 evenIndex 分别向后移动。

另一种是我们在原数组中将数组放置到正确的位置。由于数组中一半数据是奇数，一半是偶数，如果有一个奇数出现在偶数下标位置，只能说明肯定有一个偶数也是错位的，只要找到错位的偶数并与之交换数据就都能保持正确位置。对比前一种算法，因为都只是对数组进行了一次循环遍历，时间复杂度都是 $O(n)$，但第二种方法没有使用额外的存储空间，在空间复杂度上只有 $O(1)$，表现更加优秀。

## 代码实现

```java
// by new array
public static int[] sortArrayByParityII(int[] A) {
    int[] result = new int[A.length];
    for(int i = 0, oddIndex = 1, evenIndex = 0; i < A.length; i++) {
        if(A[i] % 2 == 0) {
            result[evenIndex] = A[i];
            evenIndex += 2;
        } else {
            result[oddIndex] = A[i];
            oddIndex += 2;
        }
    }

    return result;
}

// in place
public static int[] sortArrayInPlaceByParityII(int[] A) {
    for(int oddIndex = 1, evenIndex = 0; evenIndex < A.length - 1; evenIndex += 2) {
        if(A[evenIndex] % 2 == 1) {
            while(A[oddIndex] % 2 == 1) {
                oddIndex += 2;
            }

            swapArray(A, oddIndex, evenIndex);
        }
    }

    return A;
}

private static void swapArray(int[] A, int i, int swapIndex) {
    int temp = A[i];
    A[i] = A[swapIndex];
    A[swapIndex] = temp;
}
```

****
> **代码详情可点击查看 [我的 GitHub 仓库](https://github.com/CloneableX/leetcode/)**