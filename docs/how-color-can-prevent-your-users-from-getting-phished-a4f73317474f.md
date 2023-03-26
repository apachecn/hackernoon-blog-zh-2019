# 颜色如何防止你的用户被钓鱼

> 原文：<https://medium.com/hackernoon/how-color-can-prevent-your-users-from-getting-phished-a4f73317474f>

![](img/2265837fb1d9d809a91c9c35951afb25.png)

Photo by [Scott Webb](https://unsplash.com/@scottwebb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

想知道你收到的一封邮件是否值得信任是非常困难的。

为什么这么难？你不能相信发信人的名字。你不能相信邮件的美学。而你*绝对*不能相信副本。为了安全起见，您需要验证发件人的域名和所有出站链接的域。

把自己放在一个随机用户的位置上，试着破译“efax.hosting.com.mailru.co”或“drive . Google . com . download-photo . net”。期望普通用户每次打开收件箱都保持高度警惕是不合理的，所以让我们找到一个更好的方法。

# 使用颜色

最根本的问题是，你需要用来确定电子邮件是否安全的信息 1)很难找到，2)很难理解。在一个完美的世界里，这些信息 1)很容易浏览，2)不可能被误读。

有几种方法可以验证你是否在和正确的人交谈。您可以验证 1)他们是什么(生物识别)，2)他们有什么(2FA 代码)，或 3)他们知道什么(密码)。目前还不清楚我们如何将电子邮件与发件人的生物特征或 2FA 代码联系起来，但我们肯定可以研究出“电子邮件密码”的想法。

我们为网站提供密码，以便随时验证我们的身份。如果网站需要向*用户*提供一个密码，这样我们就可以验证他们是谁，会怎么样？如果这些密码不是复杂的字符串，而是易于浏览的颜色呢？

![](img/b4d6015dfef4dc05cc66395afb9d7006.png)

[https://github.com/turbomaze/colorful-phish](https://github.com/turbomaze/colorful-phish)

想象一下:当你为一个网站创建了一个密码，他们也为你创建了一个。在你的欢迎邮件中，他们告诉你这个密码是什么:一种特定的颜色，是你的账户独有的。从那一刻起，你可以放心知道，如果一封电子邮件不包含确切的颜色，那么你得到了网络钓鱼。

以下是一封欢迎电子邮件的示例:

![](img/fdac36a58d0d1b2ed653509aeb7501f5.png)

我们结束了。颜色实现简单，容易略读，不可能误解。查看 GitHub 上的[colorful-phish](https://github.com/turbomaze/colorful-phish)获得 Node.js 实现，它将帮助你用 3 行代码消除网站上的网络钓鱼。

如果你有任何想法或想要重复相关的想法，你可以在推特上找到我，电话是[https://twitter.com/@imigliu](https://twitter.com/@imigliu)。