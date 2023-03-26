# 使用 LiveData 的 Android 单向数据流

> 原文：<https://medium.com/hackernoon/android-unidirectional-flow-with-livedata-bf24119e747>

## 改进 Coinverse 的性能和结构

![](img/bc6e9a85f753d73ed027338760f19314.png)

pc — [Ned Scher](https://www.linkedin.com/in/ned-scher-60bbb152/), Waterfall at Yosemite National Park (2015)

T **何*单向数据流*【UDF】模式自 2 月份推出第一个测试版以来，提高了**[**coin verse**](https://play.google.com/store/apps/details?id=app.coinverse)**的可用性和性能。Coinverse 是第一个以加密货币形式创建涵盖技术和新闻的音频广播的应用程序。使用 UDF 的升级包括更有效的新闻订阅源创建，删除相邻的原生广告，以及更快的音频播放加载。**

TLDR:

[](https://android.jlelse.eu/sample-app-android-unidirectional-data-flow-b9f8ad0fbca3) [## 示例应用——Android 单向数据流

### 在 Coinverse 中使用 LiveData

android.jlelse.eu](https://android.jlelse.eu/sample-app-android-unidirectional-data-flow-b9f8ad0fbca3) 

UDF 模式将应用程序组织成三个主要区域，视图**状态、事件**和**效果**，确保应用程序模块化和可靠。我从[片段播客](https://fragmentedpodcast.com)、 [*进化中的 Android 架构(第一部分)*](https://fragmentedpodcast.com/episodes/148/) 与 [*复制*](https://medium.com/u/b85c7e530b1f#copying) 功能很有用。

## 第 4 步，共 6 步—使用 LCE 模式管理网络请求

为了管理网络请求， [Firebase 云功能](https://medium.com/u/b85c7e530b1f#call_the_function)将逻辑完全卸载到后端。

LiveData 在线程等方面没有太多的定制。随着今年最新的 Google I/O 更新，协程似乎可以轻松地与 LiveData 集成，提供更多的定制。

# Coinverse 后续步骤

*   **单向数据流—** 扩展到 Coinverse 应用的其余部分
*   **JUnit 测试**——现在，大多数 newsfeed 逻辑都在 *ViewModel* 中模块化了，JUnit 测试会更容易，对组件的模仿会更少。
*   **Kotlin 协同程序—** 通过使用与 LiveData 集成的 [Kotlin 协同程序](https://developer.android.com/kotlin/coroutines)控制线程来提高性能

接下来:

[](/@AdamHurwitz/udf2-0-5052c3e1c62a) [## 使用 LiveData - 2.0 的 Android 单向数据流

### 改进状态模型+协程流程

medium.com](/@AdamHurwitz/udf2-0-5052c3e1c62a) 

# 资源

*   **在 Play Store 上查看 coin verse:**

[](https://play.google.com/store/apps/details?id=app.coinverse) [## Coinverse -加密货币新闻、比特币、以太坊 Google Play 上的应用

### 安卓上没有？-订阅资料片更新@ bit.ly/coinverse-beta1.你是开发者吗？-检查开放的…

play.google.com](https://play.google.com/store/apps/details?id=app.coinverse) 

*   探索并运行 Coinverse 代码:

[](https://android.jlelse.eu/sample-app-android-unidirectional-data-flow-b9f8ad0fbca3) [## 示例应用——Android 单向数据流

### 在 Coinverse 中使用 LiveData

android.jlelse.eu](https://android.jlelse.eu/sample-app-android-unidirectional-data-flow-b9f8ad0fbca3) 

*   [幻灯片](https://docs.google.com/presentation/d/1bj-lG2ghJO5EVIJRrCt3eH-lZsz8ChPc1hVXZ_KYpD8?rm=minimal):

UDF Slides

*   [注意事项](https://docs.google.com/document/d/13fmrGJbGHNEPo3FN7IDmwQjxjXK3qoACSRqjyawV32k?rm=minimal):

UDF Notes

非常感谢麦德林安卓聚会[的](https://www.meetup.com/Medellin-Android/)[克里斯蒂安·戈麦斯](https://twitter.com/Iyubinest)和[卡洛斯·丹尼尔](https://medium.com/u/eaf44c102d99?source=post_page-----bf24119e747--------------------------------)组织这次演讲！如果你在麦德林，我建议你去他们的聚会。

![](img/7c31f8511b3caaeac309300daf84a4b6.png)

[Medellín Android talk — Unidirectional Data Flow por Adam Hurwitz](https://www.youtube.com/watch?v=Elp-Z-pQTpM) (2019)

![](img/ca4727e683da6a69980e9e6d985b2934.png)