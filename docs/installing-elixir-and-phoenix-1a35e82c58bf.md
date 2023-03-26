# 安装仙丹和凤凰

> 原文：<https://medium.com/hackernoon/installing-elixir-and-phoenix-1a35e82c58bf>

今晚有一点空闲，我想我应该去看看长生不老药和凤凰。Elixir 是一种相对较新的语言，运行在 Erlang VM 上，Phoenix 是它的 web 框架。

在过去的几年里，黑客新闻上有很多关于[长生不老药](https://hackernoon.com/tagged/elixir)的讨论。众所周知，WhatsApp 在其后端运行 Erlang，这是它如此出色地扩展的关键因素。这是我对它感兴趣的主要原因。

不久前，我看了凯文·彼得写的[跟我学:长生不老药](https://inquisitivedeveloper.com/tag/lwm-elixir/)博客系列，了解了更多关于长生不老药的知识。我推荐这个系列——我只看了前几章，它已经给了我关于长生不老药的很好的背景知识。

所以我想我应该安装长生不老药来尝试一下。

![](img/6a4a6b4f6b10ef78ff906d77f243d975.png)

## Elixir 安装—使用版本管理器

所以我的目标是安装 Elixir(和 Erlang VM)。过去几年我一直在使用 nodejs，我非常喜欢`nvm`在您的环境中处理 node 的安装和版本切换的方式。从之前的 PHP 世界来看，这是一个巨大的改进，因为 PHP 是在系统级安装的，很难调整。

正因为如此，我避开了用操作系统的包管理器安装的默认指令(`brew`/`pacman`/`yum`/`apt-get`/等等)。)，又找了类似`nvm`的东西。

[Elixir 的安装页面](https://elixir-lang.org/install.html)提到了几个版本管理器。在阅读了他们的简历后，我决定选择克尔和基克斯。`kerl`是一个 Erlang 版本管理器，而`kiex`的构建类似于 kerl。两者在概念和用法上都和`nvm`相当相似，这是我想要的。

为了远离系统的包管理器，我简单地下载了`kerl`并把它移动到我家的一个目录下进行安装:

```
curl -O [https://raw.githubusercontent.com/kerl/kerl/master/kerl](https://raw.githubusercontent.com/kerl/kerl/master/kerl)
chmod a+x kerl
mkdir ~/.kerl
mv kerl ~/.kerl
```

然后我编辑我的`.bashrc`文件来添加这一行:

```
export PATH=$HOME/.kerl:$PATH
```

现在`kerl`在我的 bash 上可用了。

接下来，安装`kiex`也很容易，类似于`nvm`如何从他们的 repo 运行脚本:

```
curl -sSL [https://raw.githubusercontent.com/taylor/kiex/master/install](https://raw.githubusercontent.com/taylor/kiex/master/install) | bash -s
```

现在`kerl`和`kiex`都准备好了！

## 安装一个版本的 Erlang VM 和 Elixir

关于安装 Erlang VM，需要注意的是您必须构建它。如果你运行`kerl list releases`,然后简单地运行`kerl install 21.2`,你会遇到一个错误，说它没有建立。所以你会想跑:

```
kerl build 21.2
```

这将开始下载 Erlang 源代码并编译它。这需要一段时间。(在我的笔记本电脑上花了 25 分钟。下载 80+mb，花了几分钟，剩下的就是编译了。)在编译过程中，没有进度条可能是个问题，所以如果您想知道它是否在做任何事情，您可以在一个单独的终端中跟踪构建日志，看看它在做什么:

```
tail -f ~/.kerl/builds/21.2/otp_build_21.2.log
```

我们现在设置的好处是，由于我们将`kerl`脚本放在了`~/.kerl`中，它的构建位置自动位于目录中，类似于`nvm`的操作方式。

构建完成后，我们可以进行 kerl 安装:

```
kerl install 21.2 ~/.kerl/installs/21.2
```

安装后，您必须激活它:

```
. ~/.kerl/installs/21.2/activate
```

最后两步都应该非常快。

注:使用`kerl_deactivate`随时取消安装。使用`kerl active`随时验证激活的安装。

现在我们准备用`kiex`安装仙丹。我们可以使用`kiex list known`来查看版本列表，并安装最新版本，如下所示:

```
kiex install 1.8.1
```

现在，在您当前的环境中安装了一个特定的 Elixir 版本。最后，切换到它来使用它:

```
kiex use 1.8.1
```

就是这样！像`elixir`、`iex`和`mix`这样的命令现在应该可以在您的终端上使用了。

## phoenix——让服务器运行的最快、最少依赖的方式(感受一下)

Phoenix framework 真的是功能齐全，所以很自然地，它会默认使用 Postgres，并希望你建立一个数据库等等。要出发了。

如果您想以最快的方式启动并运行 Phoenix 服务器，您可以首先安装 Phoenix:

```
# install hex, the package manager of elixir
mix local.hex# install phoenix
mix archive.install hex phx_new 1.4.0
```

然后创建一个新的 Phoenix 项目，使依赖性最小化:

```
mix phx.new --no-ecto --no-html ~/phoenixtest
```

最后，只需使用`cd ~/phoenixtest`启动服务器:

```
mix phx.server
```

您的服务器将启动并运行在 localhost:4000 上！

注:`GET /`的路线会返回错误:)