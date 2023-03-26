# 无服务器功能的更好的本地开发

> 原文：<https://medium.com/hackernoon/better-local-development-for-serverless-functions-b96b5a4cfa8f>

![](img/75f002e2d4bd7f441e978da8349a0692.png)

Lambda 是一个很棒的套件，包含了 AWS 产品页面上列出的所有好处，T2 是一个非常有用的开发 Lambda 函数的框架。然而，如果您要解决的问题并非完全无关紧要，那么在本地开发无服务器应用程序是一件非常痛苦的事情。

当事情变得复杂，你的 Lambda 函数开始与其他 AWS 服务集成时，事情就真的开始出问题了。有一些东西看起来像银弹，我会在这里分享它们，并解释为什么它们对我不起作用，然后给你一个我自己努力找到的工作示例(因此我写了这篇文章)。

# 本地堆栈

[Localstack](https://github.com/localstack/localstack) 绝对是这里最大的银弹尝试。基本上，它在本地模拟了一大块 AWS，你可以将你的无服务器功能连接到其中。我没有使用它的原因:

1.  比特工作，但如果你有任何类型的半复杂的设置(如美国东部 1 或欧盟西部 1 以外的地区)，它会崩溃。上传一个完整的 cloudformation 模板是一场服务间链接断裂的灾难。我开始编辑 localstack 代码，试图解决我遇到的一个又一个问题，然后放弃了。如果产品不试图做一些超级复杂的事情，它会很酷。
2.  这也是开发人员机器上的巨大资源消耗。
3.  从根本上说，你不应该为了测试你的代码而去模拟一半的 AWS。

# 无服务器脱机

无服务器脱机相当简单。它应该允许您在本地运行您的函数，并在需要时调用它们。对我来说，这需要你的 lambda 函数有 HTTP 端点，我们所有的 Lambda 函数都是由 SQS 事件或 Cron 驱动的。我们还在代码中实例化了 SNS 主题，因此它会不断尝试创建 SNS 主题，AWS 会抛出错误。

我也尝试过使用 [serverless-offline-sqs](https://www.npmjs.com/package/serverless-offline-sqs) ，但是无法将事件驱动到我们的 Lamdba 函数中。

我花了很多时间来设置开发环境，我意识到这根本就是错误的方法。我需要开始编写适当的单元测试，并使用 mocking 来模拟基础设施(我的意思是，理想情况下，它们应该被适当地隔离，基础设施/功能应该彼此完全不了解，但那是另一天的事了)。

# 这个例子

假设我们正在开发一个应用程序，它利用了:

*   希腊字母的第 11 个
*   MongoDB
*   SQS
*   社交网站（Social Network Site 的缩写）

你的 lambda 函数连接到你的 MongoDB，做一件事，偶尔向其他服务发送 SNS 通知/SQS 消息。

**λ**

对于正在运行的实际代码，我将使用 [jest](https://www.npmjs.com/package/jest) 开始编写测试。Jest 工作得很好，并且很好地集成到无服务器中。

**MongoDB**

为了模拟 MongoDB，我将使用一个[内存版本的 MongoDB](https://www.npmjs.com/package/mongodb-memory-server) ,它很好地结合到我们的 Mongoose 模型中。

**SQS/SNS**

对于任何亚马逊基础设施，我们将使用 [aws-sdk-mock](https://www.npmjs.com/package/aws-sdk-mock) 。这是一个模仿 AWS 基础设施的极好的包装器，对单元测试非常有用，稍后你会看到。

# 把所有东西绑在一起

所以首先让我们安装我们的依赖项。

```
npm install aws-sdk-mock mongodb-memory-server sinon jest --save-dev
```

让我们也创建一个 tests 文件夹来保存所有的测试代码。

```
mkdir tests
```

**设置 MongoDB**

现在，为了让我们的代码与内存中的 Mongo-DB 服务器集成，我们需要添加一些安装/拆卸函数和一个 Mongo 环境。克隆这个[回购](https://github.com/vladgolubev/jest-mongodb)并放入:

*   `setup.js`
*   `teardown.js`
*   `mongo-environment.js`

到新的测试文件夹中。

在您的根目录中添加一个包含此内容的`jest.config.js`。

```
module.exports = {
  globalSetup: './tests/setup.js',
  globalTeardown: './tests/teardown.js',
  testEnvironment: './tests/mongo-environment.js',
  roots: ['./tests/'],
};
```

这应该足以启动并运行 MongoDB。现在进入我们实际的测试脚本。

**设置我们的测试**

现在，最后，您可以用下面的代码片段将它们联系起来:

将 jest 添加到您的包中。json

```
"scripts":{   
  "test": "jest"
},
```

和`npm test`一起跑。

需要注意的一个要点是，aws-mock-sdk 需要一种非常特殊的方式来实例化 aws 资源(在功能测试的范围内)，因此如果您在 AWS 区域周围看到任何错误，请仔细检查您是否遵循了规则。