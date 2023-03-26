# 去亚特兰蒂斯:以太坊经典(ETC) ECIP-1054 硬叉

> 原文：<https://medium.com/hackernoon/going-to-atlantis-ethereum-classic-etc-ecip-1054-hard-fork-a9a84ef6a785>

## 以太坊经典的下一个硬叉子的权威指南

![](img/7350b2591b19fae659dc18f3090badf9.png)

亚特兰蒂斯 ECIP-1054 硬叉已经宣布，在 ETC 生态系统的利益相关者中受到了极大的欢迎。提议的改变将“使杰出的以太坊基金会*伪龙*和*拜占庭*网络协议在以太坊经典网络上以代号为*亚特兰蒂斯*的硬分叉升级，以实现这些网络之间的最大兼容性。”

## 您可以在 [ETC Labs Core Github](https://github.com/etclabscore/ECIPs/blob/master/ECIPs/ECIP-1054.md) 上找到硬分叉建议:

[](https://github.com/etclabscore/ECIPs/blob/master/ECIPs/ECIP-1054.md) [## etclabscore/ECIPs

### 以太坊经典实验室 ETC 改进提案的工作存储库。- etclabscore/ECIPs

github.com](https://github.com/etclabscore/ECIPs/blob/master/ECIPs/ECIP-1054.md) 

该团队可就 [ETC 实验室核心 Discord 服务器](https://discordapp.com/invite/8jztJk5)进行提问和进一步讨论。

硬分叉的目标是以太坊经典 mainnet 上的块`8_750_000`，预计在 2019 年 9 月中旬的某个时间

**经过这次硬分叉，以太坊经典网将受益:**

## 一个更可预测的 ETC 发行利率

*   [EIP 100](https://eips.ethereum.org/EIPS/eip-100) (将难度调整改为目标平均阻滞时间，包括大叔)

目前的公式不包括“叔叔率”，如果操纵“叔叔率”，可能导致更高的发行率。

通过添加此公式来计算包含的叔叔的确切数量:

```
adj_factor **=** max(1 **+** len(parent**.**uncles) **-** ((timestamp **-** parent**.**timestamp) **//** 9), **-**99)
```

难度调整算法现在将目标锁定在一个恒定的平均生成率，包括叔叔

## 更简单的分散式应用程序开发

操作码和预编译契约的目标是使分布式应用程序的开发更加高效

**操作码**

*   [EIP 140](https://eips.ethereum.org/EIPS/eip-140) (以太坊虚拟机中的`REVERT`指令)

`REVERT`指令为**提供了一种停止执行和恢复状态变化的方法，**无需消耗所有提供的气体，并具有返回原因的能力。目前没有办法在不消耗所有剩余气体的情况下做到这一点。

*   [EIP 211](https://eips.ethereum.org/EIPS/eip-211) (新操作码`RETURNDATASIZE`和`RETURNDATACOPY`)

**一种允许在 EVM** 中返回任意长度数据的机制。在一次调用之后，返回的数据被保存在一个虚拟缓冲区中，调用者可以将它(或其中的一部分)复制到内存中。在下一次调用时，缓冲区被覆盖。

*   [EIP 214](https://eips.ethereum.org/EIPS/eip-214) (新操作码`STATICCALL`)

这个提议增加了一个新的操作码**，可以用来调用另一个契约**(或者它自己)，同时不允许在调用过程中对状态进行任何修改。这允许契约进行明显非状态改变的调用，让开发人员和评审人员放心，重入错误或其他问题不可能从那个特定的调用中产生；**它是一个纯函数，返回一个输出，不做其他任何事情。**

**预编译合同**

*   [EIP 198](https://eips.ethereum.org/EIPS/eip-198)(BIGINT 模幂运算预编译契约)

这个预编译合同是为了在 EVM 内部进行**高效的 RSA 验证。基于位的指数计算是专门为经常使用的指数 2(用于乘法)和 3 以及 65537(用于 RSA 验证)进行的。**

## **预编译契约**以便于执行 zkSNARKS

*   [EIP 196](https://eips.ethereum.org/EIPS/eip-196) (椭圆曲线上加法和标量乘法的预编译契约`alt_bn128`
*   [EIP 197](https://eips.ethereum.org/EIPS/eip-197) (椭圆曲线上最优 ate 配对检查的预编译契约`alt_bn128`

**zkSNARKS 提供隐私和可扩展性解决方案**，这些解决方案目前过于昂贵，无法在区块气限制内验证。这些预编译的合同将**降低天然气成本**并且仍然足够灵活，可以对 zkSNARKS 进行进一步的研究。

## 以太坊经典上更高水平的**性能**

*   [EIP 161](https://eips.ethereum.org/EIPS/eip-161) (状态-trie 清算)

该 EIP 专注于通过移除空帐户来“去模糊”区块链状态大小，提供优化，如**更快的同步时间**

*   EIP 170 (合同代码尺寸限制)

这种 EIP **防止了一种攻击场景**，在这种攻击场景中，可以以固定的开销重复访问大量的帐户代码。大小限制设置为 24576 字节，比任何当前部署的契约都大。

*   [EIP 658](https://eips.ethereum.org/EIPS/eip-658) (在收据中嵌入交易状态代码)

随着 EIP98 的过时和 [EIP 140](https://eips.ethereum.org/EIPS/eip-140) 中`REVERT`的引入，调用者没有明确的机制来确定事务是否成功以及其中包含的状态更改是否被应用。**该 EIP 用状态码**替换中间状态根，0 表示失败(由于任何可能导致事务或顶级调用恢复的操作)，1 表示成功。

**每次变更都记录在 EIP 中。每个 EIP 的技术规格可分别在这些文件中找到:**

*   [EIP 100](https://eips.ethereum.org/EIPS/eip-100) (将难度调整改为目标平均阻滞时间，包括大叔)
*   [EIP 140](https://eips.ethereum.org/EIPS/eip-140) ( `REVERT`以太坊虚拟机中的指令)
*   [EIP 161](https://eips.ethereum.org/EIPS/eip-161) (州-特里清算)
*   EIP 170 (合同代码尺寸限制)
*   [EIP 196](https://eips.ethereum.org/EIPS/eip-196) (椭圆曲线上加法和标量乘法的预编译契约`alt_bn128`
*   [EIP 197](https://eips.ethereum.org/EIPS/eip-197) (椭圆曲线上最优 ate 配对检查的预编译契约`alt_bn128`
*   [EIP 198](https://eips.ethereum.org/EIPS/eip-198)(BIGINT 模幂运算的预编译契约)
*   [EIP 211](https://eips.ethereum.org/EIPS/eip-211) (新操作码`RETURNDATASIZE`和`RETURNDATACOPY`)
*   [EIP 214](https://eips.ethereum.org/EIPS/eip-214) (新操作码`STATICCALL`)
*   [EIP 658](https://eips.ethereum.org/EIPS/eip-658) (在收据中嵌入交易状态代码)

**实施这些变更的建议模块如下:**

*   `1_039_000`在 Kotti Classic PoA-testnet 上(2019 年 8 月初)
*   `4_723_000`在现代经典 PoW-testnet 上(2019 年 8 月初)
*   `8_750_000`关于以太坊经典 PoW-mainnet(2019 年 9 月中旬)

最近有一次与几个利益相关者的关于亚特兰蒂斯的讨论，已经被记录下来并在这里[提供](https://youtu.be/plrV7HNsEKo)

有兴趣更多地参与 ETC 吗？我们致力于加速以太坊经典的开发，需要您的帮助！请联系我们，看看您今天如何能更多地参与进来！

我们在招人！——[https://www.linkedin.com/jobs/view/1144896854/](https://www.linkedin.com/jobs/view/1144896854/)

**我们的团队链接:**[**Github**](https://github.com/etclabscore)**，** [**中**](https://medium.com/etclabscore) **，** [**推特**](https://twitter.com/etclabscore)

**来和我们一起聊聊** [**不和**](https://discordapp.com/invite/NgzMPaj)