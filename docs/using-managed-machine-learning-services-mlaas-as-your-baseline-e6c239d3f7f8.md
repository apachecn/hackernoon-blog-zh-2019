# 使用托管机器学习服务(MLaaS)作为基线

> 原文：<https://medium.com/hackernoon/using-managed-machine-learning-services-mlaas-as-your-baseline-e6c239d3f7f8>

## 构建与购买:MLaaS 是否符合您的数据科学项目的需求，您如何评估各个供应商？

![](img/ce7a99e1b74d394a4deaca45a8c81ead.png)

Making a build or buy decision at the start of any data science project can seem daunting — let’s review a

几乎每个主要的云提供商现在都提供定制的机器学习服务——从[谷歌云的 AutoML Vision Beta](/p/1795457dce9#fd1c) ，到[微软 Azure 的定制视觉预览](/p/1795457dce9#8fce)，以及 [IBM Watson 的视觉识别](/p/1795457dce9#4d2d)服务，计算机视觉领域也不例外。

也许您的团队已经陷入了这种**是建造还是购买的困境？**

从市场营销的角度来看，这些托管 ML 服务定位于刚刚建立数据科学团队或团队主要由数据分析师、BI 专家或软件工程师组成的公司(他们可能[正在向数据科学](https://medium.freecodecamp.org/if-youre-a-developer-transitioning-into-data-science-here-are-your-best-resources-c31928b53cd1)过渡)。

然而，即使是中小型数据科学团队也可以从评估这些“机器学习即服务”(MLaaS)产品中获得价值。因为这些供应商生产并访问如此多的数据，所以他们能够在内部构建和训练自己的机器学习模型——这些经过预训练的模型可以快速提供更高的性能。使用这些 MLaaS 产品作为基准与内部模型进行比较也很有用。

**这篇文章将为您提供一个清晰的框架，让您了解如何评估这些不同的供应商及其 MLaaS 产品。**

这篇文章的灵感来自于[](https://medium.com/u/e815ce4c9ea5#247</h2><div class=)

### [今天我们请到了 URBN 的数据科学家 Tom Szumowski，URBN 是 Urban Outfitters、Anthropologie 和…](https://medium.com/u/e815ce4c9ea5#247</h2><div class=)

[twimlai.com](https://medium.com/u/e815ce4c9ea5#247</h2><div class=)

在采访中，Tom 描述了他的团队为建立机器学习管道以自动化时尚产品属性标记所做的努力。这实质上意味着拿一件像[这件衣服](https://www.urbanoutfitters.com/shop/uo-johnson-tie-back-mini-tank-dress?inventoryCountry=US&color=030&mrkgcl=671&mrkgadid=3275540791&cm_mmc=SEM-_-Google-_-PLA-_-414308616711_condition_new_product_type_womens_product_type_apparel_product_t&product_id=50762863&utm_campaign=BRAND_SHOPPING&adpos=1o2&creative=251335296627&device=c&matchtype=&network=g&gclid=EAIaIQobChMIz4iF-q654QIVxksNCh2yuAthEAQYAiABEgKyIfD_BwE)这样的产品:

![](img/c7c5e552de3110333bc7039424f59cdb.png)

并能够自动和准确地生成关于它的属性，如袖长(无)，领口(大圆领)，长度(迷你裙)，图案(条纹)，颜色(绿色，黄色，红色)等…

使用机器学习——特别是计算机视觉——来处理这个产品属性分类过程对 URBN 来说是有意义的，因为他们提供了大量不同的产品目录。对这些属性的访问有助于业务跨越一些关键计划:

*   围绕内容和产品推荐的个性化
*   提高用户体验中的可发现性和搜索
*   库存管理的预测/计划

URBN 团队预计属性的数量将继续增长，并确定了内部解决方案的机会成本(根据团队花费的时间)，因此他们有兴趣找到一个可扩展的解决方案来管理所有这些属性的模型。

**以下是他们团队评估的定制视觉服务供应商的概述:**

![](img/9c3bc850b08d8328728c4e68a0d6470e.png)

Vendors included: [Google Cloud AutoML Vision Beta](/p/1795457dce9#fd1c), [Clarifai](/p/1795457dce9#9ffa), [Salesforce Einstein Vision](/p/1795457dce9#ed3d), [Microsoft Azure Custom Vision Preview](/p/1795457dce9#8fce), [IBM Watson Visual Recognition](/p/1795457dce9#4d2d), [In-House: Keras ResNet](/p/1795457dce9#01ff) and [In-House: Fast.ai ResNet](/p/1795457dce9#c881)

[Tom Szumowski](https://medium.com/u/7af18257d59d?source=post_page-----e6c239d3f7f8--------------------------------) 写了一篇关于案例研究具体细节的精彩文章——探索定制视觉服务的自动化时尚产品属性([第一部分](/urbn-engineering/exploring-custom-vision-services-for-automated-fashion-product-attribution-part-1-1795457dce9)和[第二部分](/urbn-engineering/exploring-custom-vision-services-for-automated-fashion-product-attribution-part-2-2c928902db47))——并分享了[演示文稿](https://github.com/URBNOpenSource/custom-vision-study/tree/master/presentations)。

# 这如何适用于我的数据科学团队？

虽然 URBN 的具体用例很有趣，但 URBN 团队用来评估这些不同的 MLaaS 产品的思维过程和框架被证明是非常有用的(这将是本文剩余部分的重点)。

## 评估 MLaaS 产品的框架

以下是评估这些不同 MLaaS 产品的起始框架。您会注意到可见性、可用性、灵活性、成本和性能等属性是如何跨越工作流的每个部分的。我们将在下面更深入地研究每一部分。

![](img/95ded3ec1eb06b00ee9c6aebf696a1b7.png)

该框架及其问题将帮助您了解使用 MLaaS 产品是否是您团队的正确方法—从您输入 MLaaS 产品的数据到您收到的实际模型结果。为了防止这些系统成为昂贵的黑匣子，您应该围绕评估实际上是如何发生的以及结果是否可以更新提出问题。

![](img/76c4c0aeae87db59dad63965ff66d3ba.png)

**See this diagram in slide format** [**here**](https://docs.google.com/presentation/d/1-1AZrlgGws26b_3cpL6TQF7wqoA11JfgIagEexp-Zio/edit?usp=sharing)

也许你会像 URBN 团队一样，对照内部模型和方法来评估这些 MLaaS 选项？

如果是这样的话，拥有**实验可比性是至关重要的。** [可比性](https://mlinproduction.com/ml-metadata/)意味着您使用的数据、环境和模型是一致的，以便对这些不同的选项做出公平的评估(毕竟，即使有不同的库版本也会影响性能)。像 [**Comet.ml**](http://bit.ly/2FZTTXt) 这样的工具可以帮助自动跟踪这些实验的超参数、结果、模型代码和环境细节。通过这些不同实验的可视化，您的团队可以很容易地直接比较不同模型的表现，并确定最佳方法。

![](img/7cf736a4b8f7ad27f9a2cf07a2bf94d4.png)

Project-level visualizations in [Comet.ml](http://bit.ly/2FZTTXt) make it easy to identify which model and parameter set performed the best. You can share these visualizations as reports to your manager, procurement team, or fellow data scientists!

# 实践中的评价

那么 URBN 团队的评测结果如何呢？下表比较了基准数据集(如 MNIST、时尚 MNIST、CIFAR-10 和清洁的 UO 礼服数据)的分类准确性指标:

![](img/0eb9ea863477b07e8073f8ed254d7b50.png)

From [the URBN presentation](https://github.com/URBNOpenSource/custom-vision-study/tree/master/presentations), Google’s AutoML had the best AUC for the UO Dresses set with Einstein and Fast.ai behind.

他们的团队还整理了一个很棒的可用性比较表，在那里他们比较了关键的功能，比如部署选项、再培训选项、API 等等。

![](img/2ace72f98923c2db41a68978ddca3a2e.png)

感谢阅读！我希望这个框架和示例对您的数据科学项目有用。如果你有问题或反馈，我很乐意在 cecelia@comet.ml 或⬇️⬇️⬇️下面的评论中听到它们

> 如果你还没有听过 TWiMLAI，我强烈推荐[整个系列](https://twimlai.com/shows/)。我最喜欢的几集是 *(1)* [在 Linkedin 上用 Hema Raghavan 和 Scott Meyer](https://twimlai.com/twiml-talk-236-scaling-machine-learning-on-graphs-at-linkedin-with-hema-raghavan-and-scott-meyer/) ， *(2)* [用 Sebastian Ruder](https://twimlai.com/twiml-talk-216-trends-in-natural-language-processing-with-sebastian-ruder/) 研究自然语言处理的趋势，以及 *(3)* [用 rnn 和 Adji Bousso Dieng](https://twimlai.com/twiml-talk-160-designing-better-sequence-models-with-rnns-with-adji-bousso-dieng/) 设计更好的序列模型
> 
> NY AI & ML 将于 5 月在纽约与 Adji 举办一场聚会— [加入我们的](https://www.meetup.com/NYC-Artificial-Intelligence-Machine-Learning/) *🚀🚀)*

## 👉🏼想要更多牛逼的机器学习内容？[关注我们](https://medium.com/comet-ml)。