---
title: leetcode 题号232 用栈实现队列
categories:
  - leetcode解题
tags:
  - leetcode
p: leetcode/stack/easy/leetcode题号232_用栈实现队列
date: 2019-06-09 15:39:26
---

查看题目详情可[点击此处](https://leetcode-cn.com/problems/implement-queue-using-stacks/submissions/) 「**来源：力扣（LeetCode）**」。
## 题目

使用栈实现队列的下列操作：

- push(x) -- 将一个元素放入队列的尾部。
- pop() -- 从队列首部移除元素。
- peek() -- 返回队列首部的元素。
- empty() -- 返回队列是否为空。

**示例:**

> MyQueue queue = new MyQueue();
>
> queue.push(1);
queue.push(2);  
queue.peek();  // 返回 1
queue.pop();   // 返回 1
queue.empty(); // 返回 false

**说明:**

- 你只能使用标准的栈操作 -- 也就是只有 push to top, peek/pop from top, size, 和 is empty 操作是合法的。
- 你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。
- 假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）。

## 解题思路

此题解题思路与使用队列实现栈的相似，同样需要两个栈，一个栈作为「**临时栈**」，负责数据入队时临时存放，另一个栈作为「**队列栈**」用来模拟队列。

当进行 `pop`、`peek`、`empty` 的队列操作时，都可以使用队列栈的 `pop`、`peek`、`empty` 的栈操作。在入队操作时，需要将入队数据暂存在临时栈中（临时栈永远为空），再将队列栈的数据按入栈顺序依次转移至临时栈中，接着临时栈与队列栈引用交换，交换完成后将临时栈清空。

## 代码实现

```java
class MyQueue {

    private Stack<Integer> tempStack;
    private Stack<Integer> stackQueue;

    /** Initialize your data structure here. */
    public MyQueue() {
        tempStack = new Stack<>();
        stackQueue = new Stack<>();
    }

    /** Push element x to the back of queue. */
    public void push(int x) {
        if(stackQueue.empty()) {
            stackQueue.push(x);
        } else {
            tempStack.push(x);
            tempStack.addAll(stackQueue);

            Stack<Integer> temp = stackQueue;
            stackQueue = tempStack;
            tempStack = temp;
            tempStack.clear();
        }
    }

    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if(stackQueue.empty()) {return 0;}

        return stackQueue.pop();
    }

    /** Get the front element. */
    public int peek() {
        return stackQueue.peek();
    }

    /** Returns whether the queue is empty. */
    public boolean empty() {
        return stackQueue.empty();
    }
}
```