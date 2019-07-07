---
title: 'Failed to connect to github.com port 443: Timed out(Windows)'
date: 2019-06-22 16:48:26
categories:
  - tip
  - tool
tags:
  - github
---

文章前先声明，本人此次出现问题时使用操作系统为 Win 10。

本来风和日丽的一天，想往 GitHub 上提交点文件，结果无论 push 还是 pull 都出现 `Failed to connect to github.com port 443: Timed out` 错误，一天故事的开端变成了事故的开端。

面对超时我先尝试访问 GitHub 网站，畅通无阻，再使用 `ping` 命令测试 「github.com」 是否能够连接，结果与错误一样也是连接超时，通过在网络上搜索资料，基本都是说上网时使用了代理，所以导致问题的发生。

了解问题后决定确认一下，进入「网络和 Internet」设置的代理设置界面，看见「使用设置脚本」是打开的，并且有一个脚本地址，在浏览器中访问脚本地址会自动下载名为「pac」的文件，文件内容是需要进行代理的域名配置和使用代理的逻辑，搜索一下可以找到「github.com」也位列其中，那问题应该就是代理导致的了。

![](https://i.loli.net/2019/06/22/5d0df6c5e5d8d40438.png)

![](https://i.loli.net/2019/06/22/5d0df65d014a497716.png)

想解决这个问题，在 「pac」文件中找到 「proxy」的值，将其设置为 git 的 http.proxy 的值就行，设置参数可以通过下列两个 git 命令完成。

`git config --global http.proxy 127.0.0.1:1080` 为全局的 git 项目都设置代理

`git config --local http.proxy 127.0.0.1:1080` 为某个 git 项目单独设置代理

http.proxy 值为下图中标记部分

![](https://i.loli.net/2019/06/22/5d0df6f4aa74232947.png)