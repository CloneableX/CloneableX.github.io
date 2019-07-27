---
title: Java 中 HashMap 的简单分析
p: tip\java\Java中HashMap的简单分析
date: 2019-07-20 11:20:04
categories:
  - tip
  - java
tags:
  - java
---

在 Java 的 Map 中， HashMap 可以算最常用的子类之一了，因为学习了散列表，就简单分析一下 HashMap 作为实践。

HashMap 中的「Hash」表明了它和散列表之间有着某些不可告人的秘密，对于散列表而言，首当其冲的是散列函数，我们就先看一下 HashMap 的散列函数。

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

散列函数应该保持高效的散列值计算，在代码的计算中最高效的无非位运算，HashMap 的散列函数也是秉承散列函数的这一原则。

散列表最麻烦的处理还是在插入操作时解决散列冲突的问题，我们来看一下 HashMap 是如何解决这一问题的。

```java
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}
```

HashMap 插入操作的核心逻辑是在 `putVal` 方法中，继续跟进。

```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
    if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
    if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
    else {
        Node<K,V> e; K k;
        if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
        else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        else {
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                        treeifyBin(tab, hash);
                    break;
                }
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
                p = e;
            }
        }
        if (e != null) { // existing mapping for key
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null)
                e.value = value;
            afterNodeAccess(e);
            return oldValue;
        }
    }
    ...
}
```

方法中先用两个 `if` 判断解决 `tab` 没有生成或者散列的槽位没有结点的情况，看见结点就大概猜到 HashMap 使用的是链表法解决冲突。在 `else` 中才是插入操作的重点，这段逻辑使用 `for` 循环遍历链表，判断需要插入的数据是否已经存在，如果查找至链表尾部也未查询到与需插入数据匹配的结点，就在链表尾部插入新结点。在插入新结点后还增加了一个链表长度与 `TREEIFY_THRESHOLD` 的判断，当链表长度达到一定阀值时为避免散列表执行效率退化，将链表转变为树型结构。

对于 HashMap 的简单分析就到这里了，目前我掌握的知识只能分析到此，在对数据结构和算法有更加深入的学习之后，我会尝试作出更加深入的分析。