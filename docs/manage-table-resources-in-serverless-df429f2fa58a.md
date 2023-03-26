# 在无服务器中管理表资源

> 原文：<https://medium.com/hackernoon/manage-table-resources-in-serverless-df429f2fa58a>

![](img/b8a0ddfd78f856a66196747de9975e9d.png)

Photo by [Blake Parkinson](https://unsplash.com/@blakeparkinson) on [Unsplash](https://unsplash.com/photos/3IX-Tz_0ZN0)

无服务器是管理无服务器基础设施的绝佳平台。它大大减少了设置服务和定义功能的样板代码。它甚至允许为不受开箱即用支持的东西定义特定于平台的资源。问题是服务提供商可能需要复杂的模板来支持基本的基础设施。例如，AWS CloudFormation 要求在单个表的两个不同字段下定义 DynamoDB 键。因此，我创建了一个插件来简化表格定义，减少样板代码，并拥有智能默认值:`[serverless-plugin-tables](https://github.com/chris-feist/serverless-plugin-tables)`。

# 让我们看一个例子

下面是在 [DynamoDB 文档](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html)中描述的表的两个示例`servlerless.yml`定义。第一个例子是使用[标准云形成模板](https://serverless.com/framework/docs/providers/aws/guide/resources#configuration)，第二个例子是使用`serverless-plugin-tables`。

如您所见，`serverless-plugin-tables`的实现显著减少了样板文件(1/3 的代码行)。它也有相同的默认值，使用[按需计费模式](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadWriteCapacityMode.html#HowItWorks.OnDemand)和字符串作为默认数据类型。尽管定义如此简单，但它是完全可定制的，在发布时支持每个云形成选项。参见自述文件中的完整[示例。](https://github.com/chris-feist/serverless-plugin-tables#aws-example)

# 听起来很棒！还是并入吧！

首先，将依赖项添加到现有的无服务器项目中:

`yarn add -D serverless-plugin-tables`

现在将插件添加到您的服务文件中:

```
# serverless.ymlplugins:
  - serverless-plugin-tables
```

现在定义“资源”部分(如果还没有的话)，添加一个`tables`属性，并开始创建表🤩

```
# serverless.ymlresources:
  tables:
    # Start defining tables here
```

下面是另一个用户表格示例，配置稍微多一点:

要了解更多配置选项，请查看 GitHub 自述文件，它描述了每个选项以及 AWS 文档的链接。

## 其他提供商和数据库呢？

我通过为 AWS DynamoDB 实现它来开始这个项目。然而，我将它设计成可以轻松扩展以支持其他服务提供商和数据库平台。因此，如果您希望支持另一个平台，请在此发表评论或在 [GitHub](https://github.com/chris-feist/serverless-plugin-tables) 上提交问题。

让我知道你的想法。请在下面留下评论，给我一些掌声。你按拍手按钮的时间越长，我得到的就越多👏👏👏