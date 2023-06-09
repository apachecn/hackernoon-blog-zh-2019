# 如何在 AWS 上玩 PUBG

> 原文：<https://medium.com/hackernoon/how-to-play-pubg-on-aws-db2e75fa599b>

![](img/7ad45517f423e04efb6eee6b1876f40c.png)

AWS GPU 实例以深度学习为目的而闻名，但它们也可以用于运行视频游戏。本教程介绍了如何设置自己的 EC2 GPU 优化实例来运行最畅销和最受欢迎的游戏“**玩家未知的战场** ( **PUBG** )”。

首先，确保你在离你最近的 AWS 区域，选择**微软 Windows 服务器**作为 AMI 和设置实例类型为 *g2.2xlarge* 。该实例由 Nvidia Grid GPU(**Kepler GK 104**)、来自**英特尔至强 E5–2670**的 8 个硬件超线程和 15GB 内存支持。

![](img/fa13bfc8a251b4dc8e1f71cd6945c762.png)

> 对于资源密集型的游戏，你应该使用下一代 GPU 实例:P2、P3 和 G3(最多 4 个 NVIDIA Tesla M60 GPUs)。

完成后，单击“**启动实例**”，您应该会看到一个屏幕，显示您的实例已经创建:

![](img/76774ab6df7aff2bc757898cb199708d.png)

若要连接到您的 Windows 实例，您必须检索初始管理员密码，并在使用远程桌面连接到您的实例时指定此密码:

> 在尝试使用远程桌面连接登录之前，您必须打开连接到您的实例的安全组上的端口 3389

![](img/3874ff96a77e2cf6e4722035fa0d4ffc.png)

连接后，安装 Chrome 后再安装 Microsoft Direct X11(这样可以节省很多时间):

![](img/3dc5ba24547446c50a7c545acb7daa9a.png)

接下来，安装[图形驱动程序](https://www.nvidia.com/download/driverResults.aspx/74642/en-us)以获得最佳游戏性能:

![](img/bfbb3f0aea32fac8f4f07968671560f2.png)

安装后，请确保重新启动实例以使更改生效:

![](img/851560a9c39fd922c8f1f4bd7f0e9b21.png)

然后，安装 [Steam](https://store.steampowered.com/about/) ，使用你的账号登录，从“**库**部分安装 PUBG:

![](img/95381db34a8a6208a33711f5dc216328.png)

您可以利用 AWS 的高网络性能(高达 10 Gbps 的带宽):

![](img/35465082d226ef618d68befbfa2ae7d9.png)

一旦游戏安装完毕，你就可以在你的虚拟化 GPU 实例上玩 PUBG 了:

![](img/add471dbbe61e84d3513dda9c0a90158.png)

你可以更进一步，使用 [Steam 家庭流媒体](https://store.steampowered.com/streaming/)功能将你的游戏从 EC2 实例传输到 Mac:

![](img/613121a3b8bea3cbcd96f72981b06536.png)

享受比赛吧！现在，您可以在连接到同一网络的任何设备上玩游戏:

![](img/0b1fe54a035934c6f62d0da241578cf0.png)

您可能想要基于您的实例烘焙 AMI，以避免下次您想要玩时重新设置它，并使用点实例来降低实例成本。此外，确保在一天结束时停止实例，以避免产生费用。GPU 实例的成本很高(磁盘存储也是有成本的，如果您的磁盘占用空间很大，这一点可能很重要)。