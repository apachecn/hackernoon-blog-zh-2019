# Spark 自定义流源

> 原文：<https://medium.com/hackernoon/spark-custom-stream-sources-ec360b8ae240>

![](img/9a2fc5a276a181e27ca670349f8e9676.png)

Apache Spark 是最通用的大数据框架之一。能够在内存处理和函数风格中混合不同种类的工作负载，这使得任何人都希望从事数据处理领域的编码工作。

Spark 的一个重要方面是它是为可扩展性而构建的。为 **RDD** API 编写新的连接器，或者扩展**数据框架** / **数据集** API，可以让第三方轻松集成 Spark。大多数人会使用内置的 API，比如 Kafka 用于流处理，或者 JSON / CVS 用于文件处理。然而，有些时候我们需要更具体的实现，离我们更近。例如，我们可能有一个在我们公司使用的专有数据库，但是没有用于它的连接器。我们可以简单地编写一个，就像我们在上一篇文章中解释的那样([***)Spark 数据源 API。扩展我们的 Spark SQL 查询引擎***](https://hackernoon.com/extending-our-spark-sql-query-engine-5f4a088de986) )。

从 Spark 2.0 开始，我们可以从流中创建源，这赋予了 *Spark 结构化流 API* 生命。正如我们可以想象的那样，有一些内置的流媒体源，Kafka 就是其中之一，还有 FileStreamSource、TextSocketSource 等等…

使用新的*结构化流 API* 应该优于旧的**数据流**。然而，同样的问题又出现了。我们如何扩展这个新的 API，以便使用我们自己的流媒体源？这个问题的答案就在这个帖子上。

# 扩展点

让我们首先回顾一下为了创建我们自己的流源，我们需要接触的主要组件。

首先， **StreamSourceProvider** 是指示什么源将被用作流读取器的。

其次， **DataSourceRegister** 将允许我们在 Spark 中注册我们的源，这样它就可以用于流处理。

第三， **Source** 是我们需要实现的接口，因此我们提供类似流源代码的行为。

# 我们的流媒体源

为了这篇文章，我们将实现一个相当简单的流源，但是同样的概念也适用于任何你需要自己实现的流源。

我们的流源叫做**inmemorrandomstrings**。它基本上生成一系列随机字符串及其长度，这些字符串被视为一个数据帧。

因为我们希望保持简单，所以我们将批处理存储在内存中，并在过程完成时丢弃它们。 **InMemoryRandomStrings** 不具备容错能力，因为数据是在处理时生成的，而内置的 Kafka 数据源中的数据实际上位于 Kafka 集群中。

我们可以从定义我们的 **StreamSourceProvider** 开始，它定义了我们的**源**是如何创建的。

类 **DefaultSource** 是我们的 **StreamSourceProvider** ，我们需要实现两个必需的函数， **sourceSchema** 和 **createSource** 。

**inmemoryrandomstrings . schema**是我们将在示例中使用的固定模式，但是该模式可以动态传入。

**createSource** 函数然后返回一个**inmemorrandomstrings**的实例，这是我们实际的**源。**

# InMemoryRandomStrings

现在，让我们分部分地查看**inmemorrandomstrings**代码，这样我们就可以专注于所有的细节。

返回我们的源使用的模式，在我们的例子中，我们知道模式是固定的。

`getOffset`应该返回我们的源看到的最新偏移。

请注意，我们添加了一个名为`offset`的变量，它将跟踪所看到的数据。然后，如果我们的源没有看到任何数据，我们返回`None`，否则返回`Some(offset)`。

现在，让我们看看我们的源是如何产生一些数据的，我们将为它使用一个运行线程。请注意**dataGeneratorStartingThread**函数。

在这里，我们添加了一个线程，它生成随机值并递增偏移量，同时将值和偏移量存储在内部缓冲区中。我们的源代码一创建，线程就开始运行。`stop`功能停止正在运行的线程。

在这一点上，我们离我们的目标只有两个功能。

`getBatch`返回一个**数据帧**给 spark，数据在传递的偏移范围内。

我们可以看到，我们正在从内部缓冲区获取数据，这样数据就有了相应的索引。从那里，我们生成**数据帧**，然后发送回 Spark。

最后，`commit`是 Spark 如何向我们表明它不会请求小于或等于被传递的偏移量。换句话说，我们可以用小于或等于传递给`commit`的偏移量从内部缓冲区中移除所有数据。这样，我们可以节省一些内存，避免耗尽内存。

现在，我们已经完成了我们的源代码，整个代码如下。

# 使用我们的定制源

现在，我们需要通过指明要使用的正确格式，将我们的源代码插入到 *Spark 结构化流 API* 中。

这里我们使用常规的`.readStream` API，并指定流格式是我们对`StreamSourceProvide`的实现，即*com . github . anicolaspp . spark . SQL . streaming . default source*。

现在我们可以像查询任何其他数据帧一样查询我们的流源。

输出如下所示。

我们从我们的数据源生成的数据的连续聚合中看到的。

# 结论

Apache Spark 是处理大规模数据的不二之选。它的特性几乎胜过任何其他工具。此外，它可以以许多不同的方式进行扩展，正如我们所看到的，我们可以编写自己的数据源和流源，以便可以轻松地将它们插入到我们的 spark 代码中。

***整个项目和源代码可以在这里找到***[***SparkStreamSources***](https://github.com/anicolaspp/SparkStreamSources)***。***

*快乐编码。*