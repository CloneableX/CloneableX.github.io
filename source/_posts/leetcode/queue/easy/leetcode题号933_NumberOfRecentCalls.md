---
title: leetcode 题号933 Number of Recent Calls
categories:
  - leetcode解题
  - queue
tags:
  - leetcode
p: leetcode\queue\easy\leetcode题号933_NumberOfRecentCalls
date: 2019-07-08 08:39:32
---

查看题目详情可[点击此处](https://leetcode.com/problems/number-of-recent-calls/)。

## 题目

Write a class RecentCounter to count recent requests.

It has only one method: ping(int t), where t represents some time in milliseconds.

Return the number of pings that have been made from 3000 milliseconds ago until now.

Any ping with time in [t - 3000, t] will count, including the current ping.

It is guaranteed that every call to ping uses a strictly larger value of t than before.

**Example 1:**

> Input: inputs = ["RecentCounter","ping","ping","ping","ping"], inputs = [\[],[1],[100],[3001],[3002]]
Output: [null,1,2,3,3]
 
**Note:**

1. Each test case will have at most 10000 calls to ping.
2. Each test case will call ping with strictly increasing values of t.
3. Each call to ping will have 1 <= t <= 10^9.

## 解题思路

当将 t 作为参数调用 ping 方法时，需要去除在 t - 3000 至 t 范围外的值，由于每次调用 ping 的数值是从小至大，所以 t 是现在通过 ping 存储过的最大值，不满足条件的数据肯定只能小于 t - 3000，那从被存储的最小值开始找直到数据满足条件为止，返回存储的数据量即可。

这个操作使先调用 ping 方法存储起来的值被先删除掉，满足先进先出的特点，使用队列比较合适，我也尝试使用数组的方式，利用二分查找查找最后一位小于 t - 3000 数据元素的方式解决，不过通过 leetcode 测试用例的耗时和使用队例的差距并不大，应该是每次满足删除条件的数据量不多，所以逐个遍历和使用二分查找需要比较数据个数差距不大。

## 代码实现

```java
private Queue<Integer> queue = new LinkedList<>();
// by queue
public int ping(int t) {
    queue.add(t);
    while(queue.peek() < t - 3000) {
        queue.poll();
    }
    return queue.size();
}

private List<Integer> array = new ArrayList<>();
private int startIndex = 0;
// by array
public int pingByArray(int t) {
    array.add(t);
    int targetIndex = searchLastLess(array, t - 3000);
    if(targetIndex >= 0) {
        startIndex = targetIndex + 1;
    }

    return  array.size() - startIndex;
}

private int searchLastLess(List<Integer> array, int target) {
    if(array == null || array.size() < 1) {
        return -1;
    }

    int low = 0, high = array.size() - 1;
    while(low <= high) {
        int mid = low + ((high - low) >> 1);
        if(array.get(mid) >= target) {
            high = mid - 1;
        } else {
            if(mid == array.size() - 1 || array.get(mid + 1) >= target) {
                return mid;
            } else {
                low = mid + 1;
            }
        }
    }
    return -1;
}
```

****
> **代码详情可点击查看 [我的 GitHub 仓库](https://github.com/CloneableX/leetcode/)**