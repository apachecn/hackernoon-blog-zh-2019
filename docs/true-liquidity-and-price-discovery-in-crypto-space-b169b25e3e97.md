# 加密空间中的真实流动性和价格发现

> 原文：<https://medium.com/hackernoon/true-liquidity-and-price-discovery-in-crypto-space-b169b25e3e97>

## 比特币基地是 BTC/美元对中已建立的数字资产交易所的流动性领导者。Bitstamp 和北海巨妖紧随其后，Bitfinex、币安和 Bittrex 紧随其后。

## TL；速度三角形定位法(dead reckoning)

(1)比特币基地 Pro 提供了加密货币市场中最好的比特币流动性和执行机会。Bitstamp 和北海巨妖分别是机构投资者和散户投资者非常好的选择。Bitfinex 在账面顶部提供了深度流动性。

(2)流动性是选择交易所的重要标准，因为它比人工流动性更容易产生人工(虚假)交易量，这通常意味着欺诈性交易所的市场风险。

(3)对订单动态的分析说明了价格发现是如何工作的。这个生态系统是高度相关的，最初的价格变动几乎会立即引发其他交易所的价格变动。然而，报告的高交易量并不自动意味着价格发现。2019 年 4 月 2 日的飙升可能源于 Bitfinex，而其他交易所也追随了这一趋势。

(4)币安有一个非常奇怪的连续交易间隔柱状图。它看起来像一个下降趋势的正弦函数。然而，其他交易所也有可疑的异常值。

该分析基于 2019 年 4 月 12 日世界协调时 0:00 至 2019 年 4 月 17 日世界协调时 0:00 六个选定交易所的 BTC/美元(币安为 BTC/USDT)数据:

1.  币安
2.  Bitfinex
3.  位戳
4.  比特雷克斯
5.  比特币基地专业
6.  北海巨妖

![](img/7bfc15527aac51fcf748e1f7859bbbd7.png)

Best bid-offer spread. Source: Crypto Integrity

## 内容:

1.  为什么流动性很重要？
2.  流动性指标
3.  流动性评分
4.  分析和结果
5.  2019 年 4 月 2 日比特币价格飙升
6.  数据
7.  已知问题

# 为什么流动性很重要？

流动性衡量在不对价格产生重大影响的情况下购买/出售资产的能力。我们已经写了[为什么流动性很重要](/crypto-integrity/why-liquidity-matters-90cf906c1bad)，但是让我们再给你举一个例子。比如说，目前的买卖价差是 5%。对交易者或投资者来说意味着什么？如果一个人想在市场上以 100 美元购买一项资产 XXX，如果价格没有任何变化，他将不得不以 95 美元出售 XXX，不考虑交易费和滑点。换句话说，XXX 必须升值至少 5%(更准确地说，升值 5.3%)，投资者才能在不亏损的情况下卖出。

此外，流动性显示了最终影响价格的现有需求和供应。然而，这可能并不明显，流动性确实能很好地洞察价格发现背后的机制。大多数秘密交易所只是从主要交易所复制价格，这对任何人来说都不是秘密——套利者和做市商都做得很好。真正的需求和真正的供给在主要的交易所达成价格协议。这里的问题是什么是主要的(和领先的)交换？

*   我们要不要看看他们报告的数量？我们已经在[二月份关于虚假销量](/crypto-integrity/fake-volumes-in-cryptocurrency-markets-february-report-fec9329f1f98)的研究中写道*报告的*销量是一个糟糕的指标。
*   我们要不要看看他们钱包的余额和活动情况？它可能会揭示一个交易所的可信度——用户有多渴望在这个交易所存储他们的资产。此外，与其他交易所相比，它可能表明报告的交易量与客户资产的比率存在一些异常。声誉良好的交易所预期会有较低的比率。然而，它给我们提供的关于流动性和价格发现的信息很少。
*   我们要不要看看交易所网站的访问量？Twitter 或 Telegram 群组中的订户？尽管所有这些数字都有可能被夸大，但它们有助于估计交易所的受欢迎程度。然而，受欢迎程度对价格发现和流动性只有间接影响。

我们确信，这是说明价格发现如何工作的指令簿。对订单簿深度及其动态的分析为零售、机构和算法流程的混合提供了初步见解，这些流程最终定义了资产的当前价格。

# 流动性指标

