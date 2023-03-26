# 如何使用 Lambda 向 DynamoDB 添加新的认知用户

> 原文：<https://medium.com/hackernoon/how-to-add-new-cognito-users-to-dynamodb-using-lambda-e3f55541297c>

![](img/21e0a39bf61250fd97eee9d2947aae8f.png)

That’s a lot of servers

# 动机

AWS 为开发人员提供了一些非常强大和有用的工具，但弄清楚它们是如何连接的可能是一个艰巨的过程。许多开发人员花费了无数的时间来随意筛选文档、论坛评论、未回答的 StackOverflow 问题和教程，结果却发现自己又回到了起点，没有编写任何代码。

影响我的一个例子是，我试图用 Amazon Cognito 认证新用户，然后将他们添加到 DynamoDB 数据库中。这篇文章主要是为我自己的参考而写的，但我希望它能帮助其他搁浅并需要重新下水的开发人员，而不是涉水通过代码的海洋。

好了，双关语说够了。让我们开始编码吧。

# 先决条件

本教程假设您已经设置了一个 Amazon Cognito 用户池以及一个带有用户表的 Dynamo 数据库。

# 初始设置

## 为 Lambda 函数创建 IAM 角色

首先，您需要为将要创建的 Lambda 函数设置一个 IAM 角色。

访问 IAM 管理控制台，从左侧菜单中选择**角色**。点击**创建角色**，选择 **AWS 服务** **Lambda** 角色。两者都突出显示后，单击**下一步:权限**。

出于这个简单教程的目的，我们将选择:
1。DynamoDBFullAccess
2。AmazonCognitoDeveloperAuthenticatedIdentities
3。AmazonCognitoPowerUser

点击**下一步:标签**

可选地为您的标签设置键值对，然后单击**下一步:查看**

想给你的角色起什么名字都行，只要你能认出它，然后点击**创建角色。**

## 创建新的 Lambda 函数

访问您的 AWS Lambda 仪表板并点击**创建功能**。

从头选择**作者**，命名您的函数并保持 **Node.js 10.x** 运行时选项处于选中状态。

在**权限**下拉列表中，从第一个下拉列表中选择**使用现有角色**，并在下面出现的**现有角色**下拉列表中选择您刚刚创建的 IAM 角色。

点击**创建功能**。

# 编写您的函数

在**设计器**下方的**功能代码**窗口中，我们可以开始创建功能，该功能将执行向数据库添加新用户所需的操作。

*感谢* [*vbudilov*](https://github.com/vbudilov) *的* [*首发码*](https://github.com/vbudilov/cognito-to-dynamodb-lambda/blob/master/cognitoToDDB.js) *帮助我入门。*

将以下内容粘贴到**功能代码**窗口中。

Cognito to DynamoDB

## 这个主旨到底是怎么回事？

该处理程序使用 [aws-sdk](https://aws.amazon.com/sdk-for-node-js/) DynamoDB API，您可以看到它在文件的顶部被实例化，但是我们需要在实际调用将数据添加到数据库的方法之前建立一些参数(我们稍后会这样做)。

一旦配置了变量，就存在一个条件语句，只有当检测到一个事件并且请求包括 userAttributes 时，该条件语句才返回 true，user attributes 是 Cognito 分配给每个新用户的唯一标识符。

然后我们基于 AWS SDK DynamoDB API [文档](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#updateTable-property)创建一个对象，该对象被传递给 putItem 函数并返回一个承诺。

假设 promise 返回一个成功的响应，用户被添加到数据库中，函数结束。

在文章的后面，我们将会详细讨论 Lambda 如何检测实际的用户注册。

# 设置环境变量

请注意以下三个变量:

```
const tableName = process.env.TABLE_NAME;    
const region = process.env.REGION;    
const defaultAvi = 'https://YOUR/DEFAULT/IMAGE';
```

除了这个功能之外:

```
aws.config.update({region: region});
```

虽然您*可以*输入自己的 TABLE_NAME 和 REGION，但是最好使用 Lambda 内置的**环境变量**功能来动态定义这些变量。您会发现**环境变量**输入字段就在您正在编写函数的函数代码窗口的下面。

如您所料，您可以分别用 REGION 和 TABLE_NAME 填充**键**，用 DynamoDB 表名 AWS Region 填充**值**。

# 根据您的喜好修改数据库参数

您会注意到我创建了一个数据库对象，它带有符合我的数据库表的**项**参数。您应该修改此部分以满足您的需要。

```
// -- Write data to DDB        
let ddbParams = {            
  Item: {                
    'id': {S: event.request.userAttributes.sub},
    '__typename': {S: 'User'},
    'picture': {S: defaultAvi},
    'username': {S: event.userName},
    'name': {S: event.request.userAttributes.name},
    'skillLevel': {N: '0'},
    'email': {S: event.request.userAttributes.email},
    'createdAt': {S: date.toISOString()},
  },
  TableName: tableName        
};
```

# 测试功能

在我们用 Cognito 用户池实际实现这个新功能之前，我们知道它按照我们想要的方式工作是很重要的，对吗？幸运的是，Lambda 提供了一个奇妙的测试框架，允许您用一个简单的 JSON 对象提交请求。

在 Lambda 管理控制台的顶部附近，您会发现一个下拉菜单，默认显示 ***选择一个测试事件*。**点击下拉菜单，选择**配置测试事件。**

在**事件名称**字段中输入一个容易记住的名称，并将以下代码粘贴到下面的编辑器中。

Test user request

再次编辑请求以适应您已经创建的数据库模式。

当您对测试请求感到满意时，点击 **Create。**

关闭模式并点击页面顶部事件下拉菜单右侧的**测试**按钮。

测试将在页面标题和编写函数的**函数代码**编辑器的控制台中返回响应。

成功！

现在，如果您检查您的 DynamoDB，您应该在用户表中看到您的新测试用户。

# 在 Cognito 中创建确认后触发器

访问您的 Amazon Cognito 用户池，点击左侧的**触发器**菜单项。

找到 **Post 确认**卡，选择您刚刚创建并测试的 Lambda 函数。最后，点击**保存更改**保存您的工作。

现在，当用户通过您的 Amazon Cognito 注册表单注册时，他们将自动添加到您的 DynamoDB 中，并可以以您认为合适的任何方式进行管理。

# **就这样**

恭喜，您已经创建了一个由 Cognito 后确认触发器调用的 Lambda 函数。

如果你觉得我错过了什么或者可以改进这个教程，请不要犹豫，在评论中告诉我！