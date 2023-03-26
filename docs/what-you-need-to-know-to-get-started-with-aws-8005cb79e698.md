# 自动气象站简介

> 原文：<https://medium.com/hackernoon/what-you-need-to-know-to-get-started-with-aws-8005cb79e698>

![](img/db56bbec7d7e36e8aceb55a8b659e397.png)

Photo by [Eugen Str](https://unsplash.com/@eugen1980?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

AWS 是最流行的云计算服务之一。AWS 是亚马逊网络服务的缩写，天哪，他们有很多服务。

我们每天使用的许多应用程序都托管在 AWS 上。其中包括网飞、Airbnb 和 Slack 等等。

也许你在一家使用 AWS 的公司工作，你想了解更多。或者您可能想将应用程序部署到 AWS。或者你只是在某个地方听到了 AWS，一直挥之不去。不管怎样，这是你需要知道的。

# 服务器

![](img/26b5ec9e977e36ad6a415d43db2adf7e.png)

您的应用程序，无论是您自己的还是您工作的公司的，都可能运行在服务器上。当你开发应用程序时，你是在你的计算机上运行它。但是当您部署应用程序时，您希望在其他人的计算机上运行它。

AWS 可以提供那台电脑。它被称为[EC2](https://aws.amazon.com/ec2/)T4 或者弹性计算云。

此外，如果你对数据科学感兴趣，你可以在那里运行你的 Jupyter 笔记本。

# 数据库ˌ资料库

![](img/4645361b8391f0f41b0010d016861cdd.png)

您的应用程序可能使用数据库。PostgreSQL，MySQL，甚至 Oracle，都无所谓。你需要知道的是，AWS 有一项针对数据库的服务，它叫做 [RDS](https://aws.amazon.com/rds/) **(关系数据库服务)**。

您可以在另一个 EC2 实例上运行您的数据库，就像在您的个人计算机上运行一样，但是 RDS 有很多优点。这里有几个例子:

*   成本效益
*   自动软件修补
*   易于扩展
*   自动备份
*   监视

您可以在这里熟悉功能的完整列表[。](https://aws.amazon.com/rds/features/)

# 部署

![](img/41dcd0d7a397137b54dab77b762ae0d3.png)

但是如何部署您的应用程序呢？在 AWS 上有很多方法可以做到这一点。但是如果你刚刚开始，我建议你使用 [EB](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/Welcome.html) **(弹性豆茎)**。

它允许您轻松:

*   [创建环境](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.environments.html)使用您需要的所有配置(EC2、RDS)和一些您可能一开始并不需要但仍然有用的配置(负载平衡器、监控、通知)
*   将部署到环境中
*   [检查日志和服务器的 ssh](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli-troubleshooting.html)

EB 支持部署从 [Docker](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker.html) 到 [Java](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_Java.html) 到 [Ruby](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_Ruby.html) 的不同平台。你可以点击查看支持平台的完整列表[。](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/concepts.platforms.html)

如果你过去用过 Heroku，EB 和它非常相似。与单独配置所需的每个 AWS 服务相比，EB 很容易设置。但还是没有 Heroku 那么容易。

如果您想在向 git 提交之后开始部署，您可以查看 AWS[code deploy](https://aws.amazon.com/codedeploy/)。

# HTTP 路由

![](img/f803329f84d859d0fcac65d19ba1a96b.png)

一旦部署了应用程序，您就需要将您的域流量路由到它。你可以使用 [Route53](https://aws.amazon.com/route53/) 来完成。

虽然当你用 EB 部署时，你可以得到一个像`http://your-application.elasticbeanstalk.com/`这样相当不错的域名，但它仍然有那种不是你的感觉。您可以直接在 Route53 上为您的应用购买域名。如果你是在 GoDaddy 或 NameCheap 等其他服务上购买的，你也可以将它委托给 Route53。您可以使用`dig`控制台命令来检查您的域是否迁移到 AWS。

一旦你在 AWS 上有了你的域名，你需要添加一个别名记录到

*   EB 给你的域名(`[your-application.elasticbeanstalk.com](http://your-application.elasticbeanstalk.com/)`)
*   或者你的 EC2 的弹性 IP
*   或者您的负载平衡器 DNS 名称

如果你对“记录”这个术语不熟悉，我推荐你阅读[这篇关于 DNS](https://dev.to/swyx/networking-essentials-dns-1dl7) 的文章。

# 处理更多流量

![](img/2f4bff4b50f97a910ee4569987038049.png)

在某些时候，您的应用程序可能会获得一些流量，而您的一个 EC2 实例将无法处理所有传入的请求。[ELBs](https://aws.amazon.com/elasticloadbalancing/)**【弹性负载平衡器】已经在 AWS 上拯救你了。**

**负载平衡器将传入流量路由到不同的服务器。他们使用“循环”算法来实现这一点。循环算法非常简单，当一个请求进来时，负载平衡器将它发送到一个服务器，第二个请求进入第二个服务器。当所有的服务器都收到请求时，负载均衡器从头开始，再次向第一个服务器发送请求。**

# **确保安全(HTTPS)**

**![](img/b169ddd22346f3684761d86c01d7fb1d.png)**

**如果你想把 HTTPS 加入你的网站，你需要获得一个证书。可以在 [ACM](https://aws.amazon.com/certificate-manager/) **(AWS 证书管理器)**领取证书。一旦你得到了它们，你就需要使用它们，它们不会自动应用。您可以[将证书添加到您的 EC2 实例](https://aws.amazon.com/premiumsupport/knowledge-center/connect-http-https-ec2/)中。或者，如果您已经使用 EB 进行了部署，那么您应该已经获得了一个负载平衡器。在这种情况下，您可以将证书应用到负载平衡器，并且现在可以通过 HTTPS 服务请求。**

**然而，在这个简单的例子中，人们仍然可以通过 HTTP 访问您的应用程序。但是如果它们来自 HTTP，您可以通过重定向它们来强制它们使用 HTTPS。以前，您需要[设置 NGINX 或 Apache](https://www.keycdn.com/blog/http-to-https#5-add-301-redirects-to-new-https-urls) 重定向。但是最近，AWS 发布了应用程序负载平衡器，它们包含了使用一些自定义规则进行重定向的选项。你可以在这里阅读如何使用应用负载平衡器[设置 HTTPS 重定向。](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environments-cfg-alb.html#environments-cfg-alb-console)**

# **存储文件**

**![](img/1064fbe3d7d74f638c1e3b29019c57bf.png)**

**您经常需要在应用程序中保存文件。但是将它们存储在数据库中可能不是最有效的解决方案。AWS 有一个名为 [S3](https://aws.amazon.com/s3/) 的服务，代表**简单存储服务**。顾名思义，它只是存储数据。你可以在那里写任何你能想到的东西。它就像一个谷歌驱动器或 iCloud。**

**要在 S3 上存储文件，你可以使用适用于你的语言的 AWS SDK。你也可以找其他的图书馆，因为我确定还有更多。**

# **发送电子邮件**

**![](img/d844a16137fb1ce8943f5e6cea98b271.png)**

**大多数应用程序做的另一件事是发送电子邮件。电子邮件已经失去了用途:时事通讯、系统通知等等。**

**要使用 AWS 发送电子邮件，您需要使用 [SES](https://aws.amazon.com/ses/) **(简单电子邮件服务)**。您可以使用您的个人电子邮件或为您的应用程序注册一个新的电子邮件，并从这些电子邮件发送电子邮件。这是一个很好的起点。**

**然而，从长远来看，从个人邮箱发送邮件并不是一个好的选择。一个更好的方法是获得一个域名，并从它发送电子邮件。为此，你需要[在 SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-domain-procedure.html) 中验证你的域名。之后，你可以使用支持多种编程语言的 AWS SDK 发送[电子邮件。或者你可以自己找一个图书馆或者创建一个。](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-an-email-using-sdk-programmatically.html)**

# **结论**

**在本文中，我讨论了最流行的 AWS 服务以及为什么和如何使用它们。现在你应该知道你应该**

*   **在 EC2 上运行您的服务器**
*   **在 RDS 上运行数据库**
*   **使用 EB 部署到 AWS**
*   **在 Route53 中管理您的域**
*   **在 ACM 中获取 SSL 证书**
*   **通过 ELB 处理更多流量**
*   **存储 S3 的文件**
*   **用 se 发送电子邮件**

**当然，这个列表只是 AWS 服务的冰山一角，但它应该足以让你开始。**

**如果你喜欢这篇文章，一定要在下面留下你的评论，然后关注我。**

***原载于 2019 年 2 月 24 日*[*https://kholinlabs . com/what-you-need-to-know-to-get-started-with-AWS*](https://kholinlabs.com/what-you-need-to-know-to-get-started-with-aws)*。***