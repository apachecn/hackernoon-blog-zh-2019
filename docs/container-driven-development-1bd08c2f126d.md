# 容器驱动开发

> 原文：<https://medium.com/hackernoon/container-driven-development-1bd08c2f126d>

![](img/2a44418cb1f4ed494714769de6b6fdbb.png)

Photo by [Martin W. Kirst](https://unsplash.com/@nitram509?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这篇文章的目的是介绍一个开发和测试软件的工作流程，它与容器化的环境紧密相关。我喜欢称这个工作流为**容器驱动开发**。容器驱动开发背后的核心思想是从应用程序开发周期的一开始就在容器化的环境中编写、执行和测试每一行代码。

通常，我们首先编写和测试我们的应用程序，然后在发货或部署之前对它们进行容器化。对于我们大多数人来说，我们的应用程序第一次真正在容器中运行是在生产中。这并不理想，因为它会导致容器化的问题在开发周期中被发现得太晚。

另一种方法是从应用程序开发过程的一开始就引入容器。这样做的最大好处是，它使我们能够在一个看起来和行为完全像我们的生产环境的环境中开发和测试我们的应用程序。

这篇文章的剩余部分使用一个玩具应用程序来演示我通常是如何进行容器驱动开发的。因为我大部分时间都在用 python 开发，所以这篇文章是专门针对基于 python 的工作流的。如何对其他编程语言采用类似的方法。

## **项目设置**

下面是我在名为`myapp`的 python 应用上进行容器驱动开发的典型目录结构。这个目录中最重要的文件是 *docker-compose.yml* 和*docker 文件。*我们将详细了解每一项的内容。

```
myapp
├── Dockerfile
├── docker-compose.yml
├── myapp
│   ├── __init__.py
│   └── main.py
├── requirements.txt
└── tests
    ├── __init__.py
    └── test_main.py
```

## Docker 撰写

我很少需要编写独立的应用程序。我编写的大多数应用程序都依赖于其他系统——其中最普遍的是 SQL 数据库。随着 docker 的出现，我很少发现自己在安装软件。相反，我尽可能地使用各自的 docker 容器。Docker Compose 是一个管理多容器应用程序的好工具，我经常使用它。我为应用程序编写的典型 docker-compose 文件(在本例中为`myapp`)如下所示:

docker-compose 文件中的`services`节点是最重要的。在这里，我描述了我的应用程序(`myapp`)及其依赖项的构建配置。

docker-compose 文件中的第一个服务节点是`myapp`。您可能已经猜到，这是与正在构建的应用程序相对应的服务定义

`myapp`节点有四个感兴趣的子节点:`build`、`image`、`depends_on`和`volumes`。

`build`节点定义了构建配置。作为这个配置的一部分，我提供了一个名为`app_name`的构建时参数。我们马上会看到这个参数是如何在`Dockerfile`中使用的。最后，我已经将 build `context`指定为当前目录。`context`只是表示相对于 docker-compose 文件要构建的服务的`Dockerfile`的位置。

`image`节点配置正在构建的服务的命名。在这种情况下，我将名称定义为形式`myapp:${TAG}`。`TAG`是一个将在构建时定义的环境变量。

`depends_on`节点定义上游服务依赖关系。作为该节点一部分列出的服务必须在`docker-compose`文件中定义。`depends_on`配置确保依赖服务在应用服务启动之前被拉入并启动。

最后，`volumes`节点确保源代码目录被挂载到应用程序容器中的适当位置。在这种情况下，`myapp`的源代码目录被挂载到应用程序容器内的`/myapp`目录。将源代码目录安装到应用程序容器中是非常重要的，因为它使我能够在容器中编辑、运行和测试我的代码，而不是每次编辑时都将源文件复制到容器中。

除了`myapp` 服务，docker-compose 文件还包括`redis`和`postgres`的服务节点。因为我们自己并不是从头开始构建这些服务，所以所需要的只是我们需要的每个服务的图像名称和标签。在运行期间，将从 [Dockerhub](https://hub.docker.com/) 下拉相应的图像和标签。

## Dockerfile 文件

我的 Dockerfiles 通常相当简单，除非我在处理一些更深奥的东西:

在上面的 docker 文件中，我只是拉下一个基本的 python 映像，将源代码目录添加到容器中，并安装相关的 python 包。注意`Dockerfile`中的`ARG app_name`语句。这基本上声明了一个构建时参数，我们在上面的 docker-compose 文件中设置了它的值，即`app_name: myapp`。入口点`CMD`将根据应用程序的类型而变化，但通常它类似于上面 Dockerfile 中显示的内容。

## 工作流程

一旦`docker-compose`和`Dockerfile`被设置好，从这里开始的工作流程非常简单，只需要几个命令就可以了。

对我来说，起点是在我的应用程序 docker 容器中获得一个终端外壳。这可以通过简单的 docker-compose 命令来完成:

```
TAG=development docker-compose run myapp bash
```

上面的命令做了几件事。它下拉(如果还没有)并启动应用程序的上游依赖项(在本例中是`redis`和`postgres`容器)。它构建应用程序容器`myapp`并用关键字`development`标记它。它在`redis`、`postgres`和应用程序容器`myapp`之间创建一个桥接网络，并在`myapp`容器中打开一个 shell 提示符。从现在开始，我可以像在任何其他终端 shell 中一样在这个 shell 中编写和执行代码。唯一的区别是，这个 shell 是在一个隔离的 docker 容器中执行的，其中安装了我的所有源代码及其各自的依赖项。多酷啊！

我在 VI / TMUX 环境中开发代码。下面是我的开发环境在这个工作流程中的简单截图。

在截屏中，顶部窗格显示了我的 VI 文本编辑器，其中加载了源代码。底部窗格有一个在`myapp`容器内运行的终端外壳。我可以简单地在文本编辑器中编辑代码，并在容器中执行它。生活真好:)

有时，我会通过`docker-compose`为所有测试或测试子集直接执行测试运行程序，而不是在应用程序容器中获取终端外壳，然后执行测试运行程序:

```
TAG=development docker-compose run myapp pytest tests/test_main.py
```

上面的命令简单地启动应用 docker 容器(以及相关的容器依赖项，如果还没有运行的话)，在容器内执行测试运行器命令`pytest tests/test_main.py`并退出。下面是一个简短的截屏，展示了它的实际效果:

总之，我认为从应用程序开发环境的最开始就在容器内部开发有几个优点。最显著的是在类似运行时环境的生产环境中编写和测试代码的能力。这可以防止容器化的错误和问题在游戏后期被发现。

希望这篇文章中展示的工作流能让容器成为开发生命周期的重要组成部分变得更容易。