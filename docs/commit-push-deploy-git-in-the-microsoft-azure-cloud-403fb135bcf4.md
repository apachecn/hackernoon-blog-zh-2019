# 提交、推送、部署—微软 Azure 云中的 Git

> 原文：<https://medium.com/hackernoon/commit-push-deploy-git-in-the-microsoft-azure-cloud-403fb135bcf4>

![](img/642770143b10fdedfb802bc28cf71c9a.png)

在[推特](https://twitter.com/chris_noring)上关注我，很高兴接受你对主题或改进的建议/Chris

> 自从 Git 发布以来，我们就开始依赖它作为我们的默认版本控制工具，它已经成为事实上的标准，即使存在其他选择。Git 帮助我们管理我们的源代码，将它分成分支，甚至通过允许我们创建拉/合并请求，帮助我们与其他开发者在 GitHub 或 GitLab 等地方合作。如果当我们将应用程序迁移到云中时，Git 可以在那里帮助我们，这不是很好吗？

本文将涵盖以下内容:

*   **下载**一个示例应用程序，我们希望专注于理解使用 Git 的部署，所以我们将采用一些现成的代码
*   在本地运行应用程序只是为了确保它的工作，这是必须的，如果它在本地工作，它至少有机会在云中工作
*   **配置一个部署用户**，该部署用户是 web app 的`FTP`和`local Git deployment`所必需的
*   **创建一个资源组**，如果您需要一个新的逻辑分组来完成我们将要做的事情，
*   **创建一个服务计划**，我们将需要它来指定我们将如何为此付费，以及我们将创建什么样的容器
*   **创建 web 应用程序**，我们将需要运行一个命令来创建实际的 web 应用程序，我们将需要在这里说明我们如何部署它，我们将选择`git deploy`，更多信息请见下文
*   **访问**生成的网站，我们当然要享受我们的劳动成果
*   **使用`git deploy`将源代码推送到**站点
*   **管理更新**，让我们学习如何更新我们的应用程序并推送更改，使用 Git，这种部署不是一次性的，我们可以随时更改代码并重新部署

# 为什么是云，为什么是 Git？

如今，云几乎是开发者部署新应用的默认位置。原因有很多，比如它具有成本效益、可伸缩性、弹性，并且对安全性有一些很好的内置支持(尽管我们不能完全依赖云)。

Git 是一个我们很可能已经在使用的工具，当我们编码来管理我们的源代码时，尤其是当我们有许多开发人员维护相同的代码库时。将云计算和部署等技能添加到您的工具带上的最佳方式是使用您已经了解的技术。这就是，利用你已经知道的东西，加入一些云。希望你喜欢这篇文章:)

# 资源

在本文中，我们确实参考了一些文档页面，所以在这里，您可以了解更多信息:

