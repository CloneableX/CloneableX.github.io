---
title: leetcode 题号844 Backspace String Compare
categories:
  - leetcode解题
  - stack
tags:
  - leetcode
p: leetcode\stack\easy\leetcode题号844_BackspaceStringCompare.md
date: 2019-06-29 15:13:23
---

查看题目详情可[点击此处](https://leetcode.com/problems/backspace-string-compare/)。

## 题目

Given two strings S and T, return if they are equal when both are typed into empty text editors. # means a backspace character.

**Example 1:**

> Input: S = "ab#c", T = "ad#c"
Output: true
Explanation: Both S and T become "ac".

**Example 2:**

> Input: S = "ab##", T = "c#d#"
Output: true
Explanation: Both S and T become "".

**Example 3:**

> Input: S = "a##c", T = "#a#c"
Output: true
Explanation: Both S and T become "c".

**Example 4:**

> Input: S = "a#c", T = "b"
Output: false
Explanation: S becomes "c" while T becomes "b".

**Note:**

> 1 <= S.length <= 200
1 <= T.length <= 200
S and T only contain lowercase letters and '#' characters.

## 解题思路

根据题目可知，题目主要有两步逻辑，一步是对字符串进行 backspace 操作，另一步是比较两个字符串的 backspace 结果是否一致，比较字符串并不复杂，困难点在于 backspace 操作。

观察 backspace 操作可以发现，该操作其实是从头逐步读取字符，当读取到 「#」 时清除读取到的最后一个字符（如果已经有读取字符的记录），直至遍历读取完整个字符串。消除最后被读取的字符，这种操作方式十分符合栈的后进先出，所以可以使用栈实现 backspace 操作。

上面是常规的解题方式，除了利用栈实现 backspace 操作外，也可以使用原地算法实现。

原地算法的实现与插入排序相似，将数组分成已读取区和未读取区，分别由两个指针控制，未读取区指针遍历读取字符，当读取字符是「#」且读取区已经有被读取字符时已读取区指针回退，当读取字符不是「#」时直接替换至已读取区末尾，已读取区指针自增。

## 代码实现

```java
// backspace by stack
public boolean backspaceCompare(String S, String T) {
    Stack<Character> stackS = backspace(S.toCharArray());
    Stack<Character> stackT = backspace(T.toCharArray());

    if(stackS.size() != stackT.size()) {
        return false;
    }

    while(!stackS.empty() && !stackT.empty()) {
        if(!stackS.pop().equals(stackT.pop())) {
            return false;
        }
    }

    return true;
}

public Stack<Character> backspace(char[] chars) {
    Stack<Character> stack = new Stack();

    for(int i = 0; i < chars.length; i++) {
        if(chars[i] == '#' && !stack.empty()) {
            stack.pop();
        } else if(chars[i] != '#') {
            stack.push(chars[i]);
        }
    }

    return stack;
}

// backspace in place
public boolean backspaceCompareInPlace(String S, String T) {
    char[] charS = backspaceInPlace(S.toCharArray());
    char[] charT = backspaceInPlace(T.toCharArray());

    if(charS.length != charT.length) {
        return false;
    }

    for(int i = 0; i < charS.length; i++) {
        if(charS[i] != charT[i]) {
            return false;
        }
    }

    return true;
}

public char[] backspaceInPlace(char[] chars) {
    int j = 0;
    for(int i = 0; i < chars.length; i++) {
        if(chars[i] == '#' && j > 0) {
            j--;
        } else if(chars[i] != '#') {
            chars[j++] = chars[i];
        }
    }

    char[] result = new char[j];
    System.arraycopy(chars, 0, result, 0, j);
    return result;
}
```

****
> **代码详情可点击查看 [我的 GitHub 仓库](https://github.com/CloneableX/leetcode/)**