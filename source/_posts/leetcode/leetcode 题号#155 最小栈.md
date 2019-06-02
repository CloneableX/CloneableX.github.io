---
title: 最小栈'
categories:
  - leetcode解题
tags:
  - leetcode
  - 栈
p: 'leetcode/leetcode 题号#155 最小栈'
date: 2019-05-26 09:03:38
---

查看题目详情可[点击此处](https://leetcode-cn.com/problems/min-stack/)。

## 题目

设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。

- push(x) -- 将元素 x 推入栈中。
- pop() -- 删除栈顶的元素。
- top() -- 获取栈顶元素。
- getMin() -- 检索栈中的最小元素。

示例:
> MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.

## 解题思路

需要实现的栈操作中，其他操作属于常规的栈操作，没有特别之处，解题的重点在于如何实现常数时间类检索到最小值，实现此操作需要额外的存储空间对最小值进行存储，最初肯定会考虑到用一个 int 变量存储最小值，每次 push 操作的时候比较元素替换最小值，检索最小值的时候将此 int 变量返回即可。

这样的方法初看没有问题，但当进出栈操作时，如果栈顶元素就是当前的最小值，出栈后就需要在栈内元素中再搜索一遍最小值并保存起来，无疑会增加出栈操作的时间复杂度。

利用空间换时间的思想，可以创建另一个栈，当数据入栈时会存入数据栈，同时也会把此元素对应的最小值存储在最小值栈中，类似备忘录模式，记录下每个元素的最小值状态。

## 代码实现

```java
class MinStack {

    /** initialize your data structure here. */
    private Stack<Integer> intStack;
    private Stack<Integer> minStack;
    private int min;

    public MinStack() {
        intStack = new Stack<>();
        minStack = new Stack<>();
        min = Integer.MIN_VALUE;
    }

    public void push(int x) {
        intStack.push(x);

        if(minStack.empty()) {
            minStack.push(x);
        } else {
            min = Math.min(minStack.peek(), x);
            minStack.push(min);
        }
    }

    public void pop() {
        intStack.pop();
        minStack.pop();
    }

    public int top() {
        return intStack.peek();
    }

    public int getMin() {
        return minStack.peek();
    }
}
```