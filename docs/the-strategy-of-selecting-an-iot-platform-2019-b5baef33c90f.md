# 选择物联网平台的策略(2019)

> 原文：<https://medium.com/hackernoon/the-strategy-of-selecting-an-iot-platform-2019-b5baef33c90f>

在过去的两年里，我与 100 多家物联网相关公司的技术人员、销售人员和高管交谈过，我了解到两件事:1)物联网是巨大的，2)几乎每个人对物联网的定义都不一样。

在这篇文章中，我分享了我对物联网平台的了解，以及对它们进行分类和思考的一种方法。

# 什么是物联网平台？

物联网比我们大多数人认为的传统互联网更加广阔。它的增长速度也在加快。还有更多的设备、协议、安全问题、RF 频率、架构组件、服务、数据和相关产品。物联网是巨大而广泛的。

因此，“物联网平台”这个术语对大多数人来说实在是太宽泛了。我在下面概述了一个物联网平台的例子，但是，在某些方面，“物联网平台”的概念与大约 2004 年的“互联网平台”一样有意义。事实上，物联网有更多的移动部件。

![](img/36e7f27b872d688e5c09d4d0f467410f.png)

One high-level example of what an IoT Platform should manage

许多物联网项目中最具挑战性的部分之一是管理硬件设备。这主要意味着安全地供应、维护和更新传感器和边缘设备。因此，许多物联网文献(以及许多新产品和初创公司)都围绕着基于云的 ui、边缘设备和传感器之间的通信——MQTT、安全密钥、OTA 固件更新、RF 频率等。

考虑下面我创建的价值链图，它显示了一些常见的物联网技术组件在其生命周期中所处的位置。此图左下方的组件变化如此之快，物联网平台很难一致地管理它们。

![](img/c672310a5adc30be4230d8bc3e72c7a3.png)

物联网平台最重要的工作之一是让供应、更新和管理大量设备变得容易，并确保一切安全。正如您将看到的，平台在这方面还有很长的路要走，原因在于这些活动在其生命周期中所处的位置。

然而，一旦数据越过边缘进入云中，物联网项目就会变得更加熟悉(理论上；稍后将详细介绍这一点)。数据流向 API 端点，并存储在云中(通常)。因此，平台应该有助于将潜在的大量数据流式传输和加载到数据存储中，并促进这些数据的检索。由于人工智能和机器学习对物联网解决方案可能很重要，平台也应该在需要的地方(边缘、云、设备、客户端)实现算法。最后，创建和管理仪表板或“单一控制台”可能是物联网项目的重要组成部分。平台应该有助于将仪表板连接到数据。

还有许多其他组件和活动，但这些都是一些常见的。

# 谁应该购买物联网平台？

