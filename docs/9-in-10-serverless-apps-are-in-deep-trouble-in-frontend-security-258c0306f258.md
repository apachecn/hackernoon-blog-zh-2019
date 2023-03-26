# 90%的无服务器应用在前端安全方面深陷困境

> 原文：<https://medium.com/hackernoon/9-in-10-serverless-apps-are-in-deep-trouble-in-frontend-security-258c0306f258>

我们都讨厌安全漏洞，我们都厌倦了研究保护应用程序的方法。让我们面对现实吧:创造一个新的受欢迎的功能比保护某天可能被某人利用来激怒所有人的漏洞要令人兴奋得多。然而，我们都不想被发现在保护我们的软件时偷懒，这就是为什么我们关心最高标准的安全性。

![](img/a89811dcde7c33f3e75051458f1c4f26.png)

Photo by [Matthew Henry](https://unsplash.com/photos/fPxOowbR6ls?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/security?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 灵活不一定安全

例如，许多安全漏洞来自过多的灵活性或者没有遵循最小特权原则。在前端应用中，这个问题源于使用 [*本地存储*](https://developer.mozilla.org/en/docs/Web/API/Window/localStorage) 来保存秘密(即认证令牌)和敏感用户数据。

尽快停止这样做！

我知道，我知道， *LocalStorage* 非常酷:它是纯 JavaScript，它可以灵活地存储数据，附带一个简单的 API，所有主流浏览器都支持，是的，是的，它很可爱。但是，出于同样的原因，它也是一个魔鬼。通过将敏感信息存储在 *LocalStorage* 中，任何在你的应用中执行 JavaScript 的人都可以访问这些数据。如果攻击者找到了注入和执行 JS 代码的方法，你就完了，因为他几乎可以窃取每个登录用户的敏感信息。见鬼，这个人甚至可能冒充任何用户，在你不知情的情况下代表他/她发出请求。

如果你不相信，并有更多的时间阅读这个问题，请阅读 Randall Degges 的[请停止使用本地存储](https://www.rdegges.com/2018/please-stop-using-local-storage/)。相信我，它会说服你的。

好吧，这个安全漏洞可能不会影响 90%的无服务器应用程序，可能会更多，也可能更少。无论如何，为此目的使用 *LocalStorage* 是一件很常见的事情，我怎么强调重新思考这种方法的重要性都不为过。

# 我们讨厌饼干，但我们不应该

![](img/42efa2d5958944bd7b09aefb509b93a6.png)

Photo by [Food Photographer | Jennifer Pallian](https://unsplash.com/photos/OfdDiqx8Cz8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/cookies?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

听着，我也不喜欢 cookies，但是当涉及到保护身份验证令牌和敏感用户数据时，它们可以帮助我们。

我知道你在想什么:"*嘿，但是在像 AWS Lambda* 这样的分布式无服务器架构中开始管理 cookies 是不对的"！

好吧，通过使用 cookies 而不是本地存储，并不意味着你必须有一个有状态的应用程序，所以接下来的几行请耐心听我说。

如今，使用 JWT ( [JSON Web 令牌](https://en.wikipedia.org/wiki/JSON_Web_Token))或类似的策略来保护 Web 应用程序非常普遍。我们基本上打包成一个哈希:一些用户信息，如用户名、电子邮件等，一些应用程序数据和一个验证哈希机制，以保持令牌的完整性。开发人员相对经常做的是将 JWT 存储在 *LocalStorage* 中，这使得获取和发送请求到后端进行认证和授权变得容易。这个实现的问题是我们上面提到的巨大的安全风险。

# 在无服务器的情况下正确制作 Cookies

要清除应用程序中的此漏洞，应用程序应将 JWT(或任何其他类型的身份验证令牌)设置为 cookie。另外，确保它远离 JavaScript 手指。默认情况下，JS 也可以访问 cookie，但是您可以设置一个名为 *httpOnly* 的标志，它告诉浏览器特定的 cookie 应该只在 HTTP 调用中使用，远离本地 JS 代码。例如，参见 [Mozilla Cookie 规范](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie)并注意我们提到的 *httpOnly* 标志。

这也符合 [OWASP](https://owasp.org/) 指令对[使用 cookies 和 *LocalStorage*](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/HTML5_Security_Cheat_Sheet.md#local-storage) 。

![](img/92d1cc4a543f8a3a7d88116f617ead92.png)

Photo by [Pablo García Saldaña](https://unsplash.com/photos/lPQIndZz8Mo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/right?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

从传统的 web 服务器设置 cookies 通常很简单。在无服务器架构中，这并不简单。例如，在 AWS 中，您可以使用带有 [Lambda](https://aws.amazon.com/lambda/) 的 [API 网关](https://aws.amazon.com/api-gateway/)服务向您的应用程序公开定制 cookies。一旦在用户浏览器中设置了 cookie(例如，包含一个 JWT)，它将伴随所有进一步的请求到达后端。然后，您的 Lambda 函数可以解析 cookie、提取令牌、认证用户并授权操作，就像您通常所做的那样。

AWS 团队发布了一个简单易懂的教程来解释如何做到这一点:[使用 AWS Lambda 通过 API Gateway](https://aws.amazon.com/pt/blogs/compute/simply-serverless-using-aws-lambda-to-expose-custom-cookies-with-api-gateway/) 公开定制 Cookies。

# 安全永远不嫌多

既然您已经了解了这个常见的漏洞以及修复它的简单方法，那么请继续研究如何更好地保护您的应用程序。OWASP 有非常好的资源。

我们生活在一个有趣、丰富但危险的互联网中，你永远不会觉得*太安全。在您寻求保护无服务器应用程序的过程中，您的日志是一个很好的帮手。如果您正在记录[正确的信息](https://dashbird.io/blog/securing-serverless-with-critical-logs/)，并且能够高效地访问这些日志，您将有一个良好的开端来防止和减轻恶意攻击。*