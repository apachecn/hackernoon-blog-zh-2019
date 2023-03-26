# 开源 Reddit 情感分析和句子随机化器

> 原文：<https://medium.com/hackernoon/opensource-reddit-sentiment-analysis-and-sentence-randomizer-48090ddd5be>

[https://github.com/DunnCreativeSS/redditSentimentCombiner](https://github.com/DunnCreativeSS/redditSentimentCombiner)

1.  稍后连接到 g sheets:[https://docs . Google . com/spreadsheets/d/1 rx 4 mniepzliufid 7 H2 edmior _ IP 3 lhtajxj 2 vclvxg/edit？usp=sharin](https://docs.google.com/spreadsheets/d/1rX4mniePZLIUCIFd7H2EdMioR_ip3LhtAjxJ2VclvXg/edit?usp=sharing) g
2.  连接到 Reddit
3.  加载前 1000/r/比特币热门帖子
4.  获取原始文本，按换行符将它分成一个数组，过滤掉空字符串
5.  从自然语言工具 kit 的 Vader 函数中为步骤 4 中的数组中的每个字符串创建一个判断情感数组
6.  对于步骤 5 中的每个情感，获取复合(总体)情感评级(基于关键词等的值)。，在你最喜欢的引擎上进行搜索)并检查它是高于还是低于阈值，然后将它们变成小写，将“比特币”替换为“我们的硬币”，将“btc”替换为“我们的股票”，并确保字符串不以可疑的子字符串开头，然后将结果追加到我们的正数和负数数组中
7.  循环 10 次
8.  对于最多 6 个字符串的范围，首先是正字符串，然后是负字符串，随机选择一个字符串(只要它还没有被使用)，并将其添加到一系列最终的超能力字符串集合中
9.  在步骤 7-8 的过程中向终端打印一串有用的信息
10.  最后，在 10 根超级弦的正极和负极 gsheet 中各添加一行

在理想的情况下，人们会使用这个脚本的一个变体来加载和替换许多基于加密的子编辑中的关键字字符串，然后将最终的超级字符串通过一个(好的)单词微调器，然后发布结果字符串来为给定的罪犯创建 FOMO 或 FUD。