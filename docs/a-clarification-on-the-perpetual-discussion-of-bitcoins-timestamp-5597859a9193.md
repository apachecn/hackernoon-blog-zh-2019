# 关于比特币时间戳永恒讨论的澄清

> 原文：<https://medium.com/hackernoon/a-clarification-on-the-perpetual-discussion-of-bitcoins-timestamp-5597859a9193>

![](img/864e9d05f09d3d1a8cb8e20f5ea51fe0.png)

source: [https://www.pexels.com/photo/brown-hourglass-on-brown-wooden-table-1178684/](https://www.pexels.com/photo/brown-hourglass-on-brown-wooden-table-1178684/)

# 它是关于什么的？

时间戳只是传统共识系统中**对交易**排序的一种便捷方式。在经营银行业务时，银行想知道哪笔交易先发生，哪笔交易后发生。需要某种排序，无论是时间戳还是简单的递增数字。然而，在分布式账本技术(DLT)及其新型共识机制中，如何处理排序是非常不同且具有挑战性的。依靠 NTP(网络时间协议)服务器、原子钟和类似的时间服务是不够的，因为它带来了集中的味道，并引入了单点故障。
当[比特币](https://hackernoon.com/tagged/bitcoin)——分布式账本技术最突出的例子——及其底层[区块链](https://hackernoon.com/tagged/blockchain)概念发布时，他们将**排序**设计为**内在特性**。排序基本上是区块链本身的连续“链”部分，因为一个块接一个块。

# **那么时间戳的目的是什么呢？**

尽管如此，比特币仍然使用时间戳，但出于两个其他目的:

*   难度计算(原点特征)
*   [锁定时间](https://bitcoin.org/en/glossary/locktime)交易(新功能)

时间戳主要用于确定难度(也是块哈希的额外变化源)。
如果没有时间戳，节点将无法确定用于每个 2016 块周期的正确难度，因为它们不知道挖掘之前的块需要多长时间。
然而，挖掘器仍然可以操纵时间戳，但是节点将根据实际时间进行检查，并将忽略特定时间范围之外的块。时间戳的条件如下:

*   大于过去 11 个街区的中值(为什么是 11 个？11 个街区似乎是一个足够好的数字)
*   在网络调整时间的 2 小时内(为什么是 2 小时？2 小时似乎也是一个足够好的数字)

**网络调整时间**是如何分散同步节点时间的过程。一个节点向它的所有对等节点询问它们的 UTC 时间戳，并且如果时间戳在它自己的节点的 70 分钟之内，则将完成所有节点时间戳的网络调整时间中值计算。

为什么比特币的时间戳与排序无关的一个简单例子是以下两个区块([https://bit.ly/2FKXwTm](https://bit.ly/2FKXwTm)):

【比特币 Block # 180966】([https://www . Block chain . com/BTC/Block/00000000009109 a 556 cf 6c 997 f1 EC 10 AFA 32 fa 53064 b 59 a5 fa 0 c 026d 200 ee](https://www.blockchain.com/btc/block/00000000000009109a556cf6c997f1ec10afa32fa53064b59a5fa0c026d200ee))

相对

【比特币 Block # 180967】([https://www . Block chain . com/BTC/Block/000000000002 aab5 e 054 a 80 b 8147 BF 9 BDF 8 ef 78d 9 da 094 f 537489 e 53e 2513](https://www.blockchain.com/btc/block/000000000000002aab5e054a80b8147bf9bdf8ef78d9da094f537489e53e2513))

块 180966 的时间戳是 2012 年 5 月 20 日 23:02:53
块 180967 的时间戳是 2012 年 5 月 20 日 23:02:13

(此处[的例子来自](https://bitcoin.stackexchange.com/questions/3743/how-can-an-earlier-block-have-a-later-timestamp))

# **它是如何与 DAGs 等其他概念一起使用的？**

还有基于有向无环图(DAG)的 DLT 概念，当涉及到系统的全排序时，这些概念必须处理更多的挑战。主要问题是它的并发性。这种努力的一个已知例子是 IOTA。正如下面的论文所述:“在纠结的时间戳上”([https://bit.ly/2qKTXTk](https://bit.ly/2qKTXTk))
不可能有正确的事务时间顺序。即使所有事务都有时间戳，也不能确保所有这些时间戳都是准确的(可能有一些恶意节点想要欺骗网络其事务出现的真实时间，和/或一些节点只是时钟错误)。
那些置信区间，到目前为止，只是本文描述的理论概念，还没有实现。

如果你想拥有某些智能合约功能，排序是很重要的(这也是 IOTA 的 Qubic 宣布的— [Qubic:基于法定人数的计算—由 IOTA 提供动力]([https://bit.ly/2LcqMF9](https://bit.ly/2LcqMF9)))。订购要求最著名的例子是以太坊的 ERC20 合约(自 16 年起用于所有 ico)。