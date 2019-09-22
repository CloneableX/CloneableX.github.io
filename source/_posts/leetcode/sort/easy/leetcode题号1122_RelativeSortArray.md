---
title: leetcode 题号1122 Relative Sort Array
categories:
  - leetcode解题
tags:
  - leetcode
p: leetcode\sort\easy\leetcode题号1122_RelativeSortArray
date: 2019-09-16 16:23:37
---

查看题目详情可[点击此处](https://leetcode.com/problems/relative-sort-array/)。

### 题目

Given two arrays arr1 and arr2, the elements of arr2 are distinct, and all elements in arr2 are also in arr1.

Sort the elements of arr1 such that the relative ordering of items in arr1 are the same as in arr2.  Elements that don't appear in arr2 should be placed at the end of arr1 in ascending order. 

**Example 1:**

> Input: arr1 = [2,3,1,3,2,4,6,7,9,2,19], arr2 = [2,1,4,3,9,6]
Output: [2,2,2,1,4,3,3,9,6,7,19]
 
**Constraints:**

- arr1.length, arr2.length <= 1000
- 0 <= arr1[i], arr2[i] <= 1000
- Each arr2[i] is in arr1.
- Each arr2[i] is distinct.

### 解题思路

根据题目可知，被排序的数组 arr1 会被分为两块以不同的排序方式进行排序再组合，完成整个数组的排序。排序完成的数组前半部分是根据关联子数组 arr2 中的元素顺序排序，arr2 的元素在 arr1 都有对应数据，arr1 中会有重复数据存在的情况，arr2 中的数据都是唯一的，arr1 与 arr2 中对应不上的元素根据从小至大的顺序排列。

我是先记录 arr2 中元素分别在 arr1 中出现的次数，并且将这些元素都交换至 arr1 的靠前部分，交换结束后 arr1 的靠后部分就是与 arr2 中元素对应不上的元素，我们可以对这部分元素按从小到大排序，此时记录的 arr2 元素分别在 arr1 中出现的次数就派上用场，根据元素在 arr2 中的顺序以及在 arr1 中出现的次数（其实就是个数）在 arr1 的前部分进行填充，最后 arr1 就是题目中需要的结果。

### 代码实现

代码中使用 relativeSort 方法进行题目要求的排序。

```java
public class RelativeSortArray {
    public static int[] relativeSort(int[] array, int[] relative) {
        Map<Integer, Integer> relativeMap = generateRelativeMap(relative);
        int relativeIdx = filterRelativeArea(array, relativeMap);
        notRelativeSort(array, relativeIdx);
        fillRelativeArray(array, relative, relativeMap);
        return array;
    }

    public static Map<Integer, Integer> generateRelativeMap(int[] relative) {
        Map<Integer, Integer> relativeMap = new HashMap<>();
        for (int i = 0; i < relative.length; i++) {
            relativeMap.put(relative[i], 0);
        }
        return relativeMap;
    }

    public static boolean existRelative(int num, Map<Integer, Integer> relativeMap) {
        return relativeMap.containsKey(num);
    }

    public static int filterRelativeArea(int[] array, Map<Integer, Integer> relativeMap) {
        int relativeIdx = 0;
        for (int i = 0; i < array.length; i++) {
            if (existRelative(array[i], relativeMap)) {
                Integer count = relativeMap.get(array[i]);
                relativeMap.put(array[i], ++count);
                int temp = array[relativeIdx];
                array[relativeIdx] = array[i];
                array[i] = temp;
                relativeIdx++;
            }
        }
        return relativeIdx;
    }

    public static void notRelativeSort(int[] array, int relativeIdx) {
        if (relativeIdx > array.length) {
            return;
        }
        Arrays.sort(array, relativeIdx, array.length);
    }

    public static void fillRelativeArray(int[] array, int[] relative, Map<Integer, Integer> relativeMap) {
        int fillStartIdx = 0;
        for (int i = 0; i < relative.length; i++) {
            Integer toIndex = relativeMap.get(relative[i]) + fillStartIdx;
            Arrays.fill(array, fillStartIdx, toIndex, relative[i]);
            fillStartIdx = toIndex;
        }
    }
}
```

****
> **代码详情可点击查看 [我的 GitHub 仓库](https://github.com/CloneableX/leetcode/)**