我们开发了一个系统，根据报告的交易和订单簿计算 8 个流动性指标(定义如下):

1.  买卖价差，%
2.  10 BTC 的买卖价差，%
3.  买卖流动性，BTC
4.  便利的流动性，BTC
5.  交易量，BTC
6.  交易数量
7.  贸易规模分布
8.  订单不平衡

**买卖价差**，%，计算为:

*(最佳要价—最佳出价)/ [(最佳要价+最佳出价)/ 2] * 100* 。

流动性越强的资产买卖价差越小。然而，在价格阶梯较小的交易所，买卖价差可能显得更窄。Bittrex 的价格步长为 0.001 美元。币安，Bitstamp，比特币基地专业有一个 0.01 美元的价格步骤。Bitfinex 和北海巨妖的价格步长为 0.1 美元。

**买卖价差乘 10 BTC**%，计算为:

*(ask—bid)/[(ask+bid)/2]* 100*，

其中*询价*和*投标*是 10 个 BTC 的总量的加权平均价格。流动性越强的资产利差越小。

**买价-卖价流动性**，BTC，是一个订单簿中所有订单在最佳买价和最佳买价下的累计量。流动性越强的资产，买卖流动性越高。

**BTC(Handy liquidity)**是指订单簿中的累计成交量，其水平与原始中间市场价格相差 0.1%或更少。更多的流动资产有更高的流动性。便利的流动性为 0 意味着买卖价差大于 0.1%。

**不平衡**计算为:

*(ask _ liq—bid _ liq)/(ask _ liq+bid _ liq)，*

其中 *bid_liq* 为高于*中间价*(1–0.1%)*， *ask_liq* 为低于*中间价/(1–0.1%)*的报价之和。虽然该指标的单个值显示了当前买方或卖方的流行程度，但该指标的动态显示了供应和需求的变化。

我们不考虑费用，尽管它们会影响买卖差价。可以假定，做市商可以达到尽可能低的级别(在某些情况下为 0%)，或者与交易所签订定制协议，从而降低费用的影响。在撰写本文时，制造商的最低可能费用(公开披露的)如下:

1.  币安:如果交易 150，000 BTC 或以上(最近 30 天)，则为 0.02%；如果在 BNB 支付，0.015%
2.  Bitfinex:如果交易金额达到或超过 7，500，000.00 美元，则 0%
3.  Bitstamp:如果交易额达到或超过 20，000，000 美元，则收取 0.10%的费用(但是，Bitstamp 为做市商提供[定制费用安排](https://www.bitstamp.net/fee_schedule/))。
4.  比特莱斯:0.25%
5.  比特币基地专业版:如果交易金额为 50，000，000.00 美元或以上，则为 0%
6.  北海巨妖:10，000，000.00 美元或以上的交易为 0%

# 流动性评分

最高分 8 分，最低分 0 分。更高的分数意味着更高的流动性，这是好事。每个指标的得分为 0、1 或 2。由于我们的分析涵盖了 6 个交换，因此建议采用以下方法:

*   每项指标得分最高的两个交易所被分配 2 分；
*   每项指标得分最差的两个交易所被赋予 0 分；
*   其他两次交换被分配 1 分。

**总得分** =买卖价差得分+买卖流动性得分+买卖价差 10 BTC 得分+手动流动性得分

**零售评分** =买卖价差评分+买卖流动性评分

**机构评分** =买卖价差 10 BTC 评分+便利流动性评分

![](img/e42d606355dd77f42b5c08ce6f069c5f.png)

The resulting liquidity scoring. Source: Crypto Integrity

正如你所看到的，比特币基地专业版为散户和机构投资者提供了最好的流动性。Bitstamp 和北海巨妖紧随其后:Bitstamp 更适合机构客户，而北海巨妖为零售客户提供了更好的流动性。令人惊讶的是，尽管报告的交易量很高，观察到的息差很窄，但币安的得分却很低。Bittrex 落后的原因可能是高费用和缺乏做市商激励。

# 分析和结果

在观察期内(世界协调时 2019 年 4 月 12 日 0:00 至世界协调时 2019 年 4 月 17 日 0:00，共 5 个完整日)，针对所选交易所计算了以下指标。所有值都是数学平均值。

![](img/89e8d047ff0b7be8843f5251c55c2e78.png)![](img/fd7684557b799deba9a8b9bd33803018.png)

Bid-ask spread and bid-ask liquidity. Source: Crypto Integrity

对于零售客户来说，能够以最佳买价或卖价快速执行小额交易非常重要，因此，最佳买卖价差和最佳水平的金额非常重要。虽然比特币基地专业公司领先，但北海巨妖和币安的利差也很小。然而，平均而言，币安的最佳买价和卖价总额只有 2 个比特币，而比特币基地有 11 个，Bitfinex 10 个，北海巨妖 5 个 BTC。因此，如果你需要购买或出售少于 2 个比特币，你的最佳选择是比特币基地专业版、北海巨妖或币安。如果你需要进行一笔高达 10 BTC 的交易，你最好求助于比特币基地专业版或 Bitfinex。

![](img/2a72b84e0ea699a1d94eb5270710186d.png)

Bid-ask spread on Binance, Coinbase, Kraken. Source: Crypto Integrity

值得注意的是，北海巨妖紧随比特币基地的利差(反之亦然)，而北海巨妖的利差一般稍宽一些。相比之下，币安的买卖价差非常稳定，约为 0.02%，似乎市场波动不会影响其做市商。

![](img/241bf3ca74497cc27a1c98a2d501bf5c.png)

Bid-ask spread on Bitfinex, Bitstamp, Bittrex. Source: Crypto Integrity

第二类交易所往往提供更大的价差。数据显示，Bittrex 绝对是局外人。Bitstamp 的价差比 Bitfinex 更不稳定。

![](img/163070dd176485e91439effbc8ca52a3.png)![](img/2740f4bac2cd01f4b79bcd85aed2856c.png)

It seems there is a correlation between the bid-ask spread on Bitfinex & Binance and Kraken & Coinbase. Source: Crypto Integrity

如上所述，比特币基地和北海巨妖利差的缩小和扩大似乎是相互关联的(见上图)。同样，币安和 Bitfinex 的息差似乎也存在关联，尽管程度较低。可能是因为前两个交易所支持法定美元，包括无限制的廉价存款和取款，而第二个交易所主要关注 USDT？

![](img/f82fc892ce2ec39c15cb58d03430f63f.png)![](img/209d8e2052e3cd21bd6a13bc514223d5.png)

The volume on best bid and ask levels (two groups of exchanges). Source: Crypto Integrity

对买卖流动性的分析表明，存在两组秘密交易。与 Bitstamp、币安和 Bittrex 相比，比特币基地、Bitfinex 和北海巨妖往往拥有更多流动性。但是，不要忘记，不同交易所的价格阶梯不同。

![](img/88e448188ad0d6af1e0d04366e877525.png)![](img/a5851698ba008684979534e2a142c992.png)

Bid-ask spread by 10 BTC and Handy liquidity. Source: Crypto Integrity

![](img/95e9e32d9123d65eb327b5c6aff5cf17.png)

对于机构客户来说，重要的是能够在价格影响不大的情况下执行更大的规模，因此，他们关注成交量加权买卖价差(10 BTC)和便利流动性(在价格影响为<0.1%). Coinbase has again the narrowest VW BA spread, Kraken, Binance, Bitfinex and Bitstamp drag behind. However, it is Bitstamp that offers the deepest handy liquidity.

![](img/15bf6ebf152384574392819e738b2a06.png)

Summary of liquidity metrics. Source: Crypto Integrity

**的情况下可以出售/购买的比特币总数。请注意，我们过去在观察期间只收集北海巨妖 10 个级别的订单。从 2019 年 4 月 17 日开始，我们转储 100 个级别的数据。4 月 17 日的流动性约为 77 BTC。*

## 交易额

![](img/2539e94baf50c2ab9a079e5c3b7a1429.png)![](img/088547e5f3473d55ec4048d76066bc85.png)![](img/f6670a8aa81c17437476bf8de71f5caf.png)

Distribution of trade sizes (100 bars; OY is frequency, OX is a size in BTC): Binance, Bitfinex, Bitstsamp. Source: Crypto Integrity

![](img/add79b91d15d30ecbb2d38a6c47ddbba.png)![](img/e603941b084b606c41e7f1e5352c8530.png)![](img/0e0d450a2c22b0ef5417d596f14f5f9a.png)

Distribution of trade sizes (100 bars; OY is frequency, OX is a size in BTC): Bittrex, Coinbase, Kraken. Source: Crypto Integrity

![](img/9a4724f8d57ab5a007ee449d2a18e299.png)

Distribution of trade sizes (100 bars; OY is frequency, OX is a size in BTC). Source: Crypto Integrity

虽然 Bitstamp 有很多 1 BTC 的交易，而 Bittrex 和币安很少有这种规模的交易，但所有 6 个交易所的交易规模分布情况相似。相比之下，平均交易的规模就不同了。币安和 Bittrex 的交易规模最小(分别为 BTC 的 0.14 倍和 BTC 的 0.15 倍)，这可能是因为它们拥有广泛的零售客户群，而 Bitstamp 的交易规模最大(BTC 的 0.52 倍)，这可能是因为在观察期间算法交易员或机构流量的高份额。

![](img/68397447beb351d4a4c1861c11be122b.png)

Trade volume vs the number of trades. Source: Crypto Integrity

![](img/1732646fd97b23ee9496ac38c50a2a99.png)

Reported trade volume (ball size) vs 10 BTC bid-ask spread (OY). Source: Crypto Integrity

30 天跟踪成交量的曲线看起来很相似。然而，币安，尤其是 Bittrex，似乎比其他选定的交易所更享受 4 月初的交易量飙升(我们在这里写了更多关于交易量概况、虚假交易量和价格发现的内容)。

![](img/efbd958ab2f1e44a5316209db1a6ee8e.png)

30-day trailing volume, USD. Source: Coinmarketcap

![](img/8d19e6ce2f9f2217804165f2f5277618.png)![](img/50cc9e577c5f95150816d5fc13ca0da4.png)

30-day trailing volume — Binance (left) and Bitfinex (right). Source: CoinGecko

![](img/721368ea5e37c777a49b85334a7328bf.png)![](img/af5e5ce9465d9daf20ba8fc282338f1f.png)

30-day trailing volume — Bitstamp (left) and Bittrex (right). Source: CoinGecko

![](img/d3c4f755659a718485fcdd68c7a4e5a0.png)![](img/b00edfd0b8277c16bf625b89230a08b2.png)

30-day trailing volume — Coinbase Pro (left) and Kraken (right). Source: CoinGecko

## 贸易频率分布

以下交易频率分布图基于从世界协调时 2019 年 3 月 17 日 0:00 至世界协调时 2019 年 4 月 17 日 0:00 收集的数据。我们主要关注两个范围:从 0 到 1.2 秒和从 0 到 70 秒。每个直方图有 1000 个条形。我们发现了一些值得一提的特性:

1.  **币安**有一个非常奇怪的两种交易区间柱状图。第一个看起来像一个下降趋势的正弦函数。第二个明显不同于同行，因为几乎没有交易发生的频率低于 15 秒。
2.  **Bitfinex** 有几个出色的频率，特别受欢迎:150 ms 左右，950 ms，5，10 秒。
3.  Bitstamp 在两次连续交易之间有一个最小的 70 毫秒的间隔。此外，每 3 秒钟就有大量的交易者出现。
4.  比特币基地的形象在某种程度上与币安相似。
5.  **北海巨妖**每 60 秒就有大量的交易者出现。

这些现象可能有一些解释。首先，匹配引擎可能会定期而不是立即匹配订单(最有可能的情况是 Bitstamp)。第二，如果时间戳是在网关级别上标记的，那么网关可能有特定的算法在通过 WS(币安？).第三，市场参与者可能会使用 TWAP 等算法指令，这些指令会以固定的时间间隔产生交易。然而，如果没有交易所的进一步反馈和披露，这些异常值看起来很可疑，几乎没有道理。

![](img/85b77ce8dec45982322c428a082186df.png)![](img/d178d259478710ea6f25579b324d8640.png)![](img/2f396186530e68e743fa11b06b61d6f2.png)![](img/c261fc65be029162f6cada3847804c7b.png)![](img/1a9d13d09360ea288851e903b2f1d1b9.png)![](img/fae408cf349b8547a0da9b930affdb6c.png)![](img/5ed268c4e44649044bce0c6a44cc2652.png)![](img/309abc9839fb5a9489a981bbe67704ff.png)![](img/707a2047ccb5a1f53d11120d06316ce4.png)![](img/4cc048b03a463a1c351b9afecce23267.png)![](img/9e8e8eacf58fd3fad34467eab03ee317.png)![](img/f85864559526b87d25c789bc61c72a51.png)

# 4 月 2 日，比特币价格飙升

2019 年 4 月 2 日比特币价格飙升 20%，也导致整体交易活动大幅增加(由于这一飙升，我们甚至写了一篇关于另一个检测虚假交易量的指标[的文章](/crypto-integrity/volume-profile-and-fake-volumes-368fca9bf6d3))。让我们在微观层面上更仔细地看看在这一事件中发生了什么(北海巨妖被排除在这一分析之外，因为我们没有这一时期的数据)。

![](img/241f3ae31c758577da296d3a7244fea8.png)

Bitfinex BTC/USD price & volume chart (1–3 April 2019). Source: TradingView

![](img/1cc9e84b97f364da2ba287ea268aac3a.png)

Best bid-offer across selected exchanges on 1–3 April 2019\. Source: Crypto Integrity

如图所示，价格上涨对应于所有选定交易所买卖价差的扩大，这是意料之中的。奇怪的是，Bitfinex 从 UTC 时间 7:30 到 15:30 的价差扩大了，尽管价格波动很小，交易量也很低。在其他交易所也观察到了同样的平静(看看下面的比特币基地专业图表)。其中一个解释可能是，某个单一做市商的技术问题，该做市商必须报出较窄的价差，而其他做市商没有这种义务。据我们所知，Bitfinex 没有官方的做市商计划，这让我们认为这可能是一个*附属*做市商的案例。

![](img/8270b0f31de504772868ff600602b200.png)

Coinbase Pro BTC/USD price & volume chart (1–3 April 2019). Source: TradingView

![](img/f2a6ba9fe11083e27cb0363c4b3d4509.png)

Bid-offer by 10 BTC. Source: Crypto Integrity

让我们回到比特币泵。10 BTC 成交量加权买卖价格图显示，Bitfinex 受价格变动的影响比其他交易所更大。这可能意味着 Bitfinex 是最初的交易所。相比之下，币安经历了非常温和的利差扩大；因此，这可能表明，在这种情况下，币安是一个落后的交易所，套利可能会受到阻碍。

![](img/a995d9b5a7144fef541c3d5ebb14858b.png)

Bitfinex BTC/USD price & volume chart (2 April 2019). Source: TradingView

对 10 BTC 成交量加权买卖价差的进一步分析显示，购买分为 3 组，可在下图中识别为三个峰值。

![](img/69c18cdb4d7359c09c02c9017a3bdc37.png)

Bid-offer by 10 BTC. Source: Crypto Integrity

![](img/e9817d94d12c4f3e732b59419cfef36b.png)

Handy liquidity (quantity of BTC within 0.1% from mid-price). Source: Crypto Integrity

便捷流动性图表导致两个主要推论:(1)我们观察到所有四个交易所在抽水期间便捷流动性的下降，(2)与其他交易所相比，比特币基地和 Bitstamp 经历了最显著的下降。一方面，这可能意味着价格变动是从那里开始的，对订单簿流动性产生了最大的压力。另一方面，不管价格冲击的来源是什么，在市场深度最有价值的动荡时期，流动性已经从比特币基地和 Bitstamp 消失了(尽管有做市商的存在)。

让我们仔细看看高峰的第一阶段——从 UTC 时间 4:30 到 4:40。由于并非所有交易所都在订单快照或更新中公布交易所时间，因此我们使用了服务器的本地时间进行比较。我们知道这对我们的实验有影响，因为我们的服务器位于法兰克福，法兰克福在地理上更接近 Bitstamp 和 Bitfinex，而不是比特币基地和币安(尽管我们没有币安匹配引擎位于哪里的确切信息)。然而，在几分钟到几百毫秒的时间范围内，这可以用地理来解释，影响很小。

![](img/9bf0ee310e636689c8f875f37cde02d1.png)

Bid-ask spread of selected crypto exchanges during the first spike (local time). Source: Crypto Integrity

上图显示，Bitfinex 上的买卖价差经历了几次买家攻击(直线垂直移动)，而人们可以观察到其他交易所的价格波动更加平稳。此外，值得一提的是，最初，Bitstamp 的价格(4:32-4:35)以及后来币安的价格(4:36-4:40)的上涨速度明显慢于其他交易所。在第二次冲动(4:48–4:58)和第三次冲动(5:16–5:24)期间观察到类似的动态。

![](img/cf64060faff664a076ea4ea9c261e831.png)

Bid-ask spread of selected crypto exchanges during the second spike (local time). Source: Crypto Integrity

![](img/49fc5e77babfec0514b74dc48254ea62.png)

Bid-ask spread of selected crypto exchanges during the third spike (local time). Source: Crypto Integrity

在此期间发生的交易分析已经证实，大量购买是在 Bitfinex(直垂直线)上发起的。然而，人们可以在币安看到较小的变化。Bitstamp(对 Bitstamp 的 BTC/欧元汇率进行了分析)和比特币基地 Pro 都没有表现出这种转变。是否可以用更深的流动性来解释？和 USDT(系绳)有关系吗？

![](img/223b2cf6d8da85861099f8d58a7dfe8d.png)

Trade prices of selected crypto exchanges during the first spike (exchange time). Source: Crypto Integrity

![](img/6ebb924a38185139b283cce3539e61a1.png)

Trade prices of selected crypto exchanges during the second spike (exchange time). Source: Crypto Integrity

![](img/355b1c53c9e94619b2569c3d04c8e4a8.png)

Trade prices of selected crypto exchanges during the third spike (exchange time). Source: Crypto Integrity

订单不平衡图表显示，在第一次价格冲击期间，所有交易所都出现了负不平衡，只有 Bittrex 例外。向上的压力从 4:30 开始，一直持续到 5:30。然而，我们在此期间观察买卖双方。当买家和卖家努力达成价格平衡时，Bitstamp 的不平衡最不稳定。

![](img/0b1d022cdb899fb4607ac3d037caa32c.png)

BTC/USD order book disequilibrium. Source: Crypto Integrity

# 数据

本研究中使用的指标适用于由匿名交易和订单簿组成的公共数据集。数据收集器订阅交易所的数据馈送，并保存订单簿的一定数量的级别。所有数据都是通过 WebSocket 技术接收的。

Comparison of market data public APIs. Source: Crypto Integrity

在研究范围内，我们分析了 L2 数据，即几个要价和出价水平。所有选定的交易所在加密领域都拥有比大多数同行更先进的技术。然而，只有 Bitstamp 具有以微秒计的时间戳，而 Bittrex 在订单簿中根本没有时间戳。我们每秒钟从币安接收一次数据，尽管它的时间戳精度为 1 毫秒。高质量的数据使得更容易发现市场操纵，因此，可以假定，一个关心其数据的交易所寻求更大的透明度。

# 已知问题

**时间戳**。为了识别和定位原始价格变动的来源，需要在所有被分析的交易所的订单簿和交易中有精确同步的时间戳。不幸的是，加密交易所应用的当前技术水平远低于金融行业的标准(例如，CBOE 的[政策](https://markets.cboe.com/europe/equities/regulation/mifid/clock_synchronisation/)；你也可以看看 Corvil 写的关于计时技术的漂亮的[文章](https://www.corvil.com/solutions/electronic-trading/solutions/rts-25-clock-synchronization)。缺乏精确的同步和对 UTC 的持续可追溯性会导致交易所在交易和订单簿中报告的时间与 UTC 时间的差异。反过来，这会导致基于时间戳不正确的数据得出错误的推论。我们已经向所有选定的交易所发出了正式请求。

1.  北海巨妖目前正在使用 NTP，并将在不久的将来迁移到 PTP。
2.  Bitstamp 没有透露它的服务器如何保持服务器上的时间，但将所有服务器的时间更新为全球 UTC 时间。
3.  其他交易所尚未回复我们的请求。

**订单书深度**。为了确保我们有足够深度的订单簿来正确计算便利的流动性，我们转储以下数量的级别。为了检查数据的质量，我们计算了转储订单簿的深度，分别作为更远水平与最佳出价或最佳要价的比率。如果得到的深度低于 0.1%，我们排除这些值。未来，我们计划调整我们的数据收集机制，以增加倾倒深度。

1.  币安:20 级+更新
2.  Bitfinex: 50 级
3.  Bitstamp:全部
4.  Bittrex: 30 级
5.  比特币基地专业版:150 级
6.  北海巨妖:10 级+更新