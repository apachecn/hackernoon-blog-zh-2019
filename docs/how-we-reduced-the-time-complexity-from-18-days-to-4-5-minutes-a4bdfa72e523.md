# 我们如何将时间复杂性从 18 天减少到 4.5 分钟。

> 原文：<https://medium.com/hackernoon/how-we-reduced-the-time-complexity-from-18-days-to-4-5-minutes-a4bdfa72e523>

![](img/27c7fa2101232ded9b376175f9db9a8b.png)

Photo on [Unsplash](https://unsplash.com/search/photos/scaling?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> 垃圾几乎无处不在。和电脑没什么区别。如果我们想一想，即使我们没有付钱，我们的钱包里也会有很多垃圾。

数据处理通常被认为是使用自动化脚本和请求从各种文本中提取模式。使用 python 进行数据挖掘的伟大命令可以从[这里](https://www.pythonforbeginners.com/mechanize/browsing-in-python-with-mechanize)学到

数据处理通常被认为是许多人的痛苦。从较小大小的文件中提取标签需要努力，但与包含一百万个条目的语料库相比，相对容易。

> ***对了，我是***[***codes myth***](https://medium.com/codesmyth)***的策展人，一个简化代码，打破神话的线上平台。***

这个职位不需要事先了解，因此是一个漫长的。所以，请原谅我。

# 我们的情况

我们的工作影响了电信领域，即电话和互联网相关的东西。因此，为了使它更容易，当一个人从世界上的任何地方打电话给世界上的任何其他人时，一个**呼叫详细记录(CDR)** 在他们的谈话结束后被转储，以便根据可用的服务对他们适当地收费。在我们的例子中，CDR 是一个单行，包含字段的数量，如谁给谁打电话、持续时间、到达时间、结束时间等。

![](img/ac4e5040f92516dd76de50a083dbe37a.png)

Photo by [Mario Caruso](https://unsplash.com/photos/0C9VmZUqcT8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/telecommunication?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

当来自一个服务提供商(比如 A)的用户呼叫某个其他服务提供商(比如 B)的另一个用户时，LRN 值被转储到 CDR 中。

> 按照 Google 的说法，位置路由号码(LRN)是一个名为服务控制点(SCP)的数据库中的 10 位数字，用于标识本地电话交换机的交换端口。

> 基本上，LRN 是一个映射到呼叫方的区号和服务提供商的唯一值。

# *输入* —

A)一个**语料库，比如说 C1 [XML 格式]** ，它由分布在多个目录中的多个文件中的近*800 万条记录组成*。每个记录由大约 30 个条目组成，其中一些条目是*主叫号码、被叫号码、始发区号、终接区号、LRN、设施*等。

b)另一个**语料库，比如 C2** ，其包括分布在多个文件夹中的多个文件中的 8500 万个 CDR 条目。这就是要改正的文集。

![](img/2ffb9d913dbc1dbf24a9bfdd24bce4ed.png)

Photo by [Pankaj Patel](https://unsplash.com/photos/u2Ru4QBXA5Q?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/html?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 我们的任务—

> 我们将根据 C2 语料库的被叫号码，把 C1 语料库的字段[LRN]的值代入 C2 语料库。

简而言之，我们所要做的就是从 C2 语料库的每一个 CDR 中提取一个“**被叫号码**”参数，然后基于 C2 语料库的“**被叫号码**”参数在整个 C1 语料库中搜索“**【LRN】**”参数，最后用 C2 的 LRN 参数替换那个特定的 CDR。这一过程将对近 8500 万条记录执行。

# 之前的脚本—

在我们的组织中，有人为上述任务编写了一个脚本(用 C 编程语言),并最终使用暴力算法来实现它。想象一下以下程序需要的时间—

1 —提取 C2 的被叫号码。

2 —逐个打开 C1 子目录下的文件，然后搜索相应被叫号码所需的 LRN。[800 万条记录]

3 —一旦我们有了它，就用 C1 CDR 内的 LRN 代替相应的被叫号码。

4 —对 8500 万条记录重复此操作。

*   更不用说许多不必要的打印、冗余的变量赋值、包含关于控制流的不必要的库以及被转储的不必要的日志。打印到控制台也会占用大量不必要的时间和资源。

# 我们如何优化它？

![](img/062ed1666e2771b12aebaec319d16b68.png)

Photo by [Nate Grant](https://unsplash.com/photos/QQ9LainS6tI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> 在运行原始脚本一天之后，它已经处理了将近 0.5 GB 的 CDR，CDR 的总容量为 9GB。一个简单的数学计算告诉我们，如果顺利和无干扰，将需要将近 18-20 天，如果大多数组织都有资源中心，这是不可能的。

我们都同意运行这个脚本没有意义，没有人有时间等这么长时间。整个过程将不得不从头开始。此外，语料库包含大量不必要的数据，并且存在错误替换的可能性。如果发生上述情况，所有的工作和资源利用都将是徒劳的。

第一步:

> 有时候，你要找的东西已经在那里了。
> ——阿撒·富兰克林

没有必要一遍又一遍地阅读 C1 文集。这就像重读圣书，但像一百万次。这个 C1 语料库以 XML 格式存在。我们已经有了 XML 解析器。

这是我们所做的——

*   每当我们在 C1 语料库中读取一个文件中的记录时，我们就搜索 XML 标记" **Called_Number** 和" **LRN** "，然后提取它们，并将输出打印到一个文件中，文件格式如下，用分号("**)隔开；**”)。我们可以考虑("**)；**))作为字段之间的分隔符。

因此，我们不是读取整个记录，而是以如下所示的格式读取该记录的一行。

> **被叫 _ 号码；LRN**

*   我们对所有其他记录做了同样的事情，最终创建了有近 800 万行的输出文件，因为有 800 万条记录。假设文件是(A)。
*   *“A”由许多冗余的*组成，正如预期的那样——
    **1**——在数字之间和数字末尾有冗余字符。
    **2** -对于一些被叫号码，没有 LRN 值。
    **3** -来电号码大小不统一，应为 10 个。它们就像打印在被叫号码字段的垃圾值。
    **4** -文件中有重复值。
*   为了删除它们，我们在解析语料库 C1 和创建输出文件(“A”)时进行了以下检查。
    ***** 字符串“ **Called_number** ”要用 c 中的“ **atoi()** ”函数转换成整数，如果失败，我们不会把输出写到输出文件( **A** )。(**办案 1** )
    ***** 字符串“ **Called_number** ”的长度应为 10。(**办案三**)
*   一旦我们创建了输出文件(" **A** ")，我们就在 vim 中编辑它，并使用两个 vim 内部命令处理(**案例 2 和案例 4** )

> **:g/；$/d**[删除所有以“；”结尾的行]】
> **:sort u** 【用于升序排序，去除重复行】
> 
> 在文本编辑器中，删除文件中的重复行通常很容易。例如，Atom。

![](img/e6f8d0a3d65b4a35045b03e42bb402be.png)

Photo by [Caspar Camille Rubin](https://unsplash.com/photos/89xuP-XmyrA?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/atom?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

> 有些人可能会说，我们可以为它写一个简单的程序。但是如果已经构建了解决问题的方法或工具，就应该使用它，而不是从头开始。从节省下来的时间和精力中，在你现有的应用/产品/平台上创造一些新的东西，从而为其他人提供一个垫脚石，帮助社区成长。这是开源的基本概念。

这个输出文件(“A”)是我们的最终文件，它将用于 800 万条目的 C1 语料库。经过适当处理后，保留在数据库中的条目总数只有 220 万条。

从庞大的 800 万条记录(每条记录由 30 个条目组成)，我们缩小到 220 万条。

> **缩放后的语料库规模= (8000000*30)/2200000 = 109 倍**。

这是很重要的，如果我们在分析了我们可以利用的资源之后再去思考它。

下一步是优化 C2 语料库的 CDR 处理—

与 CDR 的情况一样，我们在整个文件中(" **A** ")线性地[ **使用线性搜索**)搜索特定的键。由于我们新生成的来自 C1 语料库的文件(“ **A** ”)已经被排序，我们决定实现**二分搜索法。**

> 因此，将复杂度从 **O(N)** 转换为 **O(logN)** 。

来自科珀斯 C2 的 cdr 分布在多个目录中，每个目录对应不同的区号(大约 252 个目录)。早些时候，当我们处理 CDR 时，只有在前一个目录被处理时，我们才跳转到下一个目录。[一次处理一个目录]。

> 这意味着整个进程在单个用户线程上运行。线程
> 是操作系统的一个基本概念，在软件或应用中实现并行处理时使用。

因此我们**在多核处理器上实现了多线程。**运行脚本的刀片由 80 个内核组成。**因此，并行地，在通过 shell 实现线程化之后，我们能够同时执行来自 80 个目录的 cdr。因此，我们的总时间减少了 80 倍。**

我们刚刚创建了一个简单的 shell 脚本，它只是—

> **。/script _ which _ process _ cdr[目录名][输出目录名] &**

下一个挑战是，虽然处理时间大大减少了，但是每个目录的每个脚本都是用一个新的进程 ID 创建的，并且与另一个线程同时运行的进程的进程 ID 没有任何关联。因此，来自语料库 C1 的文件(“A”)被加载到内存中近 242 次，这是相当浪费资源的。

> 因此，我们决定用 C 语言实现多线程，而不是通过 shell 应用多线程，我们的脚本最初就是在 C 语言中编写的。

*我们决定，一旦语料库文件成功加载，我们就开始线程化。*使用 **pthread** ，非常容易，而且线程对文件( **A** )数据的修改也没有问题，因为执行的进程都不包括对文件( **A** )的写入。

简而言之，我们所做的就是在删除某些冗余之后，在文本文件( **A** )中获得语料库的输出( **C1** )，然后将该文件加载到我们刀片的内存中的一个二维数组中。如果加载到内存成功，二分搜索法用于识别和替换键-值对。为了使处理速度更快，我们实施了多线程，并创建了线程来并行处理大量目录。

从时间函数计算出的执行欢呼时间为

> $ time process _ cdr . sh <directory of="" the="" path="" were="" cdr="" present="">$ 4 分 27 秒执行完毕。</directory>

> 经过长达 18 天的处理，我们的数据处理仅用了 4 分 27 秒就完成了。

**从上述事件中，我感受到了时间复杂性的重要性。时间是唯一我们买不到也拿不回来的东西。从上面的例子中，我相信无论我们的脚本是如何编码的，总有某个地方或某些方面可以优化它，无论是空间或时间复杂度还是文档。我们只需要在没有任何外部障碍的情况下，给它更多的时间去分析和思考。**

> **祝你编码愉快，祝你更有力量。**

![](img/ee7488a99e1ea6ebdecb773d5d19fe6e.png)

Photo by [Austin Distel](https://unsplash.com/photos/Jn1csk3lWDA?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/scaling?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)