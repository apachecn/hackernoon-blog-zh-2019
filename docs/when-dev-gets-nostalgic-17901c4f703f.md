# 当戴夫怀旧的时候

> 原文：<https://medium.com/hackernoon/when-dev-gets-nostalgic-17901c4f703f>

![](img/fe7da7830985a8d239c539a929ebc93f.png)

# **TL；艺术字。**

我出生于 80 年代末。这意味着当我在 90 年代长大时，我有过一些艰难的选择。其中之一是为学校作业选择一个[艺术字](https://en.wikipedia.org/wiki/Microsoft_Word#WordArt)的标题。

作为我日常工作的一部分，我需要升级影响许多开发者的东西。它冻结了一些部署，让开发人员不太高兴:-)

我需要用一些创造力来做这件事，在[Ran Greenberg](https://medium.com/u/11a05c32556?source=post_page-----17901c4f703f--------------------------------)告诉我“你的字体看起来像艺术字”并想起我小时候的挣扎后，我的怀旧情绪发作了。

![](img/bd69c9447183542ca0fab0a54731a578.png)

# 解决办法

我为 react 创建了一个名为`react-wordart`的库。现在部署冻结看起来好多了，超级用户友好。

![](img/942cfe6ac59da69ff069a3e4e4dd5947.png)

我使用[https://github.com/transitive-bullshit/create-react-library](https://github.com/transitive-bullshit/create-react-library)来生成库，并且仅仅是写代码，如此巨大的荣誉归于 [**特拉维斯·菲舍尔**](https://github.com/transitive-bullshit) 。

那里的大部分工作是编写 css。幸运的是，我有互联网…我用了这篇文章和 css。还使用了 [classnames](https://www.npmjs.com/package/classnames) 包来控制动态类名和 voilà。

使用 create-react-library，我得到了开箱即用的示例项目，运行`npm run deploy`，它出现了:[https://yershalom.github.io/react-wordart](https://yershalom.github.io/react-wordart/)

当我在 Reddit 的帖子上看到这个评论后，我决定将 css 拆分成自己的回购:[https://github.com/yershalom/css-wordart](https://github.com/yershalom/css-wordart)

用法非常简单:

就这样，我有了我的怀旧时刻，并创建了一个开源软件。

希望你喜欢阅读！

[](https://github.com/yershalom/react-wordart) [## yershalom/react-艺术字

### 我们知道的怀旧艺术字就在 react 里。通过创建帐户为 yershalom/react-艺术字的开发做出贡献…

github.com](https://github.com/yershalom/react-wordart) [](https://github.com/yershalom/css-wordart) [## yershalom/CSS-艺术字

### 艺术字 css。通过在 GitHub 上创建一个帐户，为 yershalom/CSS-艺术字的开发做出贡献。

github.com](https://github.com/yershalom/css-wordart) [](https://github.com/transitive-bullshit/create-react-library) [## 传递-废话/创建-反应-库

### ⚡CLI 轻松创建可重用的 react 库。-传递-废话/创建-反应-库

github.com](https://github.com/transitive-bullshit/create-react-library)