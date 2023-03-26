# 在发布应用程序之前我应该知道什么

> 原文：<https://medium.com/hackernoon/what-i-should-have-known-before-publishing-an-application-898d8f72a0e2>

![](img/fa2f2cd5141ffc7e2335f6af8ad6953c.png)

Photo by [Nick Fewings](https://unsplash.com/@jannerboy62?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

编写应用程序是一个艰难而漫长的过程。你从一个想法开始，试着把它变成有用的东西，然后开始开发过程。在开发过程中，你会不断地问自己，我的应用程序是否已经到了我想要发布的时候了，或者我是否应该开发更多的特性？也许我应该确保它能在尽可能多的设备上运行？这些都是很好的问题，但是我在这里告诉你，不要担心可能会发生什么，你应该担心在可预见的未来肯定会发生什么。

有什么比从别人的错误中学习更好的方法来为你的下一份工作做准备呢？我在这里分享我的，没有羞愧，但有一点遗憾。你很快就会明白为什么。

## 马后炮是 20–20

我们在这篇文章中的用例将是我最近的应用程序，它是我发布给谷歌 Play 商店的。一些无聊的数据供你浏览:

*   它是三个多月前出版的
*   它使用 Firebase 进行分析和推送通知
*   它仅适用于安卓系统和特定国家
*   在巅峰时期，下载量达到了 100 多次

我知道这不是一个高容量、功能丰富的应用程序，但是相信我，从我的错误步骤中可以学到很多东西。

## 阅读各类手册

如果你不知道这个缩写代表什么，[查一下](https://en.wikipedia.org/wiki/RTFM)。谷歌对发布应用程序有严格的政策。发布应用程序的[过程的一部分](https://medium.freecodecamp.org/how-to-publish-an-application-in-the-play-store-8ddcc6dc3587)，要求你，开发者，填写一份隐私政策。没有应用程序，就无法发布应用程序。未能遵守您的隐私政策，将导致您的应用程序从 Play store 中删除。所以你不能说你不知道，这里的[是谷歌隐私政策指导页面的链接。](https://developers.google.com/actions/policies/privacy-policy-guide)

我是吃了苦头才知道的。也就是说，几个月后，我的应用程序被 Google 从 Play store 中删除了。我收到一封电子邮件，声称我违反了《开发者分发协议》的第 4.8 节。听起来很可怕，不是吗？

要确保您的隐私政策涵盖所有您需要的内容，请使用:

[](https://app-privacy-policy-generator.firebaseapp.com/) [## 应用隐私策略生成器

### 一个简单的 web 应用程序，为您的 Android/iOS 应用程序生成通用隐私策略

app-privacy-policy-generator.firebaseapp.com](https://app-privacy-policy-generator.firebaseapp.com/) 

## 第三方库

如果您在应用程序中使用它们，请确保理解这些库本身导入了什么以及它们在内部做了什么。在大多数情况下，这些库帮助我们解决一些我们没有时间或知识去写的东西。但明智的做法是，不要把它们仅仅视为一个会变魔术的黑盒子，而是一个需要调查的盒子。可能出现的一些问题是，该库将来可能无法更新或维护，它可能有[安全漏洞或隐私问题](https://www.androidauthority.com/3rd-party-android-app-privacy-issues-67669/)。

如前所述，我在应用程序中使用了 Firebase Analytics。我不知道的是，除了发送我配置的带有特定数据的特定事件之外，Firebase 还允许发送其他与我无关的数据。部分数据违反了开发者发布协议，因此我的应用程序被关闭了。Touché Firebase，这一轮你赢了我。

如果你想使用 Firebase Analytics，但对收集用户的[广告 id](https://support.google.com/googleplay/android-developer/answer/6048248?hl=en) 不感兴趣，可以使用下面的代码片段(适用于 Android):

Put this inside your AndroidManifest.xml inside the Application tag

## 保存应用程序签名

发布应用程序时，您必须创建一个签名的 APK。为此，您必须生成上传密钥和密钥库。 [***不要为了任何意图和目的，丢失密钥库文件***](https://stackoverflow.com/questions/4322367/i-lost-my-keystore-file) 。你会后悔一辈子的。没有您的密钥库文件，您将无法对您的应用程序进行数字签名。不签署你的应用程序，谷歌将无法确定你是已经发布的应用程序的所有者。这将导致您无法更新您在 Play store 中发布的应用程序。不得不在不同的包下重新发布应用程序就像第一次发布一样。你会失去所有的下载、评论和其他好东西。

我想我没必要告诉你我发生了什么，对吧？你们可以自己解决这个问题。

![](img/e823050faae55270ef185859d0fb1cc0.png)

Photo by [Alice Donovan Rouse](https://unsplash.com/@alicekat?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 不要等待

就像我在开始提到的，有时你不知道在发布应用程序和尝试添加更多功能之间划一条线。虽然它与我们作为示例使用的应用程序没有直接关系，但是一旦您对当前的应用程序感到满意，发布该应用程序仍然是一个好的规则。这背后的逻辑是双重的:

1.  重新发布应用程序是一个简单的过程
2.  把你的脚放在众所周知的应用程序商店的门会让你有好处，确保你的想法是第一个出现的

引用拿破仑·希尔的话，

> 别等了。时机永远不会恰到好处。

展望未来，我希望你不会犯和我一样的错误。如果你有任何自己的不幸，请在下面的评论中分享或者联系我。你永远不知道昨天的错误什么时候会成为明天的胜利。