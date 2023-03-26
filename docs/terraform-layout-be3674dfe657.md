# 地形布局

> 原文：<https://medium.com/hackernoon/terraform-layout-be3674dfe657>

## 从基本布局到多环境定义

![](img/dad232839be81fd0e924f3011e27d136.png)

Terraform 是描述我们的基础设施的强大工具，但有时小型基础设施到大型基础设施之间的变化可能很难管理，因为大型基础设施包含多个环境、大量元素以及针对每个元素的小型定制。让我们一步一步地回顾一下当您的基础架构开始增长时的演变。

# TL；速度三角形定位法(dead reckoning)

**(剧透！！)最终布局:**

```
├── Readme.md
|   
├── environments/
|   |
|   ├── development/
|   |   ├── provider.tf
|   |   ├── development.auto.tfvars
|   |   ├── development.backend.tfvars
|   |
|   ├── production/
|   |   ├── provider.tf
|   |   ├── production.auto.tfvars
|   |   ├── production.backend.tfvars
|   |
|   ├── staging/
|   |   ├── provider.tf
|   |   ├── staging.auto.tfvars
|   |   ├── staging.backend.tfvars
|   |
├── manifests/
|   ├── backend.tf
|   ├── back_office.tf
|   ├── common_network.tf
|   ├── dbs.tf
|   ├── frontend.tf
|   ├── output.tf
|   ├── persistent_data.tf
|   ├── provider.tf
|   ├── security.tf
|   ├── modules/
|   |   ├── module1/
|   |   ├── module2/
|   |   ├── ...
|
```

**使用方法:**

```
$ cd environments\<env>$ terraform init \
      -backend-config=<env>.backend.tfvars \
      ../../manifests$ terraform plan \
      -out=tfplan \
      -var-file=<env>.tfvars \
      ../../manifests$ terraform apply \
      -var-file=<env>.tfvars \
      ../../manifests \
      tfplan$ terraform output
```

# 婴儿计划

当一个 Terraform 项目诞生时，它就像一个婴儿:小，纯洁，没有坏习惯。你想向所有人展示:

```
├── Readme.md
├── main.tf
├── output.tf
├── variables.tf
├── terraform.tfstate
```

你需要管理它的命令很小很好(吃，睡，便便，等等。):

```
$ terraform init$ terraform plan \
      -out=tfplan$ terraform apply tfplan
```

![](img/424945796ef4ce4b043f42ff4396d2ee.png)

