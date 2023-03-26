# 让无服务器变量为您服务

> 原文：<https://medium.com/hackernoon/making-serverless-variables-work-for-you-900dcf0607f3>

![](img/74d8b6344d1babf8475b839d0a8a0d7a.png)

Photo by [Caspar Camille Rubin](https://unsplash.com/@casparrubin) on [Unsplash](https://unsplash.com/photos/fPkvU7RDmCo)

[无服务器](https://serverless.com/)，一个管理云应用的框架，是一个非常强大的工具。随着您的应用程序变得越来越复杂，基于不同阶段(开发、QA、生产)管理部署变量可能会很困难。目前，引用特定于部署的定制或环境变量并不容易。我知道必须有一种更好的方法来处理这些变量，同时使它们可组合。在对无服务器架构做了一些研究后，我想出了一个插件来做这件事:`[serverless-plugin-composed-vars](https://github.com/chris-feist/serverless-plugin-composed-vars)`。

# 它是做什么的？

`[serverless-plugin-composed-vars](https://github.com/chris-feist/serverless-plugin-composed-vars)`允许您定义阶段特定变量文件。它覆盖了在您的`serverless.yml`或单独的`variables.yml`和`environment.yml`文件中分别为自定义和环境变量定义的变量。要定义特定于阶段的文件，只需将阶段名插入文件中，如下所示:`variables.stage.yml`或`environment.stage.yml`。例如，如果您想为名为“prod”的阶段创建变量文件，您应该将文件命名为`variables.prod.yml`和`environment.prod.yml`。

让我们来看看它的实际应用:

使用上面的示例文件，对于“生产”部署阶段，自定义和环境变量被组合并计算成以下内容:

```
custom:
  googlesWebsite: www.google.com
  myEndpoint: api.endpoint.com

environment:
  THE_ANSWER_IS: 42
  USER_TABLE_NAME: Users
  MY_ENDPOINT: api.endpoint.com
```

对于一个更清晰的`serverless.yml`服务文件，您可以将您的默认变量分离到它们自己的`variables.yml`和`environment.yml`文件中。`[serverless-plugin-composed-vars](https://github.com/chris-feist/serverless-plugin-composed-vars)`将在编写部署变量时自动读取这些文件。注意，当使用默认变量文件时，`serverless.yml`文件变量的优先级最低。

# 我如何使用它？

## 安装

使用您最喜欢的软件包管理器安装插件:

`npm install -D serverless-plugin-composed-vars`

或者

`yarn add -D serverless-plugin-composed-vars`

## 使能够

将插件添加到您的服务文件中:

```
# serverless.ymlplugins:
  - serverless-plugin-composed-vars
  - other-serverless-plugin
```

**注意**:为了保证与其他插件的兼容性，建议`[serverless-plugin-composed-vars](https://github.com/chris-feist/serverless-plugin-composed-vars)`成为插件列表中的第一个插件。

## 创造

创建部署阶段变量文件。定制和环境变量的默认文件名是`variables.stage.yml`和`environment.stage.yml`。以下是自定义变量的项目结构示例:

## 高级用法

有关高级用法和配置，请参见 [GitHub 自述文件](https://github.com/chris-feist/serverless-plugin-composed-vars#advanced-usage)。

# 它是如何工作的？

在幕后，`[serverless-plugin-composed-vars](https://github.com/chris-feist/serverless-plugin-composed-vars)`利用了这样一个事实，即无服务器在钩子被执行之前不会计算变量[。这允许插件在打包您的服务进行部署之前编写部署阶段变量并重写它们。](https://serverless.com/framework/docs/providers/aws/guide/plugins/#serverless-instance)

# 看看吧！

既然您已经看到了使用部署阶段变量是多么容易，那么就试试这个插件吧。在这里留下任何评论或建议，或者在 [GitHub](https://github.com/chris-feist/serverless-plugin-composed-vars) 库上提交问题。