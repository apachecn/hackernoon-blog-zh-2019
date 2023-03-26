# 将 openvpn 作为 systemd 服务运行。

> 原文：<https://medium.com/hackernoon/running-openvpn-as-a-systemd-service-545224782c72>

![](img/d06134c5a052d04cb11a89ceedf0ac78.png)

photo by: mike fettis

虚拟专用网风靡一时。所有酷小孩都在用。最酷的孩子只在他们的 linux 上使用终端。将 openvpn 作为 systemd 服务运行，并有一个交互式提示让您知道您的 vpn 正在运行，怎么样？

输入 systemd 和 PS1。诀窍是将 openvpn 命令作为 systemd 服务运行。然后设置为失败 15 秒后自动重启。如果我们在 opvn 配置文件中添加一行，*auth-user-pass/home/Linux user/pass . txt*，然后在所述 txt 文件中有 openvpn 用户名和密码，openvpn 将能够自动连接到 vpn 主机。最后，锦上添花的是更新我的 PS1(终端提示)改变颜色，如果 vpn 连接。这可以通过两种不同的方式实现。
1)。如果 traceroute 将第一跳解析为 10。* ip 地址然后我就知道我的 vpn 连接上了
2)。如果服务处于活动状态，那么我知道它正在运行。

深入的过程如下。

现在，vpn 服务将在启动时启动，并将一直运行。如果有问题，它会在 15 秒后自动重启。只是为了确保一切正常，因为我们总是想知道 vpn 是否在运行；)我们的终端提示会根据情况改变颜色。如果您不希望整个终端改变颜色，而只是有一个 ea 符号，可以做进一步的增强。如果你看看 magicmonty 完成的 git 集成，这是可以做到的。希望这是一个快速简单的阅读，并祝你所有的 VPN-ing 好运！

[](https://github.com/magicmonty/bash-git-prompt) [## magicmonty/bash-git-prompt

### 一个为 Git 用户提供的丰富而有趣的 bash 提示符

github.com](https://github.com/magicmonty/bash-git-prompt) 

链接！！

 [## 系统 d .服务

### systemd 服务可以通过""语法接受单个参数。这种服务被称为…

www.freedesktop.org](https://www.freedesktop.org/software/systemd/man/systemd.service.html) [](https://www.cyberciti.biz/faq/bash-shell-change-the-color-of-my-shell-prompt-under-linux-or-unix/) [## BASH Shell 改变 Linux 或 UNIX 上 Shell 提示符的颜色

### 如何在 Linux 或 Unix 操作系统下改变我的 shell 提示符的颜色？你可以改变你的颜色…

www.cyberciti.biz](https://www.cyberciti.biz/faq/bash-shell-change-the-color-of-my-shell-prompt-under-linux-or-unix/) [](http://brettterpstra.com/2009/11/17/my-new-favorite-bash-prompt/) [## 我最喜欢的狂欢提示——BrettTerpstra.com

### 我在终端做了很多。有时候，更容易。有时候会更快。有时候我宁愿把它打出来。无论如何…

brettterpstra.com](http://brettterpstra.com/2009/11/17/my-new-favorite-bash-prompt/)