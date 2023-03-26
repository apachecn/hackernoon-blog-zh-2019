# 我们如何构建每分钟 750 万笔交易的实时游戏内货币服务

> 原文：<https://medium.com/hackernoon/how-we-built-a-real-time-in-game-currency-service-with-7-5m-transactions-per-minute-375ebf0cc34a>

对于我们这些在 [Loco](https://getloconow.com) 的技术人员来说，没有什么比致力于前沿解决方案、使用新产品和解决在 StackOverflow 上没有解决方案的问题更令人兴奋的了。最近，我们遇到了一个这样的问题，并发现了一种这样的[技术](https://hackernoon.com/tagged/technology)来解决它。

我们围绕 [Tarantool](https://tarantool.org) ，一个高性能[数据库](https://hackernoon.com/tagged/database)，设计了我们新的高性能交易货币服务。它作为我们的 SSOT(真实的单一来源)数据库来发布一个新的特性。

## 为什么我们会想到塔兰图尔？

我们必须建立一个游戏中的货币交易系统，要求至少 15 万并发用户实时获得硬币。用户应该能够使用硬币，只要他们得到它。对于我们来说，*实时*就是 *~300ms* 。对于**所有**我们的用户，所有 **150K 并发用户**。

解决这个问题的一个方法是使用 Redis。将其用作 NoSQL 数据库的直写缓存。这很快就会变得痛苦。我们必须保持一致性，处理竞争情况，处理缓存失效、驱逐、过期等。

*耐用性* — Redis 专注于具有定期快照功能的内存处理。但是 Tarantool 有全面的 WAL 日志和快照，我们不需要管理；不像 Redis。

*原子性* —没有一种简单的方法可以跨 Redis 和后台 NoSQL 数据库执行原子操作。这意味着在这个解决方案中没有 SSOT ( *真理的单一来源*)。

还有一大堆其他问题。

听起来很简单，对吗？仅仅是每秒几十万次数据库事务。

*什么？你说没有？*

## 塔兰图尔是如何解决我们的问题的

Tarantool 是一个全面的 NoSQL 数据库，内置了 Lua 应用服务器、事务、存储过程和真正的持久性。

Tarantool 提供了两种存储引擎:Memtx(内存存储引擎)和 Vinyl(磁盘存储引擎)。这两个引擎都提供了可靠的 ACID 事务。

当 MemTx 崩溃时，MemTx 使用 WAL(预写日志)和快照来满足一致性。

它可以有任意数量的二级索引。

它支持 Lua 中的服务器端脚本，用于批处理操作之类的操作，这本身就是一个很好的特性。

***Tarantool 拥有超快的 CRUD 运算***

*它通过在单线程上使用纤程执行事务来实现这一点。*

*这是什么意思？*

没有多个线程同时在数据库上运行。纤维是一种轻质线。事务处理器线程将执行纤程中的所有指令，直到发生让步，然后继续执行另一条指令。纤维被期望使用*合作多任务*来防止饥饿*。*此外，所有 IO 任务(磁盘、网络等。)触发隐性屈服。开发人员在大多数情况下不需要担心纤程。

![](img/5a526e65c2a840ddef8a8924f0d49648.png)

## 这东西有多快？

我们的目标是用 5 秒钟跑完 50 万公里。

***我们使用 Memtx 在 3.6 秒内完成了这项工作！***3.6 秒 50 万交易！

请记住，这是针对所有事务的完整 ACID 属性。

越来越好了。对我们来说，一个事务有一个写操作和一个更新操作。

Tarantool 一开始完全是一个实验。我们意识到了风险。我们成功了，并获得了良好的基准测试结果。而且，现在已经投入生产了！

***优点*** :

1.  极快
2.  服务器端自定义 Lua 函数(我们可以减少批处理操作的网络调用)
3.  单线程(Tarantool 中只有一个事务处理器线程)
4.  一个持久的内存数据库(Memtx)
5.  服务器管理( *tarantoolctl* 实用程序)
6.  数据库分片(V [shard](https://github.com/tarantool/vshard) )和复制
7.  乙烯基磁盘存储可以很好地替代 NoSQL 数据库
8.  [Tarantool 电报组支持](https://t.me/tarantool)(原 Tarantool 开发人员在此组上响应)

***缺点*** :

1.  基础设施(如果您正在试用 Community edition，您需要自己管理快照的实例、分片和定期备份。)
2.  不是一个很大的社区。任何问题都要依靠他们的电报组。
3.  AWS 或 Azure 等云平台不提供(AWS marketplace 中存在旧版本。)所以我们必须为 Tarantool 实例构建自己的监控系统。

Tarantool 企业版具有解决这里列出的一些缺点的功能。但是它不是免费的。

你应该开始使用 Tarantool 吗？

这是一个百万美元的问题。在某些情况下，确实如此。

问自己这些问题:

1.  你真的需要一个快速处理的数据库吗？这是面向用户的关键需求吗？
2.  最终一致性对您的场景不起作用吗？
3.  您是否已经计划在另一个数据库之前使用 Redis/cache 层？
4.  您是否愿意管理通常由云平台支持的所有基础架构需求？

如果你的答案是肯定的，那么是的！马上选择塔兰图尔。也许你可以用小微服务试试，然后做大！！！

就我个人而言，我们真的很喜欢 Tarantool。投产三个月，都没有什么大问题！

*要想每天赢得现金，玩 Loco 吧！* [*现在就下载*](https://getloconow.onelink.me/eQ3k/loco) *这个 app 吧。*