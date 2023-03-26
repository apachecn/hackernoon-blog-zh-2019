# Masterode/Proof-of-Stake:一个新的共识协议综述

> 原文：<https://medium.com/hackernoon/masterode-proof-of-stake-an-overview-of-a-new-consensus-protocol-9fc7b1cf33c8>

![](img/1ed7655c13ef07b3651c0a0f036ebfdf.png)

对于普通大众来说，像以太坊这样的区块链网络有时看起来像是传统支付系统的神奇替代品:像“免费交易”和“即时支付”这样的短语很常见。现实远没有这么迷人:区块链可能非常慢，交易也远非免费。然而，一种被称为 MpoS 的新型共识机制可能会提供一个有希望的解决方案。

**以太坊困境**

以太坊引入智能合约标志着初创企业群体思想的一场革命:现在，任何人都可以在不与风险基金打交道的情况下创建和众筹创新项目，可以在没有银行的情况下接受和发送资金，并可以达成无风险交易。事实上，智能合同——一个在满足一组预定条件时自动执行的程序——可以为任何交易编写。不出所料，数百家区块链涌现出来，它们都使用以太坊智能合约来分发代币(希望未来的商品和服务也能通过智能合约销售)。

然而，差不多四年后，每个人都清楚以太坊有缺陷——严重的缺陷。期待已久的向 PoS 的转移可能还需要几年时间，许多有前途的项目发现在以太坊上运行复杂的 dApp 既耗时又昂贵。例如，一个拥有 2000 个用户的平台，每个用户平均每天进行 2 次交易，如果它希望提供零费用，则必须每年支付 1 46 万次交易。按照目前每笔交易 0.1 美元的平均汽油费，那就是 46 000 美元——一笔大数目。在这个成本上，我们应该加上 12.5 秒的漫长阻塞时间，这无疑是对君士坦丁堡时代之前的 21 秒的巨大改进，但对于时间关键的应用程序来说仍然太长了。简而言之，如果密码爱好者不断向我们承诺的大规模采用成为现实，就需要一个新的解决方案。

有一段时间，EOS 似乎可以成为解决方案，因为它看似民主的社区投票系统可以选择挖掘交易的代表。EOS 交易速度破纪录，[在某些时候几乎达到 4000](https://cryptonews.com/news/eosio-gets-new-update-for-better-transaction-speed-3261.htm) TPS。然而，对于大多数有抱负的矿工来说，EOS 被证明是一种虚幻的希望:要运行官方推荐的服务器配置，每天需要支付超过 600 美元——几乎是一个流行节点收入的一半。毫不奇怪[大多数 EOS 节点都在亏损](https://news.8btc.com/eos-price-hits-yearly-low-75-5-nodes-are-losing-money)。

**权威证明和主节点:从不同角度解决问题**

一个非常有趣的解决方案可能在于结合以太坊智能合约、利益相关者共识机制和主节点的概念。在这种情况下，铸造新块的过程类似于以太坊测试网 Rinkeby 使用的 Clique 协议。在 Clique 中，每一个新的区块都必须对照所谓的签名者列表进行验证。这个授权签名者的列表是动态的，这意味着任何一个签名者都可以被其他人投票选举。为了使这个民主程序成为可能，Clique 使用“nonce”字段进行投票，并使用一个扩大的“extradata”字段进行矿工签名。这种共识机制被称为权威证明，它可以被认为是一种 PoS，区别在于节点的挖掘能力不取决于赌注的硬币数量，而是来自社区本身。PoA 是网络自我监管和保护个体矿工免受大型采矿作业侵犯的好方法，同时降低了 51%的攻击风险。

主节点在这里起什么作用？由于 DASH 而变得流行的 Masternodes 是为社区提供有价值的服务的特殊节点——首先也是最重要的，它们维护区块链的最新完整副本，以换取固定的定期收入。要运行 masternode，用户必须保持服务器全天候运行，并有足够的可用空间来容纳整个数据集(例如，以太坊区块链的当前大小约为 140 GB)。主节点还必须下注合理数量的硬币，以确保它不会因为变得恶意而拿自己的地位和赌注冒险。在许多当前的实现中，主节点本身并不挖掘事务——相反，如果挖掘者滥用规则，它们有权拒绝阻塞。来自 masternode 的收入既稳定又相当大，超过了租赁 VPS 的成本，而 VPS 是不间断运行所必需的。

在混合 masternode/PoA 系统中，节点将下注特定有限数量的硬币来参与块签名，以换取块奖励的良好份额。签名者的名单将是动态的，每个授权的主节点将有平等的机会铸造一个区块，这与标准 PoS 情况相反，在标准 PoS 情况下，最大股份的用户具有明显的优势。同时，恶意节点仍然可以通过其他签名者的投票从列表中删除(取消授权)。

**主节点/利害关系证明:走向融合**

目前唯一上线的混合 masternode/PoA 系统是由 [EtherZero](https://etherzero.pro/) 于 2018 年 8 月推出的系统。目前，它拥有超过 200 个 masternode，每个 master node 必须投入大约 2000 美元(以当前 ETZ/美元价格计算)才能推出一个 master node。所有块奖励的 75%分配给主节点。

该系统的一个特点是一个名为 Power 的参数——一种非交易令牌，由所有主节点自动生成，用于支付网络中的交易费用。一个帐户产生的能量取决于它的余额，正是由于能量，网络可以提供零交易费。目前的容量约为每秒 1400 个事务，平均等待时间约为 1 秒。EtherZero 完全向后兼容所有 Solidity 智能合约。作为测试过程的一部分，开发人员成功导入了一系列项目，包括分散式交易所、令牌排放合同、投资应用和游戏。

MPoS 能被证明是以太坊的可行替代品吗？接下来的几个月很可能会证明这一点。诚然，目前只有一个功能齐全的实验在运行，尽管它提供了零交易费用和高处理速度。另一方面，以太坊的问题显然没有被最近的更新解决，dApp 开发者可能会更仔细地看看那里的混合共识选项。