物联网平台还为时尚早。例如，在 [Gartner 魔力象限](https://sightmachine.com/blog/why-iot-platforms-cant-summit-the-gartner-magic-quadrant/)中还没有工业物联网平台。根据 [Gartner 对新兴技术的炒作周期](https://www.gartner.com/smarterwithgartner/5-trends-emerge-in-gartner-hype-cycle-for-emerging-technologies-2018/)，物联网平台刚刚越过膨胀的期望的顶峰，正在向幻灭的低谷倾斜。

虽然听起来很糟糕，但如果你正在进行战略思考，并希望获得竞争优势，现在实际上是选择物联网平台的最佳时机。否则就是最坏的时候。带着膨胀的期望和没有战略进入一个高度分散和混乱的市场不是一个好兆头。

换句话说，如果你是一个早期采用者，请小心行事，但不要指望一个物联网平台在没有大量时间和精力的情况下自己就能做很多事情。如果你是一个落后者，当你读到这里的时候，你可以让你的[亚马逊机器人](https://www.cnbc.com/2019/01/17/amazon-remars-conference-starts-june-4-in-las-vegas.html)为你搭建平台。

![](img/06c12fa2472dc159ba8807522152fed8.png)

# 物联网平台市场

物联网市场复杂且高度分散。然而，对物联网价值链影响最大的云市场并不分散。按照收入运行率(CR3 为 0.792)，前三大云服务提供商[占据了大约 80%的市场份额](https://www.zdnet.com/article/top-cloud-providers-2018-how-aws-microsoft-google-ibm-oracle-alibaba-stack-up/)。这创造了一个有趣的动态，我将在后面讨论。

这种市场细分意味着组成任何特定物联网解决方案的组件和产品对该项目的具体要求高度敏感。构建一个系统的最佳组件可能与构建另一个系统的组件有很大不同。这并不总是正确的，但今天却是正确的。例如，下面是我做的价值链图，其中包括一个大型工业物联网项目正在考虑的公司/产品。这个图表是 10 个月前的，其中 30%已经对项目过时了。

![](img/085e629f286351ce24025cfe93041156.png)

Companies/products considered for a large enterprise IoT project

# 物联网平台的类型

如果我们将物联网平台定义为*任何可以在其上构建和运行物联网解决方案的东西，*那么在这个高抽象层次上，基本上有两个平台类别:

1.  **CSP 物联网平台*** (广义)
2.  **基于 CSP 构建的平台**** (专业)

****CSP —云服务提供商***

** *大多数物联网平台都是基于云基础设施构建的，即使它们有本地选项；许多物联网平台声称支持多个通信服务提供商，即使他们与一个通信服务提供商有合作关系*

## (1) CSP 物联网平台

三家[最大的云服务提供商](https://www.zdnet.com/article/top-cloud-providers-2018-how-aws-microsoft-google-ibm-oracle-alibaba-stack-up/)是亚马逊、微软和 IBM。这(3)家提供商占据了近 80%的市场份额，营收达到 520 亿美元(2018 年 12 月)。每个电信运营商都提供自己的物联网平台:

*   [AWS 物联网](https://aws.amazon.com/iot/) & [AWS 物联网核心](https://aws.amazon.com/iot-core/)
*   微软 [Azure IoT](https://azure.microsoft.com/en-us/overview/iot/)
*   [IBM 物联网](https://www.ibm.com/internet-of-things)

其他著名的通信服务提供商——甲骨文、谷歌和阿里巴巴——也提供物联网平台。

## (2)基于 CSP 的专业化物联网平台

其次，还有构建在 CSP 基础设施之上的物联网平台。有数百个这样的平台，它们由大大小小的公司提供。考虑以下物联网平台及其合作的电信运营商:

**工业物联网平台:**

*   GE Predix →天蓝色
*   [西门子思维圈](https://siemens.mindsphere.io/en) → AWS，Azure

**其他一些专门的物联网平台:**

*   [C3 物联网](https://c3.ai/) → AWS
*   [SAP Leonardo IoT](https://www.sap.com/products/iot-bridge.html)→[AWS、Google、Azure](https://blogs.sap.com/2017/05/17/sap-leonardo-internet-of-things-iot-runs-on-google-cloud-platform/)
*   [思科动力](https://www.cisco.com/c/en/us/solutions/internet-of-things/iot-kinetic.html) → [蔚蓝](https://www.zdnet.com/article/cisco-announces-kinetic-iot-operations-platform/)

**更小的专业化物联网平台:**

*   [水手](https://www.mariner-usa.com/) →蔚蓝
*   [Altizon](https://altizon.com/) Datonis→ AWS，Azure

## CSP 物联网平台与专用物联网平台

CSP 物联网平台提供广泛且高度灵活的服务和组件，可用于构建几乎任何物联网解决方案。如果您有时间、预算和资源，CSP 平台可以提供基本的构建模块。

专业化的物联网平台应该加快上市时间，并作为垂直行业和特定应用的外包专业知识。较小的专业化物联网平台公司往往在几个垂直领域运营，并专注于产品，而较大的专业化物联网平台，如 GE Predix 和西门子 Mindsphere，则提供面向整个行业的市场级平台，如工业物联网平台。

**如果平台是乐高**，CSP 物联网平台将提供小型乐高积木

![](img/19147e81d1c61772efd067f7a5b34b95.png)

CSP IoT Platforms

专门的产品会提供预先组装好的乐高玩具。

![](img/29acc5860121be1bae68937a9236dabc.png)

Specialized IoT Platforms

至少理论上是这样。实际上，事情并不是这样的。首先，电信运营商一直在更换乐高玩具。一分钟后会有更多。第二，专门的物联网平台没有充分适应垂直行业，无法提供许多优势。它们太笼统了。通常，它们是围绕一两个大型客户的特定需求开发的，然后作为平台销售，而实际上它们只是用典型的 CSP 堆栈构建的应用程序，可以通过项目开发工作进行扩展。基本上，他们只是使用与开发初始代码集相似的思想和架构组件为您编写项目代码。现在必须是这样。你马上就会明白为什么了。

## 电信运营商正在颠覆专业化平台

通信服务提供商控制着物联网价值链，并正在破坏建立在这些价值链之上的公司。例如，在 2018 年 11 月的 re:invent 上，亚马逊宣布了一个新的[时间序列数据库](https://blog.timescale.com/what-the-heck-is-time-series-data-and-why-do-i-need-a-time-series-database-dcf3b1b18563)，名为 [TimeStream](https://aws.amazon.com/timestream/) 。时序数据库比通用数据库更好地处理加载和读取物联网数据。在 TimeStream 之前，如果你想在 AWS 上建立一个时间序列数据库，就必须在 AWS 的 EC2 (Linux)上运行一个第三方产品，比如 [Prometheus](https://prometheus.io/) ，然后自己管理它。 *(TimeStream 目前仅在早期版本中可用；2018 年 1 月)*

2018 年，我与 30 多家在 AWS 上建立了专业物联网平台的公司进行了交谈。这些公司大多使用 MongoDB、DynamoDB(一个完全托管的 AWS NoSQL 数据库)或 [AWS RDS](https://aws.amazon.com/rds/) (大多是 Postgres、MySQL 或 Oracle)。我采访过的一些人，甚至是技术人员，从来没有听说过时间序列数据库。其他人抱怨 AWS 没有提供一个，并且 [AWS 物联网分析](https://aws.amazon.com/iot-analytics/)是一个太多的黑箱，无法开发。无论哪种方式，大多数供应商最终都在代码中编写复杂的逻辑来弥补时间序列数据库的不足。

TimeStream 的宣布立即排除了这些物联网平台架构的大部分，同时使新平台进入者更容易进入市场。这就是众所周知的云服务提供商向上移动。这对建立在通信服务提供商基础上的公司及其客户来说极具破坏性。这是你在选择物联网平台时应该注意的事情。电信运营商将继续蚕食专业化平台。

# 物联网平台是合作伙伴，而不是资产

我曾经共事过的一些高管有意将第三方软件视为一个黑盒，而忽略了组成它的组件。软件被视为一种金融资产，如卡车或仓库，具有成本、效用、折旧或运营费用，并且可以很容易地用可量化的金钱、时间和内部资源进行替换。根据我的经验，这种方法可以很好地用于商品化的软件解决方案，如人力资源、销售和会计系统，但它不是目前处理物联网平台的最佳方法。

![](img/ffec6f86b04c1c372a12c8de00459613.png)

新兴技术，如物联网平台，应该更像战略合作伙伴而不是资产或商品，因为技术项目的时间和费用预测，一般来说，[是糟糕透顶的](https://www.information-age.com/projects-continue-fail-alarming-rate-123470803/)。(我在之前的文章中写过这个[。)对于新兴技术项目来说，它们比糟糕透顶还要糟糕。如果不能准确量化费用，那么把第三方软件当做资产就没有意义。反正残值一般都是负数。](/datadriveninvestor/how-to-start-failing-today-in-tech-7fc3a8778a7d)

随着通信服务提供商的不断升级，相关平台将被迫进入越来越小的技术领域。同时，在 CSP 平台上实施物联网解决方案将变得更加容易。这意味着专业化物联网平台的价值(如果存在的话)将体现在垂直专业领域。培养这种关系有助于创造一种互补资产，有助于保护和发展您的长期物联网计划。

结束了。还会有第二篇。此处见[。](http://www.redchipventures.com/thanks-for-interest/)

*想多聊聊/有个有趣的提议？在 daniel@redchipventures.com 联系我*