*   [安装 Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?wt.mc_id=devto-blog-chnoring&view=azure-cli-latest) ，我们将需要 Azure CLI 的一切。Azure CLI 非常强大，可以让你在门户中执行几乎所有你能做的事情。
*   [本地 Git 部署到 Azure 应用服务](https://docs.microsoft.com/en-us/azure/app-service/deploy-local-git?wt.mc_id=devto-blog-chnoring)，这一页展示了如何使用 Git 将你的应用部署到 Azure，但也谈了一点 Azure DevOps build
*   Git 部署快速入门，这是一个比本文更快的版本。它的速度更快，因为它为资源组、应用服务计划使用了一些智能默认设置，并且它还使用了一种叫做`zip deploy`的东西，这是另一种类型的部署，而不是我们在这里描述的`git deploy`

# 下载并运行一个示例应用程序

先来一个样例 app。最简单的方法是安装 Git(如果您还没有安装的话),并从 GitHub 中获取示例项目:

```
git clone https://github.com/Azure-Samples/python-docs-hello-world cd python-docs-hello-world
```

上面的应用程序是一个 Python 应用程序，使用库`Flask`为我们提供了一个 REST API。这个知识库给了我们什么？

```
application.py // this is where our application livesrequirements.txt // this is a list of libraries that needs to be installed for our app to work.gitignore // just a file Git reads so it knows what files to NOT include when we push changes to the repo
```

让我们看看`application.py`以及它是如何为我们设置一些路线的:

```
// application.pyfrom flask import Flaskapp = Flask(__name__) @app.route("/")def hello(): return "Hello World!"
```

上面的代码所做的是定义一条路线`/`，它让方法`hello`处理提到的路线，并输出文本`Hello World`，这是一个非常标准的短语，当你第一次使用新的编程语言或库时。

## 在本地运行应用程序

好了，现在我们对摆在我们面前的东西有了更多的了解。下一步是尝试在本地运行这个应用程序，然后再将它部署到云中。

要启动并运行 Python 应用程序，我们需要安装 Python 和它附带的依赖库。根据我们使用的操作系统(Windows、Linux 或 Mac ),相关说明会有所不同。如果你在 Mac 上，你已经安装了 Python。然而，您将需要包管理器`Pip`，我们可以通过运行以下命令来获得它:

```
sudo easy_install pip
```

如果你是一个认真的 Python 开发者，那么看一下即将到来的链接是值得的。如果你不是，你只是为了 Git 和 Azure 而来，请随意跳过下面的段落。

> [http://flask.pocoo.org/docs/0.12/installation/#installation](http://flask.pocoo.org/docs/0.12/installation/#installation)

简而言之，它要求你安装一个叫做`virtualenv`的东西。它创建了不同的工作环境，所以如果你需要一个单独的 Python 版本或一些库的单独版本，这是前进的方向。

接下来我们需要做的是安装依赖库，我们通过在终端中调用以下命令来完成:

```
sudo pip install -r requirements.txt
```

快速浏览一下`requirements.txt`文件，就会发现我们有一些应用程序需要的依赖库。正如我们在下面看到的，这是我们的 API 框架`Flask`和一些支持库:

```
click==6.7
**Flask==1.0.2**
itsdangerous==0.24
Jinja2==2.10
MarkupSafe==1.0
Werkzeug==0.14.1%
```

然后我们只需要设置环境变量`FLASK_APP`指向我们的启动文件`application.py`，就像这样:

```
FLASK_APP=application.py
```

然后

```
python -m flask run
```

这将在`http://localhost:5000`启动我们的应用程序，就像这样:

![](img/ace77b7dd92a3987274e14f292294f93.png)

太好了，我们已经为下一步做好了准备，部署到云。

# 配置部署用户

要对 Azure 进行任何类型的部署，你都需要一个所谓的`deployment user`。该部署用户需要有一个全球唯一的名称*。*

为了创建我们的部署用户，我们需要使用命令`az webapp deployment`。然后我们需要在`--user-name`标志后提供一个用户，在`--password`标志后提供一个密码。密码长度至少需要 8 个字符，并且包含以下三个元素中的两个:字母、数字和符号。因此，我们需要运行的完整命令如下所示:

```
az webapp deployment user set --user-name <username> --password <password>
```

下面是调用上述命令的一个例子。让我们调用用户`deployment-user-chris`并给它密码`test1234`。因此，我们的命令现在看起来像这样:

```
az webapp deployment user set --user-name deployment-user-chris --password test1234
```

这应该会产生如下所示的输出:

![](img/039833649cb495d7990030e4dfbecc17.png)

上面的输出意味着我们的命令成功了，我们可以继续进行下一步。

# 创建资源组

资源组是一个逻辑分组，您可以将所有的东西放在其中，如数据库、服务计划、应用程序等等，我们几乎做任何事情都需要一个资源组。

您可以使用现有的资源组之一，也可以创建一个新的资源组，在这种情况下，您可以键入以下内容:

```
az group create \ 
--name [your name for a resource group] \ 
--location westeurope
```

# 创建服务计划

Azure 中的应用服务计划为应用的运行定义了一组计算资源。这相当于更传统的 Web 托管中的服务场。服务计划附带一个定价层。定价层决定您获得的应用服务功能以及您为计划支付的费用。这里还有很多需要了解的，感兴趣的读者可以看看下面的[链接](https://docs.microsoft.com/en-us/azure/app-service/overview-hosting-plans?wt.mc_id=devto-blog-chnoring)来更详细地了解它是如何工作的。

对于本文来说，理解所有运行在 AppService 上的应用程序都需要一个服务计划作为先决条件就足够了。

创建服务计划的完整命令如下:

```
az appservice plan create --name [your name for an app service plan] --resource-group [you name for a resource group] --sku B1 --is-linux
```

值`B1`意味着我们有一个基本的服务计划(相对便宜，所以你可以测试一下),`--is-linux`意味着我们将得到一个 Linux 容器

我们使用上面的`appservice`命令和子命令`plan`来创建我们的服务计划。这也应该给我们一个 JSON 响应，返回值为`Succeeded`的`provisioningState`键

# 创建 web 应用程序

下面是创建 web 应用程序的命令。我们将需要陈述应用程序的`resource group`、`pricing plan`、`name`、Python 的版本，以及最后我们将如何部署它，即`git deploy`:

```
az webapp create --resource-group [your name for a resource group] --plan [your name for a service plan] --name [your name for an app name] --runtime "PYTHON|3.7" --deployment-local-git
```

对上述命令的一个评论是`app name`需要是*全局唯一的*。

在 JSON 对此的回应中，我们寻找两条有趣的信息:

*   git 存储库的名称，查找名为`deploymentLocalGitUrl`的键。
*   公共 URL，查找名为`defaultHostName`的密钥

您所完成的是一个启用了 git 部署的空 web 应用程序。空的原因是因为我们还没有放任何代码进去。

# 访问网站

尽管我们还没有把任何代码放到我们的网站上，但是它仍然包含了一些我们可以访问的默认登陆页面。要访问网站，请转到`http://<app name>.azurewebsites.net`

这将向您显示一个默认站点，如下所示:

![](img/679766e4c3a4643c9049624d2408d646.png)

这似乎行得通，太好了！好了，我们已经完成了大部分繁重的工作，现在是时候卷起袖子，打开指关节了，因为我们要让我们的网站变得生动起来。

![](img/96844d2d3b0c90c8f15010531d5363ec.png)

# 将代码推送到网站

好了，最后一步是正确设置 git deploy 并推送代码，然后查看站点是否用您的代码进行了更新。我们通过键入以下命令设置了`git deploy`:

```
git remote add azure <deploymentLocalGitUrl-from-create-step>
```

现在是时候输入代码了，所以我们只需输入:

```
git push azure master
```

这将提示您输入部署用户密码。只需键入它，您的部署正在进行中。

这将需要一点时间，所以去喝一杯热饮料:)它应该最终转储出一长串日志，以下列结尾:

![](img/fb7efddfe3f890c7d1ebbd79e258c4e8.png)

这意味着您的应用已成功部署，现在您可以返回到您的应用 URL，您应该会看到以下内容:

![](img/7af24380274e8f60b19d12093aa89b48.png)

现在，就是这么做的。现在，庆祝…:)

![](img/38590a59d99cef34999297a2275eb288.png)

# 管理变更和重新部署

这很容易，因为我们已经激活了 git deploy。让我们执行以下操作:

*   对文件进行一些更改
*   提交更改
*   将代码发送到我们的远程分支机构

# 做一些改变

让我们进入`application.py`并将其更改为:

![](img/593284807b5da9d27cd2429b48edd721.png)

# 提交更改

我们像通常在 git 中一样提交:

```
git commit -am "changed text"
```

# 把密码推到我们的遥控器上

现在，让我们通过键入以下命令进入我们的远程分支:

```
git push azure master
```

这将生成一堆日志条目，并以`deployment successful`结束。让我们通过访问我们的应用程序 url 来验证这一点:

![](img/8194b52c5f9dc51a5a38e609f6eedfda.png)

那看起来不错:)

