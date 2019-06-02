---
title: Mockito初探（二）——常用功能
p: tip/mockito/Mockito初探（二）——常用功能
date: 2019-05-26 16:28:16
categories:
  - tip
  - mockito
tags:
  - mockito
---

通过前一章 《 Mockito初探（一）—— 初识 》 的学习，我们对 Mockito 的一些基本知识有了初步了解，这章我们继续对 Mockito 的一些基本使用进行讲解。

### @Mock 注解的使用

我们可以使用 @Mock 注解 mock 指定的对象，但需要注意的是**在测试类中的 before 方法中加上 `MockitoAnnotations.initMocks(testClass);`**。

```java
@Mock
private List mockedList;

@Before
public void before() {
    MockitoAnnotations.initMocks(this);
}
```

### 参数匹配

对被 mock 的对象的某个方法进行 stub 时可以对方法的参数进行匹配，以返回期望的结果。

```java
@Test
public void stubWithArgMatcher() {
    ArrayList mock = mock(ArrayList.class);

    when(mock.get(anyInt())).thenReturn("mock get method");

    System.out.println(mock.get((int)(Math.random() * 10)));
}
```

Mockito 的参数匹配默认是调用对象的 `equals()` 方法，不过 Mockito 也提供以 any 开头的方法进行参数匹配，这种匹配方式更加灵活，可读性也更好。

### Stub 无返回值方法

对于无返回值的方法该如何 stub？

```java
@Test
public void stubVoidMethod() {
    doNothing().when(mockedList).add(anyInt(), any());
    doThrow(new RuntimeException("throw runtime exception")).when(mockedList).clear();

    mockedList.add(0, "123");
    mockedList.clear();
}
```

`doNothing()` 方法可以 stub 无返回值的方法，Mockito 还提供了一系列的 do 开头的方法，都是类似使用，不过被 stub 的方法不再是写在 when 中，而是写在 when 外部。

### verify 额外次数

verify 方法默认是验证相应方法被调用一次，不过 verify 方法也提供了验证多次或额外次数的方法。

```java
@Test
public void verifyExtraNum() {
    mockedList.add("once");

    mockedList.add("twice");
    mockedList.add("twice");

    mockedList.add("three times");
    mockedList.add("three times");
    mockedList.add("three times");

    verify(mockedList).add("once");
    verify(mockedList, times(1)).add("once");

    verify(mockedList, times(2)).add("twice");
    verify(mockedList, times(3)).add("three times");

    verify(mockedList, never()).add("never happen");

    verify(mockedList, atLeastOnce()).add("three times");
    verify(mockedList, atLeast(2)).add("three times");
    verify(mockedList, atMost(2)).add("twice");
}
```

verify 方法接受 VerificationMode 参数， 也就是不同的验证方式，times 方法返回验证不同次数的验证方式，例如 `times(1)` 就是验证相应方法被调用 1 次，以此类推，需要注意的是 **verify 验证方法是包括方法调用时的参数都进行匹配的**。

`never()` 方法就是验证方法没有被调用过，`atLeast()` 就是验证方法至少被调用 2 次，`atLeastOnce()` 是方法至少被调用 1 次，`atMost()` 相反是验证方法最多被调用的次数。

这章先介绍到这里，在本章中我们主要了解了使用 @Mock 对对象进行 mock，stub 时对参数的匹配方法和 stub 无返回值方法的方式，以及 verify 额外次数的使用。

****
本章节示例代码已经上传 GitHub，可[点击跳转](https://github.com/CloneableX/mockito-demo)查看代码详情