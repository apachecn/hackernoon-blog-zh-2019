# 将文件挂载到 Kubernetes Pod，而不删除现有文件

> 原文：<https://medium.com/hackernoon/mount-file-to-kubernetes-pod-without-deleting-the-existing-file-in-the-docker-container-in-the-88b5d11661a6>

## 当我试图将配置映射挂载到 docker 容器中时，我了解到 Kubernetes 在同一个目录中已经有了一个文件。

![](img/1b2eccfd5696d3378f809ec0fe35a348.png)

Photo by [Patrick Lindenberg](https://unsplash.com/@heapdump?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

几周前，我做了一个名为[tweeter or](https://github.com/bxcodec/tweetor)的简单应用，是关于在谷歌云平台(GCP)上创建 Kubenernetes 集群的简单教程。这是我做的一个名为“Kube X-mas”的项目的一部分。这个项目是用印度尼西亚语写的一系列文章，在 GCP 的 Kubernetes 中创建一个实时应用程序，你可以在这里看到这个系列:[https://medium . com/easy read/Christmas-tale-of-software-engineer-project-kube-xmas-9167 ebca 70d 2](/easyread/christmas-tale-of-sofware-engineer-project-kube-xmas-9167ebca70d2)

在这个项目中，我将在 GCP 的 Kubernetes 集群中部署一个 Golang 应用程序。我们知道，当开发一个应用程序时，它必须分离它的配置文件，不管它是在环境变量中，还是仅仅在一个配置文件中。json，。yaml，。toml 等)。因此，它可以更加灵活，我们可以改变环境的准备、生产或测试。

在我的例子中，我将在 Docker 映像中 dockerize 我的应用程序，然后在将它部署到 Kubernetes 时，我将使用 Kubernetes 控制器注入配置文件，因此它可以使用注入的配置文件运行。

所以，基本上，在 Kubernetes 中，我们可以将文件注入(挂载)到容器中。在我的例子中，我将把应用程序的配置文件挂载到 docker 容器中。为此，我可以使用 Kubernetes [ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) 和 [Volumes](https://kubernetes.io/docs/concepts/storage/volumes/) ，它允许我们将配置文件注入到 docker 应用程序容器中。

## 问题

明确地说，我有一个 Go (Golang)应用程序，它以 docker 映像构建和容器化。这个应用程序已经编译和构建并保存在 docker 映像中。

这是我的`Dockerfile`。

因此它将创建一个 Docker 映像，该映像将包含在`/app/engine`中编译的 go 应用程序。但是这个应用程序需要一个配置文件，这样它才能运行。我需要注入配置文件，这样它才能运行。

![](img/7dcd0ee0c2784dd851b7bfc1a20345a8.png)

inject the config to docker container

然后，我使用 Kubernetes 配置映射来注入我的应用程序配置文件。

但是，问题发生了。我所期望的是，当在相同的路径上注入这个配置文件时。假设我在容器中的工作目录是`/app`

上一个:

```
app
└── engine
```

在注入带有挂载卷的配置映射后，我期望的是:

```
app
├── config.toml
└── engine
```

实际情况是:

```
app
└── config.toml
```

Kubernetes 挂载卷将替换所有文件，并删除现有文件，而不是添加新文件。

我在这个问题上卡了 4 个小时，因为我没有首先意识到这一点。我的豆荚不断崩溃和重启。在那之前，我试着慢慢调试它，并意识到如果我挂载一个文件，如果它有相同的目录，那么它也将删除现有的文件。

## 解决方法

意识到这个问题后，我有两个选择。

*   将 Docker 容器中我的工作目录与容器中我的应用程序目录分开。因此，取而代之的是使用`/app`作为我在容器中的工作目录，我将创建一个新的目录来保存我的`WORKDIR`。我会把我的配置文件放在那个文件夹里。
*   试图找出它，直到它解决。

选项 1 非常明显，它解决了问题。当时，当面对这个问题时，我使用选项 1。但是几天后，出于好奇，我试着解决选项 2。如果任何人遇到这种问题以及如何解决它，我会尝试阅读许多资源(官方文档)和博客帖子。

但是，我什么也没找到，除了这个[https://blog . sebastian-das chner . com/entries/multiple-kubernetes-volumes-directory](https://blog.sebastian-daschner.com/entries/multiple-kubernetes-volumes-directory)。在这篇文章中，他解释了关于多个 Kubernetes 卷的目录。所以我很好奇这是否也能解决我的问题。

在那篇文章中，他说要使用`subPath`，那就这样吧，然后我也尝试在我的 Kubernetes 应用程序部署中使用`subPath`。

前情提要:

```
- name: configs          
  mountPath: /app
```

并改成了:

```
- name: configs
  mountPath: /app/config.toml
  subPath: config.toml
```

瞧，成功了。它不再替换和删除我在容器中的现有文件。而且可以流畅运行。

我的完整 Kubernetes deployment.yaml 文件:

当解决这个问题时，我学到了一些重要的东西。甚至当我“独自”解决这个问题时，我坚持了 4-6 个小时，但我在这里学到了宝贵的一课。并且无法期待下一次的冒险。

不管怎样，如果你觉得这个有用，你介意分享这个故事吗，这样大家就不会掉进同一个洞里。请关注我，了解我的软件工程学习之旅的最新动态:)

参考资料:

*   [https://blog . sebastian-das chner . com/entries/multiple-kubernetes-volumes-directory](https://blog.sebastian-daschner.com/entries/multiple-kubernetes-volumes-directory)

这里翻译成印尼语:[https://medium . com/easy read/mount-file-ke-kubernetes-pod-tanpa-menghapus-existing-file-1 d9 fc 444 c 91 a](/easyread/mount-file-ke-kubernetes-pod-tanpa-menghapus-existing-file-1d9fc444c91a)