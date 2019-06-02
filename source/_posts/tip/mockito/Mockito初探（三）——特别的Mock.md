---
title: Mockito初探（三）——特别的Mock
p: tip/mockito/Mockito初探（三）——特别的Mock
date: 2019-06-02 17:20:28
categories:
  - tip
  - mockito
tags:
  - mockito
---

Mockito 除了常用的 stub 和 verify 功能外，还可以完成一些特别的 stub 操作以及使用 spy 进行特别的 mock。

### Stub 返回不同值

```java
@Mock
private List mockedList;

@Before
public void before() {
    MockitoAnnotations.initMocks(this);
}

@Test
public void stubMultipleResult() {
    when(mockedList.get(anyInt())).thenReturn("first get").thenReturn("more than once get");

    System.out.println(mockedList.get(0));
    System.out.println(mockedList.get(0));
    System.out.println(mockedList.get(0));
}
```

可以通过上面的方式使第一次调用的时候返回第一个 thenReturn 的值，第二次调用返回第二个 thenReturn 的值，这种返回值方式只能顺序调用返回，当返回值进行到最后一个 thenReturn 时，无论再进行多少次调用都只会返回最后一个 thenReturn 的结果，当然也可以使用 `thenThrow`、 `thenAnswer` 等方法替换 thenReturn。当然，实现同样功能也有简洁写法。

```java
when(mockedList.remove(anyInt())).thenReturn("first remove", "twice remove", "more than twice remove");

System.out.println(mockedList.remove(0));
System.out.println(mockedList.remove(0));
System.out.println(mockedList.remove(0));
System.out.println(mockedList.remove(0));
```

将返回值依次作为 thenReturn 方法的参数传入也可以实现同样效果。

需要注意的是，不能将两次 thenReturn 分开写，如果分开写，后面一个 thenReutrn 会覆盖前一个 thenReturn 的结果。

```java
when(mockedList.remove(anyInt())).thenReturn("first remove");
when(mockedList.remove(anyInt())).thenReturn("twice remove");
```

### Stub 自定义返回

使用 stub 时可以返回一些简单的结果，如同之前提到的一些 stub 的例子，如果我们想要根据调用方法的参数等进行逻辑处理，再生成相应结果返回呢？这种情况可以使用 `thenAnswer` 方法，加上自定义实现 `Answer` 的 `answer` 方法解决。

```java
@Test
public void stubCallback() {
    when(mockedList.set(anyInt(), any())).thenAnswer(new Answer<Object>() {
        public Object answer(InvocationOnMock invocationOnMock) throws Throwable {
            Object[] args = invocationOnMock.getArguments();
            Object mocked = invocationOnMock.getMock();
            return "arguments are: " + args;
        }
    });

    System.out.println(mockedList.set(1, "Answer callback"));
}
```

### 亦真亦假的 Spy

通过 spy 方法也可以生成一个 mock 对象，不过 spy 与 mock 相比的特别之处在于，如果被调用的 spy 对象的方法没有 stub，会直接调用真实对象的相应方法。

```java
@Test
public void spyObject() {
    ArrayList<Integer> arrayList = new ArrayList<Integer>();
    ArrayList spyList = spy(arrayList);

    doReturn(100).when(spyList).size();

    System.out.println(spyList.size());

    spyList.add(0);
    spyList.add(1);

    System.out.println(spyList.size());
    System.out.println(spyList.toString());

    verify(spyList).add(0);
    verify(spyList).add(1);
}
```

当你只是需要 mock 部分方法，而又需要另外一些方法的真实调用时，使用 spy 再适宜不过。不过，需要注意的是，对 spy 对象进行 stub 时尽量使用 `doReturn.when(spyObj).someMethod()` 的方式，因为当使用 `when(spyObj.someMethod()).thenReturn()` 方式时 `someMethod` 相当于在 stub 之前被调用了一次，而 spy 对象调用未被 stub 的方法时，会直接调用原对象的相应方法，容易造成异常。

如下的例子中，`spyList` 在没有元素的情况下调用  `get` 方法就会造成下标越界的异常。

```java
ArrayList<Integer> arrayList = new ArrayList<Integer>();
ArrayList spyList = spy(arrayList);

when(spyList.get(0)).thenReturn("call get");
```

### 总结

这章描述了多次调用 stub 方法返回不同结果和自定义 stub 方法响应逻辑，还有 spy 这种特别的 mock 方式以及使用这些功能时的一些注意事项。