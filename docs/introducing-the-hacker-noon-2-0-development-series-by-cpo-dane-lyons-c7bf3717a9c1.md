# 简介:CPO Dane Lyons 的黑客正午 2.0 开发系列

> 原文：<https://medium.com/hackernoon/introducing-the-hacker-noon-2-0-development-series-by-cpo-dane-lyons-c7bf3717a9c1>

## 但首先，一些快速的公司更新:

1.  *对于所有作者:您现在可以通过访问*[*【contribute.hackernoon.com】*](http://contribute.hackernoon.com)*向黑客中午提交故事/检查提交状态！在* [*中了解更多关于如何向黑客提交故事中午*](https://hackernoon.com/how-to-contribute-a-story-to-hacker-noon-1536c2587063) *。*
2.  *还有，我们是* [*还是*](https://hackernoon.com/calling-for-hacker-noon-community-we-are-hiring-dcce55252a75) *！目前的角色包括* [*以用户为中心的前端开发人员，他们对设计有着浓厚的兴趣*](https://jobs.hackernoon.com/jobs/7582854-user-centric-frontend-developer-with-a-great-taste-for-design-at-hacker-noon) *，以及* [*具有社交媒体专业知识的内容策划人员*](https://jobs.hackernoon.com/jobs/7235163-content-strategist-with-social-media-expertise-at-hacker-noon) *。* [*应用今日 jobs.hackernoon.com*](http://jobs.hackernoon.com)*或向下滚动到最后获得更多。*
3.  *查看我们最新的播客:“* [*与 TrustToke 的 Danny An 一起破解资产令牌化*](https://podcast.hackernoon.com/e/hacking-the-tokenization-of-assets-with-danny-an-of-trusttoken/) *”。没有* [*数字海洋*](https://do.co/2TTnFCL) *，这一集是不可能的。点击*[*iTunes*](https://itunes.apple.com/us/podcast/hacking-tokenization-assets-danny-trusttoken/id1436233955?i=1000428667040&mt=2)*或* [*谷歌播客*](https://play.google.com/music/m/Dpduijvmxazvajl7jeoukgmourm?t=Hacking_The_Tokenization_of_Assets_with_Danny_An_of_TrustToken-Hacker_Noon_Podcast) *。*

![](img/84623fe42ff8df180a6bcd3b98a3fe42.png)

Artist rendition of Nikola Tesla reading Hacker Noon

以今天的标准来看，互联网已经过时了。早在 20 世纪初，尼古拉·特斯拉就设想了我们现在认为理所当然的世界。20 世纪 60 年代，随着阿帕网的出现，这些愿景开始变成现实。TCP/IP 是在 1983 年 1 月 1 日我出生后不久发明的。

万维网是在我三年级的时候推出的。一个具有历史意义的重要事件，但对我来说，这无关紧要。我们在小学有一个小电脑室，每个教室有一台电脑。我从未想象过被接入全球网络。对我来说，电脑的设计是为了让我能尽快完成我的数学工作，这样我就可以玩一些类似 atari 的游戏。我很高兴生活在那些受限的数字世界里。

![](img/bb9f301c468803a059816a63442e39e6.png)

初中的时候，开始听到谣言。一些孩子谈到“网上冲浪”，我想我把互联网等同于 Pogs。有什么意义？我甚至不记得我第一次上网的经历。在我看来，电脑是为个人体验而设计的，而不是共享体验。我想象的最共享的体验是设计一个兽人或精灵的高分辨率图形，并以某种方式展示给我的朋友们。

![](img/f0b24f71b4b39821df96b5566109aa22.png)

初中快结束的时候，这些碎片开始组合在一起。我上过一堂计算机课，向我介绍了 LOGO 和 BASIC 编程。太迷人了。能够用简单的逻辑创造东西改变了我的一切。我觉得一切皆有可能。我立即开始想象游戏的含义。

高一的时候，买了一台 TI-86 计算器。我一生中最划算的购买之一。全天候访问计算设备似乎好得难以置信。在数学方面，我使用了和小学一样的策略，我编写程序来仔细阅读我的作业，这样我就可以花时间来创建游戏。我做了一个 RPG 叫竞技场。在游戏中，你控制 4 个角斗士在竞技场中对抗神话中的野兽。为了用如此有限的资源构建我心目中的东西，我必须想出很多有创意的方法。

![](img/faf8ba3456b479deedfb80ad41fe4a4e.png)

**快进 20 年。**

我最终找到了 web 开发的道路，现在我在 Hacker Noon 领导产品团队。我今天面临的技术挑战是我在高中时无法想象的。也就是说，使用[区块链](https://hackernoon.com/tagged/blockchain)来建立一个分散的发布平台是否有意义，或者我们应该专注于建立在云上的集中体验？这个问题对我的高中大脑来说毫无意义。

这让我想到了我最喜欢的[科技](https://hackernoon.com/tagged/technology)的一句名言。艾萨克·牛顿的经典之作，“如果我看得更远，那是因为我站在巨人的肩膀上。”我知道，从技术上来说，这是一句科学引语，但让我们想象一下，牛顿今天还活着，正在谈论他的最新技术突破。

![](img/8a9aed130977f1b657969278c891fce2.png)

你可能会想，我喜欢这句话，作为“不要重新发明轮子”的一种说法。通过拼凑已知的设计模式和成熟的技术来构建应用程序”。这绝对不是我的理解。如果你变得过于执着于复制现有的创新，你就会陷入对过去的回顾。

牛顿引用的重要部分是“看得更远”这一点。我们需要利用我们所知道的一切作为一个平台，尽我们所能去窥视未来。很艰难。过去就像用望远镜看着晴朗的星空。未来在一片浓雾之后。我们看不到另一边。但是每前进一步，我们就多透露一点，看得更远一点。

回到我最初的故事，我没有看到互联网的未来，因为我没有站在一个基础上获得有利位置。我注意到这个模式适用于大创意，也适用于小创意。这就是我对产品迭代的想法。你不能总是从心中的最终目标开始工作，因为你可能看不到最终目标。相反，尽你所能展望未来，并朝着那个愿景努力。每前进一步，你看得更远一点，你的下一步变得更清晰。

作为一名读者，我可能会想“纸上谈兵听起来不错，但这如何转化为现实世界呢？”。现在故事和理论已经够多了。我们正在开始[黑客正午 2.0 开发系列](https://hackernoon.com/development-series/home)。我将发布我们在构建 Hacker Noon 2.0 及更高版本时面临的现实挑战。您将看到特性、设计和工作流从一个粗糙但功能性的状态发展到更好的状态。您的评论和见解将帮助我们看到更远的未来。也许我们甚至会在这个过程中发现一些创新。

![](img/e412b60fa310e474f5bef2610aec3d9f.png)

[**黑客正午 2.0 开发系列**](https://hackernoon.com/development-series/home) **(即将推出):**

*   产品理念:双轨敏捷
*   原型:表情符号给予互动
*   Git 保护

**附言**我们正在寻找 [**以用户为中心的前端开发人员，对设计有很大的品味**](https://jobs.hackernoon.com/jobs/7582854-user-centric-frontend-developer-with-a-great-taste-for-design-at-hacker-noon) 来帮助我们塑造[**黑客正午 2.0**](https://www.startengine.com/hackernoon) **。**感兴趣？太棒了。在这里申请，如果你对这个角色很感兴趣，就设计一个你希望在 Hacker Noon 2.0 中看到的功能原型，写下你为什么认为它很重要，然后[提交一篇 Hacker Noon 帖子](http://contribute.hackernoon.com)发表在 Hacker Noon 1.0 上。