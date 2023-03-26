# 模块化在区块链平台中的重要性

> 原文：<https://medium.com/hackernoon/the-importance-of-modularity-in-a-blockchain-platform-f9af03b80b80>

![](img/953726f095b17d61cae8727ad6cd19a9.png)

Project Ara: an idea for a modular smartphone.

模块化指的是一个系统分解成可以分离和重组的组件的能力。最好的软件开发工具通常是高度模块化的，并允许重用“组件”或“模块”。这个原则是 Java、C#和 Go 等开发框架的核心。

模块化促进了创新，因为新想法变得很容易实现。当创建一个新的 web 应用程序时，大部分运行的代码实际上都在外部开发的包中；无论是数据库连接、页面渲染引擎、输入验证器等等。因此，开发者很容易就能设计出一个新网站的原型。

# 输入:加密

然而，如果你去看看最有价值的加密代码库，比特币核心库，你会发现它非常不灵活。非模块化代码通常以长文件和类为特征，类的单个实现通常依赖于其他具体实现。举例见[比特币](https://hackernoon.com/tagged/bitcoin)核心的 [validation.cpp](https://github.com/bitcoin/bitcoin/blob/82ffd4d91832c275f791a17f697534cc677c89fd/src/validation.cpp) 。这不仅仅是比特币核心的情况。大多数最大的[区块链](https://hackernoon.com/tagged/blockchain)节点库——另一个例子是[Go ether eum](https://github.com/ethereum/go-ethereum)——绝对没有考虑到灵活性。

现在需要注意的是，在比特币的例子中，模块化并不是重点。如果你相信你正在构建未来的*单一*“货币”，为什么要让别人更容易地用你的代码来构建呢？比特币的核心是高度集成的，但它经受住了时间的考验，而且它是有效的。这不是对比特币核心代码库的批评。

# 用区块链做实验

随着区块链被认为是一切问题的答案，许多项目开始尝试不同的节点实现。特别是在敏感数据和私有链的情况下，或者外部数据是链操作的核心的情况下，项目发现基于智能契约的方法不允许他们有足够的灵活性。不幸的是，直到最近，这意味着为了创新，开发人员正在拆开像比特币那样的集成节点，或者从头开始新的节点。

这两种方法都很痛苦、耗时，并且容易出错(不安全)。

当一个拥有全新功能的区块链网络能够像 T1 一样快速运转起来时，这个领域将会真正繁荣起来。

# Stratis —模块化平台

对我来说，Stratis 最令人兴奋的事情是我们正在考虑从根本上改变区块链的建设。不同的共识算法、智能契约执行者、钱包和双向 peg 实现都是 one codebase 中的特性。

目前，您可以从一个代码库运行:

*   比特币(PoW)节点
*   战略(位置)节点
*   一个 Cirrus (PoA +智能合约)节点。

此外，由于节点的灵活性，我相信我们可以为比特币现金、黄金、私人、Doge、莱特币等构建完整的节点集成。在*天*各。在尝试了帐户模型和其他特性之后，我们可以扩展它来支持更多的链类型。

在中建立新网站时。NET Core，你可以预装一个模板，并根据你的需要快速调整组件，在一天之内就可以完成一个新的 web 应用原型。我们希望 Stratis 的开发者能够做同样的事情，但他们自己的区块链网络。

```
IFullNode node = new FullNodeBuilder() .UseNodeSettings(nodeSettings) .UseBlockStore() .UseMempool() .AddRPC() .AddSmartContracts() .UseCLRExecutor() .UseApi() .Build();
```

是不是很美？我们的[全节点回购在这里](https://github.com/stratisproject/StratisBitcoinFullNode)。

# 最后

模块化==创新。

在构建区块链网络的强大组合方法方面，我认为没有人比我们走得更远。

在推特上关注我们，了解更多:[https://twitter.com/stratisplatform](https://twitter.com/stratisplatform)

或者你只是想挥挥手:[https://twitter.com/codingupastorm](https://twitter.com/codingupastorm)