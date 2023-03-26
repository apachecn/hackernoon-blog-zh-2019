# Android 应用程序开发如何成为 Kotlin-first

> 原文：<https://medium.com/hackernoon/how-android-app-development-became-kotlin-first-c79e493e02fb>

![](img/f352d85edc81086e6a9f1024053804bf.png)

Source: AndroidPub

如果你点击了这篇文章，你可能会对我的方向有一个公平的想法。在一年一度的 I/O 开发者大会上，谷歌在 Android 应用开发社区的掌声中宣布了对 Kotlin 的热爱。但是这给 Java 留下了什么呢？它如何改变[手机 app 开发](https://www.valuecoders.com/)？是什么让谷歌优先考虑 Kotlin 而不是 Java 呢？

# 为什么谷歌想要比 Java 更好的东西？

![](img/56691c737a39a62ab5a03658a5991664.png)

Source: TechYourChance

首先，谷歌并不打算摆脱整个 Java 生态系统，尽管它确实希望它可以。但它一直在寻找 Java 编程语言的更好替代品，用于 [**android 应用开发**](https://www.valuecoders.com/hire-developers/hire-android-developers) **。**

这可能要追溯到 2010 年，当时 Sun Microsystems 的新东家甲骨文起诉谷歌 [**抄袭 Java API**](https://en.wikipedia.org/wiki/Oracle_America,_Inc._v._Google,_Inc.) 用于构建 Android 操作系统。这场历时 9 年的法律战经历了各种波折。谷歌在此案中最好的辩护是 API 没有版权。但是甲骨文坚持说他们是，谷歌在使用 Java API 之前没有授权。

这并不意味着如果甲骨文胜诉，我们会失去安卓。因为谷歌通过用 JDK (Java 开发工具包)的 [**开源版本**](https://venturebeat.com/2015/12/29/google-confirms-next-android-version-wont-use-oracles-proprietary-java-apis/) 构建从 Android 7.0(牛轧糖)开始的所有版本，避免了这样一个可能的场景。

但是，谷歌一直想脱离这个生态系统。Java 是一种近乎通用的编程语言。但是不管安卓开发者是害怕它，还是逃避它，对 Java 的需求还是以这样或那样的形式出现。这就是为什么他们不得不寻找一种不取代 java，而是成为它的替代品的东西。一种比 java 使用起来更有趣的语言，一种可以与之互操作的语言。科特林就这样来了！

***也读作:*** [***十一大趋势 AI 聊天机器人平台***](https://hackernoon.com/eleven-trending-ai-chatbot-platforms-ed58f250f841)

# 为什么 Google 选择 Kotlin 作为 android 应用开发的主要语言？

Kotlin 不是谷歌开发的新语言。它是一种开源语言，由 JetBrains(Google 公认的开发合作伙伴)在 2011 年开发。但它从未得到应有的认可，直到谷歌在 2017 年的年度 I/O 上推出 Kotlin 作为 android 应用开发的官方语言[](https://www.youtube.com/watch?v=NqlRg1_bCC4)**。**

**从那以后，科特林就再也没有回头路了。让这道菜变得更好的是 Kotlin 从 IDE 中获得了它所需要的所有支持。这是因为 Kotlin 背后的公司 JetBrains 也构建了 Android Studio 的核心，即 IntelliJ。**

**Google 和 JetBrains 对 kotlin 的协作和支持确保了 Android 开发人员可以轻松地从 Java 迁移到 Kotlin，不会出现任何问题。很快，Android 开发人员开始意识到 Kotlin 相对于 Java 在 Android 应用程序开发方面的优势，其中包括:**

1.  **与冗长的 Java 编码相比，开发人员可以编写简洁而富有表现力的代码。**
2.  **使用 Java 开发 Android 应用程序的一个主要问题是 NullPointerException。Kotlin 通过强制开发人员明确允许变量为空并否定任何此类问题来解决这个问题。**
3.  **开发人员通常很难迁移到新的语言，尤其是当你习惯于在 Java 这样古老的语言上开发 android 应用程序的时候。Android Studio 的 [**Java 到 Kotlin 的转换**](https://try.kotlinlang.org/#/Kotlin%20Koans/Introduction/Java%20to%20Kotlin%20conversion/Task.kt) 功能很容易解决这个问题，该功能允许开发人员将 Java 代码直接转换为 Kotlin。**

**这些关键的好处和更多的好处最终导致 50%的专业 android 开发人员转向 kotlin 并接受这一变化。根据 Stack Overflow 在 2018 年和 2019 年的年度开发者调查结果，Kotlin 是当今最受欢迎的语言之一:连续两年！**

**![](img/1811b331c26c9bb1290e63d582493af7.png)**

> **根据 Stack Overflow 年度开发者调查，Kotlin 是 2018 年第二大最受欢迎的编程语言。来源: [StackOverflow](https://insights.stackoverflow.com/survey/2018#technology)**

**![](img/b6d2cd1bad9fccde4c69bda7b09cc078.png)**

> **根据 Stack Overflow 的年度开发者调查，Kotlin 在 2019 年最受欢迎的编程语言中排名下降了 2 位。来源: [StackOverflow](https://insights.stackoverflow.com/survey/2019#technology)**

**锦上添花的是在 5 月份的谷歌 I/O 2019 上，谷歌自己宣布 Android 开发将进入 [**Kotlin-first**](https://insights.stackoverflow.com/survey/2019#technology) ，并鼓励开发者利用 Kotlin 语言进行移动应用开发。**

# **使用 Kotlin 开发 Android 应用程序的未来之路**

**谷歌不打算通过引入 Kotlin 并推广其在 android 应用开发中的使用来取代 Java。但它只是需要一些东西来配合前者。**

**Kotlin 本身运行在 Java 虚拟机(JVM)上，这就是为什么对于最终用户来说，新的编程语言不会有太大的不同。因此，就像最近发生的那样，将 Kotlin 与 Java 进行比较是不公平的。 **Kotlin *就是* Java** 。您可以将您的 Kotlin 代码转换成 Java，无论如何，您都可以让您的 kotlin 代码在 JVM 上运行。**

**但是 Kotlin 是发展最快的编程语言之一这一事实是无可争议的。在 6 年的时间里，Kotlin 设法进入了 TIOBE 索引中的前 50 种编程语言。这本身显示了 kotlin 作为开发 android 应用程序的有趣、高效的编程语言的潜力。**

**但是这种增长是永恒的，还是 Kotlin 最终会被一种不同的即将到来的语言超越？目前来看，这种情况发生的可能性似乎不大。但是甲骨文知道 Java 在 android 应用开发和其他领域的重要性。因此，对他们来说，在 Java 的下一个版本中进行一些升级来挑战 Kotlin 并不困难。**

**总而言之，Kotlin 成为谷歌移动应用开发的推荐选择，是因为谷歌希望它是这样的！Kotlin 被设计得比 Java 更好。它本来是一个梯子，android 应用程序开发公司可以爬上去，从 Java 迁移到更好的地方。**