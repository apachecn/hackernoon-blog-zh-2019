# 密码经济建模的(简短)介绍

> 原文：<https://medium.com/hackernoon/a-short-intro-to-cryptoeconomic-modelling-1-2-a840fa55bd18>

![](img/c3c42d81b8d1d15b56ecdae55ac8c243.png)

> 用 Python 设计和测试协议机制

# 概观

如果你一直在关注 2017 年加密热潮后的区块链空间，你可能会遇到关于可扩展性、互操作性和治理的讨论。

今天，摩根大通宣布创造他们的新稳定硬币“JPM 硬币”,以支持他们新颖的企业支付网络。无论你是一家使用公共链技术的初创公司，还是一家使用基于许可的分类账的企业，今天都标志着一场标记化世界的竞赛的开始。

经济学的未来将是可互操作的，这意味着资产、代理和交易逻辑可能都存在于不同的分类账/平台中。因此，虽然竞赛已经开始，但围绕分类帐和分类帐间系统的可伸缩性和健壮性，仍有重要的协议级挑战需要解决。

## TL；速度三角形定位法(dead reckoning)

本文是探讨经济和加密机制设计的两部分系列文章的第一部分。

*   第一部分:我们将使用 Python3 构建一个简单的区块链(本文)。
*   第二部分:我们将确定网络的期望结果，并使用第一部分中的区块链设计/测试机制。

设计协议级技术并不容易，但是许多这样的机制可以用来创建“部分分散”的组织(两害相权取其轻)。

权力下放不会在一夜之间发生。无论你是分析师、程序员还是“壁橱里的”经济学家，这个系列都旨在加强你对协议的理解，并为你提供一个设计和测试**实用**加密经济机制的框架。

# 第一部分:使用 Python 构建一个简单的区块链

在第一篇文章中，我们将使用 Python3 构建一个简单的区块链。我推荐克隆脚本的 Jupyter 笔记本版本或使用 colab 链接(您不需要在那里安装依赖项)。这个区块链模拟将作为测试本系列第二部分中讨论的未来加密和经济机制的基础。

