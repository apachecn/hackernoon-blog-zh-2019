# 别管工作了，让我们达成共识

> 原文：<https://medium.com/hackernoon/forget-about-the-work-lets-stake-the-consensus-a5b76c862a25>

![](img/4cb84e6a7847a35c5b34d9285bd73dcc.png)

Photo by [Michał Parzuchowski](https://unsplash.com/@mparzuchowski?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在一个不可信的区块链网络中达成共识不是一件容易的事情，但是不要认为我是理所当然的，问问我们的朋友 Satoshi。他在设计和实现比特币时就面临这个问题，**他是如何修复的？提出工作验证(PoW)一致算法，**，即迫使网络中的节点，即矿工，将他们的硬件资源专用于解决一个困难的数学问题。

这个问题的解决者将被允许封闭链中的下一个块，决定交易的顺序，并因此因其帮助保持网络的服务而得到奖励。

在比特币等生产系统上运行了十年之后，**的工作证明已经被证明是一种可靠的共识算法，被密码界广泛接受，几乎不可能被攻击。**然而，工作证明仍然存在某些缺点，例如:

*   **浪费精力。矿工们不断试图解决一个数学难题，即使他们最终没有因为解决了这个难题而获得奖励。所有这些计算能力本可以用于更有用的任务，比如发现一种新的蛋白质来治愈癌症(谁知道呢？).**
*   **可扩展性。**工作证明的操作很难扩展到每秒十几个事务，这对于某些用例来说是不可接受的。
*   **集权。**在某些基于 PoW 的区块链平台中，难度的增加导致了拥有网络大部分计算能力的大型矿池的出现，并增加了 51%的攻击可能性。

> **正是这些缺点让区块链的开发者和全世界的研究人员开始了设计全新共识算法的艰难探索。**

这一探索的结果是发现了利害关系的证据。与 PoW 相比，proof-of-stage(PoS)是一种更节能的共识算法。**该算法不是基于计算能力的随机化分组求解，而是基于网络成员所下注的令牌数量的随机化。**

> 如果一个节点想要有资格解决下一个块，它需要在钱包中放置一组令牌。

一个人的赌注越大，他被选为下一批交易验证者的几率就越大。与 power 一样，block verifiers 也因其服务而获得奖励。

股权证明增加了权力下放，允许更高的交易吞吐量，并促进权力下放。然而，**基本 PoS 算法仍然存在一些问题，如**经典的*“富者愈富，贫者愈贫”*。

如果你能买得起一大袋某个区块链的硬币，那么如果你下注，你获得奖励的几率会更高，并且你的持股会通过所述下注而增加——这又增加了你获得奖励的几率，以此类推。

幸运的是，PoS 的智能变体已经被提出来增强它的普通版本:

*   **委托利益证明(dPoS):** 在 dPoS 中，有固定数量的验证者被授权来保护网络。令牌持有者不是直接下注他们的令牌，而是使用它们来投票给下一个授权节点以验证交易。拥有最多票数的验证者将成为代表，验证交易并因此获得奖励。Lisk、Tron、Steem、Bitshares 和 EOS 等平台使用 dpo 的变体。
*   **流动性风险凭证(lPoS):lPoS 委托中的**是可选的。代币持有者可以将验证权限委托给其他代币持有者，而无需保管，即他们不会从钱包中丢失代币。在这种 PoS 变体中，如果在链中检测到攻击，只有验证器会受到惩罚。LPO 还提供投票权，只是作为令牌持有者，你可以直接投票表决协议修正案，而不仅仅是像 dpo 那样投票表决谁来保护网络。Tezos 等平台使用 LPO。
*   **保税股权证明(BPO):**BPO 与 LPO 非常相似，授权是可选的、非托管的，令牌持有者从协议修正案的投票权中受益。虽然，它被称为 BPoS 是有原因的:在安全性或活性故障的情况下，一部分验证者和委托者的股份将被削减。在 LPoS 中，只有验证者有被砍的风险，而委托者的唯一风险是在验证者不诚实或没有效率的情况下错过一些奖励/利益。BPoS 的优势在于为赌注比率(类似于资本要求)问题提供了一个明确的解决方案，如果 LPoS 协议上的一些验证者不想被过度委托并让他们的委托者失望，他们就必须维护这个比率。虽然它解决了这个问题，但这也意味着委托者需要在委托之前进行额外的尽职调查，并在验证验证者的性能时保持主动。在 Cosmos 等项目中可能会发现 BPO。
*   **混合证据(HPoS):** 混合 PoS/PoW 通常被称为工作证据和证据的混合。在 HPoS 中，挖掘器通过 PoW 产生新的块，然后 PoS 验证器对这些块的有效性进行投票。HPoS 通过增强利益相关者投票的散列能力，提供了对多数攻击的强大威慑。

这些只是已经实现的 PoS 变体的一些主要代表，**尽管如此，在寻找完美 PoW 替代品的过程中，更多全新的共识算法和 PoS 变体将及时出现，**如果你不相信我，请尝试快速搜索 Wave 的租赁 PoS、Tomochain PoS 投票、Dash 的 Masternode staking 或 NEO 的委托拜占庭容错。我们将不得不继续关注这一探索的结果。

**还想了解更多关于 PoS 及其变体的信息吗？点击以下链接了解 ever stake:**

网址: [https://everstake.one](https://everstake.one/)

电子邮箱: [inbox@everstake.one](http://inbox@everstake.one)

电报:[https://t.me/everstake_chat](https://t.me/everstake_chat)

https://www.reddit.com/r/Everstake/

***参考文献:***

让我们达成共识:[https://medium . com/coin monks/let-agree-about-the-consensus-ee 792 e 50 e 073](/coinmonks/lets-agree-about-the-consensus-ee792e50e073)

共识算法讲解:[https://www . investinblockschain . com/consensus-Algorithms-Explained/](https://www.investinblockchain.com/consensus-algorithms-explained/)

PoS Downsides:[https://www . Reddit . com/r/ether eum/comments/7 duvqm/are _ there _ any _ Downsides _ to _ proof _ of _ stake/](https://www.reddit.com/r/ethereum/comments/7duvqm/are_there_any_downsides_to_proof_of_stake/)

PoW vs PoS vs dpo:[https://www . bitcoinmarketjournal . com/consensus-mechanisms-区块链/](https://www.bitcoinmarketjournal.com/consensus-mechanisms-blockchain/)