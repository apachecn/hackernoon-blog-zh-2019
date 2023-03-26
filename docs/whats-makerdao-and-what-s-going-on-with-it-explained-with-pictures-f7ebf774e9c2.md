# MakerDAO 是什么，它是怎么回事？用图片解释。

> 原文：<https://medium.com/hackernoon/whats-makerdao-and-what-s-going-on-with-it-explained-with-pictures-f7ebf774e9c2>

![](img/e44805bcb81b5097ba5decb65a12a04a.png)

# 什么是马克尔道？

MakerDAO 是稳定硬币 DAI 背后的一个协议，DAI 是一种加密货币，与美元保持 1:1 的联系。把 1 戴想象成 1 美元。它的独特之处在于，每个 DAI 都是由以太支持的，而不是声称拥有所需抵押品的第三方。由于乙醚是易挥发的，这给保持钉住汇率带来了一些有趣的挑战。

![](img/7a0eff4acd0fc065c8f1ba6f00291cd9.png)

该项目始于 2015 年，没有进行 ICO，而是选择私下出售 MKR 代币，以资助未来的发展。制造商的戴稳定硬币于 2018 年初推出，自那以来经历了巨大的牵引力。

## 我们为什么需要戴？

处理密码的易变性是一个问题。正如圈子里的许多人所知，戴并不是这个圈子里第一个稳定的硬币。前辈包括 Tether，TrueUSD 等少数几个。然而，所有这些项目的风险是，持有实际美元的保管方将出于任何监管原因拒绝兑换稳定硬币。这违背了无许可加密的精神。此外，我们必须相信保管解决方案实际上有正确的美元数额，而不是制造人为的通货膨胀。

## 谁在使用它？

戴可以说是在这个时间点上建立在以太坊上的最成功的项目。它目前在其智能合约中持有 2%的乙醚，并在其系统中发行了超过 7700 万美元的 DAI(债务)。此外，Maker 在 DAI 发行方面继续保持 20%的月环比增长，71%的用户在购买 DAI 后立即消费。这标志着用法的转变，而不是投机。

![](img/b285e86ebf430065a99d02cd78bf13bc.png)

