---
title: GitLab 的基本使用
p: tip\tool\GitLab的基本使用
date: 2019-07-06 20:10:57
categories:
  - tip
  - tool
tags:
  - gitlab
---

### GitLab 初次见面

基于 Git 托管代码的平台常使用 GitHub，这纯属于自己玩玩闹闹，在项目中也使用过类似 GitHub 的平台，但只是简单地进行代码版本控制，最多也就是结合 Jinkins 进行一番自动部署，直到现在有机会在项目中接触并开始将工作的点滴都体现在 GitLab 平台上，才感受到如此多的便利。

在工作中主要将项目的任务使用 issue 体现，从 issue 的 Board 上可以观察到 issue 进行到哪个流程，issue 关联MileStone 后，通过 MileStone 可以体现项目阶段性的进展，这是项目任务及流程的方面，在代码管理方面 GitLab 也有一番自己的作为。issue 关联的 Merge Request 可以观察 issue 关联分支的代码变化，代码比对当然也可以在平台上进行，甚至可以对代码进行 Review 以及在相应代码片段进行备注，CI/CD 也被集成在平台，不过工作中只是接触过 CI 的功能。

虽然工作中已经接触了一个项目在 GitLab 上经过的大部分流程，但官方的介绍将一个项目在 GitLab 上生命周期分为了十个部分，接下来我结合工作中的使用粗略介绍一下，如想详细了解 GitLab Workflow 可[点击此处](https://about.gitlab.com/2016/10/25/gitlab-workflow-an-overview/)。

![](https://about.gitlab.com/images/blogimages/idea-to-production-10-steps.png)

### 工作中的使用

我在工作中主要经历从 plan 至 review 这部分流程，项目中我们对于需求功能的讨论没有体现在 GitLab 上，只是讨论完成后会按阶段建立 Milestone，并将此阶段的工作分解为 issue，相当于需要完成的任务，issue 可以关联 Milestone 以体现此阶段工作的进展。

![关联 Milestone 的 issue](https://i.loli.net/2019/07/06/5d20a252630af70907.png)

![被 issue 关联的 Milestone](https://i.loli.net/2019/07/06/5d20a252761b758098.png)

作为一名 coder 执行的 issue 多数为代码任务，issue 可以通过创建关联的 Merge Request 和 branch 关联上代码，了解到当前工作的具体内容。

![关联 Merge Request 和 branch 的 issue](https://i.loli.net/2019/07/06/5d20a32edceb154915.png)

![被 issue 和 branch 关联并且提交过代码的 Merge Request](https://i.loli.net/2019/07/06/5d20a32ef21ea28745.png)

如果只是想了解 issue 进展到哪个阶段可以结合 Label 和 Board 对 issue 的进展有更加直观的感受。GitLab 在平台上整合了 CI，如果是 Java 代码可以结合 Junit 和 Maven，使代码推送远端之后 GitLab 对代码进行单元测试。

![issue 标注 label 后在 Board 上的直观展示](https://i.loli.net/2019/07/06/5d20a3c2c35e324673.png)

推送远端成功的代码可以通过 GitLab 的在线比对工具进行查看，甚至可以在 Review 代码时直接在代码段旁添加自己对于这段代码的不同见解。

![对代码片段的 Review 及备注](https://i.loli.net/2019/07/06/5d20a3c2b599a74346.png)

### 体验

GitLab 把众多功能集成，可以说一个项目的生命周期已经可以在平台完整体现，分不同平台对项目的不同生命周期进行管理。GitLab 的操作也相当简单，留言等有文字性内容的地方都支持 Markdown 语法，许多功能支持拖拽。GitLab 名字中带有 Git 也不是白叫的，GitLab 上的代码管理工具就是 Git，所以在代码管理这块可以很好的契合 Git 工作流。