# AWS 无服务器的权威速成班:使用 Kinesis 和 Lambda 的集中式日志记录

> 原文：<https://medium.com/hackernoon/the-definitive-crash-course-on-serverless-with-aws-centralized-logging-with-kinesis-and-lambda-bfbc3439ceac>

![](img/0be15e991574cdd6c3b01c78b8e105dd.png)

当 API 失败而你完全不知道原因时，你难道不讨厌它吗？现在，假设您无法访问运行软件的虚拟机、集群或容器。想让我继续这个噩梦吗？

是的，这就是调试 AWS Lambda 函数的趋势。一场可怕的噩梦，不知道发生了什么，也不知道事情失败的原因。本文将向您展示一种记录函数调用的方法。让您可以跟踪和监控故障和错误，同时还为您提供了一个很好的记录信息和调试日志的结构，以便在您需要对行为进行故障排除时使用。

关键是将所有日志发送到一个中心位置，在那里您可以对它们进行分组、过滤和理解。 [Sematext](https://sematext.com/cloud/) 是针对整个软件堆栈的全堆栈可观察性解决方案。这意味着您可以在任何现有的基础设施上实现函数日志，比如 Kubernetes 集群和容器。

准备好了吗？我们开始吧！

# 将 CloudWatch 用于日志

[CloudWatch](https://aws.amazon.com/cloudwatch/) 是显示 AWS Lambda 日志的默认解决方案。

> *CloudWatch 以日志、指标和事件的形式收集监控和运营数据，为您提供 AWS 资源、运行在 AWS 上的应用和服务以及本地服务器的统一视图。*
> 
> *— AWS 文档*

通俗地说，这是一个 AWS 服务，用于显示所有 AWS 服务的日志。我们想知道它是如何处理 AWS Lambda 日志的。当一个 Lambda 函数执行时，无论你向控制台写什么，Go 中的一个`fmt.printf()`或者 Node.js 中的一个`console.log()`，都会在后台异步发送到 CloudWatch。幸运的是，它不会增加函数执行时间的任何开销。

在函数运行时使用日志代理会增加执行开销，并增加不必要的延迟。我们希望避免这种情况，并在日志被添加到 CloudWatch 后对其进行处理。下面您可以看到从通用 *Hello World* 函数生成的日志事件示例。

![](img/4bca1fc4c3d0d9830d86d8527ae20a9d.png)

让我们退一步，看看更大的画面。每个函数都会在 CloudWatch 中创建一个名为**日志组**的东西。单击特定的日志组。

![](img/c3589039289afd20e4085fc73f599e2e.png)

这些日志组将包含**日志流**，这些日志流在字面上等同于来自特定功能实例的日志事件。

![](img/c2e3ebebedbc5c4c215d3b5ccecd2432.png)

对于系统洞察力和对你的软件正在做什么有一个适当的概述来说，这几乎不是一个足够好的解决方案。由于其结构，很难看到和区分日志。将日志放在一个中心位置更有意义。您可以使用自己的 Elasticsearch 或托管设置。Sematext 为您的基础设施的每个部分提供了全栈可观察性，并公开了一个 [Elasticsearch API](https://sematext.com/docs/logs/) 。让我向您展示创建 AWS Lambda 函数的 CloudWatch 日志处理并将其传输到 Sematext [日志应用程序](https://sematext.com/logsene/)是多么容易。

# 创建集中式日志记录解决方案

通过使用 CloudWatch 日志组订阅和 Kinesis，你可以将所有的 Lambda 日志集中到一个专门的函数，这个函数会将它们发送到 Sematext 的 Elasticsearch API。在那里，您有一个存放所有日志的中心位置。您可以搜索和过滤所有函数的日志，并轻松洞察函数的行为和健康状况。

我将演示**如何构建一个您可以自己使用的单命令部署解决方案**。它是用[无服务器框架](https://serverless.com/framework/)和 Node.js 构建的。但是，你可以随意使用 [AWS SAM](https://aws.amazon.com/serverless/sam/) 或 [Terraform](https://www.terraform.io/) ，以及任何你想要的编程语言。这个概念将保持不变。

这是最后的样子。

![](img/383eb852088c6ece1c6fcd9184d91f3f.png)

比 CloudWatch 漂亮多了，而且实际上可以找到你要找的东西！

# 设置无服务器项目

首先安装无服务器框架，配置您的 IAM 用户，并创建一个新项目。完整指南可在[这里](https://hackernoon.com/a-crash-course-on-serverless-with-node-js-632b37d58b44#422a)找到。

```
$ npm install -g serverless
$ sls config credentials \
    --provider aws \
    --key xxxxxxxxxxxxxx \
    --secret xxxxxxxxxxxxxx
$ sls create --template aws-nodejs --path lambda-cwlogs-to-logsene
$ cd lambda-cwlogs-to-logsene
$ npm init -y
$ npm i logsene-js zlib serverless-iam-roles-per-function
```

太棒了。现在转到 serverless.yml。

# 配置资源

在代码编辑器中打开**lambda-CW logs-to-logs ene**目录并检查 serverless.yml。可以随意删除所有内容并粘贴进来。

让我们一点一点地分解它。shipper 函数将由 Kinesis 流触发，它有一些用于配置 [Sematext 日志](https://sematext.com/logsene/)的环境变量。Kinesis 流本身在底部的资源部分定义，并通过使用其 ARN 在函数事件中引用。

继续讨论订户功能。它有三种触发方式。由你选择。如果您有许多现有的日志组，那么您可能希望点击 HTTP 端点来初始订阅它们。否则，让它每隔一段时间触发一次，或者只在创建新的日志组时触发，就可以了。

LogsKinesisStream 是我们订阅日志组的 Kinesis 流，CloudWatchLogsRole 是 IAM 角色，它允许 CloudWatch 将记录放入 Kinesis。

这样一来，您现在可以看到我们缺少了一个 secrets.json 文件。但是，在我们继续之前，跳到 [Sematext](https://sematext.com/) ， [log in](https://apps.sematext.com/ui/login) 并创建一个 [Logs App](https://apps.sematext.com/ui/logs) 。按下绿色小按钮来添加日志应用程序。

![](img/0c121168907421741045f767b915e62c.png)

添加应用程序的名称和一些基本信息后，您会看到一个*等待数据*屏幕弹出。按下**集成指南**并复制您的令牌。

![](img/6952f59ffed208fc9f19bdb11abbd9c8.png)

现在您可以将令牌粘贴到`secrets.json`文件中。

```
{
  "LOGS_TOKEN": "your-token",
  "REGION": "us-east-1",
  "BATCH_SIZE": 1000,
  "LOG_GROUP_RETENTION_IN_DAYS": 1,
  "KINESIS_RETENTION_IN_HOURS": 24,
  "KINESIS_SHARD_COUNT": 1
}
```

# 添加订户功能

我喜欢说 Kinesis 是卡夫卡的简化版。它基本上是一个管道。您订阅要发送到其中的数据，并告诉它一旦满足某个批量大小，就触发一个 Lambda 函数作为事件。

拥有订阅者功能的目的是为所有日志组订阅一个 Kinesis 流。理想情况下，它们应该在创建时就被订阅，当然，最初当你想订阅所有现有的日志组到一个新的 Kinesis 流时。作为备用方案，我也喜欢在想要手动触发订阅者时使用 HTTP 端点。

在代码编辑器中，创建一个新文件，并将其命名为`subscriber.js`。将此片段粘贴到。

检查一下`processAll()`功能。它将从 CloudWatch 获取所有匹配前缀的**日志组**，并将它们放入一个易于访问的数组中。然后你将把它们传递给一个`subscribeAll()`函数，该函数将通过它们进行映射，同时将它们订阅到你在`serverless.yml`中定义的 Kinesis 流。

另一件很酷的事情是将保留策略设置为 7 天。你很少需要比这更多的东西，这将减少在你的 AWS 账户中保存日志的成本。

请记住，您还可以编辑获取日志的`filterPattern`。现在，我选择保持空白，不过滤任何东西。但是，根据你的需要，你可以根据你选择的记录器创建的模式来匹配它。

太好了，做完这些，让我们继续运送一些原木吧！

# 添加发货人功能

在 Kinesis 流从 CloudWatch 接收到日志后，它将触发一个 Lambda 函数，专门将日志发送到 Elasticsearch 端点。对于这个例子，我们将使用 [LogseneJS](https://github.com/sematext/logsene-js) 作为日志发送者。如果你分解它，它是相当简单的。一批记录将在事件参数中发送给 shipper 函数。您解析日志，给它们您想要的结构，并将它们发送给 Sematext。这是它的样子。创建一个新文件，将其命名为 shipper.js，并粘贴以下代码。

shipper Lambda 的核心在于`parseLogs()`和`shipLogs()`函数。前者将接受事件参数，提取所有日志事件，解析它们，将它们添加到一个数组中，并返回该数组。而后者将采用相同的日志数组，将每个日志事件添加到 [LogseneJS](https://github.com/sematext/logsene-js) 缓冲区，并一次性发送它们。该位置是您在上面创建的日志应用程序。

您还记得本文开头的图片吗？在该图片中，您看到了典型函数调用的日志事件。您可以看到它生成了 4 种不同类型的日志事件。

```
START RequestId 
... 
END RequestId 
REPORT RequestId
```

它们可以以这三种模式中的任何一种开始，其中省略号表示在函数运行时(Node.js 中的`console.log()`)中输出到 stdout 的任何类型的字符串。

`parseLog()`函数将完全跳过开始、结束和报告日志事件，并且根据它们是用户定义的标准输出还是函数运行时、配置或持续时间中的任何类型的错误，只返回用户定义的日志事件作为调试或错误。

默认情况下，日志消息本身是结构化的，但并不总是如此。默认情况下，在 Node.js 运行时，它的结构如下所示。

shipper 中的代码被配置为与上面的结构一起工作，或者与只有消息部分的结构一起工作。如果您正在使用另一个运行时，我建议您使用结构化日志来为您的日志事件创建一个公共结构。

编码部分完成后，您就可以部署和测试您的定制日志传送程序了。

# 部署和测试您的集中式日志记录解决方案

使用像无服务器框架这样的基础设施作为代码解决方案的好处在于部署是多么简单。你可以用一个命令把所有东西都推到云端。跳回到您的终端，在您的项目运行目录中:

```
$ sls deploy
```

您将看到输出被打印到控制台。

```
[output]
Serverless: Packaging service...
Serverless: Excluding development dependencies...
Serverless: Uploading CloudFormation file to S3...
Serverless: Uploading artifacts...
Serverless: Uploading service .zip file to S3 (2.15 MB)...
Serverless: Validating template...
Serverless: Updating Stack...
Serverless: Checking Stack update progress...
............
Serverless: Stack update finished...
Service Information
service: lambda-cwlogs-to-logsene
stage: dev
region: us-east-1
stack: lambda-cwlogs-to-logsene-dev
api keys:
  None
endpoints:
  GET - https://.execute-api.us-east-1.amazonaws.com/dev/subscribe
functions:
  shipper: lambda-cwlogs-to-logsene-dev-shipper
  subscriber: lambda-cwlogs-to-logsene-dev-subscriber
layers:
  None
Serverless: Removing old service artifacts from S3…
```

就是这样。现在，您已经设置好将所有日志从 Lambda 函数发送到 [Sematext Cloud](https://sematext.com/cloud/) 中。确保触发订阅功能，将日志组订阅到 Kinesis 流。触发订阅者后，您将看到订阅者在 Sematext 中生成的日志，您可以放心它是有效的。

![](img/383eb852088c6ece1c6fcd9184d91f3f.png)

上面你可以看到我是如何添加严重性过滤的。您可以轻松选择要过滤的值，这为您提供了一种跟踪错误、超时和调试日志的简单方法。

# 成本呢？

在你的 AWS 账户中有这样一个设置的成本是相当便宜的。单个 shard Kinesis 流的固定成本大约为[14 美元/月，加上流数据量的额外成本](https://aws.amazon.com/kinesis/data-streams/pricing/)。单个 shard 的接收能力为 1MB/秒或 1000 条记录/秒，这对大多数用户来说是合适的。

Kinesis 的花费被分成**碎片时间**和**放置 25KB 大小的有效载荷单位**。一个碎片每天花费 0.36 美元，而一百万个 PUT 有效载荷单元花费 0.014 美元。假设，如果你有一个碎片和每秒 100 个 PUT 有效载荷单位，那么在 30 天的时间里，你最终会花费**10.8 美元的碎片和 3.6288 美元的有效载荷单位。**

Lambda 函数被配置为使用尽可能少的内存，即 128MB，这意味着在适度使用的情况下，开销通常会停留在空闲层。那是你最不担心的。

# 包扎

拥有一个存放日志的中心位置至关重要。尽管 CloudWatch 以其自身的方式很有用，但它缺乏全局观念。通过使用一个中心位置，您不需要切换上下文来调试不同类型的应用程序。Sematext 可以监控你的整个软件栈。让你的 [Kubernetes](https://sematext.com/kubernetes/) 日志、[容器](https://sematext.com/docker/)日志和 Lambda 日志放在 [Sematext 日志](https://sematext.com/logsene/)中是一个很大的好处，在那里你可以很容易地跟踪一切。

如果你需要再次检查代码，[这是回购](https://github.com/sematext/cloudwatch-sematext-aws-lambda-log-shipper/tree/develop)，如果你想让更多人在 GitHub 上看到它，给它一颗星。您还可以克隆 repo 并立即部署它。不要忘记先添加你的日志应用令牌。

如果你需要一个软件栈的可观察性解决方案，请查看 [Sematext](https://sematext.com/) 。我们正在推动[开源我们的产品](https://github.com/sematext)并产生影响。

*希望你们喜欢读这篇文章，就像我喜欢写这篇文章一样。如果你喜欢，点击那个小小的分享按钮，那么更多的人会看到这个教程。下次见，保持好奇，玩得开心。*

*原载于 2019 年 3 月 15 日*[*sematext.com*](https://sematext.com/blog/centralized-aws-lambda-logs-kinesis-serverless/)*。*