*   Google colab 链接(推荐):[https://colab . research . Google . com/drive/1 u 3 ZP 3 sckhwusslox 6 ko 4 ljcnuzl 0 LPE 3](https://colab.research.google.com/drive/1U3Zp3SckhwussLox6Ko4lJCnUzl0lpE3)
*   Jupyter 笔记本:[https://github . com/albertocevallos/Intro _ To _ Cryptoeconomic _ modeling/blob/master/block chain . ipynb](https://github.com/albertocevallos/Intro_To_Cryptoeconomic_Modelling/blob/master/blockchain.ipynb)
*   github repo:[https://github . com/albertocevallos/Intro _ To _ Cryptoeconomic _ modeling](https://github.com/albertocevallos/Intro_To_Cryptoeconomic_Modelling)

我们开始吧！

## 步骤 1:导入依赖关系

您需要导入以下依赖项:

```
**import** **datetime**
**import** **hashlib**
```

“datetime”将用于生成时间戳，“hashlib”包含创建块时使用的哈希算法。您可以使用 [pip](https://pypi.org/project/pip/) 安装依赖项。

## 步骤 2:创建一个块

现在，我们将定义“块”数据结构。在这个模拟中，每个块有 7 个属性。

**block no:**block 的编号。
**数据**:存储在块中的任何数据(如交易、证书、图片)。
**下一个**:指向下一个块的指针。
**hash** :唯一 ID 和完整性验证机制，包含块属性的签名(不变性机制)。
**nonce** :数字只使用一次。
**previous_hash** :存储链中前一个块的 hash (ID)。
**时间戳**:包含块生成时的时间戳。

```
**class** **Block**:
    blockNo = 0
    data = **None**
    next = **None**
    hash = **None**
    nonce = 0
    previous_hash = 0x0
    timestamp = datetime.datetime.now()

    **def** __init__(self, data):
        self.data = data

    **def** hash(self): h = hashlib.sha256() h.update(
        str(self.nonce).encode('utf-8') +
        str(self.data).encode('utf-8') +
        str(self.previous_hash).encode('utf-8') +
        str(self.timestamp).encode('utf-8') +
        str(self.blockNo).encode('utf-8')
        )
        **return** h.hexdigest()

    **def** __str__(self):
        **return** "Block Hash: " + str(self.hash()) + "**\n**BlockNo: " + str(self.blockNo) + "**\n**Block Data: " + str(self.data) + "**\n**Hashes: " + str(self.nonce) + "**\n**--------------"
```

' **init** '函数将在数据存储后生成一个块。' **hash** '函数包含计算' hash '属性的逻辑。

![](img/979cdd36497d0b79763a6f0b4d64daea.png)

Each ‘hash’ serves as the next block’s ‘previous_hash’ attribute, forming a chain.

哈希函数是确定性的，这意味着如果您在给定的块中更改一个事务，它将更改该给定块的哈希，并更改在它之后产生的所有块。

我们将使用 SHA-256 来生成代表一段文本的几乎唯一的 256 位签名。散列函数的输入将是由 5 个块属性(nonce、数据、previous_hash、时间戳和块号)组成的串联字符串。

## 第三步:创建区块链

让我们来定义‘区块链’数据结构本身。它将由通过散列指针链接在一起的块组成。

我们将为 sybil 控件使用一个基本的工作验证机制。请记住，区块链将 PoW 或 PoS 机制与共识协议(如最重/“最长”链选择(Nakamoto 共识)或 pBFT、Tendermint)结合起来以实现共识。

在我们的模拟中，我们只有一个节点(miner)，它需要计算不同的 nonce 值，直到块的散列小于或等于网络的当前目标(难度)。

难度或差异决定了开采一个区块在计算上的严格程度。' **maxNonce** '是我们可以在 32 位数字中存储的最大数，而' **target** '是挖掘块的目标哈希值(需要添加到链中)。

然后，我们创建我们的“创世纪块”,并将其设置为我们的区块链的头部。

让我们定义一下我们的' **add** 函数。我们只需要添加块作为我们唯一的参数。我们使用链中最近的块的散列(ID)来更新' **previous_hash** 属性。我们还将通过将前一个块的编号加 1 来更新“blockNo”属性(因为这是链中的下一个块)。

我们将设置(新的)块等于它自己，使它成为链的头。

```
**class** **Blockchain**:

    diff = 20
    maxNonce = 2**32
    target = 2 ** (256-diff)

    block = Block("Genesis")
    head = block

    **def** add(self, block):

        block.previous_hash
        block.blockNo = self.block.blockNo + 1

        self.block.next = block
        self.block = self.block.next

    **def** mine(self, block):
        **for** n **in** range(self.maxNonce):
            **if** int(block.hash(), 16) <= self.target:
                self.add(block)
                print(block)
                **break**
            **else**:
                block.nonce += 1
```

为了挖掘链的头部，块将需要具有等于或小于“**目标**”值的散列。我们将运行一个循环，直到这个语句为真。

## 第四步:打印区块链

最后，让我们初始化区块链，并运行挖掘功能 10 次。打印出 10 块的内容。

```
blockchain = Blockchain()

**for** n **in** range(10):
    blockchain.mine(Block("Block " + str(n+1)))

**while** blockchain.head != **None**:
    print(blockchain.head)
    blockchain.head = blockchain.head.next
```

# 结论

我们可以使用 Python 对区块链架构中使用的基本加密经济机制和数据结构进行建模。接下来，我们将分析:

*   攻击媒介(例如 sybil 攻击)
*   治理(如决策、共识)
*   产品要求(如安全级别、速度)

了解你的网络的预期结果对于设计经济激励(例如，阻止奖励、惩罚)和加密协议(例如，P2P 加密消息)是至关重要的。这些机制中的许多可以融入新兴创业公司和成熟企业的代码中，确保更高水平的隐私、安全和信任。

在未来，所有的公司都会以这样或那样的方式涉足科技。企业家、分析师和程序员可以利用这种思维来建立部分分散的组织，并将它们连接到更大的基础设施，如[以太坊](https://www.ethereum.org/)或 [Polkadot](https://polkadot.network/) 。

应用研究@[keter . io](https://keter.io/)
[https://twitter.com/acvlls](https://twitter.com/acvlls)
[https://www.linkedin.com/in/albertocevallos/](https://www.linkedin.com/in/albertocevallos/)

![](img/578859d799a6b2f106c19d1fcc0f9bf5.png)

The race to tokenization has begun!