# 摘要

我们已经了解了如何将应用部署到 Azure，甚至更新和重新部署。一篇文章就这么多。让我们简单看一下我们为此采取的步骤:

*   **下载了**一个样本应用。显然，在真实的场景中，我们会创建自己的代码，而不是像在这种情况下使用 Python 和 Flask 的示例应用程序
*   **创建了一个部署用户**，这是我们的部署所需要的。这也促使我们选择部署类型，我们选择的是`git deployment`。
*   **创建了资源组**。这为我们的应用程序创建了一个逻辑组。这是一个组，我们可以重用其他事情，如添加数据库或应用程序可能需要存在的其他东西。
*   **创建了服务计划**。这意味着我们决定如何为此付费，以及我们希望应用程序在什么样的容器中运行。
*   **创建了网站本身**，或者说是一个我们的应用程序可以存在的占位符。
*   **使用 git 将我们的代码**推送到新创建的网站。我们还展示了如何使用 git 来更新我们的网站和重新部署。

这教会了我们使用 Azure 开发 web 应用程序的基础知识，并为我们未来的冒险做好了准备。敬请关注云中的更多乐趣:)

> 最后，从这篇文章中你需要记住的几乎所有东西就是使用 Git，然后推送，你已经成功地实现了云计算…

![](img/da3cda172fc395532a2d4932858e37a3.png)