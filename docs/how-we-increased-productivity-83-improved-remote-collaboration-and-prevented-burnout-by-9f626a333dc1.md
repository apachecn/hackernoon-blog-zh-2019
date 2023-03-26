# 我们如何提高 83%的工作效率，改善远程协作，并通过加快速度来防止疲劳

> 原文：<https://medium.com/hackernoon/how-we-increased-productivity-83-improved-remote-collaboration-and-prevented-burnout-by-9f626a333dc1>

![](img/52d968f6daa9a536ae77f15163bc132d.png)

Photo by [Mockupeditor.com](https://www.pexels.com/@mockupeditor-com-56805)

成千上万快乐的客户验证了我们的软件——但是 dogfooding [Velocity](http://codeclimate.com) 验证了我们的价值观。

几年前，我们的工程领导者是轶事驱动的，就像行业中的其他人一样。我们知道一个工程师在一个新功能上取得了多大的进展，因为我们问了。我们知道为什么我们错过了发布新功能的最后期限，因为我们在回顾中讨论了它。

但是我们不能回答简单的问题，比如:

*   随着时间的推移，我们的效率是在提高还是在降低？
*   什么工作让我们慢了下来，最大的改进机会在哪里？
*   现在，我团队中的哪些开发人员陷入了困境？

即使在一个小的开发团队中，我们对自己生产力的洞察力也是有限的。

去年，我们推出了 Velocity，这是一款[工程分析工具](http://codeclimate.com)，旨在帮助大大小小的团队解决我们自己经历的问题。在迭代和改进平台的过程中，我们的工程经理 Ale Paredes 为我们的应用提供了支持。Velocity 的每一项功能一建立，她就开始使用，并亲身体验了数据驱动工程的好处。

有了 Velocity，她解决了倦怠问题，改进了远程协作，并帮助重组了工程团队。结果是生产力提高了 83%,开发团队更加健康，更加投入。

下面是狗食[速度](http://codeclimate.com)对代码氛围的影响。

# 我们平抛，效率几乎提高了一倍

扁平结构不可扩展。在每个工程团队的成长过程中，都会出现这样一个点:曾经建立了友谊和共同目标的相同设置，现在助长了脱离和缺乏专注。

Ale 第一次注意到这种脱离是在日常站立时。一些人正在分区；其他人在打电话。几乎没有讨论发生。这是 20 分钟的工程师轮流发言，一个接一个。当她在那周的一对一会谈中提出这个问题时，她发现团队达成了一致意见:会议效率低下，占用了太多时间。

Ale 通过查看过去 6 个月的每日推送量来对照指标检验她的假设。

![](img/a291b02e427a60a537e26bc70faa4ce8.png)

A custom [Velocity](http://codeclimate.com) report Ale built to see how Pushes per Day has been affected as the team scaled.

Velocity 向她证实，在过去的四个月中，团队的效率实际上一直在稳步下降。

Ale 将这一见解带回给了领导团队，他们一起决定是时候放弃了。软件开发本质上是协作性的，如果工程师需要支持太多的同事，他们就无法感受到对彼此工作的投入。

我们决定把十人小组分成三四个工程师小组。我们假设根据 Bezo 的 2-pizza 法则(T1)解散团队将有助于单个工程师专注于更少的工作。因此，会议将占用更少的时间，更有吸引力，因为较小的团队只讨论与少数人相关的问题。

当 Ale 实施这一变革时，反馈是非常积极的。工程师们做更多他们喜欢的事情(编码)，减少他们讨厌的事情(会议)。在几个月的过程中，度量证实了团队的感受:我们开始更快地前进。

![](img/29c9b92950303c207d59dda0c55b6ccc.png)

A custom [Velocity](http://codeclimate.com) report Ale built to see how Pushes per Day has increased since the re-org.

我们从 11 月的平均 **12 推/天**增加到 1 月的平均 **21 推/天**，增加了 83%。

# 我们指导远程团队成员进行更好的协作

敏捷软件开发依赖于强有力的沟通，并且在许多情况下，依赖于频繁的面对面交流，所以管理远在大洋彼岸、三个时区之外的团队成员是很不容易的。很难通过懈怠来感知挫败感。通过 Zoom 很难理解工作量。

Ale 尽了最大努力与远程团队成员保持联系，并找出进展顺利的地方以及他们可能需要指导的地方。但并不是每个人都是开放的——在这些会议上，她的许多远程团队成员都不愿意提出麻烦的拉取请求或瓶颈。

为了找到这些对话的起点，Ale 开始每周查看一些贡献者指标。她会特别关注总体吞吐量(合并的 PR 的数量)、返工(开发人员处理他们自己最近合并的代码的频率)和周期时间(代码从开发人员的笔记本电脑到成为合并的 PR 需要多长时间)。

通过整理速度图表，她开始注意到一种模式。远程开发人员比总部团队成员有更长的周期时间。

![](img/b5c0ecbf7589bf0abaeb60277f11663a.png)

A custom [Velocity](http://codeclimate.com) report Ale built to see Median Cycle Time by team member.

在与 Legolas Greenleaf 进行 1:1 会谈之前，Ale 深入研究了 Velocity，以了解贡献者在 PR 流程中的确切位置。

![](img/e8140cb4a3a7adcd1c2bdf2156b1ae73.png)

A [Velocity](http://codeclimate.com) chart that shows how contributors are performing on metrics compared to the industry standard.

Ale 发现 Legolas 在打开一个拉取请求并让别人查看他的工作之前，花了很多时间在他的笔记本电脑上写代码。

下一次他们坐下来，Ale 讨论了她注意到的趋势。远程团队成员能够更具体地讲述是什么让他慢了下来，以及他面临着哪些挑战。在他们的谈话中，她引导勒苟拉斯学习如何更好地接近最后几个公关。

他们一起发现问题的核心是他的 PRs 太大了。将项目分成更小的部分是总部团队经常听到的工作习惯，但是 Ale 和远程工作人员之间的距离意味着他们在最佳实践方面不一致。这些数据引起了 Ale 对这个问题的注意，并给了她一些客观的东西来为她的教练课程做准备。

# 我们防止了工程师的倦怠

按时完成任务和开发优秀的产品并不是优秀团队的唯一衡量标准。我们还希望工程师们积极参与，不要过度劳累，这样我们的步伐才是可持续的。然而，太多时候，筋疲力尽是不可能察觉的，直到为时已晚。

去年 12 月，该团队进展顺利。我们每隔一天发布一次功能，我们已经重新设计了应用程序中的大部分页面，每个人都对我们正在构建的所有酷的新东西感到兴奋。一切似乎都很好。

然而，在每周例行检查中，Ale 开始注意到一个趋势。

工程师们对他们的工作感到兴奋，但每个人似乎都感受到了发布功能的巨大压力——即使我们比计划提前了。一位高级工程师提到，他几乎没有时间进行代码审查。其他团队成员开始忽略结对编程，而倾向于花更多的时间把东西拿出去。

Ale 使用 Velocity 来查看团队的情况:

![](img/13daa4fe75295cbc6d3f7a9ccc4b6167.png)

[Velocity](http://codeclimate.com)’s Activity Log showed leadership how much individuals were working.

她注意到一些开发人员有太多的活动，几乎排不进他们的行列。在一些情况下，我们甚至在周末也在努力，这对我们的组织来说是极不寻常的。

查看工程团队制作的所有 PRs 和评论，Ale 开始怀疑人们在加班。她使用 Velocity 的 Reports Builder 创建了一份报告，查看每周晚上 8 点后有多少推送:

![](img/5ca174c67e6b0c46087bb80942da0d0b.png)

Ale built a custom Velocity report to see who was working late hours.

在过去的两周里，这个团队加班的时间比之前三个月加起来还多。Ale 将这些数据带给了领导团队，并成功地倡导缩减工作，给工程团队一个应得的假期。

现在，Ale 定期检查这些仪表板，并发出每月调查，检查工作如何分配以及团队成员的感受。

# 衡量和动员

指标不是一切。度量标准并没有帮助 Ale 注意到工程师在会议期间没有参与，或者远程团队成员没有提出问题。相反，度量帮助她更透明地检查自己的直觉，了解正在发生的事情。他们还让 Ale 证实了她所做的改变提高了我们的工作效率。

这种直觉检查在帮助我们更快地行动和更好地沟通方面是非常宝贵的，无论是在工程团队内部还是与整个组织。

如果您想了解更多有关 Velocity 如何让您更深入地了解您团队的工作习惯(并帮助您至少加快 20%)的信息，请在此注册观看演示[。](https://codeclimate.com/velocity/signup/)

*最初发表于*[T5【codeclimate.com】](https://codeclimate.com/blog/dogfooding-to-improve-developer-productivity/)*。*