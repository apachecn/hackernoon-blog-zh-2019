# 区块链利益的安全证明:解释

> 原文：<https://medium.com/hackernoon/secure-proof-of-stake-in-blockchain-explained-f7fbea5a787a>

![](img/cb77996b940e183ac7d4d8dd81bfa62a.png)

利益一致机制的证据表明，一个人可以根据他或她拥有多少硬币来挖掘或验证大宗交易。现在，这意味着一个矿工拥有的比特币或替代币越多，他或她拥有的采矿权力就越大。

第一种采用这种股权证明方式的数字货币是 [Peercoin](https://peercoin.net/) 。

让我们详细讨论这个问题

# 到底什么是利害关系的证明？

利害关系证明(PoS)共识算法最早于 2011 年在 Bitcointalk 论坛上推出，旨在解决使用中最流行的算法——工作证明的问题。

如果两种算法都有在区块链达成共识的共同目标，则两者实现这一目标的过程是完全不同的。

# 这种机制是如何工作的？

利益证明算法利用一种选择节点作为下一个块的验证者的伪随机选举方法，该方法基于某些因素的组合，这些因素可以包括随机化、利益年龄和节点的财富。

值得注意的是，在股权证明制度中，区块被认为是伪造的，而不是开采的。使用 PoS 的数字货币通常从出售预先开采的硬币开始，或者它们启动 PoW 算法，然后切换到股权证明。

在基于 PoW 的系统中，创建了许多加密货币作为对矿工的奖励，而 PoS 系统通常使用交易费作为奖励。

# 用户参与

参与伪造过程的用户需要将固定数量的硬币锁定到网络中作为他们的赌注。赌注的大小决定了一个节点被选为下一个伪造另一个块的验证者的机会，这意味着赌注越大，机会就越大。

在随机块选择方法中，通过搜索具有较少散列值和最高股份的组合的节点来选择验证器。由于赌注的大小是公开的，下一个伪造者通常可以由网络中的其他节点来预测。

如果节点希望停止成为伪造者，那么它的股份以及获得的块奖励将在特定时期后被释放，给网络时间来验证没有欺诈块被节点包括到区块链中。

# 这种机制的安全性如何？

对于不验证或创建欺诈性交易的伪造者节点来说，股份起着金融影响者的作用。如果网络发现任何欺诈交易，那么伪造者节点将失去一部分股份以及将来作为伪造者参与的权利。

因此，只要赌注高于回报，验证者失去的数字硬币将会比试图欺诈时得到的更多。

为了有效地控制网络以及批准欺诈交易，节点需要拥有网络中的多数股权。基于加密货币的价值，这将是非常不切实际的，因为要获得网络的控制权，你需要获得 51%的流通供应量。

能量效率和安全性是利害关系一致性证明算法的主要优点。

非常鼓励更多的用户运行这些节点，因为它既便宜又简单。这使得网络更加分散，因为不再需要开采池来开采区块。由于不需要发行很多新硬币来获得奖励，这有助于某种硬币的价格在更长的时间内保持稳定。

# 将股权证明机制向前推进一步

这一数字时代的新浪潮席卷了整个世界，新的创新不断出现。在公司从工作证明转向利益证明机制的情况下，有一些公司已经超越了利益证明，即*安全利益证明(SPoS)* 。

安全利益证明(SPoS) 是流行的利益证明机制的改进版本，它通过分布式公平性确保长期安全性，同时避免了对高能耗工作一致性算法证明的需求。

有几家公司在使用和采用 SPoS 机制，包括[algrand](https://www.algorand.com/)、[埃尔隆德](https://elrond.com/)和 [Zilliqa](https://zilliqa.com/) 以及[以太坊](https://www.ethereum.org/)等巨头也在使用同样的机制。[](https://www.linkedin.com/in/beniaminmincu/)[埃尔隆德](https://elrond.com/)的首席执行官 Beniamin Mincu 告诉记者，他们已经推出了这种机制的不同版本，并进行增强改进，旨在减少延迟，允许碎片中的每个节点在一轮开始时确定块提议者和验证者。这是可以实现的，因为最后一个块的聚合签名被用作随机化因子。它的安全利益证明机制通过各种方面使自己与众不同:它提出了减少延迟的改进，通过添加称为评级的额外权重因子来改进其机制，使用 Bellare 和 Neven multisig 方案消除签名算法中的一个通信领域，等等。

也有人说只是冰山一角！当您采用 SPoS 机制时，有几个问题可以解决，其中可能包括集中化、攻击媒介、可伸缩性问题、能源强度、启动成本等，这只是组织如何执行的问题。