---
title: JavaScript 正则匹配的 Unicode 模式
p: tip/other/JavaScript正则匹配的Unicode模式
date: 2019-09-22 17:23:51
categories:
    - tip
    - other
tags:
    - regrex
---


### 疑惑的 unicode 模式

前两天室友正在看 js 关于正则表达式的博客，发现 js 正则表达式中有个 u，可以用于开启 unicode 模式，并且被博客举的两个例子搞懵了，例子如下：

```javascript
/^\uD83D/.test('\uD83D\uDC2A') // true
/^\uD83D/u.test('\uD83D\uDC2A') // false
```

为什么会这样？我们看见这个例子的时候也是这样想的。其实 js 有个大前提，JavaScript 内部，字符以 UTF-16 的格式储存，每个字符固定为 2 个字节。对于那些需要 4 个字节储存的字符（Unicode 码点大于0xFFFF 的字符），JavaScript 会认为它们是两个字符。

例子中的第一个就能够解释，用 \uD83D 匹配两个字符，其中包含 \uD83D，匹配当然成功，但为什么开启 unicode 模式之后就匹配失败？因为在 UTF-16 中还有自己的规则，UTF-16 会用两个字节表示基础字符，面对扩展字符使用四个字节表示，也就是说有些情况下四个字节的 UTF-16 才能表示一个字符，很不幸，上面的例子就是这个情况。因为在 UTF-16 中 `0xd800 - 0xdbff` 表示高位代理，`0xdc00 - 0xdfff` 表示低位代理，其实两个代理就是在告诉计算机，它们属于扩展字符，需要使用四个字节表示一个字符。

于是，例子中的第二条在开启 Unicode 模式之后，\uD83D\uDC2A 被识别为一个字符，\uD83D 作为一个字符中的一部分匹配整个字符肯定是以失败告终，如果对 Js 的这个神奇现象想进行更加深入的了解，可以阅读 [Unicode-aware regular expressions in ES2015](https://mathiasbynens.be/notes/es6-unicode-regex) 一文。