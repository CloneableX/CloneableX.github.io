---
title: leetcode 题号349 Intersection Of Two Arrays
categories:
  - leetcode解题
tags:
  - leetcode
p: leetcode\sort\easy\leetcode题号349_IntersectionOfTwoArrays
date: 2019-07-21 14:04:43
---

查看题目详情可[点击此处](https://leetcode.com/problems/intersection-of-two-arrays/)。

## 题目

Given two arrays, write a function to compute their intersection.

**Example 1:**

> Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]

**Example 2:**

> Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]

**Note:**

- Each element in the result must be unique.
- The result can be in any order.

## 解题思路

题目主要想获得两个数组的交集，可以通过嵌套循环逐个元素比对，利用 Set 去除重复的相同元素即可，但这种方法的执行效率不高。不过可以考虑把对数组先排序，排序后的数组因为元素有序，可以将长度较小的数组放入队列中，循环长度较大数组，并将循环到的元素与队列首部比较做出队操作，也是使用 Set 去除重复交集元素，这种执行效率就提高了许多。

但还有效率更高的方法，可以循环遍历数组，使用 Map 存储元素，元素值为 key，遍历第一个数组时值为 1，遍历第二个数组时将存在的 key 值置为 2，两个循环之后 Map 中 value 为 2 的 key 值就是题目需要的交集结果。

## 代码实现

```java
private static int[] convertSetToArray(Set<Integer> intersectionSet) {
    Integer[] intersection = new Integer[intersectionSet.size()];
    intersectionSet.toArray(intersection);
    int[] result = new int[intersection.length];
    for (int i = 0; i < intersection.length; i++) {
        result[i] = intersection[i];
    }
    return result;
}

// by nested loop
public static int[] intersectionByLoop(int[] nums1, int[] nums2) {
    if(nums1.length < 1 || nums2.length < 1) {
        return new int[]{};
    }

    int[] shortArray = nums1;
    int[] longArray = nums2;
    if(nums1.length > nums2.length) {
        shortArray = nums2;
        longArray = nums1;
    }

    Set<Integer> intersectionSet = new HashSet<>();
    for(int i = 0; i < shortArray.length; i++) {
        for(int j = 0; j < longArray.length; j++) {
            if(shortArray[i] == longArray[j]) {
                intersectionSet.add(shortArray[i]);
            }
        }
    }

    return convertSetToArray(intersectionSet);
}

// by sort and queue
public static int[] intersectionBySort(int[] nums1, int[] nums2) {
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

    LinkedList<Integer> linkedList = new LinkedList();
    for(int i = 0; i < shortArray.length; i++) {
        linkedList.add(shortArray[i]);
    }

    Set<Integer> intersectionSet = new HashSet<>();
    for(int i = 0; i < longArray.length; i++) {
        if(linkedList.isEmpty()) {
            break;
        }

        if(linkedList.peek().equals(longArray[i])) {
            intersectionSet.add(linkedList.poll());
        } else if(longArray[i] > linkedList.peek()) {
            linkedList.poll();
            i--;
        }
    }

    return convertSetToArray(intersectionSet);
}

// by map
public static int[] intersectionByMap(int[] nums1, int[] nums2) {
    if(nums1.length < 1 || nums2.length < 1) {
        return new int[]{};
    }

    int[] shortArray = nums1;
    int[] longArray = nums2;
    if(nums1.length > nums2.length) {
        shortArray = nums2;
        longArray = nums1;
    }

    Map<Integer, Integer> intersectionMap = new HashMap<>();
    for(int i = 0; i < shortArray.length; i++) {
        if(!intersectionMap.containsKey(shortArray[i])) {
            intersectionMap.put(shortArray[i], 1);
        }
    }

    for(int i = 0; i < longArray.length; i++) {
        if(intersectionMap.containsKey(longArray[i])) {
            intersectionMap.put(longArray[i], 2);
        }
    }

    Set<Integer> set = new HashSet<>();
    for(Integer key : intersectionMap.keySet()) {
        if(intersectionMap.get(key) == 2) {
            set.add(key);
        }
    }

    return convertSetToArray(set);
}
```

****
> **代码详情可点击查看 [我的 GitHub 仓库](https://github.com/CloneableX/leetcode/)**