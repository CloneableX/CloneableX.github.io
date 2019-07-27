---
title: leetcode 题号242 Valid Anagram
categories:
  - leetcode解题
  - sort
tags:
  - leetcode
p: leetcode\sort\easy\leetcode题号242_ValidAnagram
date: 2019-07-20 08:52:10
mathjax: true
---

查看题目详情可[点击此处](https://leetcode.com/problems/valid-anagram/)。

## 题目

Given two strings s and t , write a function to determine if t is an anagram of s.

**Example 1:**

> Input: s = "anagram", t = "nagaram"
Output: true

**Example 2:**

> Input: s = "rat", t = "car"
Output: false

**Note:**

You may assume the string contains only lowercase alphabets.

## 解题思路

两个字符串可以使用嵌套循环遍历比较，但时间复杂度为 $O(n^2)$，需要想其他的方法提高效率。

比较两个字符串可以认为它们只是对同样的字符放置在不同的位置上，如果我们使用同样的规则使相同的字符被排列到相同的位置，就可以直接进行最简单的字符串相等判断，这种规则我们可以使用将字符由小至大排序，再比对字符串即可。因为快时间复杂度为 $O(n\log{n})$，循环比较字符也只是 $O(n)$ 的时间复杂度，所以这个算法的时间复杂度为 $O(n\log{n})$，相比嵌套循环的方式执行效率有所提高。

不过我们还可以换个角度考虑，对于这两个字符串可以不考虑具体的字符排列，但字符串中每种字符的个数统计必然是相同的，我们可以将字符转换为 ASCII 码并计算与字符 a 的 ASCII 码的偏移，以此为数组下标，将字符统计量保存在数组中，最后比较字符统计量即可。这种算法因为只是使用到循环，时间复杂度为 $O(n)$，这种算法是三种里面效率最高的。

## 代码实现

```java
// by sort
public static boolean isAnagramBySort(String s, String t) {
    if(s.length() != t.length()) {
        return false;
    }

    char[] sChars = s.toCharArray();
    char[] tChars = t.toCharArray();

    Arrays.sort(sChars);
    Arrays.sort(tChars);

    for(int i = 0; i < sChars.length; i++) {
        if(sChars[i] != tChars[i]) {
            return false;
        }
    }

    return true;
}

// by count
public static boolean isAnagram(String s, String t) {
    if(s.length() != t.length()) {
        return false;
    }

    int[] charCount = new int[26];
    char[] sChars = s.toCharArray();
    char[] tChars = t.toCharArray();
    for(int i = 0; i < sChars.length; i++) {
        charCount[charHash(sChars[i])]++;
        charCount[charHash(tChars[i])]--;
    }

    for(int i = 0; i < charCount.length; i++) {
        if(charCount[i] != 0) {
            return false;
        }
    }

    return true;
}

private static int charHash(char sChar) {
    return (int) sChar - 'a';
}
```

****
> **代码详情可点击查看 [我的 GitHub 仓库](https://github.com/CloneableX/leetcode/)**