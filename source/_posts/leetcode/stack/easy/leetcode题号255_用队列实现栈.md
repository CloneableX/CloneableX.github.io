---
title: 'leetcode 题号255 用队列实现栈'
categories:
  - leetcode解题
tags:
  - leetcode
  - 栈
p: 'leetcode/stack/easy/leetcode题号255_用队列实现栈'
date: 2019-06-02 19:55:58
---

查看题目详情可[点击此处](https://leetcode-cn.com/problems/implement-stack-using-queues/submissions/)。

## 题目

使用队列实现栈的下列操作：

- push(x) -- 元素 x 入栈
- pop() -- 移除栈顶元素
- top() -- 获取栈顶元素
- empty() -- 返回栈是否为空

**注意:**

你只能使用队列的基本操作-- 也就是 `push to back`, `peek/pop from front`, `size`, 和 `is empty` 这些操作是合法的。
你所使用的语言也许不支持队列。 你可以使用 list 或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。
你可以假设所有操作都是有效的（例如, 对一个空的栈不会调用 pop 或者 top 操作）。

## 解题思路

首先应该明确栈和队列两种数据结构的不同特点，栈是先进后出，队列是先进先出，如果要用队列实现栈，需要使队列的队首元素保持为栈顶元素，并且队列中元素的出队顺序要保持与栈中一致。

我的解题思路是使用两个队列实现，一个队列（以下称为「**栈顶队列**」）为存储栈顶数据，一个队列（以下称为「**非栈顶队列**」）保存非栈顶数据但出队顺序与出栈顺序保持一致。当数据入栈时判断栈顶队列是否为空，为空时数据入队；否则判断非栈顶队列是否为空，为空时将数据入队非栈顶队列，然后将非栈顶队列与栈顶队列交换；如果两个队列都不为空，将非栈顶队列数据按顺序入队至栈顶队列，并清空非栈顶队列，将入栈数据入队至非栈顶队列，最后将两个队列交换。

需要注意的是出栈操作上，当栈顶队列进行出栈操作前，需要将非栈顶队列的队首元素出队并入队至栈顶队列，否则出栈操作之后栈顶队列就会空。

## 代码实现

```java
import java.util.concurrent.LinkedBlockingQueue;

class MyStack {

    private Queue<Integer> tempQueue;
    private Queue<Integer> notTopStackQueue;
    private Queue<Integer> stackQueue;

    public MyStack() {
        notTopStackQueue = new LinkedBlockingQueue<>();
        stackQueue = new LinkedBlockingQueue<>();
    }

    public void push(int x) {
        if(stackQueue.isEmpty()) {
            stackQueue.add(x);
        } else if(notTopStackQueue.isEmpty()) {
            notTopStackQueue.addAll(stackQueue);
            stackQueue.clear();
            stackQueue.add(x);
        } else {
            stackQueue.addAll(notTopStackQueue);
            notTopStackQueue.clear();

            // swap stackQueue with notTopStackQueue
            tempQueue = stackQueue;
            stackQueue = notTopStackQueue;
            notTopStackQueue = tempQueue;

            stackQueue.add(x);
        }
    }

    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        if(!notTopStackQueue.isEmpty()) {
            stackQueue.add(notTopStackQueue.poll());
        }
        return stackQueue.poll();
    }

    /** Get the top element. */
    public int top() {
        return stackQueue.peek();
    }

    /** Returns whether the stack is empty. */
    public boolean empty() {
        return stackQueue.isEmpty();
    }
}
```