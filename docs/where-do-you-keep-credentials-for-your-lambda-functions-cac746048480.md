# 你把 Lambda 函数的凭证放在哪里？

> 原文：<https://medium.com/hackernoon/where-do-you-keep-credentials-for-your-lambda-functions-cac746048480>

如果你的 Lambda 函数必须访问一个数据库(或者任何其他需要凭证的服务),你在哪里以及如何存储那个配置？

最近，我们一直在迭代我们的 MVP，我们的应用程序的需求和规模有所增长，我们一直在讨论如何安全地处理不同环境/阶段和相关用户/密码的数据库配置。

有很多很多可能性，让我们来看看其中的一些:

# 只需将主机、用户和密码硬编码在您的文件中。

![](img/a330c744324272cac25c956381663b31.png)

请不要。我真的应该告诉你为什么吗？

# 使用提交给 repo 的. env 文件

![](img/af4fcf3ac0742311047f51aa2c913801.png)

尽管这种解决方案可能允许更多的灵活性，但它仍然非常糟糕。每个可以访问您的回购协议的人都可以立即看到您的凭据。

# 使用. secrets 文件(基本上是。上面的 env 文件，但是通过[无服务器秘密插件](https://github.com/serverless/serverless-secrets-plugin)加密

![](img/ac0fe90545c87bae3fe66fbdef9acab2.png)

这是我们的第一个快速方法，但事实证明效果并不好，因为:

*   一旦 lambda 函数被部署，凭证在 AWS UI 控制台中清晰可见(env 变量在部署时被*嵌入到代码中)*
*   有人错误提交解密文件的风险很高
*   我们不得不在许多共享类似凭证的回购中复制这些文件
*   最重要的是，问题出现了——**我们把解密这些秘密的密码存放在哪里**？

```
plugins: - serverless-secrets-plugin custom: secrets: ${file(secrets.${self:provider.stage}.yml)}
```

# 在你的 serverless.yml 中使用 SSM 加密的 [env 变量](https://serverless.com/framework/docs/providers/aws/guide/variables#reference-variables-using-aws-secrets-manager)

![](img/29cab4cb09812d4414ad09f3d24fc468.png)

