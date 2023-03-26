# 开发 Chrome 扩展时要记住的事情

> 原文：<https://medium.com/hackernoon/things-to-keep-in-mind-while-developing-chrome-extensions-1ec15199aa66>

![](img/9672d22e55de2e9025e08a159182c01f.png)

> Chrome 扩展是安装到 Chrome 中的程序，可以增强浏览器的功能。这意味着添加新的功能或修改现有的功能，使其对用户更方便。
> 
> 作为一家知名的定制 chrome 扩展开发公司，我们经常被客户询问为什么要开发 chrome 扩展。
> 
> 在这篇文章中，我们将给出 chrome 扩展开发的目的，以及在雇佣 chrome 扩展开发者之前需要记住的事情。

通过插件和扩展，Chrome 的功能可以得到广泛的提升，这使得执行各种黑客攻击成为可能。它们改变了人们的工作方式。Chrome 扩展可用于任何东西。

让我们以 Gmail 为例——通过安装几个专门针对 Gmail 的流行 Chrome 扩展，有无数种方法来增强和扩展它的功能。电子邮件跟踪，会议安排，安排你的外发电子邮件，离线电子邮件查找，语法检查等等，这些都是你不需要打开任何额外的 chrome 标签就可以访问的。

扩展有能力将业务推向新的高度，在正确的 chrome 扩展的帮助下，您可以自动化和简化业务领域，否则将需要大量的手动工作。看看我们这里的一篇文章，就知道 chrome 扩展在[的重要性，以及企业如何从 chrome 扩展开发中获得额外收入。](https://www.binaryfolks.com/blog/why-chrome-extension-is-important-for-small-businesses-how-to-monetize-your-chrome-extension?utm_source=hackernoon&utm_medium=social&utm_campaign=Chromeextensiondevelopmentthingsinmind!)

[在这里雇佣 Chrome 扩展开发者。](https://www.binaryfolks.com/services/hire-experts/hire-chrome-extension-developers?utm_source=hackernoon&utm_medium=social&utm_campaign=Things_in%20Mind_Chrome_Extensions)

**想为你的企业开发 Chrome 扩展吗？先回答我下面的几个问题。一步一步来，你知道！**

1.  为什么需要开发特定的 chrome 扩展，chrome 扩展将为您的用户解决什么业务问题。
2.  chrome 扩展将如何改变用户的生活？
3.  在 chrome 扩展的帮助下，你的企业会得到什么？如果它不存在，你会失去什么？

为什么我要求你分析这些，是因为理解 chrome 扩展开发背后的目的将使你与你的目标保持一致，并帮助你保持专注。永远记住，最好的想法是能解决你每天面临的问题的想法。

现在，假设你自己不是 chrome 扩展开发者，你将需要雇佣 chrome 扩展开发者来为你做这项工作。Chrome 扩展基本都是用 [HTML](https://hackernoon.com/tagged/html) ，CSS，JavaScript 写的。所以，当你雇佣一个 chrome 扩展开发者来做开发工作的时候，这三种技术应该是你首先考虑的。如果你想了解更多关于[雇佣定制软件开发人员的经济有效的方法](https://www.binaryfolks.com/blog/5-effective-ways-to-reduce-custom-software-development-costs?utm_source=hackernoon&utm_medium=social&utm_campaign=Chromeextensiondevelopmentthingsinmind!)，看看我们的文章。

从最艰巨的任务开始——创建详细的业务规范。无论合约是一年还是两周，BRD(业务需求文档)都很重要。虽然与最先出现的 chrome 扩展开发团队签订合同并开始与他们合作确实很方便，但最好在开始时就做好尽职调查，以便确保您将获得您预想的 chrome 扩展。

**在继续开发 chrome 扩展之前，看看你应该记住的一些提示。**

![](img/c49c5f8d3509865ad20239a932f70617.png)

**【1】对 chrome 环境有所了解**

在 chrome 扩展[开发](https://hackernoon.com/tagged/development)开始之前，确保你对 chrome 架构有起码的了解。我并不建议去阅读所有关于 chrome 扩展开发的文档，但是对它的总体工作原理有一个模糊的概念会对你有好处。

![](img/7ed2cdc30c783c8327327466d4fe9227.png)

**[2]确保它不会增加浏览器内存消耗**

要记住的一件非常重要的事情是，尽管 chrome 扩展很有用，但 chrome 扩展的开发有时会增加浏览器的内存消耗，从而使浏览器变慢。

即使在为 chrome 扩展开发目的编写第一行代码之前，也要确保你的 chrome 扩展开发人员为该网站使用的扩展使用相同版本的 javascript 库。

如果不需要加载复杂的库，也建议使用普通的 Javascript。此外，可以尽量减少使用页面上已经存在的库。

**【3】支持谷歌账户**

如果你的 chrome 扩展需要用户登录，那么一定要支持谷歌账户。这是因为当用户从 chrome 网上商店购买或安装 chrome 扩展时，用户很可能已经登录了谷歌账户。因此，减少登录次数可以改善用户体验。

![](img/de21670642d747c8936f7c89d1540708.png)

**[4]知道用户凭证存储在哪里**

因此，你可以将数据存储为 window.localStorage 或 chrome.storage.local。我们在[binary 乡亲](https://www.binaryfolks.com/about-us?utm_source=hackernoon&utm_medium=social&utm_campaign=Chromeextensiondevelopmentthingsinmind!)强烈推荐 chrome.storage.local。这是因为 window.localStorage 是 HTML5 存储。除非在后台运行，否则可以将数据设置到特定域的存储中。因此，除非在后台页面中使用 localStorage，否则无法在不同的 web 页面之间共享数据。

另一方面，chrome.storage.local 是为 chrome 扩展设计的，用于将数据存储在更集中的位置。这样，每个扩展都有自己的存储空间。有了无限制的存储权限，它可以保存任意大量的数据。此外，如果启用了扩展同步，它可以在登录的 Chrome 实例之间自动同步。

此外，根据 chrome 的基本准则，即使在用户取消订阅或卸载 Chrome 扩展后，用户数据也应至少存储 30 天。用户退订或卸载可能有很多原因，即使他们的退出是有意的，他们有时也会回来。

**[5]发布 chrome 扩展到商店时遵循基本准则**

最后但同样重要的是，一旦 chrome 扩展开发完成，明智地选择你的主要应用类别，因为这决定了你的应用将出现在网络商店的哪个位置。每个类别被进一步组织成逻辑组，您将显示在已过滤类别的组标题下。

此外，列出关键字以提高搜索相关性，或影响类别列表的未来版本。大多数情况下，所有的 chrome 扩展都会经过一个自动审查过程，然后几乎立即在 chrome 商店发布。在某些情况下，发布应用程序前可能需要进行手动审核。

**-::临别赠言::-**

为了在竞争中保持领先，以客户为中心的 chrome 扩展开发是必须的。无论你是刚刚起步还是已经确立的市场领导者，你都需要一个

一致的设计，成功完成的详细项目规范，以及及时开发和发布的清晰 chrome 扩展开发愿景。

现在你已经有了一些关于 chrome 扩展开发的知识和一个好主意，我想，你还在等什么呢？是时候为你的 chrome 扩展开发找一个合适的开发团队，把你的想法变成现实了。在这里看一些我们的 Chrome 扩展开发工作的例子。

*最初发表于*[T5【www.binaryfolks.com】](https://www.binaryfolks.com/blog/chrome-extension-development-things-to-keep-in-mind)*。*