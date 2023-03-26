# 使用 Virtualenv 在 Windows 上安装多个 Python 版本

> 原文：<https://medium.com/hackernoon/installing-multiple-python-versions-on-windows-using-virtualenv-333ed06ef43a>

![](img/3e7fefef45da63199dff3db2643f8546.png)

你在这里是因为:

1.  您使用的是 Windows 操作系统版本 10+
2.  您希望在同一台计算机上使用多个 Python 版本
3.  你已经厌倦了互联网告诉你“只用 Virtualenv”

# TL；速度三角形定位法(dead reckoning)

1.  打开`Command Prompt`并输入`pip install virtualenv`
2.  下载想要的`python`版本(不要添加到 PATH！)，并记住新安装版本的`path\to\new_python.exe`
3.  要创建虚拟人，打开`Command Prompt`并输入
    
4.  如果使用`PyCharm`，更新`Project Interpreter`和`Code compatibility inspection`。
5.  要安装软件包:
    (I)激活 virtualenv:打开`Command Prompt`，输入`path\to\env\Scripts\activate.bat`或`activate env_name`。
    (II)安装所需的软件包
    (III)使用`deactivate`停用。

# 长篇版；务必阅读

## 序言

如果你使用的是 Anaconda 应用程序,使用他们的 GUI 这个过程可能会更容易。我自己没有尝试过，如果你要走这条路，请告诉我们效果如何:)

## 1.安装虚拟

如果您已经有了一些虚拟环境，或者正在使用 Anaconda，请确保接下来的步骤是从所有这些环境之外的**执行的。**

## 2.安装 Python

你可以从[官方网站](https://www.python.org/)下载 python，例如`python3.7.3`到[这里](https://www.python.org/downloads/release/python-373/)。

您应该下载的文件名为`Windows x86–64 executable installer`，或者如果出于某种原因您使用的是 32 位 windows，则名为`Windows x86 executable installer`。

下载完成后，打开可执行文件，会出现安装提示。

*   您不希望将新的 python 添加到您的路径中，因为我们将在同一台计算机上有多个 python 版本，并且我们希望每个应用程序只知道一个 python 版本。
*   使用新 python 的默认建议位置，或者提供您选择的位置。不管怎样，记住这个位置，让我们从现在开始用`C:\<some_path>\Python37`来表示它。

![](img/a08862594fd05f141509f05ab4dc93d0.png)

## 3.创建虚拟环境

打开`Command Prompt`，或者如果你正在使用 Anaconda，打开`Anaconda Prompt`。

决定你希望你的 virtualenv 在哪里，比如
`C:\Users\<your_username>\Anaconda3\envs\<env_name>`。

输入:

`virtualenv C:\Users\<your_username>\Anaconda3\envs\<env_name> -p C:\<some_path>\Python37\python.exe`

## 4.更新 PyCharm 解释器

如果您正在使用 PyCharm，请打开您想要处理的项目(即/将用新的 python 版本编写的项目)，然后转到`File -> Settings -> Project -> Project Interpreter`按齿轮图标，然后再按`Add..`。

![](img/b0d6bfc96ef58468c9fe9a833f7453f8.png)

这将打开一个提示窗口，允许您定义一个新的解释器:

![](img/efad604c702e33b0ebcf3eed7e9346c5.png)

假设您正在使用代码检查，您可能需要告诉 PyCharm 要检查哪个 python 版本。转到`File -> Settings-> Editor -> Inspections -> Python -> Code compatibility Inspection`，确保顶部的方框指示您正在进行的具体项目，并勾选您的 python 版本的方框。

![](img/6c4e9e4d69e5a8484c52a395cfbddfdb.png)

If you don’t see your python version on the Options list, this might also be the time to update PyCharm… yup, happened to me too…

## 5.安装软件包

目前，您的`virtualenv`只包含关键包`pip`和`setuptools`。要安装更多软件包:

1.  打开`Command Prompt`或`Anaconda Prompt`，输入
    `C:\Users\<your_username>\Anaconda3\envs\<env_name>\activate.bat`
    或`activate env_name`**激活**你的虚拟人。我这辈子也不明白为什么我的一些美德对第一个有用，而对另一些有用。
2.  像平常一样使用`pip`安装软件包。
3.  **输入`deactivate`关闭**您的 virtualenv。

# 收场白

今天早上，当我决定用不同的 python 版本打开一个新项目时，我想，“是的，我就用一个 virtualenv”，因为互联网上说我可以“就这么做”。

好吧，它现在工作了，所以亲爱的互联网没有恶意，但说真的，“只是”真的有道理吗？仅仅因为我想进行正确的代码检查而重新安装-py charm*属于“公正”的范畴吗？？*

不管怎样，一路上我偶然发现了几个有用的指南，但每个都带我“只是”走了一步，所以我决定把它们都放在一个地方。

我希望我的旅程对你有所帮助，并希望我们都享受快乐的编码，尽可能少的 IT 摩擦:D

![](img/b3ee4ce52d4f31f78e6bcca23cb0d223.png)