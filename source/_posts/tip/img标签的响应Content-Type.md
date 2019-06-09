---
title: img 标签的响应 Content-Type
p: tip/Img的响应Content-Type
date: 2019-06-09 14:52:32
categories: 
  - tip
  - 前端
tags:
  - 前端
  - img标签
---

### 前言

img 标签的 src 属性使用了未携带图片后缀的 url，导致某些图片显示异常，呈现图片损坏的样式，img 标签样例如下。

```html
<img src="/yoursite/imgId"/>
```

### 问题的解决

当时并不是所有图片都显示异常，而只是少量图片出现异常现象，于是就开始怀疑图片本身是否已经损坏，或者图片过大请求时间过长等类似原因，但恢复使用旧的 api 时一切正常，使用旧 api 时 img 标签样例如下。

```html
<img src="/yoursite/imgName.png" />
```

img 的 url 会带有图片类型后缀，怀疑的重心也因此转移至图片类型后缀上，通过加上后缀解决问题治标不治本，并且看来也不像造成此现象的真因。

最后通过比对两种不同请求的 Response Header 中 [Content-Type](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_types) 值得到结论，没有后缀情况下 Content-Type 默认使用 `text/html` 值，导致出现图片显示异常的现象，只需要在 api 代码中为 Response 添加上 Content-Type 值为 `image/png`，问题得到解决。

### 疑问

虽然以上方法解决了问题，但为何 img 的 url 没有图片类型后缀情况下，绝大多数图片依然能够显示正常，受限于我的知识，以及查找了许多资料依然没有得到答案，希望了解此情况的网友能够帮我解惑。