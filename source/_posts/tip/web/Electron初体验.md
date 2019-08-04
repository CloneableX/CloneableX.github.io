---
title: Electron 初体验
p: tip/web/Electron初体验
date: 2019-08-04 10:33:06
categories: 
  - tip
  - 前端
tags:
  - 前端
---

### Electron 是个啥

Electron 基于 Chromium 和 Node.js，使用 Html，CSS，JS 开发的一种跨平台桌面轻应用。它的跨平台特性应该是来自于 Chromium，打包为不同平台的桌面应该也就是打包不同平台的 Chromium，既然是 Chromium 为基础，那使用 Html + CSS + JS 进行应用开发也就不奇怪了，作为桌面应用来说能够调用各种平台功能的 API 也必不可少，Electron 提供了类似的 API，而 Node.js 也为 JS 提供了大量实用方便的 API。

### 我的体验

在 Electron 官网看了基于它制作的诸多应用，界面美观大方，在功能上也是多种多样，前端人员使用 Electron 开发桌面应用可以说基本没有门槛，只是 API 需要熟悉了解，不过 Electron 的应用有主进程和渲染进程，渲染进程只能通过 Electron 的 remote 模块使用 Electron 的 API，也可以使用 ipcRender 和 ipcMain 在两个进程间通信。

我用 Electron 制作了一个简单的应用，可以离线和跨平台使用，不需要访问任何页面，使用最简单的 Html + CSS + Js 就能够实现，因为只是使用简单的样式，界面就丑到天际，包括原生的弹窗也是相当不能看，不过 Electron 是可以集成 Vue 开发的，到时引入类似 element-ui 的 UI 组件，界面应该不是问题，再加上使用 Vue 对于前端人员来说与开发页面应用并没有区别。