Photo by [Colin Maynard](https://unsplash.com/@invent?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

但是所有的婴儿都在成长，你的项目也是如此。它开始变得更加复杂，没有什么是你不能管理的，当然，你已经为此做好了准备，你已经阅读了它，你能够改进它:你划分了创建一些[模块](https://blog.gruntwork.io/how-to-create-reusable-infrastructure-with-terraform-modules-25526d65f73d)(有一天我们会谈到它们)的责任，你从主文件中分离出一些组件，你(仍然)为此感到自豪。

```
├── Readme.md
**├── backend.tf**
├── main.tf
├── output.tf
**├── provider.tf**
├── variables.tf
|
**├── modules/
|   ├── module1/
|   ├── module2/
|   ├── ...**
```

# 婴儿长大了

有那么一个瞬间，你不知道怎么但是萌娃项目变成了一个不可收拾的丑陋少年项目，你需要去处理。
你不知道有些环境在做什么:发育中的她在吸毒吗？他在 staging 学习还是聚会？你仍然爱它，但是你不像过去那样为它骄傲了…

![](img/6b234d3edc4b384e3c56427b9e8d59db.png)

Photo by [Parker Gibbons](https://unsplash.com/@parkergibbons?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

你需要处理它。而你处理。
你知道这不是一个好的解决方案，但是你*需要*去做，所以[你开始在环境之间复制代码](https://craftsmanshipforsoftware.com/2015/05/25/the-cost-of-duplicated-code/)。你在不同的环境之间复制你的代码(你知道这不是最好的解决方案，但至少你尽力做到最好)，并且你添加一些[变量定义来配置不同的环境](https://learn.hashicorp.com/terraform/getting-started/variables.html)。

```
├── Readme.md
|
**├── development/**
|   ├── backend.tf
|   ├── main.tf
|   ├── output.tf
|   ├── provider.tf
|   ├── variables.tf
|   ├── **development.tfvars**
|   |
|   ├── modules/
|   |   ├── module1/
|   |   ├── module2/
|   |   ├── ...
|
**├── production/**
|   ├── backend.tf
|   ├── main.tf
|   ├── output.tf
|   ├── provider.tf
|   ├── variables.tf
|   ├── **production.tfvars**
|   |
|   ├── modules/
|   |   ├── module1/
|   |   ├── module2/
|   |   ├── ...
|
**├── staging/**
|   ├── backend.tf
|   ├── main.tf
|   ├── output.tf
|   ├── provider.tf
|   ├── variables.tf
|   ├── **staging.tfvars**
|   |
|   ├── modules/ssh-rsa 
|   |   ├── module1/
|   |   ├── module2/
|   |   ├── ...
|
├── test/
|   ├── backend.tf
|   ├── main.tf
|   ├── output.tf
|   ├── provider.tf
|   ├── variables.tf
|   ├── **test.tfvars**
|   |
|   ├── modules
|   |   ├── module1/
|   |   ├── module2/
|   |   ├── ...
```

这并不都是坏事，管理方式有所改变，但仍然很容易和很好地执行。不是什么都需要成为问题！

```
$ **cd <env>**$ terraform init$ terraform plan \
      -out=tfplan \
      **-var-file=****<env>.tfvars**$ terraform apply \
      **-var-file=****<env>.tfvars** \
      tfplan$ terraform outputcommon_network
```

但是，有那么一刻，你知道有些事情不太顺利。
当你在 staging 里改东西的时候，你需要复制到测试、制作、开发……这不是一个搞笑的工作，某天某个人(也许是午饭后的某个星期五？)完全忘记了在环境之间传播一些变化。而这就是**乱**。你需要解决它，你想保留它，你开始搜索它。

你会发现很多提议，比如[这个基本款](/hdeblog/terraform-sane-practices-project-structure-c4347c1bc0f1)，或者[这个其他款](https://surminus.github.io/post/terraform-structures-and-layouts/)，甚至[这个](http://2ndwatch.com/blog/how-we-organize-terraform-code-at-2nd-watch/)。如果你搜索的时间足够长，你会被指向 Terragrunt。

现在，让我谈谈我们的需求以及我们在“重新布局”中搜索的内容。

# 我们的要求

*   我们需要可以在不同环境间重用的东西。我们的具体情况是，所描述的基础设施除了最小的变化(实例名和其他一些东西)之外，完全相同。
*   我们搜索一些容易阅读的东西。我们已经放弃使用 Terragrunt 或 Terraform [工作空间](https://www.terraform.io/docs/state/workspaces.html)，因为增加了我们试图避免的复杂性。
*   我们需要一些快速的东西来实现。将所有当前的基础设施重新配置为模块会增加额外的时间，而我们没有这些时间。(可能改天再聊)。
*   我们想要容易维护的东西。我们面临的问题是在多个环境中手工重复相同的更改，在每个环境中更新相同的文件，我们正在努力避免这种情况。
*   我们渴望一些容易升级的东西。我们确信这种基础设施将会有很大的发展，因此有必要在未来设计 it 思维。
*   我们渴望一个容易理解的代码。我们知道，不同的团队会使用这些代码，他们对 Terraform 和基础设施本身有不同的了解，所以我们在决定解决方案时考虑到了他们。
*   我们想要一个尽可能安全的项目。我们经历过这样的情况:由于人为错误，有人在错误的环境中修改了基础架构。这是一个绝对的混乱(和几个小时的痛苦和眼泪)，我们不想重复。

考虑到所有这些因素，当我们的“青少年项目”变成成人项目时，我们到达了这个奇怪的点。

# 成人项目

![](img/69fb85d54d8644c2139c276c51c08f8e.png)

Photo by [Heng Films](https://unsplash.com/@hengfilms?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

记住下面描述的所有要点，我们完成了代码重构。在这一点上，基础设施本身没有任何变化。同样，唯一改变的是布局、文件组织和变量声明。

首先，我们将所有清单移动到一个名为 *(uah！一个原创的名字！)*载货单。使用这种方法，我们只需要编写一次更改，而不是为每个环境编写一次。

为了配置不同的环境，我们为每个环境声明了一个后端(用于远程 tfstate)和一个变量文件。为了避免错误地进入不需要的文件夹，我们决定用当前环境的名称来命名所有的后端和变量文件( <env>.auto.tfvars 和 <env>.backend.tfvars)。</env></env>

```
├── Readme.md
|   
├── **environments/**
|   |
|   ├── development/
|   |   ├── development.auto.tfvars
|   |   ├── **development.backend.tfvars**
|   |
|   ├── production/
|   |   ├── production.auto.tfvars
|   |   ├── **production.backend.tfvars**
|   |
|   ├── staging/
|   |   ├── staging.auto.tfvars
|   |   ├── **staging.backend.tfvars**
|   |
├── **manifests/**
|   ├── backend.tf
|   ├── main.tf
|   ├── output.tf
|   ├── provider.tf
|   ├── variables.tf
|   ├── modules/
|   |   ├── module1/
|   |   ├── module2/
|   |   ├── ...
|
```

对于这种布局，管理它的方式有点不同。我们需要访问所需的环境，并将 manifests 文件夹引用为 plan/init 文件夹(这一小小的区别大大简化了我们的生活)。

```
$ **cd environments\<env>**$ terraform init **\**
      **-backend-config****=<env>.backend.tfvars \
      ../../manifests**$ terraform plan \
      -out=tfplan \
      **-var-file=****<env>.tfvars** **\
** **../../manifests**$ terraform apply \
      **-var-file=****<env>.tfvars** **\
** **../../manifests** **\** tfplan
```

使用这种方法，布局简单，易于维护，一目了然。我们达到了预期的状态，我们能够比以前更顺利地工作。

此时，我们对清单进行了“一点”重新配置。现在，将它们都放在一个文件夹中，并在不同环境之间共享，我们就能够在所有环境中快速地更改和测试它们。

```
├── **manifests/**
|   ├── backend.tf
|   ├── **backoffice.tf** |   ├── **database.tf**
|   ├── **frontend.tf**
|   ├── output.tf
|   ├── **persistent_storage.tf**
|   ├── provider.tf
|   ├── **security.tf**
|   ├── modules/
|   |   ├── module1/
|   |   ├── module2/
|   |   ├── ...
|
```

我们从最初的范型 *main.tf* 、 *output.tf* 和 *vars.tf* 改成了基于“函数”的一组文件。如果我们需要改变前端基础设施，我们将转到前端*。tf* 文件。通过这种方法，我们获得了两件事:

*   速度检测变化:如果代码的任何部分发生变化，我们将看到它应用于架构的哪个元素，只需查看文件名。
*   独立性:如果我们需要删除基础架构的某些部分，我们只需要删除一个文件。或者如果我们需要添加一些我们不需要接触的 500 行文件，只需创建一个新的。

**地形输出(我们不引以为豪的部分)**

使用这种方法“我们只面临一个问题”，`terraform output`不起作用，因为在< env >文件夹中没有定义提供者。解决这个问题的快速方法是在每个环境中都有一个 provider.tf，作为 manifests 文件夹中的 provider . TF 的副本，具有相同的内容:

```
provider "PROVIDER_NAME" { }
```

当然，也可以通过 <env>.auto.tfvars 文件进行配置。</env>

# **接下来的步骤**

**模块** 如果我们有使它更可重用的需求(也许在项目之间)，我们将重新配置，把基础设施定义为不同的模块。到目前为止，我们只有针对更低级元素的模块。

**版本控制**
也许我们会面临这样的问题，如果我们需要在特定的环境中改变基础架构的某些部分，我们将需要开始添加条件定义(*嗯……太难看了……*)。但是当我们同意这个问题不会影响我们的时候(我们需要环境中完全相同的基础设施，并且可以通过变量进行改变)。这可以通过在 git 中标记基础设施版本或添加“有条件的”基础设施来解决，或者…当我们需要它时，我们会解决它；)

# 结论

项目不断变化，开始时规模很小，有时会增长很多。或者不是。
出于这个原因，重要的是**停下来想一想我们在代码中需要什么**。描述我们的需求以及我们为什么要重构。

> 不要试图一步到位达到最佳方案，从有用的东西开始，一步一步完善。

所有的项目都是不同的，要理解一个项目为什么会是现在这个样子，有必要了解它的背景和“发展历史”，不要试图过度保护一个项目，在一些你可能永远不会尝试的事情上花费时间和精力。

并非所有的项目都需要相同的解决方案或相同的方法，所以停下来想想为什么要重构代码。