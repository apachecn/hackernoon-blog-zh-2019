# 如何设置您的本地项目来为 Laravel(或任何 PHP 库)做贡献:实用指南

> 原文：<https://medium.com/hackernoon/how-to-set-up-your-local-projects-to-contribute-to-laravel-or-any-php-library-a-practical-guide-7f2ca9910eef>

让我们设想这样一个场景:您正在开发一个 Laravel 应用程序，您发现了一个 bug，要么是在框架本身，要么是在您正在使用的某个第三方库中。你在你的项目中编辑代码，或者你安装了一个新的有问题的 Laravel 和库，你在那里编辑代码。您保存了更改，但是当您到终端提交更改时，GIT 忽略了您所做的事情。你开始想“这里发生了什么？”。

![](img/f04d4bee2293705a9ea5a00a2b394029.png)

您脑海中出现的第一个解决方案可能是阅读框架的“如何贡献”或库文档，在大多数情况下，您会发现对框架做出贡献的必要步骤，但我总觉得文档中缺少了一些东西。让我解释一下我的意思:

让我们以 Laravel 为例，如果你决定为框架做贡献，你需要克隆 [laravel/framework](https://github.com/laravel/framework) 库，将你的更改推送到你自己的库副本(在 github 上)，并创建一个 PR，但是这里缺少的是你如何在本地项目中测试你的更改(bug 修复，新特性等等)，因为如果你在那个项目中引入了更改，你就不能简单地提交它们并为它们创建 PR。 如果您更改了您刚刚克隆的`laravel/framework`存储库中的代码，您可能会为了测试这些更改而在这个存储库和您的项目之间进行大量的复制/粘贴。

幸运的是，有一种更干净的方法来处理这一切。我不会带你走一遍创建一个公关的一般步骤，我想既然你正在读这篇文章，你已经很熟悉了。我将重点介绍如何设置您的本地环境来简化整个过程，并向您展示如何在框架/库中修复错误和开发新功能，并在您创建将要发送的 PR 之前在本地测试它们。

# 准备您的测试项目和存储库

首先，您需要派生出`laravel/framework`库，然后在本地克隆它(这是为任何开源项目做贡献时的常用步骤)，然后创建一个测试应用程序，您将在其中测试这些更改(我称之为 playground)

```
➜ laravel pwd
/home/djug/laravel
➜ laravel ls
framework playground
```

# 让 Composer 从本地存储库中加载更改

现在，最重要的部分。我们需要告诉`playground`应用程序考虑我们在`/home/djug/laravel/framework`存储库中引入的变更

我们可以通过 composer 来实现，根据它的[文档](https://getcomposer.org/doc/05-repositories.md#loading-a-package-from-a-vcs-repository)，我在这里引用:

> **从 VCS 仓库中加载一个包**
> *这里有一些用例。最常见的一种是维护自己的第三方库。如果您正在为您的项目使用某个
> 库，并且您决定更改
> 库中的某些内容，您将希望您的项目使用修补版本。如果
> 库在 GitHub 上(大多数时候都是这样)，你可以
> 简单地把它分支到那里，然后把你的修改推到你的分支上。之后，
> 您更新项目的 composer.json。您所要做的就是添加您的
> fork 作为存储库，并更新版本约束以指向
> 您的定制分支。*

所以`composer.json`文件的第一部分(在我的`playground`项目中)应该是这样的:

```
{
    "name": "laravel/laravel",
    "type": "project",
    "description": "The Laravel Framework.",
    "keywords": [
    "framework",
    "laravel"
    ], "license": "MIT",
    "repositories": [
        {
            "type": "vcs",
            "url": "/home/djug/laravel/framework/"
        }
    ],
    "require": {
        "php": "^7.1.3",
        "fideloper/proxy": "^4.0",
        "laravel/framework": "master",
        "laravel/tinker": "^1.0"
    },
...
}
```

现在，每次我在`/home/djug/laravel/framework/`库中更改一些东西并提交更改(你需要提交它)，如果我在我的`playground`项目上执行`composer update`，它就会得到更改。

# 3/将更改推送到您的远程存储库并创建一个 PR

既然您的 playground 存储库可以从您的`framework`存储库中提取变更，那么您可以将您想要的所有变更添加到您的代码中，然后在一个“真正的”项目中测试它们。

完成测试后，您需要做的就是将更改推送到您的远程存储库(例如 GitHub 上您自己的`framework`存储库的“副本”),然后创建 pull 请求。

约翰·施诺布里奇在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片