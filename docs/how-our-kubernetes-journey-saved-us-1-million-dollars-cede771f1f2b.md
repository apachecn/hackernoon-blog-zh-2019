# 我们的 kubernetes 之旅如何为我们节省了 100 万美元

> 原文：<https://medium.com/hackernoon/how-our-kubernetes-journey-saved-us-1-million-dollars-cede771f1f2b>

2018 年初，我们的工程团队踏上了改造基础设施和工具的征程。本质上，我们的*开发者体验*远非我们所需要的。

![](img/27e9d162b63dfbb7dedaa9b782dff901.png)

糟糕的开发体验导致:工程师们绕过管道直接将他们的代码变更同步到 EC2。单元测试很少使用，因为管道需要很长时间才能给出反馈。调试并不容易。如果在产品中发现了一个关键的 bug，那么祝你好运，在顺利的日子里，你可以在 1 小时内修复它。

我们想做得更好，我们的目标是…

> 快速移动，让管道在不到 5 分钟的时间内交付软件

…这就是我们开始 kubernetes 之旅的原因。

**TLDR** :想节省 100 万美元并拥有一个快乐的开发团队吗？提供正确的工具。

# 那么，我们真的节省了 100 万美元吗？

撇开明显的点击诱饵标题不谈，如果我们只看我们的交付渠道，而忘记我们降低的基础设施成本，我们可以节省这笔钱。我们所有的运输管道都是用竹子(一种古老的亚特兰蒂斯产品)铺设的。它看起来像这样:

*   代码推送
*   手动运行并点击竹子中的运行构建🐌
*   竹子造了一个 EC2 AMI🐌
*   启动负载平衡器、EC2、RDS 数据库、Memcache 等(以及🐌)

这个过程平均需要大约 45 分钟(如果开发人员幸运地没有排在其他构建的后面)。

我们搬到了 kubernetes/circleci，看起来是这样的:

*   代码推送
*   触发自动构建
*   单元测试运行(1 分钟)⚡
*   建立一个码头工人形象，并推动(2 分钟)⚡
*   部署到 kubernetes 集群(1 分钟)

区别在于，没有额外的 EC2，没有额外的负载平衡器和 4 分钟的时间表。

# 那一百万美元呢

我们的团队大约有 28 名工程师，假设其中 25 人每天做 5 次构建，一年 265 天。我们的构建快了 40 分钟。快速计算:

> 5 次构建 x 40 分钟节省= 200 分钟 x 25 名工程师= 5，000 分钟 x 一年 265 天= 1，325，000 分钟/ 60 = 22，083 小时 x 平均每小时 50 美元员工成本=**110 万美元**

我听到你说，“但是工程师不是坐以待毙，他们是多任务的”。

他们这样做，但回头看；花了整整 45 分钟才发现一个构建在 AWS 上运行时是否有效，当时我们没有 docker。现在，工程师可以确信他们的测试通过了，代码在标准化的管道上工作，并在推送代码的 4 分钟内与其他团队共享开发 URL。

迁移到 Circle CI/Kubernetes 为我们提供了一个现代化的渠道，团队可以引以为豪。该团队可以在几分钟内解决关键的生产问题，而不是几个小时。

# 心中要有一个目标，提供实现目标的工具，并让您的工程师快速前进。