这是 secrets-plugin 的进一步发展， [AWS Systems Manager 参数存储](https://docs.aws.amazon.com/systems-manager/latest/userguide/systems-manager-parameter-store.html)允许您删除文件，并且只有一个配置由许多 lambda/repos 共享，可以通过 AWS UI 控制台或 AWS CLI 快速更新，但它也有同样的缺点:

*   配置值以明文形式存储为 Lambda 环境变量——您可以在 AWS Lambda 控制台中清楚地看到它们——如果该功能被攻击者破坏(攻击者可以访问 process.env ),那么他们也可以很容易地找到解密后的值
*   因为您将代码与 env 变量一起部署，所以如果您需要更改配置，您需要重新部署每个 lambda 来传播所有更改。

```
custom: supersecret: ${ssm:/aws/reference/secretsmanager/secret_ID_in_Secrets_Manager~true}
```

# 在运行时访问 SSM 或 SecretsManager(并使用缓存)

![](img/06f69bbac78c6d03c76bf8e03d8e724c.png)

在系统管理器参数存储或[机密管理器](https://aws.amazon.com/secrets-manager/)(也允许自动轮换)上安全加密存储您的凭证，并在运行时访问它们。
然后配置您的无服务器 yaml，通过 IAMRole 策略授予对 lambda 的访问权限:

```
iamRoleStatements: - Effect: Allow Action: - ssm:GetParameter Resource:"arn:aws:ssm:YOUR_REGION:YOUR_ACCOUNT_ID:parameter/YOUR_PARAMETER"
```

您可以设置粒度越来越细的权限

```
"arn:aws:ssm:*:*:parameter/*" "arn:aws:ssm:YOUR_REGION:YOUR_ACCOUNT_ID:parameter/*" "arn:aws:ssm:YOUR_REGION:YOUR_ACCOUNT_ID:parameter/YOUR_PARAMETER-*" "arn:aws:ssm:YOUR_REGION:YOUR_ACCOUNT_ID:parameter/YOUR_PARAMETER-SOME_MORE_SPECIFIC"
```

上面的代码直接指定了您的 ARN /地区/帐户——如果您想更加灵活，您可以设置自动获取这些值的权限:

```
iamRoleStatements: - Effect: Allow Action: - ssm:GetParameter Resource: - Fn::Join: - ':' - - arn:aws:ssm - Ref: AWS::Region - Ref: AWS::AccountId - parameter/YOUR_PARAMETER-*
```

由于 SecretsManager 与 ParameterStore 集成，您可以通过 SSM 访问您的秘密，只需在您的密钥前加上`aws/reference/secretsmanager/`

如果您开始使用这些权限(尤其是在 UI 控制台中编辑策略——而不是重新部署 lambda——可能需要一些时间。通常以秒为单位，但也可能是 2-5 分钟)

一旦你授予你的 lambda 访问你的秘密的权限，你就可以指定一个环境变量来告诉你的 lambda 在运行时基于环境/阶段加载哪些凭证:

```
custom: credentialsKey: production: YOUR-PRODUCTION-CREDENTIALS-KEY development: YOUR-DEV-CREDENTIALS-KEY other: YOUR-OTHER-CREDENTIALS-KEY functions: environment: SECRETS_KEY:${self:custom.credentialsKey}
```

这是一个将某种条件应用于无服务器部署的巧妙小技巧。基本上，你是在告诉 serverless 你有三个密匙:一个用于生产，一个用于开发，一个用于所有其他阶段。
在 lambda 函数的 environment 节点中，根据当前部署的阶段设置键。如果当前阶段匹配列表中的一个变量名，它将被选中，否则，它将退回到另一个变量。

在您的 lambda 中，您只需从 SSM 或 SecretsManager 加载凭证并连接到您的数据库。

```
const ssm = new AWS.SSM(); const params = { Name: process.env.SECRETS_KEY, WithDecryption: true }; ssm.getParameter(params, function(err, data) { if (err) console.log(err, err.stack); // an error occurred else console.log(data.Parameter.Value); // here you have your values! });
```

> *记得实现某种缓存，这样当 lambda 容器被重用时，你就可以避免从 AWS 加载密钥(并导致额外的成本)*

我想指出的是 **SSM 要求在实例化**时定义 aws 区域。如你所见，我没有传递那个值。这是因为`process.env.AWS_REGION`是从 AWS SDK 自动读取的，env 变量是由无服务器脱机设置的。

您不需要做任何事情，直到您有一些集成测试试图加载秘密——我们添加了一些测试来确保在每次部署之后，该 env-stage 的秘密在 SecretsManager 上是可用的。在这种情况下，您必须将该变量传递给集成测试(记住手动将其传递给集成测试)。

这是我们的 npm 脚本(我们使用 [AVA](https://github.com/avajs/ava) 进行测试，使用[伊斯坦布尔/纽约](https://github.com/istanbuljs/nyc)进行代码覆盖):

```
"test:integration": "AWS_REGION=eu-west-1 SECRETS_KEY=MY_KEY_DEVSTAGE nyc ava tests-integration/**/*.*"
```

你有其他方法来处理这个共同的——我认为是基本的/根本的——特征吗？

更多相关资源:
[https://docs . AWS . Amazon . com/en _ us/systems-manager/latest/user guide/integration-PS-secrets manager . html](https://docs.aws.amazon.com/en_us/systems-manager/latest/userguide/integration-ps-secretsmanager.html)
[https://server less . com/framework/docs/providers/AWS/guide/variables/# reference-variables-using-AWS-secrets-manager](https://serverless.com/framework/docs/providers/aws/guide/variables/#reference-variables-using-aws-secrets-manager)

*原发布于*[*https://dev . to*](https://dev.to/dvddpl/where-do-you-keep-credentials-for-your-lambda-functions-5dno)*。*