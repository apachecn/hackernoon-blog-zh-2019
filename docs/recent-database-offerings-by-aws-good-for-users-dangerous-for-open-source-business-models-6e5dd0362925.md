# AWS 最近的数据库产品——对用户有益，对开源商业模式有害

> 原文：<https://medium.com/hackernoon/recent-database-offerings-by-aws-good-for-users-dangerous-for-open-source-business-models-6e5dd0362925>

亚马逊网络服务(AWS)是领先的云提供商之一，在其产品组合下向客户提供一系列云服务。虽然许多 AWS 云服务产品是内部构建的，但其中一些是完全托管的开源软件解决方案。此外，很少有内部服务也与众所周知的开源兼容性驱动程序、接口等兼容。

![](img/570c80190ae93c241a85f68773296a54.png)

# 云中的开源和托管服务

在过去的十年中，包括 AWS 在内的领先云提供商已经将重点转移到提供更高级别的服务上。这些不仅包括特定于云的服务，还包括流行的开源软件产品。

> 你可能想知道这是不是最近发生的事情。事实上，AWS 从 2013 年开始在亚马逊弹性缓存服务下的服务组合中提供完全托管的 Redis(开源内存数据库)。随后又推出了 Maria DB(亚马逊 RDS 下的 MySQL 开源变体)、ElasticSearch(亚马逊弹性搜索)、Kafka(亚马逊为 Kafka 管理的流媒体)、Apache ActiveMQ(亚马逊 MQ)等。

除了托管服务产品之外，AWS 还推出了自己的本地实现(或使用开源软件的定制版本),通过提供与开源软件兼容的接口来取代现有的开源软件。这方面最早的例子之一是 Amazon Aurora 企业级数据库的引入，该数据库具有 MySQL 和 Postgres 兼容的驱动程序和连接器。如果我们考虑 Aurora 数据库，正如 AWS 声称的那样，内部实现完全不同，但接口是相同的，提供了迁移现有应用程序以及使用围绕它们已经构建的生态系统的可能性。MongoDB 兼容数据库产品 Amazon DocumentDB 的最新发布引起了开源社区和 AWS 用户的极大关注。Amazon DocumentDB 从支持 MongoDB 3.6 API 开始，该 API 从一开始就使用开源的 Apache 2.0 许可证。这个消息让大多数在 AWS 中运行 MongoDB 集群并自己管理它的 MongoDB 用户大吃一惊。

# 开源商业模式

虽然让云用户在 AWS 中完全管理所有这些开源软件是一件好事，但对于一些开源企业来说却不一样。这主要是由于开源软件背后的企业采用了不同的商业模式。

让我们来看看两种开源商业模式，当 AWS 或任何其他云提供商利用他们的[开源许可模式](https://resources.whitesourcesoftware.com/blog-whitesource/top-open-source-licenses-trends-and-predictions)时，它们的业务受到了最大的冲击。

*   高级或生产支持——免费提供开源软件，并有生产支持计划。
*   托管服务托管——以优惠价格在云中提供托管和完全托管版本的开源软件。

## 高级或生产支持

如果我们考虑有生产支持的商业模式，AWS 等超大型云提供商完全有能力在 AWS 内部组建自己的专业化团队。这对客户支持业务模式有直接影响，因为 AWS 内部的团队也可以与 AWS 云支持计划集成，并提供一个简短的反馈循环。

## 托管服务托管

如果我们考虑 AWS 提供开源软件的完全管理版本作为服务，这可能会对开源公司产生直接影响，这些公司已经将此作为他们的核心业务。这就是最近发生在 MongoDB 上的情况。MongoDB 提供完全托管的 MongoDB 服务产品和高级支持，一旦 AWS 提供自己的完全托管版本的 MongoDB，这就引入了直接竞争。

# 开源业务受到云的挑战

如果我们看一下 MongoDB 事件，他们已经[在一个定制许可下重新许可了他们的开源数据库](https://www.theregister.co.uk/2018/10/16/mongodb_licensning_change/),该许可明确限制了提供 MongoDB 作为软件即服务(SaaS)产品的能力。这对开源运动来说是不健康的，因为它限制了使用的自由，但这种情况在 MongoDB 方面是意料之中的，因为 AWS 一宣布这项服务，他们就面临着市场价值的下降。

> 如果我们回顾一下历史，这并不是第一次因类似事件而重新获得许可。当其他云提供商开始提供 Redis 作为托管服务时，Redis 过去也做了类似的事情。

不幸的是，我们现在的处境是，这种情况有再次发生的趋势。这是因为云提供商不断被其用户推向提供托管服务，而开源公司受到其商业模式的影响。回顾与 AWS 发生的事件，很不确定他们是会保持合作关系还是转向自己的产品。

# 解决问题

然而，直接的出路是在云提供商和开源企业之间建立可持续的商业模式。这可以通过与开源企业建立牢固的合作伙伴关系来实现，在云中提供他们的软件作为托管服务。令人震惊的事实是，就在 AWS 决定独立之前，这些开源企业中的一些是 AWS 的合作伙伴。

当考虑 MongoDB 事件时，许多其他人声称它更像是 [**背后捅了**一刀。](https://techcrunch.com/2019/01/09/aws-gives-open-source-the-middle-finger/)然而，在不知道事情背后的细节，不知道 AWS 是否试图与 MongoDB 谈判的情况下，指责他们是不公平的。

但是我强烈希望这种冲突的趋势将在未来结束，允许与开源企业和云提供商建立健康的关系。