# 如何在 Lambda 调试上节省几十个小时

> 原文：<https://medium.com/hackernoon/how-to-save-dozens-of-hours-on-lambda-debugging-79c38f89c653>

尽管从基础设施的角度来看，AWS Lambda 是一个福音，但是在使用它的时候，我们仍然不得不面对软件开发中最不想要的部分:调试。为了解决问题，我们需要知道导致问题的原因。在 AWS Lambda 中，这可能是一个诅咒。但是我们有一个解决方案可以为您节省几十个小时的时间。

![](img/dc64b26f1483c3ddd1b10046720bb879.png)

Photo by [Aron Visuals](https://unsplash.com/photos/BXOXnQ26B7o?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# Lambda & CloudWatch 日志剖析

AWS Lambda 本质上是一个托管的容器服务。所有工作都在后台进行——我们不需要配置或管理容器本身，甚至不需要它们背后的基础设施。但在现实中，有无数的微容器运行在传统服务器之上。

每当有人请求一个 Lambda 函数时，AWS 将使用一个微容器来运行我们的代码，获得响应并发送回请求者。在此过程中，我们的应用程序(或我们使用的任何第三方库)可以记录任何内容，从信息性消息到反对警告，再到错误和异常堆栈跟踪。这些木头都放在哪里？

在他们推出 Lambda 的时候，AWS 已经有了日志服务，我们称之为 CloudWatch。因此，他们所做的是将所有这些文本数据(日志)从我们在 Lambda 中运行的应用程序传输到 CloudWatch。每个 Lambda 函数都有其专用的“日志组”。可以把它看作是 Lambda 日志的“存储库”。

在一个“日志组”(或单个 Lambda 的日志报告)中，CloudWatch 创建多个“日志流”。可以把它想象成一个实际写入日志的文本文件。Lambda 每次创建一个新的微容器，CloudWatch 中也会创建一个新的日志流。不管该容器服务 1 个还是 100 个请求，都只有一个日志流，这意味着:一个流可能包含来自多个调用的日志。

如果多个请求同时到来(也称为并发)，AWS 将旋转多个容器，为同一个 Lambda 创建多个日志流。最重要的是，每个容器可能会存在几分钟，甚至几个小时，而且它可能会为成千上万个请求提供服务，这取决于 Lambda 函数的使用频率。

# 问题是

现在假设您注意到您的应用程序失败了，您需要知道是什么导致了这个问题，以便计划一个修复方案。当然，首先要看的是日志。但是现在你去 AWS CloudWatch，你知道错误发生在今天，在早上 8:00 到 8:05 之间的某个时间。很简单，对吧？只需要 5 分钟的时间去寻找。好吧，这里有一些问题可能会让你浪费很多非生产性的时间。

首先，同一时间段可能有多个日志流，因此您需要浏览所有日志流以找到特定的失败请求。似乎这还不够麻烦，在每个日志流中，您可能需要浏览几十、几百甚至几千个日志才能找到您要找的日志。

请记住，日志流不一定是在请求到来时创建的。包含失败请求日志的流可能是在几个小时前创建的，并且它将包含上午 8:00 之前的日志。

光是在 CloudWatch 中查找日志就可能会花费你很多时间。你甚至还没有开始解决问题本身！想象一下，你的团队中有人问你修复 bug 的进展如何，你花了 30 分钟，也许一个小时，仍然不知道是什么导致了这个问题？有人可能会说这对于专业开发人员来说是不可接受的。

# 解决办法

正如我在介绍中告诉您的，有一种方法可以解决所有这些问题，并更快地获取您的日志。

![](img/88f1f04cde98658fd1f393a1b66099fd.png)

Photo by [Mohamed Nohassi](https://unsplash.com/photos/odxB5oIG_iA?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/awesome?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Lambda 函数的每次新调用都会产生一个初始对数线。这是默认记录的，你不能控制它。无论您的运行时是什么(Python、Node、Java 等)，日志看起来都是一样的。方便的是，这个初始对数线看起来总是这样:

![](img/32733f4cc1a82dca9f0e88f479f99a4b.png)

“START RequestId”总是相同的，每次调用后都有不同的散列。当调用终止时，不管调用是成功还是出错，都会生成另一个默认的日志行，如下所示:

![](img/36da42f9390bcfc249df3a222df0149b.png)

因此，现在我们有办法扫描任意数量的 CloudWatch 日志，并确定调用的开始和结束位置。酷毙了。

现在，另一个有用的知识是:CloudWatch 日志[支持订阅](https://aws.amazon.com/pt/about-aws/whats-new/2015/06/amazon-cloudwatch-logs-subscriptions/)。这意味着你可以给你的日志订阅 [AWS Kinesis](https://aws.amazon.com/kinesis/) ，这样它就会*一直监听*你的 Lambdas 输出的所有内容。然后你可以配置一个特殊的 Lambda(称之为“日志解析器”)，当 Kinesis 接收到新的日志数据时自动调用它。Log Parser 将获取日志并将它们分成调用(使用上面的开始/结束符号来识别断点)。最后，与一个且仅一个调用相关的每条日志都可以单独保存在数据库中(比如 [CloudSearch](https://aws.amazon.com/cloudsearch/) )，以供以后搜索和调试。

**嘭！** **现在您已经通过调用个性化了您的日志。**不再为了得到一个调用数据而对成千上万的日志进行无效筛选。下面是这种概述实现的示意图:

![](img/1ca9f8f833c96d4729bd6b7a4c54f9e5.png)

Created with CloudCraft ([https://cloudcraft.co](https://cloudcraft.co/))

# 在一分钟内实施该解决方案

我知道，设置并运行该解决方案可能需要几个小时(也许几天)，并且需要一段时间来维护并确保其平稳运行。

因为我们知道您还有许多其他更重要的事情要做，所以我们将该解决方案作为托管服务来实施，并增加了从异常检测到事件管理和警报等一系列功能。我们称之为 dash bird T1，成千上万的开发人员已经在使用并喜爱它:我们正在监控+200，000 个 Lambda 函数，并且还在继续。欢迎你免费使用，想用多久就用多久。