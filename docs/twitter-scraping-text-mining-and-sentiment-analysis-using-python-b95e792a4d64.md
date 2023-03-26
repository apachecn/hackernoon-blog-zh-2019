# 使用 Python 的 Twitter 抓取、文本挖掘和情感分析

> 原文：<https://medium.com/hackernoon/twitter-scraping-text-mining-and-sentiment-analysis-using-python-b95e792a4d64>

我不太喜欢唐纳德·特朗普。严格来说，我一点也不喜欢他。然而，他有这种魅力的轰动效应，占据了大多数报纸和社交媒体的所有时间。人们对他的态度是戏剧性的和双边的。他的描述性文字或高度肯定或高度否定，是文本挖掘和情感分析的一些完美素材。本次研讨会的目标是使用一个网站抓取器来阅读和抓取关于唐纳德·特朗普的推文。然后，我们将结合使用文本挖掘和可视化技术来分析关于唐纳德·特朗普的公众声音。

# 您应该继续阅读:

1.  如果你不知道如何在社交媒体上刮内容/评论。
2.  或者/和如果你懂 Python 但是不知道怎么用它做情感分析。

这个研讨会很容易理解。即使你对编程一无所知，当你阅读这篇文章的时候，你应该会感觉很舒服。您可以随意复制代码并亲自尝试。如果您是初学者，我建议在与本研讨会中的代码进行比较之前，先试用您的代码。

# Twitter 抓取:

让我们从网页抓取开始，我需要一个有效的网页抓取工具来帮我完成所有枯燥的工作。任何网络刮刀工具都可以工作。我推荐 [**Octoparse**](http://www.octoparse.com/) ，因为它是免费的，没有页数限制。

我从它的官方网站下载了它，并按照说明完成了注册。我登录后，打开了他们内置的 Twitter 模板。

![](img/0a56eacaf53f3f3f660985984380f7c7.png)

八解析抓取模板

模板上的刮擦规则预先设置了数据提取字段，包括**名称、ID、内容、注释等。**

![](img/c24776b90a81d7eb0d1c3485388a5009.png)

我在外围文件中输入“唐纳德·特朗普”来告诉爬虫关键词。就像看起来那么简单，我收到了大约 1 万条推文。你可以尽可能多的抓取推文。还有一些其他的方式来抓取数据，可能你能得到比我更好的结果。欢迎与我分享你的创新爬行经验，我永远是一个充满激情的学习者:)

获得 tweets 后，将数据导出为文本文件，将文件命名为“data.txt”。

# 文本挖掘:

在开始之前，确保您的计算机上安装了 Python 和文本编辑器。我用的是 P [ython 2.7](https://www.python.org/download/releases/2.7/) 和 [Notepad++](https://notepad-plus-plus.org/download/v7.6.6.html) 。

然后我们用两个观点词表对抓取的推文进行分析。你可以从这里的[下载。](https://github.com/jeffreybreen/twitter-sentiment-analysis-tutorial-201107/tree/master/data/opinion-lexicon-English)这两个列表包含了来自[的胡敏清和刘兵关于社交媒体中呈现意见词的研究](https://www.cs.uic.edu/~liub/FBS/sentiment-analysis.html)所总结的正面词和负面词(情感词)。

**这里的想法是从列表中取出每个观点词，返回到推文中，并计算每个观点词在推文中的频率。因此，我们在 tweets 和 count 中收集相应的意见词。**

首先，我用两个下载的单词列表在第 5 行和第 13 行创建了一个正面和负面列表。它们存储从文本文件中解析出的所有单词。

然后，我用下面的代码处理文本和数据，去掉所有的标点符号和数字。

因此，数据仅由标记化的单词组成，这使得分析更容易。(这是我发现对数据科学中的文本预处理有用的[博客](/@datamonsters/text-preprocessing-in-python-steps-tools-and-examples-bf025f872908)。)

之后，创建三个字典:word_count_dict、word_count_positive 和 word_count_negative。

接下来，我定义了每个字典。如果数据中存在意见词，则通过将 word_count_dict 值增加“1”来对其进行计数。

在单词计数之后，我们需要决定一个单词听起来是积极的还是消极的。如果是正词，word_count_positive 将其值增加“1”，否则正词典保持相同值。word_count_negative 分别增加其值或保持相同的值。如果这个单词既没有出现在肯定列表中，也没有出现在否定列表中，那么它就通过了。

完整版本的代码可以在这里下载([https://gist . github . com/octo parse/fd9e 0006794754 edfbdaea 86 de 5b 1 a 51](https://gist.github.com/octoparse/fd9e0006794754edfbdaea86de5b1a51))

# 极性:正极与负极

![](img/4c39daf75fa09809345705b1e9351aa6.png)

5352 个否定词和 3894 个肯定词

如图所示。正面词的使用是单方面的。只使用了 404 种肯定词。

![](img/cd9b70db1646ccba0db31f8228a715f8.png)

出现频率最高的词有，比如“**喜欢”“很棒”“对”**。大多数词语的选择都是基本的、口语化的，比如**、【哇】和【酷】，**，而负面词语的使用则更加多样化。共有 809 种否定词，其中大部分是正式的和高级的。最常用的是**、【非法**、**谎言**、**种族主义**。其他高级词如“**不良**”、“**炎性**”、“**伪君子**”也有。

![](img/d0059a1e5d8c47dcd424df915e664958.png)

用词清楚地表明，支持者的教育水平低于反对者。显然，唐纳德·特朗普在推特用户中并不那么受欢迎。

# 总结:

在本文中，我们讨论了如何使用 Octoparse 在 Twitter 上抓取推文。我们还讨论了使用 python 进行文本挖掘和情感分析。

这项研究有一些限制。我删了 15K 条推特。然而，在搜集的数据中，有 5000 条推文要么没有文本内容，要么没有显示任何意见词。因此，情绪分析是有争议的。此外，本文中的分析只关注两极分化的观点(无论是积极的还是消极的)。细粒度的情感分析应该更加精确到不同的程度(非常正面、正面、中性、负面、非常负面)。

## 为什么说“非法”？

最后，我想分享一些关于结果的想法。“非法”一词是与唐纳德·特朗普相关的最负面的词。这个词排名第一并不奇怪，因为唐纳德·特朗普自上任以来一直致力于关注移民问题。然而，我对人们开始滥用这个词感到惊讶。我找出了关于这个词的推文，大多数都是“非法移民”和“非法外国人”这让我想到，从什么时候开始“无证”等同于“非法”？

最后，我想引用埃利·威塞尔的话，“你们这些所谓的非法外国人必须知道，没有人是非法的。这是一个矛盾的说法。人可以漂亮或更漂亮，可以胖或瘦，可以对或错，但不违法？一个人怎么可能违法？”

## 资源:

1.  [https://www.octoparse.com/](https://www.octoparse.com/)
2.  [https://medium . com/@ data monsters/text-preprocessing-in-python-steps-tools-and-examples-BF 025 f 872908](/@datamonsters/text-preprocessing-in-python-steps-tools-and-examples-bf025f872908)
3.  [https://www.cs.uic.edu/~liub/FBS/sentiment-analysis.html](https://www.cs.uic.edu/~liub/FBS/sentiment-analysis.html)
4.  [https://github . com/Jeffrey breen/Twitter-opinion-analysis-tutorial-2011 07/blob/master/data/opinion-lexicon-English/positive-words . txt](https://github.com/jeffreybreen/twitter-sentiment-analysis-tutorial-201107/blob/master/data/opinion-lexicon-English/positive-words.txt)
5.  http://nohumanbeingisillegal.com/Home.html