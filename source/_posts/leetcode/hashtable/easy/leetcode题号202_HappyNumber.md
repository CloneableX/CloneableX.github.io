---
title: leetcode 题号202 Happy Number
categories:
  - leetcode解题
tags:
  - leetcode
p: leetcode\hashtable\easy\leetcode题号202_HappyNumber
date: 2019-09-22 18:25:41
---

查看题目详情可[点击此处](https://leetcode.com/problems/happy-number/)。

### 题目

Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

**Example:** 

> Input: 19
Output: true
Explanation: 
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1

### 解题思路

其实题目是想一个将数字的每一位平方的和作为一个新数字，这个数字如果为 1，那它就是 Happy Num，如果不是 1 将这个新数字计算每一位的平方之和，又以此为一个新的数字，直到计算得到 1 或者计算的数字已经出现过就结束运算。

计算过的数字需要一个容器存储，并且能够快速查找，那 HashMap 再合适不过，最终就是计算每一位的平方和进行判断。

### 代码实现

```java
class Solution {
    private static final int DIGIT_NUM = 10;
    private static final int[] DIGIT_SQUARE_NUMS = new int[]{0, 1, 4, 9, 16, 25, 36, 49, 64, 81};
    
    public boolean isHappy(int n) {
        int num = n;
        Map<Integer, Integer> computeNums = new HashMap<>();
        while (num != 1) {
            num = sumSquareDigits(num);
            if (computeNums.containsKey(num)) {
                return false;
            }

            computeNums.put(num, 1);
        }
        return true;
    }
    
    public static int sumSquareDigits(int num) {
        int sum = 0;
        while (num != 0) {
            sum += DIGIT_SQUARE_NUMS[num % DIGIT_NUM];
            num /= DIGIT_NUM;
        }

        return sum;
    }
}
```

****
> **代码详情可点击查看 [我的 GitHub 仓库](https://github.com/CloneableX/leetcode/)**