# 使用内置 AWS 功能定制业务洞察力

> 原文：<https://medium.com/hackernoon/custom-business-insight-using-built-in-aws-features-c399272b44d>

## 使用无服务器框架轻松创建仪表板

为了这篇文章的目的，我们将进入一个新创建的网上赌场的主人的鞋子。我们的第一个游戏是轮盘赌，它的后端是 AWS 上的一个无服务器 API。我们将会发现如何获得一些关于我们游戏的指标。

![](img/b3a1177518a63e74a7a6808d298bfcc3.png)

Photo by [Kay](https://unsplash.com/@kaysha?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/gambling?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 轮盘 API

我们的 API 提供了一个端点来处理玩家的赌注，并返回一个 0 到 36 之间的随机整数。使用[无服务器框架](https://serverless.com/)，您可以轻松实现这一点:

The definition of our service, we will complete it later

A simplified roulette game (only checks for the exact value)

我们的代码已经准备好了，您可以通过运行:

`serverless deploy —-stage staging --region eu-west-3`

在部署日志中找到 API URL，并通过查询 API 开始播放:

```
export CASINO_URL=[https://xxxxxxxxxx.execute-api.eu-west-3.amazonaws.com/staging/roulette](https://ls7sjcayle.execute-api.eu-west-3.amazonaws.com/staging/roulette)curl -X POST $CASINO_URL -d '{"bet":50,"number": 13}' -H 'Content-Type: application/json'{"win":false,"result":12}
```

太好了，我们有一个正常运转的赌场，多亏了我们的广告宣传，玩家们开始使用它了。为了推动我们的下一步决策，我们需要一些商业洞察力。特别是，我们想知道下注值在一天中如何随时间变化。我们的工程团队已经在忙于构建一个扑克游戏，所以我们需要一些真正快速设置。

好消息，各位！你可以在 CloudWatch 上创建这种自定义指标/仪表板，甚至有一个无服务器插件来实现自动化。您将看到我们甚至不需要更新 JavaScript 代码。

# 云观察指标

> 度量是 CloudWatch 中的基本概念。**指标代表发布到 CloudWatch 的一组按时间排序的数据点。**将一个指标想象成**一个要监控的变量**，数据点代表**该变量在一段时间内的值**。例如，特定 EC2 实例的 CPU 使用率是 Amazon EC2 提供的一个指标。数据点本身**可以来自您收集数据的任何应用程序或业务活动**。— [文档](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html#Metric)

在我们的例子中，我们将创建一个定制的指标:随时间变化的 bet 值。我们已经记录了请求体，因此下面是我们的自定义指标属性:

*   **过滤模式:** `{ $.bet = * }` 该语法使用 JSON 路径来过滤我们想要用于度量的日志。在我们的例子中，我们对打印请求体的日志感兴趣(包括`bet`属性)。
*   **度量值:** `$.bet` 仍然使用 JSON 路径，定义您想要保留什么属性作为度量值:在我们的例子中是 bet 值。
*   **日志组名称:** `/aws/lambda/online-casino-staging-roulette` 表示数据的来源。我们希望我们的指标显示出对轮盘游戏的洞察力，而不是对新扑克游戏的洞察力。还有，别忘了更新名字里的舞台。

你可以在这里得到更多关于过滤器和模式语法的信息。

# 无服务器插件指标

就像几乎所有的一样，有一个无服务器插件来自动创建定制的指标。首先，安装它，我们将在我们的`serverless.yml`后面添加几行来创建指标:

`npm install --save-dev serverless-plugin-metric`

Append these lines to the existing serverless.yml file

# CloudWatch 仪表盘

现在我们已经配置了数据源，让我们创建仪表板。将这些行追加到您的`serverless.yml`中:

Append these lines to the existing serverless.yml file

一些注意事项:

*   您可以在仪表板中添加许多小部件。每个小部件都由一个配置和一个指标列表组成。相同的配置将用于同一个小部件中的每个指标。
*   `period`属性定义了计算一个值的频率。这里我们设置 300 秒(每 5 分钟一次)。
*   `stat`属性允许您选择如何聚合值。对于我们的第一个图表，我们想要平均下注值，所以我们使用了`Average`。对于第二个，我们需要下注的数量，我们使用了`SampleCount`。最后，我们将使用`Sum`显示该期间的总下注额。其他可能性包括`Minimum`、`Maximum`和各种百分点。
*   有三种不同的视图可用:
    - `"view": "singleValue"`显示单个值
    - `"view": "timeSeries"`显示折线图
    - `"view": "timeSeries", "stacked": true`显示堆积折线图
*   您可以在 CloudWatch 控制台中创建仪表板，并在编辑时在 source 选项卡中导出小部件 JSON 定义。

# 结果

通过 API 发送随机值一小时后，结果如下:

![](img/31bbabeca3b100835c3fc85586969b2f.png)

Our wonderful dashboard

对于要求的少量工作来说已经很不错了，你不觉得吗？哦，这是定价:

*   3 美元/仪表板/月
*   0.30 美元/米/月

这使得这个仪表板每月 3.30 美元。

# 好处:在控制台外使用小部件

最后一件事:我们想与赌场的执行董事会分享这些图表。但让他们访问 CloudWatch 控制台似乎不是一件应该做的事情。嗯，你很幸运，因为有这样一个 API:[GetMetricWidgetImage](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_GetMetricWidgetImage.html)。除了在您的`serverless.yml`中创建仪表板，您还可以从电路板可以访问的 web 应用程序中检索小部件图像，并且您可以为工程师保留 AWS 控制台。

瞧！感谢您的阅读。欢迎在评论中分享你的想法和**快乐仪表板**！