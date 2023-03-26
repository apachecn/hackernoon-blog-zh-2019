# 一个比 ZCash 更不可信，比 Monero 更可扩展，比 MimbleWimble 更用户友好的隐私区块链

> 原文：<https://medium.com/hackernoon/stegos-more-trustless-than-zcash-more-scalable-than-monero-more-user-friendly-than-mimblewimble-c25ec11307e2>

![](img/5ee0ed17d3d1d648bd2950a68dbdc8d6.png)

Photo by [Dayne Topkin](https://unsplash.com/@dtopkin1?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们生活在一个前所未有的国家监控和打击交易、言论甚至思想自由的时代。但是隐私是一项普遍的人权，我们必须为之奋斗。有各种各样的工具可以保证你的隐私，比如加密的电子邮件和信息应用程序，但是这些会留下非常明显的线索，让你知道你是谁，你在和谁说话。

隐写隐私区块链是保护您的数据、交易和通信的最佳方式。与传统的电子邮件和在线信息服务不同，它是完全去中心化的，加密安全，不会留下任何泄露秘密的线索。这是不可能的，看看你发送或接收来自谁的信息，甚至看看你是如何连接到隐写区块链。除了收件人之外，没有人可以看到您发送的内容，也没有任何东西可以将这些信息或通信与您的真实身份联系起来。

# 现有隐私区块链实施的问题

已经有好几个隐私区块链，包括 [Verge](https://vergecurrency.com/) 、 [Dash](https://www.dash.org/) 、 [ZCash](https://z.cash/) 、 [Monero](https://ww.getmonero.org/) 、 [Grin](https://grin-tech.org/) 和 [Beam](https://www.beam.mw/) ，都提供不同程度的隐私和保密。不幸的是，所有这些区块链也有缺点。例如， [Verge 提供的隐私很少，也没有什么独特的](https://bitcoinmagazine.com/articles/battle-privacycoins-verge-offers-little-privacy-and-nothing-unique/)， [Dash 并不是真正的隐私](https://bitcoinmagazine.com/articles/battle-privacycoins-why-dash-not-really-private/)， [ZCash 需要你去信任它](https://bitcoinmagazine.com/articles/battle-privacycoins-zcash-groundbreaking-if-you-trust-it/)， [Monero 很难规模化](https://bitcoinmagazine.com/articles/battle-privacycoins-why-monero-hard-beat-and-hard-scale/)。

Grin 和 Beam[都基于 MimbleWimble 技术](https://bitcoinmagazine.com/articles/battle-privacycoins-what-we-know-about-grin-and-beams-mimblewimble/)，需要发送方和接收方同时在线才能完成交易，这对于现代全球通信和业务需求来说是不切实际的。此外，Grin/Beam 网络上的任何节点都可以监听和跟踪正在交换的硬币，因此它们的硬币是不可替换的，很容易被污染。

最后但同样重要的是，上述区块链都没有为构建隐私应用程序提供平台，这大大降低了它们的实用性和可访问性。

# 隐写实现的隐私技术

使用隐私技术，如[基于配对的密码术(PBC)](https://www.math.uwaterloo.ca/~ajmeneze/publications/pairings.pdf) 、 [BLS 签名](/cryptoadvance/bls-signatures-better-than-schnorr-5a7fe30ea716)、Schnorr 签名、[机密交易(CT)](https://www.mycryptopedia.com/what-are-confidential-transactions/) 、隐形地址、[bullet proof](https://crypto.stanford.edu/bulletproofs/)、 [ValueShuffle](https://www.researchgate.net/publication/321146472_ValueShuffle_Mixing_Confidential_Transactions_for_Comprehensive_Transaction_Privacy_in_Bitcoin) ，加上通过 [OmniLedger 分片](https://infoscience.epfl.ch/record/255586?ln=en)的可扩展性，Stegos 修复了现有隐私币的缺点，并提供了完整和完全的隐私，没有可用性缺陷。

我们通过内部开发的技术 *BlockCrunch* 、 *Snowball* 和 *SafeData* 以及*可信应用容器*来改善区块链的现状，以便轻松方便地部署基于 Stegos 平台构建的新隐私应用。

# 匿名性、可替代性和不可追踪性

匿名性、可替代性和不可追踪性是隐私硬币的基本要求。例如，比特币不是匿名的，因为钱包地址是公开的。比特币也不是不可追踪的，因为使用 block explorer 和专门的区块链分析工具可以很容易地跟踪交易历史。

可替代性是一种商品或货币的一个单位可以自由地与另一个单位交换的能力。例如，美元是可替代的，因为任何一张一美元的钞票都可以换成另一张一美元的钞票而不会贬值。比特币不可替代的原因与它不匿名的原因相同——所有比特币支付都可以被自由追踪，如果被用于非法活动，比特币可以被贴上*污点*的标签。加密交易所和企业可能会拒绝接受这些受污染的硬币，使它们与其他硬币相比价值更低。没有价值损失的交换不再可能，这些硬币被认为是不可替代的。

可替代性很重要，因为任何受污染硬币的最近接收者可能会被留在*手中*，尽管不知道他们之前的非法使用。如果被污染的硬币受到当局的制裁，他们甚至可能无法获得他们的钱。*保密交易*通过对每笔交易的输入和输出进行加密来提高可替代性，使得区分受污染的硬币和未受污染的硬币变得更加困难。但是它们并没有完全解决问题。

像 Monero 和 ZCash 一样，Stegos 使用一次性支付地址。这使得不可能识别交易的接收者，因为每个交易都被定向到一个新的和唯一的(秘密的)地址。

我们通过隐藏每个交易中的输入和输出金额，并用它们的 [Pedersen 承诺](https://crypto.stackexchange.com/questions/64437/what-is-a-pedersen-commitment)来替代它们，从而实现机密交易。只有硬币的发送者和接收者知道实际使用的价值。我们通过证明所有输入的总和大于或等于所有输出的总和来保护交易。(不可能判断一个隐形金额是正还是负，所以也要对每个隐形金额的值进行防弹，这证明它在某个数值范围内。)

我们不在块中存储事务，而是将它们简化为输入和输出，类似 MimbleWimble 风格。这使得在我们的区块链上追踪交易变得几乎不可能。虽然植入区块链的恶意节点理论上可以收集和存储交易历史，以便稍后进行分析，并可能污染硬币或识别发送者和接收者，但这既不太可能，也不切实际。这也是 MimbleWimble 等其他隐私技术常见的问题。

*Snowball* ，我们混合机密事务的协议，建立在 [ValueShuffle](https://www.researchgate.net/publication/321146472_ValueShuffle_Mixing_Confidential_Transactions_for_Comprehensive_Transaction_Privacy_in_Bitcoin) 之上，完全切断了每个事务的输入和输出之间的关系，以及发送者和接收者之间的关系，提供了完全的不可追踪性和可替代性。

雪球形成发送者池，他们希望混合他们的交易，然后创建一个超级交易，使用 [DiceMix](https://eprint.iacr.org/2016/824) 混合它。然后附上集体签名，交易公布。任何人在雪球超级交易中所能看到的就是所有的投入都被花掉了，并且每个产出都与一个或多个投入相关联。不可能区分哪个输出对应于哪个输入。

# 保持区块链小

许多区块链人都在谈论达到每秒 100 万次交易(TPS ),但没有人谈论他们将如何维持如此快速增长的区块链。比特币只有 7 TPS，区块链预计到今年年底将超过 170GB。据估计，非现金交易量为每天 14 亿笔，预计将呈二次方增长，目前的交易量仅为 16k TPS。

使用平均 250 字节的比特币交易，这将每天产生 350 千兆字节，或每年 127 兆字节。这种数据量是完全不可持续的，只能在几台非常集中的超级计算机上处理。

Stegos 在其共识协议和块签名中使用 [BLS](https://en.m.wikipedia.org/wiki/Boneh%E2%80%93Lynn%E2%80%93Shacham) 代替 Schnorr 签名。这允许我们通过将块中的每个签名组合成单个签名来同时最小化网络通信、提高处理速度并保持块大小较小。

我们还利用我们内部研发的产品 *BlockCrunch* 技术直接解决不断增长的区块链问题。我们没有将事务存储在每个块中，而是将它们分解成输入和输出的 Merkle 树。在接收每个块并将其添加到链的末端之前，隐写验证器会对每个输出所消耗的输入进行加密安全修剪。那么，Stegos 区块链就不像比特币那样是每一笔交易的分类账，而更像是一个未用完硬币的数据库。这使得链条变得更小，并且没有交易历史可追踪，因此无法损害隐写术硬币的隐私和可替代性。

# 拒绝无用的智能合同

在 Stegos，我们坚信智能合约是无用的，并且在可预见的未来将继续无用，尽管有 ERC20 令牌和 CryptoKitties。然而，区块链是一种强大的去中心化和无信任数据交换机制，我们利用 *SafeData* 技术和*可信应用容器* (TAC)来利用这种力量，这两种产品都是内部研究的产物。

借助 *SafeData* 和我们的软件开发套件(SDK ),开发人员可以轻松构建移动应用，在完全保密的情况下交换数据。*可信应用程序容器* (TAC)可以轻松部署隐私应用程序，并为这些应用程序提供方便的编程接口(API)来访问存储在区块链上的数据，以及收取应用程序使用的订阅费。

受微信及其迷你应用的启发，我们将 TAC 设计为一个集成钱包的单个移动应用，可以运行多个隐私应用。隐写隐私迷你应用程序可以使用 XML、CSS 和 JavaScript 开发，所有开发人员都已经熟悉这些技术。

# 保存区块链的数据

有许多应用程序将受益于在区块链上存储数据，但由于数据需要频繁修改，它们无法做到这一点。每次收到新的报价或交易时，交易应用程序或分散式交易所(DEX)都需要复制整个订单簿。小额支付，例如为短视频流付费，是另一个有吸引力的用例，在当前的区块链方法中完全不切实际。

频繁修改的数据会消耗大量区块链空间，尽管只需要数据的最新副本。比特币和其他区块链已经开始开发第二层技术，如闪电网络和国家频道，以避免在区块链上存储频繁修改的数据。但是隐写术不需要这样的解决方案。

我们通过使用与常规支付交易相同的 Pedersen 承诺和 Bulletproofs 来保护数据交易。这使得我们也可以像修剪用过的硬币一样修剪用过的数据，从而保持隐写区块链小巧灵活。

# 利害关系证明共识

赌注证明(PoS)是一种共识算法，通过随机选择的各种组合以及赌注资金的财富和年龄来选择下一个区块的创建者。PoS 区块链比基于工作验证(PoW)算法的货币更节能。

可扩展的抗偏置分布随机性是隐写术的关键组成部分。我们用它来选择验证组和选举每个共识轮的领导者，等等。隐写随机性基于[验证的随机函数(VRF)](https://tools.ietf.org/id/draft-goldbe-vrf-00.html) 和对 [RandHerd](https://eprint.iacr.org/2016/1067.pdf) 的改进，后者是一种分布式协议，能够使潜在的大型服务器集合形成分布式公共随机性信标，该信标主动生成一系列规则的公共随机输出。

我们的随机性协议从块头上的 BLS 签名生成分布式公共随机性信标。vrf 用于排除每个共识回合的领导者进行[利益输送](https://github.com/ethereum/wiki/wiki/Proof-of-Stake-FAQs#how-does-validator-selection-work-and-what-is-stake-grinding)的可能性。

隐写一致性协议基于[实用拜占庭容错(pBFT)](https://blockonomi.com/practical-byzantine-fault-tolerance/) ，但增加了强大的一致性，使所有验证器能够就块的有效性达成一致，而无需浪费计算周期来解决分叉和不一致。一项交易一旦出现在区块链，就可以被认为是确认的。

我们还采用[集体签名(CoSi)](https://arxiv.org/pdf/1503.08768.pdf) ，这是一种可扩展的见证人共同签名协议，可确保在任何客户接受之前，每个权威声明都经过不同见证人群体的验证和公开记录。CoSi 建立在现有的加密多重签名方法的基础上，通过高效通信上的签名聚合来扩展它们，以支持成千上万的证人。

CoSi 的默认实现使用 Schnorr 签名，出于性能原因，我们将其替换为 BLS 签名。CoSi 的最初设计使用 Schnorr 签名和基于树的通信。出于安全和性能原因，我们用 BLS 签名和基于流言蜚语的通信来代替它。

# 结论

由于无需执行繁重的 PoW 计算，任何人都可以通过在口袋中的智能手机上运行隐写节点并帮助验证隐写交易来赚取硬币。

> 访问我们的[网站](http://stegos.com)，阅读[博客](http://stegos.com/blog)，加入我们的[电报聊天](https://t.me/stegos4privacy)来讨论这个帖子。