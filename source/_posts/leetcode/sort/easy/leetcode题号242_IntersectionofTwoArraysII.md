---
title: leetcode 题号242 Intersection of Two Arrays II
categories:
  - leetcode解题
  - sort
tags:
  - leetcode
p: leetcode\sort\easy\leetcode题号242_IntersectionofTwoArraysII
date: 2019-07-27 21:22:57
---

查看题目详情可[点击此处](https://leetcode.com/problems/intersection-of-two-arrays-ii/)。

## 题目

Given two arrays, write a function to compute their intersection.

**Example 1:**

> Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2,2]

**Example 2:**

> Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [4,9]

**Note:**

- Each element in the result should appear as many times as it shows in both arrays.
- The result can be in any order.

## 解题思路

这次的题目有所不同，两个数组交集元素一一对应，这种情况使用嵌套循环会非常麻烦，因为当对应上两个数组的交集元素后两个交集元素需要标记或者直接从数组移除，以免相同值元素再次比对。如果将数组元素排序后情况就会变得相对简单，交集元素匹配上后将指针右移即可，而且排序后的数组匹配交集元素不需要使用嵌套循环，只需遍历元素较多的数组，依次与另一数组元素匹配，匹配上或者满足指针右移条件就右移指针，直到任一数组穷尽为止。

## 代码实现

```java
public static int[] intersection(int[] nums1, int[] nums2) {
    if(nums1.length < 1 || nums2.length < 1) {
        return new int[]{};
    }

    Arrays.sort(nums1);
    Arrays.sort(nums2);

    int[] shortArray = nums1;
    int[] longArray = nums2;
    if(nums1.length > nums2.length) {
        shortArray = nums2;
        longArray = nums1;
    }

    int[] intersection = new int[shortArray.length];
    int k = 0;
    for(int i = 0, j = 0; i < shortArray.length && j < longArray.length; j++) {
        if(longArray[j] > shortArray[i]) {
            j--;
            i++;
        } else if(longArray[j] == shortArray[i]) {
            intersection[k++] = shortArray[i++];
        }
    }

    int[] result = new int[k];
    System.arraycopy(intersection, 0, result, 0, k);
    return result;
}
```

****
> **代码详情可点击查看 [我的 GitHub 仓库](https://github.com/CloneableX/leetcode/)**