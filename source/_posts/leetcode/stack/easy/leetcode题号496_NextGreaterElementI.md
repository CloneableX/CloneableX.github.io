---
title: leetcode 题号496 Next Greater Element I
categories:
  - leetcode解题
tags:
  - leetcode-stack
p: leetcode/stack/easy/leetcode题号496_NextGreaterElementI
date: 2019-06-23 18:01:23
---

查看题目详情可[点击此处](https://leetcode.com/problems/next-greater-element-i/)。

## 题目

You are given two arrays (without duplicates) nums1 and nums2 where nums1’s elements are subset of nums2. Find all the next greater numbers for nums1's elements in the corresponding places of nums2.

The Next Greater Number of a number x in nums1 is the first greater number to its right in nums2. If it does not exist, output -1 for this number.

**Example 1:**

>Input: nums1 = [4,1,2], nums2 = [1,3,4,2].
Output: [-1,3,-1]
Explanation:
  For number 4 in the first array, you cannot find the next greater number for it in the second array, so output -1.
  For number 1 in the first array, the next greater number for it in the second array is 3.
  For number 2 in the first array, there is no next greater number for it in the second array, so output -1.

**Example 2:**

>Input: nums1 = [2,4], nums2 = [1,2,3,4].
Output: [3,-1]
Explanation:
  For number 2 in the first array, the next greater number for it in the second array is 3.
  For number 4 in the first array, there is no next greater number for it in the second array, so output -1.

**Note:**

1. All elements in nums1 and nums2 are unique.
2. The length of both nums1 and nums2 would not exceed 1000.


## 解题思路

解题的核心在于如何有效在一次数组遍历情况下得到当前数及其下一位较大值。这里可以使用栈保存已经被遍历过且尚未找到下一位更大值的数据，被遍历到的当前数据都需要与栈顶数据比较，如果大于栈顶数据便进行出栈操作，当前数据重复比较操作，直到栈空或者小于等于栈顶数据时当前数据入栈。

这样的方式可以使栈底到栈顶的数据为从大至小排列，如果当前遍历到的数据都不是栈顶数据的下一位更大数据，就更不可能是栈内其他数据的下一位更大数据，当栈内数据遇到自己的下一位更大数据时，做出栈操作可以避免同一数据重复查找下一位更大数，也是栈内数据顺序排列的保障。

最后的问题就是将 nums1 中的数据与 nums2 遍历后的最大数据关联起来。因为题目保证数组中数据的唯一性，为了快速到达目的就以 Map 的方式存储对应关系，最后对 nums1 遍历就可以获得对应的下一位最大值。

## 代码实现

```java
public int[] nextGreaterElement(int[] nums1, int[] nums2) {
    if(nums1.length < 1 || nums2.length < 1) {return nums1;}
    Map<Integer, Integer> resultMap = new HashMap<>();
    Stack<Integer> stack = new Stack<>();

    for(int num : nums2) {
        while(!stack.empty() && num > stack.peek()) {
            resultMap.put(stack.pop(), num);
        }
        stack.push(num);
    }

    int[] result = new int[nums1.length];
    for(int i = 0; i < result.length; i++) {
        result[i] = resultMap.getOrDefault(nums1[i], -1);
    }
    return result;
}
```

****
> **代码详情可点击查看 [我的 GitHub 仓库](https://github.com/CloneableX/leetcode/)**