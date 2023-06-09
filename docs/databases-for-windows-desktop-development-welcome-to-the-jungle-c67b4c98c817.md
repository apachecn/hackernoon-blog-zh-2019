# 用于 Windows 桌面开发的数据库:欢迎来到丛林

> 原文：<https://medium.com/hackernoon/databases-for-windows-desktop-development-welcome-to-the-jungle-c67b4c98c817>

## 微软忘记了一个细节——或者故意告诉我们一些事情。

![](img/0428b950e73524088821b0c4692d2e05.png)

[Pexels](https://www.pexels.com/photo/green-leafed-trees-1089302/)

我是一个微软爱好者。我是一名 Windows 桌面开发人员。你可以判断，到此为止。或者你可能也有同样的想法，想知道为什么微软决定放弃 Windows 的开发。

我即将推出一款桌面应用。我离开 swdev 已经有几年了，但是我已经写了 30 年的代码了。使用. NET 十几年。

因此，对于我的新应用程序，我决定使用优秀的 Windows 演示基础。WinForms 消失了，UWP 看起来像孤儿，被限制在 Windows 10 中。我认识购买 Windows 桌面应用的人(当然我也是其中之一)。但我知道没有人在微软商店购买它们，甚至偶尔检查一下，我担心这种情况不会很快好转。作为警告，我仍然把我的旧 Windows Phone 放在架子上。

不用说，WPF 用实体框架。当然不一定，但是最好。

作为本地数据库，我从 mdf(也就是 SQL Server)开始。我告诉自己:“我们会看到什么是最好的，可能是 SQL Compact 或其他什么。这不是问题所在。”作为一个个人应用程序，它必须有一个单一的设置，所有包括在内，但这在我的生活中从未成为问题。

最后接触的时刻到了，看似“最后接触”的事情变成了令人头疼的事情。我离开 swdev 有一段时间了。也许太多了。

# 微软忘记了一个细节

或者故意告诉我们一些事情。记住[纳德拉](https://en.wikipedia.org/wiki/Satya_Nadella)是一个云人。

如今一切都是云——我明白了——但许多业务和生产力仍然离线运行。微软对此相当擅长。

惊喜—我的错—[**SQL Server Express**](https://www.microsoft.com/it-it/sql-server/sql-server-editions-express)无论如何都需要单独设置。我应该想到的。

我是在赌 [**LocalDb**](https://docs.microsoft.com/it-it/sql/database-engine/configure-windows/sql-server-2016-express-localdb?view=sql-server-2017) ，SQL Server Express 的简化版。[来自微软](https://docs.microsoft.com/it-it/sql/sql-server/editions-and-components-of-sql-server-2017?view=sql-server-2017):

*“SQL Server Express local db 是 Express 的一个轻量级版本，具有其所有的可编程功能，在用户模式下运行，具有快速的零配置安装和一个简短的先决条件列表。”*

用户实例。非常好。我希望提供给没有管理员权限的用户。

但是……你可以得到一个 LocalDb 的 msi 文件(所以你可以*或多或少*进行静默安装)，但是在任何情况下安装都需要管理员权限(用户*模式*，而不是用户*权限*……)。此外，它不能密码保护，也不能加密。它是为开发人员使用的，而不是为开发人员使用的。用于*设计时间*。

简而言之，在微软推出 Access 26 年后，LocalDb 并不完全是一个基于文件的数据库。

但故事并没有到此结束。SQL Server Express 2017 不支持 Windows 7 和 x86。微软认为，我的一半潜在客户使用过时的操作系统(Windows 7 仍然服务于全球三分之一的台式机/笔记本电脑用户),因此无法解决这个问题。

此外，尝试安装以前的 SQL Server Express(用于 Windows 7 支持),它无法打开您的 mdf，因为您是用 Visual Studio 2017 或最新的 SQL Server 创建的。对于这种常见问题，没有降级工具可用，也没有创建“旧”mdf 的选项。为了在 Windows 7 中读取您的 mdf，您必须手动将 mdf 迁移到以前的版本——使用“较旧”的 SQL Server 工具——并放弃使用 Visual Studio 来管理它。哇哦。在设计这个陷阱的时候，他们真的应该考虑到开发者。

好吧，我们倒回去。 **SQL 压缩**。[私人安装](https://www.codeproject.com/Articles/33661/%2FArticles%2F33661%2FCreating-a-Private-Installation-for-SQL-Compact)，不需要管理员权限，密码保护。完美。

嗯，*没有*。SQL Compact 仍然存在于微软服务器的黑暗角落，但是，从 2013 年 2 月起，它已经被弃用了。你可以自担风险，但 Windows 8 和 10 不在规格范围内，而且取消外部设置需要一些变通办法。此外，Visual Studio 和 SQL Server Management Studio 清楚地表明，SQL Compact 已经寿终正寝，被人遗忘了。你只能靠自己了。

更多回滚。**访问**？(哦，我的……)

实体框架不支持访问。是的，你没看错。微软不支持它自己推荐的 ORM 的流行数据库。

实现是有的，例如[jetintityframework provider](https://github.com/bubibubi/JetEntityFrameworkProvider)，但是它只支持代码优先。而且是来自[布比布比](https://github.com/bubibubi)。我确信 Bubi 是一个伟大的开发者，但是我希望有更多的…官方的东西。

我曾经为 Access 和。但是我的新应用已经完全基于 EF 了。

使用 [Linq to DataSet](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/linq-to-dataset-overview) ，带 Access？从未使用过，但我确信它对性能有很大的影响——除了其他问题，您必须完全填充数据集——在这方面我已经很小心了。似乎不是主流解决方案。我还没准备好为获得其他惊喜而投入大量工作。

你可以猜到我开始出汗了。

好吧，让我们走出微软的领地。EF 支持的最著名的自包含 SQL 数据库引擎是什么？ [**当然是 SQLite**](https://www.sqlite.org/index.html) 。

但是 SQLite 有一些限制。[例如，您不能删除列](https://www.sqlite.org/faq.html#q11)，也不能添加约束。想象一下在每次模式更改时都支持完整的数据库拷贝。

和密码/加密本身不受 SQLite 支持。当然，有人会告诉你插件是万能的。有一些限制。但是还有另一个插件，工作方式不同，或者几年后就消失了…

[MySQL？甲骨文，微软最激烈的竞争对手。我相信他们会尽最大努力，争取](https://www.mysql.com/it/)[的支持。NET](https://dev.mysql.com/downloads/connector/net/) 对抗 Java——我不会有更多的惊喜——但是让我担心。然后，你必须找到一个静默安装的方法，交叉手指。

你可以继续，继续，继续…航行在许多海洋中，梦想着无尽的发现和重构。

# 那又怎样？

所以，我们有很多选择，没有解决方案。

微软有两个明确的答案:桌面上的 SQL Express(有一个单独而庞大的安装程序，没有密码，没有加密)，或者 Azure(这意味着云，它不适合太多的桌面应用)。

微软忘记了——或者说想要忘记——桌面应用程序及其对基于文件的集成数据库的需求。您可以找到许多选项，但没有直接完整的解决方案，无论是现在还是将来，都没有得到微软的明确而全面的支持。

# 我该怎么办？

首先，我必须接受我不属于这个混乱的时代。我下半辈子可能都要为此而奋斗。

现在呢，用我的桌面应用程序？

嗯，我计划把我的产品扩展到团队。因此，SQL Server 是我的必经之路，因为如果可能的话，我更愿意呆在微软的地盘上。最好从一开始就使用这项技术。

我将继续使用 mdf 文件，将它迁移到旧版本(因此，使用旧的 SQL Server)并放弃从 Visual Studio 管理它。

然后，我想使用 msiexec 命令行静默部署 LocalDb，该命令行嵌入在我的设置中(假设提供了 msi):

*msiexec/I c:\ temp \ sqllocaldb 2017 . MSI*

但是… Msi 不能在另一个 Msi 中运行(我使用的是 Visual Studio Installer 项目，出于许多原因，我更喜欢它)。因此，我决定在应用程序第一次运行时运行它(否则，我可以编写一个安装包装器，或者使用不同的安装构建器)。当然，我必须这么做

*   检测可能以前安装的 SQL Server(通过检查注册表，然后检查文件)，
*   决定安装哪个引擎(Windows 7 上的 local db 2014—x86 或 x64 —否则 LocalDb 2017 — x64 —)，
*   提示用户(他肯定会对额外的设置感到高兴，尤其是作为消费者)，
*   并且可能将 SQL 安装程序与应用程序一起分发，或者作为可选的 fat 安装程序分发(否则，将需要单独的 LocalDb 下载)。

***无可奉告，微软。***

没有加密，只能通过应用程序逻辑或在 SQL Server 级别添加用户。

有用，但是我不开心。此外，我花了几天时间来做决定、实施和测试。

现在，如果你不介意的话，我有一些积压的工作要做，因为我没想到在第一个 Visual Studio 22 年后会在本地数据库上浪费这么多时间。