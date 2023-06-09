# 架构决策的细节

> 原文：<https://medium.com/hackernoon/architectural-decisions-big-and-small-6a251fbe2666>

## 成功(或失败)大多与“小”决定有关

![](img/87541792a8f35b89520691dc882517fc.png)

对一些人来说，建筑就是关于决策。一旦做出了最大和最重要的决定，架构工作就被认为完成了。遗憾的是，项目的成功很少取决于那些“重大”决策。正是那些我们经常忽略的“小”决策，决定了项目的成功。最大的决策是静态的；一旦形成，就很少会改变。正是我们每天做出的小决定，实际上决定了支持新功能的难易程度，以及随着软件的发展，团队能保持多高的生产力。

让我们检查一下这些架构决策中的一些。

# 重大技术决策

“重大”决策通常是技术选择决策。

技术堆栈的选择是最大的架构决策之一。几年前，灯堆风靡一时。LAMP 堆栈意味着您将使用 Linux 和 Apache。更必然的是，这也意味着你将使用 PHP 编码，并使用 MySQL 作为数据库。随着技术的发展，如今有了更多的组合，选择也更加细化。

后端的选择是另一个重要的架构决策。让我们看看选择 Nodejs 的影响:编码范例将是基于事件的，加载共享将跨多个 Nodejs 进程。最重要的是，您将主要用 Javascript 编码。对于数据库访问、缓存、认证等，您将依赖于许多 Nodejs 库。

选择微服务架构是另一个重大决策。它不是选择底层框架或语言，而是基于选择运行时架构。它可以让你用不同的语言构建服务。然而，它将改变服务的开发和部署方式。

# 图书馆选择

今天的发展非常依赖于开源库。对于 web 应用程序来说尤其如此。因此，在项目过程中需要做出许多与图书馆选择相关的决定。这些是架构决策。

例如，图表库的选择将影响图形和图表的显示位置。根据应用程序中完成的图表数量，影响可能是普遍的。

使用 ORM 库访问数据库是另一个这样的例子。我们使用[书架](https://bookshelfjs.org/) + [Knex](https://knexjs.org/) 来访问 PostgreSQL。最终，如果我们想从 PostgreSQL 迁移到一个 ORM 库不太支持的数据库，这将限制我们对数据库的选择。

开源库的流行可能是短暂的。随着时间的推移，许多开源库跟不上，或者它们的创建者转向了更大更好的东西。Redux-form 曾经风靡一时，但现在它有了真正的竞争对手。今天的许多最佳选择不一定是未来的首选解决方案。有些甚至可能半途而废，因为它们的创造者失去了兴趣。它是不可预测的。

# 对业务问题建模

作为程序员，我们每天做出的大多数决定都与我们如何对业务问题建模有关。它是关于我们如何分解和实现我们的业务问题的解决方案:我们如何定义相关的抽象和接口；什么进入一个类；模块和类之间的关系。

我们必须定义用户界面的组织原则，以及如何在代码中反映出来。我们还必须定义 API 的组织原则，以及如何在代码中反映出来。这些是真实世界的架构决策，将决定我们项目的成败。

这些决定需要多少思考？

# 停止责备重大决定

**重大决策通常是静态的。**一旦做出这些决定，很少会改变。如果没有重大的重写，你不会从 PHP/Apache 升级到 Java/Tomcat 或 Javascript/Nodejs。这些决定通常是在大多数程序员加入项目之前很久就做出的。

**重大决策通常是基于技能的(对建筑来说就是这样😃).**如果你很了解 Javascript，你可能会选择 Nodejs 作为后端，同时选择一个现代的 Javascript 框架作为前端。几年前，GWT 在精通 Java 的团队中很受欢迎。如果你的团队使用微软的。NET 和 C#的选择很可能是由精通 Visual Studio、ASP 或 C++的人做出的。

**重大决策影响多个** [**视图类型**](https://www.georgefairbanks.com/modeling/viewtype/) **。**值得注意的是，这些决策不仅会影响程序员，还会影响应用程序的部署方式，从更深层次来说，甚至会影响用户对应用程序的看法。

重大决策有时(尽管很少)会出错。很难一概而论。所以，是的，重大决策可能会出错，但通常不会这样。大多数主要框架都相当全面。这里有几个好的选择被证明并非如此的例子:

*   你选择了 ActiveX/IE，微软失去了对桌面浏览器的垄断。
*   你选择 AngularJS(旧 Angular)，但发现其复杂性令人生畏。(注:Angular 2+据报道改进很多)。
*   您在 MongoDB 上构建了应用程序，然后发现了对事务的需求。(注意:Mongo 4.0 现在有副本集的[事务](https://docs.mongodb.com/master/core/transactions/#transactions-and-replica-sets))。

这通常发生在新技术不成熟或者旧技术正在转变的时候。请注意，这并不意味着批评，因为选择旧的甚至不成熟的技术是有充分理由的。

**对成功至关重要的决定是在没有经过深思熟虑的情况下做出的。**代码通过新的故事和错误修复而进化。我们创建新的功能，而没有考虑什么是业务逻辑的一部分，什么是实用程序的一部分。我们剪切和粘贴而不考虑共享的抽象。这些都是“小”决定，因为团队思维很少参与其中。每个开发人员都可以按照他们认为合适的方式编写代码。没有专门用于确保设计一致性的 sprint 任务，代码评审只是忽略了这个问题。最终结果是一团乱麻，难以理解，也不可能长期维持。

我们不强调这些决策的一个潜在原因是它们易于重构。决策是可以改变的，这种改变甚至可以是渐进的。*如果它们可以修复，那么为什么要担心呢？然而，有多少项目会投资于这些改进呢？*

通常，代码复杂性问题的解决方案就在我们编写的代码中。修复它应该是敏捷开发的一部分。它应该在我们的流程中可见。要做到这一点，我们必须首先拥有它。