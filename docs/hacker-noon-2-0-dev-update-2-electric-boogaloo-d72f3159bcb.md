# 黑客正午 2.0 开发更新 2:电动布加洛

> 原文：<https://medium.com/hackernoon/hacker-noon-2-0-dev-update-2-electric-boogaloo-d72f3159bcb>

![](img/2bf60671556dd2a97b03948ece2b9635.png)

Photo by [José Miguel](https://unsplash.com/photos/cNqwm1RB7Tw?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/electric-boogaloo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在过去的几个月里，我一直在 Hacker Noon 2.0 上工作。在那段时间里，我遇到了各种各样的技术挑战、陷阱、技巧和款待。今天，我想和你，我们的社区分享一些。

让我们从我们的一些架构开始:我们从在后端使用 Firebase 开始，在前端使用 React。[正如我们在 3 月初的 Github 活动中所讨论的那样](https://hackernoon.com/lightning-talks-at-github-hq-how-a-cdn-saved-our-hosting-bill-b5cc1c6edaa5)，我们很快发现 Firebase 的成本将会失控。长话短说，我们决定使用一个混合系统，一个使用 CDN(内容交付网络)来服务高流量页面，如主页、故事页面等，一个全功能的 Firebase 应用程序将服务于故事编辑器、编辑器仪表板、管理仪表板等页面。这种结构将减少我们的托管成本约 90%！

这种架构带来了一些有趣的挑战，尤其是单点登录(SSO)。坦率地说，SSO 令人头疼，但却是必要的。我们将拥有根域、应用子域和社区子域，我们当然不希望你必须使用不同的登录方式登录 3 个独立的应用……那将是一场噩梦，大多数人根本不会这么做。

您如何使用 Firebase 实现 SSO，以及我们社区论坛的话语？[细节](https://meta.discourse.org/t/official-single-sign-on-for-discourse-sso/13045) [有点毛](https://meta.discourse.org/t/sso-with-firebase/56524/10)，但本质上过程是这样的:

1.  你点击[community.hackernoon.com](http://community.hackernoon.com/)右上角的“开始”按钮
2.  你将被重定向到[auth.hackernoon.com](http://auth.hackernoon.com/)上的认证页面，该页面在 URL 中带有“sso”和“sig”参数。
3.  你输入你的电子邮件。如果您的电子邮件地址尚未通过验证，您将收到一封电子邮件，其中包含继续操作的链接。
4.  此时，您将进入密码页面。这将接受您的电子邮件和密码，登录到 Firebase 并生成一个 Firebase ID 令牌。
5.  ID 令牌、“sso”和“sig”参数被发送到后端。
6.  后端解构“sso”参数，提取一个随机数，并在此过程中使用 HMAC 验证有效负载的真实性，并将结果与“sig”参数进行比较。
7.  后端生成一个新的有效负载，其中包含用户经过验证的电子邮件、姓名、句柄等，并将其与 nonce 一起打包到一个 base64 编码的 blob 中。
8.  这个有效载荷被发送回 Discourse(社区论坛),你就登录了！

登录的过程有一点不同，但是基本内容是一样的。这最终成为一个极其复杂的过程，需要进行大量的反复试验才能实现。我非常感谢我上面链接的指导者——没有他们，我们可能没有 SSO。

那么，这些胡言乱语的结局是什么？一些好处:

*   你可以一次登录所有的 Hacker Noon 2.0 网站，包括[community.hackernoon.com](http://community.hackernoon.com/)，社区论坛，和[app.hackernoon.com](http://app.hackernoon.com/)，在那里你可以写和提交 2.0 版本的故事。
*   由于电子邮件验证，没有人可以声称你的电子邮件是他们自己的。
*   你可以请求任何你想要的句柄，只要你不使用一个注册的名字或者冒充某人。不过假名是可以的！在我写这篇文章的时候，还有很多黄金地段，所以发挥你的想象力吧——前途无量！
*   感谢 CDN，你的故事将会被快速加载。因为它们基本上是静态页面，所有重要的数据都是静态嵌入的，所以几乎没有任何加载时间，默认情况下页面是 SEO 友好的。

接下来:我一直在努力优化我们的页面构建系统。几天前，这个系统还非常幼稚，只是在有更新的时候重建页面。很快，系统将只建立必要的东西。在接下来的几天里，我将致力于将某些东西的标记和数据分开，如表情反应、赞助等等，这将允许我们更新一个简单的 JSON 文件，而不是重新生成整个 HTML 页面。我还编写查询来“填充”页面中当前静态的部分，如“更多故事”部分或“相关标签”。

总的来说，这是一个狂野的旅程，我不会用它来交换任何东西。我们正以极快的速度工作，让这个项目启动，这真的令人激动。更别说好玩了！我正在全力以赴地构建 2.0，并对内容生成、SSO 和我们已经解决的许多其他问题的初始方法进行迭代。

最终，迭代将决定这个平台的成败。这不仅仅是清理麻烦的代码，或者重构，或者清理技术债务。这是从根本上重新考虑你的方法，你的假设。我猜它可以归入重构的范畴，但它是一种极端的形式。

对我来说，迭代意味着我可以编码快速和肮脏的解决方案，但我必须在完成后清理我的混乱。这意味着我们现在可以发布基本功能，以后再处理边缘情况。这些想法可能会给你们中的一些人敲响警钟——起初对我来说确实如此。然而，请记住，我们讨论的是在你第一次发布它们之后立即修复它们。并且[牢记童子军规则](https://deviq.com/boy-scout-rule/)，代码一直在变得越来越干净。

也就是说，如果这个概念值得追求，你就清理代码。毕竟，当一件东西要被扔掉的时候，清理它是没有意义的。测试概念，而不是代码，看看它是否有脚。然后，给定用户反馈，决定它是否有价值，并从那里清理和重构，合并你收到的反馈中更好的部分。

另一方面，有些公司的流程总是要求清洁生产代码。您不能部署“脏”代码，即使它能完成工作。有时这是有充分理由的。但是，很多时候，公司模仿大公司的流程，而没有考虑如何将它们应用到自己的特殊情况中。这将 3 天的任务变成了 3 周。

我并不是说这就是迭代的全部。这也特别适用于 UX，但我对后端代码、认证系统等并不熟悉。我可以给你举一些面向用户的功能的例子，但是[我们的首席采购官 Dane Lyons 比我做得更好](https://www.youtube.com/watch?v=Ow2lje7UBd4)。

最终，迭代意味着我们必须不断改进。这意味着发射并不是终点。这意味着我们要么游泳，要么去死。也就是说:我应该回去工作了。

*原载于 2019 年 4 月 12 日*[*community.hackernoon.com*](https://community.hackernoon.com/t/hacker-noon-2-0-dev-update-2-electric-boogaloo/1953)*。*