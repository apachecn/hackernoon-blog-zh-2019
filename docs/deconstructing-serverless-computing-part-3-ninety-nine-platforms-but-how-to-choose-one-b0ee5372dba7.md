# 解构无服务器计算第 3 部分:九十九个平台，但如何选择一个？

> 原文：<https://medium.com/hackernoon/deconstructing-serverless-computing-part-3-ninety-nine-platforms-but-how-to-choose-one-b0ee5372dba7>

## ***“计算机科学中的所有问题都可以通过另一个层次的间接方式来解决。”—大卫·惠勒***

本系列的第一部分已经让您领略了什么是[无服务器计算](https://hackernoon.com/deconstructing-serverless-computing-part-1-a-new-layer-of-abstraction-98c15334e4f7)和[它支持的用例](https://hackernoon.com/deconstructing-serverless-computing-part-2-the-good-the-bad-and-the-time-to-market-91a2b52ceaf4)，以及它的优点和缺点。有了这些知识，您可能会计划在尝试将所有应用程序迁移到这个新的体系结构之前尝试一下无服务器。当你开始在谷歌上搜索时，你会很快意识到有几十个 FaaS 平台，每个都有独特的功能。当困惑出现在你的脑海中，你开始厌倦 FaaS 供应商的吹嘘，一个问题出现在你的脑海中:

***你该选哪个？***

在本帖中，我们将浏览 FaaS 产品错综复杂的生态系统，包括托管和开源产品，并确定它们的主要功能和关键的差异化特征。目标是能够根据您的需求和用例做出明智的选择。

![](img/8a387e52578b055e183dbdfdb8c9a4d5.png)

# 托管 FaaS 平台

如果您正在寻找完整的无服务器体验，托管平台是最佳选择。所有操作问题都由提供商处理，托管平台通常更稳定，同时提供比开源平台更好的性能。它们与云提供商的生态系统相集成，并提供现成的细粒度安全性、合规性和可用性 SLA。然而，这也是有代价的:利用所有这些特性会很快让你被供应商锁定，所以要仔细计划。

# 自动气象站λ

*描述:* AWS Lambda 是第一个 FaaS 平台，于 2014 年推出。根据 CNCF 的调查，它也是最受欢迎的，拥有 [70%的市场份额。这是有充分理由的，因为它被认为是最稳定的平台，并且被证明是最具可扩展性的平台之一，具有最低的冷启动延迟。Lambda 受益于成为优秀的亚马逊网络服务生态系统的一部分，并与许多 AWS 托管服务很好地集成，这些服务可以作为事件源(S3、Kinesis、DynamoDB、Alexa 等)。).Lambda 可以通过 Lambda@Edge 服务部署在边缘(例如，与亚马逊的 CDN 产品 CloudFront 集成，并帮助尽可能接近客户端地服务请求)，工作流可以使用 AWS Step 函数创建。](https://www.cncf.io/blog/2018/08/29/cncf-survey-use-of-cloud-native-technologies-in-production-has-grown-over-200-percent/)

*一般特征:*

*   事件来源:亚马逊 S3、亚马逊 DynamoDB、亚马逊 Kinesis 数据流、亚马逊简单通知服务、亚马逊简单电子邮件服务、亚马逊简单队列服务、亚马逊 Cognito、AWS CloudFormation、亚马逊 CloudWatch 日志、亚马逊 CloudWatch 事件、AWS CodeCommit、预定事件(由亚马逊 CloudWatch 事件提供支持)、AWS Config、亚马逊 Alexa、亚马逊 Lex、亚马逊 API 网关、亚马逊物联网按钮、亚马逊 CloudFront、亚马逊 Kinesis 数据消防软管、按需调用(通过 HTTP)
*   支持的语言:节点。JS、Python、Java、C#、Go、通过 Lambda 层的自定义运行时
*   价格:每一百万次调用 0.20 美元，每秒 0.00001667 美元
*   功能的内存可以 64MB 的增量从 128MB 调整到 3GBCPU 自动随内存扩展
*   功能超时:900 秒
*   临时磁盘空间:512MB
*   每个区域的最大并发调用数(可以根据请求增加):1000

*区分特征:*

*   用[λ@ Edge](https://docs.aws.amazon.com/lambda/latest/dg/lambda-edge.html)在边缘执行
*   通过 [AWS 步骤功能](https://aws.amazon.com/step-functions/)的工作流程
*   可以在装有 [AWS Greengrass](https://aws.amazon.com/greengrass/) 的物联网设备上运行
*   可以使用 [AWS SAM CLI](https://github.com/awslabs/aws-sam-cli) 在本地运行和测试
*   用 [Lambda 层](https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html)打包公共库和定制运行时
*   [可用性 SLA](https://aws.amazon.com/lambda/sla/) :每月正常运行时间百分比为 99.95%

# Azure 函数

描述: Azure Functions 是微软对 FaaS 的接管，也是市场上第二受欢迎的平台。它于 2016 年推出，是一款开源产品，并很好地集成在 Azure 云生态系统中。使用标准计划时，它的扩展能力弱于 AWS Lambda，正如像[这个](http://pages.cs.wisc.edu/~swift/papers/atc18-serverless.pdf)和[这个](https://www.azurefromthetrenches.com/azure-functions-vs-aws-lambda-scaling-face-off/)这样的研究所证明的那样。然而，Azure Functions V2.0 在近一年前成为[普遍可用的](https://azure.microsoft.com/en-us/blog/introducing-azure-functions-2-0/)，并承诺改善性能、稳定性和开发能力。新平台可以在任何地方运行，包括 Kubernetes 或本地 Mac 和 Linux 环境，具有与 Visual Studio 和 VS 代码的原生集成，并可以在物联网设备上运行。Azure Functions 根据您使用的计划提供不同的功能。消费是默认的 FaaS 计划，下面列出的功能是该计划特有的。如果您愿意支付更多费用，Premium 提供了无限制的函数超时(默认为 30 分钟)，更多的 CPU 和 RAM，以及一种使用预热的工作程序来减轻冷启动的方法(请注意，这些工作程序全天候运行，即使不使用它们，您也将为此付费！).最后，您还可以使用应用服务计划在传统虚拟机中运行功能。

*通用特性:*

*   事件源:Azure Cosmos DB、Azure Event Hubs、Azure Event Grid、Azure Notification Hubs、Azure Service Bus(队列和主题)、Azure Storage (blob、队列和表)、On-premises(使用服务总线)、Twilio (SMS 消息)
*   支持的语言:C#、F#、JavaScript、TypeScript、Java，预览版中有 Python 和 PowerShell
*   价格:每一百万次调用 0.20 美元，每秒 0.000016 美元
*   功能内存固定为 1.5GB
*   功能超时:300 秒(默认值，最长可增加到 600 秒)
*   临时磁盘空间:500MB
*   每个函数应用程序的最大并发调用数(固定):200，对于 HTTP 事件，每 1s 可以创建一个新的函数实例，对于非 HTTP 事件，每 30s 可以创建一个新的函数实例

*区分特征:*

*   开源，可以在内部运行(也与 [Azure 堆栈](https://azure.microsoft.com/en-us/overview/azure-stack/)集成)
*   通过 [Azure 持久功能的工作流](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-overview)
*   可以在边缘和物联网设备上运行 [Azure IoT Edge](https://azure.microsoft.com/en-us/services/iot-edge/)
*   可以在本地运行和测试
*   [可用性 SLA](https://azure.microsoft.com/en-us/support/legal/sla/functions/v1_1/) :每月正常运行时间百分比为 99.95%

# 谷歌云功能

*描述:*谷歌云功能是谷歌的一个 FaaS 平台，大约在一年前[正式推出](https://cloud.google.com/blog/products/gcp/cloud-functions-serverless-platform-is-generally-available)。它在功能上与 AWS Lambda 和 Azure 函数非常相似；在性能方面，它似乎介于两者之间，优于 Azure 函数，但优于 Lambda。过去，一些[研究表明](http://pages.cs.wisc.edu/~swift/papers/atc18-serverless.pdf)谷歌云功能有时无法以期望的速度扩展，即使是低并发水平。总的来说，它是一个更简单的实现，具有强大的关键功能核心，但语言支持有限，并且缺少其他托管平台中可用的一些更复杂的功能，如对计划功能(cron 作业)的本机支持。

*一般特征:*

*   事件源:HTTP —通过 HTTP 请求、云存储、云发布/订阅、
    Firebase(数据库、存储、分析、验证)、堆栈驱动程序日志记录直接调用函数
*   支持的语言:节点。JS，Python，Go
*   价格:每一百万次调用 0.40 美元，每秒 0.0000025 美元，每秒 0.0000100 美元
*   功能的内存可以从 128MB 到 2GB 的增量进行调整；CPU 自动随内存扩展
*   功能超时:540 秒
*   临时磁盘空间:是的，消耗内存的 tmpfs 卷
*   每个后台函数的最大并发调用数(对 HTTP 触发的函数没有限制):1000

*区分特征:*

*   可以在本地运行和测试
*   [可用性 SLA](https://cloud.google.com/functions/sla) :每月正常运行时间百分比为 99.5%
*   与 [Firebase](https://firebase.google.com/) 集成

# IBM 云功能

*描述:* IBM Cloud Functions 是 IBM 的 FaaS 平台，利用了他们在 2016 年捐赠给阿帕奇基金会的开源 [OpenWhisk](https://openwhisk.apache.org/) 。作为一个托管平台，它不像 Lambda、Azure Functions 和 Google Cloud Functions 那样受欢迎，但它是一个具有开源根基的可靠平台。

*一般特征:*

*   事件源:HTTP —通过 HTTP 请求、Github、Cloudant、IBM Message Hub、Mobile Push、Slack、Watson、Cloudant、Weather、Websockets、任何实现开放事件提供者接口的事件源直接调用函数
*   支持的语言:NodeJS，Swift，Java，Go，PHP，Python，任何语言(通过 Docker 容器)
*   价格:0.000017 美元
*   功能的内存可以从 128MB 到 2GB 的增量进行调整；CPU 自动随内存扩展
*   功能超时:600 秒
*   每个函数的最大并发调用数(可以根据请求增加):1000

*区分特征:*

*   使用与可在内部部署的开源版本相同的代码库
*   可以在本地运行和测试
*   [可用性 SLA](https://www-03.ibm.com/software/sla/sladb.nsf/pdf/6605-13/$file/i126-6605-13_06-2018_en_US.pdf) :每月正常运行时间百分比为 99.95%
*   通过 Docker 容器支持任何编程语言
*   开放事件提供者接口，允许任何实现该接口的服务用作事件源
*   使用[作曲](https://github.com/ibm-functions/composer)构建工作流程

# 开源 FaaS 平台

如果你需要对你的 FaaS 平台有更多的控制，开源是一个不错的选择。您可以访问代码库，并可以轻松地在内部或公共云中部署您选择的平台。开源 FaaS 平台通常不像它们的托管平台那样完美，但是它们更容易扩展和适应你的需求。需要记住的一件重要的事情是，一般来说，你将负责平台的操作方面，否定无服务器体验的一些好处(许多开源提供商确实提供托管计划，例如 [Platform9](https://platform9.com/) 提供托管裂变，而 [Iguazio](https://www.iguazio.com/) 提供托管核)。

# 分裂

*描述:*裂变是一个开源的 FaaS 平台，原生运行在 Kubernetes 上。它是用 Go 编写的，是开源 FaaS 社区中的一个流行项目。裂变利用 Kubernetes 的大部分内部功能，但也添加了自己的逻辑来实现更好的性能(例如，为快速冷启动保留一个温暖的容器池)。根据 CPU 使用情况实施自动扩展，并计划在未来添加自定义指标，Prometheus 指标会自动公开以帮助监控。

*一般特征:*

*   事件源:HTTP、时间触发器、NATS、Azure 存储队列、Webhooks(例如 Github、Slack)、Kubernetes 事件处理程序
*   支持的语言:Python，NodeJS，Go，C#，PHP，可以扩展到任何语言

*区分特征:*

*   使用[裂变工作流](https://github.com/fission/fission-workflows)支持工作流
*   快速冷启动使用暖容器池
*   测试环境中函数的实时重载
*   可以重放函数调用
*   全自动金丝雀部署

# OpenWhisk

*描述:* OpenWhisk 是一个流行的开源 FaaS 项目，由 IBM 捐赠给 Apache 基金会。它是最稳定的开源 FaaS 平台之一，这是由于 IBM 在他们的云上以及 Red Hat 在 Openshift 上的广泛生产使用。IBM Cloud Functions 基于相同的代码库，因此大多数特性都是相同的，除了与 IBM Cloud managed services 的集成(例如日志和监控)。然而，OpenWhisk 可以利用其他开源项目，如用于定义工作流的 [Composer](https://github.com/ibm-functions/composer) 和作为轻量级开发环境的 [Shell](https://github.com/ibm-functions/shell) 。

*通用特性:*

*   事件源:HTTP —通过 HTTP 请求、Github、Cloudant、IBM Message Hub、Mobile Push、Slack、Watson、Cloudant、Weather、Websockets、任何实现开放事件提供者接口的事件源直接调用函数
*   支持的语言:NodeJS，Swift，Java，Go，PHP，Python，任何语言(通过 Docker 容器)

*区分特征:*

*   通过 Docker 容器支持任何编程语言
*   开放事件提供者接口，允许任何实现该接口的服务用作事件源
*   使用 [Composer](https://github.com/ibm-functions/composer) 构建工作流程
*   通过[外壳](https://github.com/ibm-functions/shell)开发工具

# 一无所有

*描述:* Kubeless 是一个使用 CRDs(自定义资源定义)在 Kubernetes 上原生构建的开源 FaaS 平台。Kubeless 是 100% Kubernetes 原生的，它的所有内部功能(自动缩放、API 路由、监控等。)是通过 Kubernetes 启用的；在 Knative 发布之前，它是最受欢迎的基于 Kubernetes 的 FaaS 平台。自动缩放通过 Kubernetes 的 HPA(水平 Pod 自动缩放器)完成，默认情况下启用 Prometheus 监控。

*一般特征:*

*   事件来源:HTTP，CronJob，Kafka，NATS，可以通过 CRDs 扩展
*   支持的语言:Python，Node.js，Ruby，PHP，Golang，。网络，芭蕾舞演员和自定义运行时间

*区分特征:*

*   符合 CLI，符合 AWS Lambda

# 努克利奥

*描述:* Nuclio 是一个开源的 FaaS 平台，目标是实时和数据驱动的应用。它声称拥有最快的平台，达到每秒 400，000 次调用。它提供了与事件和数据源的各种集成，并支持 Prometheus 进行监控，以及 Azure Application Insights 进行度量和日志记录。Nuclio 是第一批开始专业化的开源 FaaS 平台之一，重点关注数据科学。它是第一个支持 GPU 处理的 FaaS 平台，在提供商管理的版本中，它提供了与机器学习工具的紧密集成，如 Spark、TensorFlow 和 Jupyter 笔记本电脑。

*一般特征:*

*   事件来源:HTTP、NATS、卡夫卡、Kinesis、RabbitMQ、Iguazio v3io、Azure Event Hub、cron(本地调用)
*   支持的语言:Go，Python，。NET core，PyPy，Shell(通过 exec 调用二进制或脚本)，V8 (JavaScript 和 Node。JS)、Java

*区分特征:*

*   可以部署在内部、多云端或低功耗设备上
*   专为低延迟应用设计(高性能)
*   支持 GPU 处理
*   与机器学习工具紧密集成，如 Spark、TensorFlow 和 Jupyter 笔记本电脑

# Knative

Knative 是谷歌与 IBM、Pivotal、SAP 和 RedHat 等合作伙伴合作建立的项目。它是 Kubernetes 之上的一组构建块，作为容器编排器，Istio 作为服务网格。Knative 可以为定制 FaaS 平台的后端提供动力，也可以独立用于自动化操作。有三个松散耦合的组件:构建(云原生源到容器构建编排)、事件(事件的交付和管理)和服务(扩展到零，请求驱动的计算模型)。Knative 受益于人气飙升(部分原因是得到了谷歌的支持)。其他一些开源的 FaaS 平台现在由 Knative 支持，比如 Pivotal 的 [Riff](https://projectriff.io/) 。其他的可以和 Knative 互操作，比如 [OpenFaaS](https://github.com/openfaas/faas) 。

*区分特征:*

*   Kubernetes 之上的高级抽象提供了类似 FaaS 的功能
*   可以一起使用或独立使用的松散耦合组件的集合
*   平台无关的、多云端、内部部署

# 其他人

还有许多其他 FaaS 平台，每一个都有自己的卖点: [Fn 项目](https://fnproject.io/)(开源，来自甲骨文) [OpenFaaS](https://github.com/openfaas/faas) ， [OpenLambd](https://github.com/open-lambda/open-lambda) a(学术，主要用于实验目的) [Galactic Fog](http://www.galacticfog.com/) ， [Riff](https://projectriff.io/) (构建于 Knative 之上) [Twilio 函数](https://www.twilio.com/functions)， [Spotinst 函数](https://spotinst.com/products/spotinst-functions/)等等。最近，微软宣布了 KEDA，他们的开源替代服务。还有与平台无关的无服务器框架，其中最流行的是[无服务器框架](https://serverless.com/)。如果试图涵盖所有这些问题，这篇文章将会一直延伸下去，但是我希望这篇文章与每个人都相关，并提供了一个生态系统的体面样本。

# 结论

无服务器生态系统正在蓬勃发展，有数十个可用的 FaaS 平台和各种可与之集成的托管服务。现在还为时尚早，随着市场的成熟，这些项目中的许多将被过滤掉，但你可以放心，不管你的需求是什么，总有一个 FaaS 平台可以满足它们。无论您是想要一个能让您更快上市的托管平台和完全托管的基础架构，还是想要一个您能完全控制的开源平台，您都可以做出选择。我希望这篇文章能帮助你更好地了解现有的内容。

在下一篇文章中，我们将从开发人员的角度来看无服务器；我们将讨论这种新范式所需的视角变化，并研究有哪些工具可用。