Source: [https://medium.com/makerdao/dai-in-numbers-2710d8a5633a](/makerdao/dai-in-numbers-2710d8a5633a)

# 它是如何工作的？

这个系统的基本原理是这样的:

*   您将乙醚存入/发送至制造商的智能合约，创建抵押债务头寸(CDP)。

![](img/4052c39f175707ec33cf2f2e9446c264.png)

*   假设您存入 1 ETH(价值 100 美元)，这将允许您用 100 美元获得最多 40 DAI(假设 150%的抵押率为 100 美元/1.50 美元)。然而，如果乙醚的价格低于 100 美元，你的 CDP 将被强制关闭。为了阻止这种情况的发生，你需要首先放入更多的乙醚或取出更少的戴。**这是为了确保总有足够的资金锁定被取出的资金量。**

![](img/15ae518c317ab91d92adda66773472c2.png)

*   如果你想要回你的乙醚，你需要付还你取出的乙醚，外加一笔小额费用。

![](img/d4f1db0fd124b59b0ad7471ddcf61ae4.png)

这可能有点让人不知所措。

这里有几个简单的例子说明 CDP 的生命周期是如何进行的，假设以太坊的价格是 150 美元，您以此价格存入 1 ETH:

1.  你决定取出 **50 DAI** ，这意味着你的 CDP 被抵押 **300%** 。只要以太坊的价格不跌破 **$75** (50 * 150%)你的位置就安全了。一年后，你决定偿还 50 戴并取回被锁起来的乙醚。在平仓或与您的 CDP 进行其他互动时，您将支付年度稳定费(自 2019 年 3 月起设定为 3 **.5%** )。
2.  你决定取出 **100 DAI** ，这意味着你的 CDP 抵押率仅为 **150%** 。以太坊的价格跌至 **$100** ，这意味着你的 CDP 抵押不足**$ 50**($ 100 * 1.5 = $ 150<$ 100)。第三方将意识到你没有足够的抵押品，并代表你清算你的 CDP。这将导致您的头寸被第三方清算并处以罚金。这些第三方有各种方式从你的平仓中获利。
3.  你决定取出 **75 DAI** ，这意味着你的 CDP 抵押率为 **200%** 。由于牛市开始，以太坊的价格涨到了 300 美元。您的 CDP 现在的抵押率为 **400%** 。因为你是一个 ETH 多头，你认为价格不会下跌，你决定多取**100 DAI**，使你的 CDP 比率达到 **175%** 。

# 谁控制这个系统？

在 MakerDAO 生态系统中，他们的本地 MKR 令牌允许令牌持有者影响协议的某些方面，例如:

*   每年的借款费用应该是多少(**稳定费用**)？
*   每个 CDP 需要多少抵押品(**抵押比率**)？
*   在乙醚价格暴跌或任何其他不可预见的情况下关闭协议(**紧急关闭**)？

到目前为止，我还没有提到的一个重要信息是，当支付稳定费时，从市场上购买**美元等值的 MKR** 来支付稳定费。这意味着 MKR 实际上是一种通货紧缩货币。

其核心是，MakerDAO 就像是一个以一定利率发放贷款的信贷机构。如果利率(稳定费)低，就会鼓励人们借更多的钱(锁定更多的 ETH)。如果利率高，资本成本就高，降低了借贷的吸引力(抛售 CDP)。最近，戴一直在低于 1 美元的交易所交易。出现这种情况的一个重要原因是戴持有人施加了巨大的抛售压力。DAI 只能通过 CDP 的打开生成。为了让挂钩回到正确的目标价格，MKR 持有者投票决定**将**稳定费**从 1%提高到 3.5%** ，希望这能减少开设 CDP(并关闭现有 CDP)的动机**增加买入压力**。未来，Maker 团队计划引入 DAI 储蓄率，这将允许 DAI 持有者锁定他们的 Dai 并赚取利息。支付给持有者的利息来自稳定费，该费用目前用于购买和焚烧 MKR。

![](img/fff59dca2ca59f7bbb8fa278c1e37469.png)

现在，在 MakerDAO 系统包含的抵押品比预期的少的情况下(乙醚的快速崩溃)，额外的 MKR 被发行并在公开市场上出售，以偿还 ETH 和 DAI 持有人。正是出于这个原因，MKR 持有者不想把抵押比率定得太低，因为他们是最后的买家。然而，随着借贷成本的增加，他们不想把利率定得太高。

## 正以太网反馈回路

我们无法证实但可以推测的一种行为是 CDP 持有者加倍持有他们的头寸(或在 ETH 上“做多”)。

基本上是这样的:

*   **开立抵押率为 200%的 CDP**
*   **等待**以太坊价格升值
*   **从你的 CDP 中提取更多的 DAI** (因为你可以不降低你的抵押率)
*   用你新铸造的戴购买更多的 ETH
*   **将** **ETH** **添加到** **your** **CDP** 中，为您的 CDP 提供超额抵押，以防范任何市场下跌

![](img/5d9f1ee16a23a43322744af17a067cfc.png)

虽然这听起来没有什么害处，但如果明天 ETH 从目前的 150 美元升值到 500 美元，那么目前 250%的抵押率(平均)将上升到 75900%。根据目前的数字，这意味着可以铸造 1.5 亿 dai，这可能会对乙醚产生巨大的购买压力，导致正反馈循环。目前 1 亿英镑的债务上限限制了这种情况的发生，但随着多抵押品 DAI 的引入，这一上限很可能会提高。

# 多络戴

到目前为止，MakerDAO 实验在社区中似乎是成功的，并且每个月都在增长。然而，一个很大的限制是，你只能使用乙醚抵押你的 CDP。随着多抵押品 DAI 的推出，您可以使用任何 ERC20 代币来抵押您的 CDP。你真的可以用你包装好的比特币来抵押你的 CDP！

虽然这一切听起来很好，但有两个含义是 Maker 粉丝应该知道的:

1.  如果发行者被迫冻结资产，使用包装比特币(由 BitGo 支持)等托管资产可能会导致抵押不足的 CDP。这方面的一个例子是，当局告诉 BitGo，他们希望将来自 MakerDAO 地址的包装比特币列入黑名单。这意味着支持 CDP 的比特币毫无价值。
2.  由于以太坊不是抵押品中的唯一资产，这意味着 ETH 价格的任何正反馈循环都无法实现，因为实际贡献的 ETH 更少。我不认为这一定是一件坏事，但这是值得注意的。

# 结束语

像积极的 ETH 反馈循环和 Dai 储蓄率这样的东西将会非常令人兴奋地出现在现实生活中，因为我们从来没有见过这样大规模的东西。再加上所有其他开放金融项目，这个实验将是令人兴奋的。

我个人认为创客是继比特币和以太坊之后第三个最成功的实验。该团队忠于 crypto 的价值观，参与社区活动，并为整个领域做出了有意义的贡献。

他们也举办很棒的派对，但那是以后的事了。

## 参考资料:

*   [https://hackernoon.com/an-overview-of-makerdao-21e9f34aa1f3](https://hackernoon.com/an-overview-of-makerdao-21e9f34aa1f3)
*   https://medium.com/makerdao/dai-in-numbers-2710d8a5633a
*   【https://medium.com/pov-crypto/evaluating-mkr-def6d36092bd 
*   [https://medium . com/POV-crypto/could-maker Dao-trigger-a-positive-feedback-loop-of-increasing-eth-price-FCE 80 b 27119](/pov-crypto/could-makerdao-trigger-a-positive-feedback-loop-of-increasing-eth-price-fce80b27119)
*   [https://medium . com/maker Dao/raise-the-stability-fee-to-3-5-f0d 6731 b 1041](/makerdao/raise-the-stability-fee-to-3-5-f0d6731b1041)
*   [https://medium . com/@ vision hill _/a-maker Dao-case-study-47 a31d 858 be 5](/@visionhill_/a-makerdao-case-study-47a31d858be5)
*   [https://www . Reddit . com/r/maker Dao/comments/asb9n 6/a _ couple _ questions _ concerns _ about _ maker _ and _ mkr/](https://www.reddit.com/r/MakerDAO/comments/asb9n6/a_couple_questions_concerns_about_maker_and_mkr/)
*   [https://medium . com/maker Dao/Dai-reward-rate-earn-a-reward-from-holding-Dai-10a 07 f 52 F3 cf](/makerdao/dai-reward-rate-earn-a-reward-from-holding-dai-10a07f52f3cf)