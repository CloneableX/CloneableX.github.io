---
title: leetcode 题号1047 Remove All Adjacent Duplicates In String
categories:
  - leetcode解题
  - stack
tags:
  - leetcode
p: leetcode\stack\easy\leetcode题号1047_RemoveAllAdjacentDuplicatesInString
date: 2019-07-02 08:53:38
---

查看题目详情可[点击此处](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/)。

## 题目

Given a string S of lowercase letters, a duplicate removal consists of choosing two adjacent and equal letters, and removing them.

We repeatedly make duplicate removals on S until we no longer can.

Return the final string after all such duplicate removals have been made.  It is guaranteed the answer is unique. 

**Example 1:**

> Input: "abbaca"
Output: "ca"
Explanation: 
For example, in "abbaca" we could remove "bb" since the letters are adjacent and equal, and this is the only possible move.  The result of this move is that the string is "aaca", of which only "aa" is possible, so the final string is "ca".
 
**Note:**

> 1 <= S.length <= 20000
S consists only of English lowercase letters.

## 解题思路

这题的解题思路与 {% post_link leetcode/stack/easy/leetcode题号844_BackspaceStringCompare Backspace String Compare %} 相似，只不过回退的「#」条件替换为了相同字符回退，当然两个相同的字符是相邻的，每次的比较肯定也是从已读取字符的最后一位开始，可以使用栈的方式解决。

使用栈的方式解决时，对字符串进行遍历读取入栈中，每次读取字符时与栈顶元素比较是否相同，相同就进行出栈操作，直到遍历完字符串。

同样也有原地算法的方式解题，与插入排序相似。将字符数组分成已读取区和未读取区，利用两个指针分别操作两个区域，然后遍历未读取区，将读取到的字符与已读取区末尾字符比较（如果已读取区已经存在字符），字符相同的话已读取区指针减 1，如果不相同将未读取区的当前字符覆盖至已读取区末尾，已读取区指针增 1，直至遍历完字符串。

## 代码实现

```java
// by stack
public static String removeDuplicates(String S) {
    char[] chars = S.toCharArray();
    Stack<Character> stack = new Stack<>();
    for(int i = 0; i < chars.length; i++) {
        if(stack.empty() || !stack.peek().equals(chars[i])) {
            stack.push(chars[i]);
        } else {
            stack.pop();
        }
    }

    char[] result = new char[stack.size()];
    while(!stack.empty()) {
        result[stack.size() - 1] = stack.pop();
    }

    return new String(result);
}

// by in place
public static String removeDuplicatesInPlace(String S) {
    char[] chars = S.toCharArray();
    int j = 0;
    for(int i = 0; i < chars.length; i++) {
        if(j == 0 || chars[j - 1] != chars[i]) {
            chars[j++] = chars[i];
        } else {
            j--;
        }
    }

    char[] result = new char[j];
    System.arraycopy(chars, 0, result, 0, j);
    return new String(result);
}
```

****
> **代码详情可点击查看 [我的 GitHub 仓库](https://github.com/CloneableX/leetcode/)**