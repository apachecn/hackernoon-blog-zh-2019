# 零知识证明应用程序演示

> 原文：<https://medium.com/hackernoon/zero-knowledge-proof-application-demo-2a457cfc73c1>

ZKP 是一项有前途的快速发展的技术，很快将成为我们许多安全协议的关键部分。

在这篇文章中，我们将接触到 libsnarks、truffle 和 docker。

在我们做任何事情之前，让我们确保我们拥有运行我们的应用程序所需的所有程序。如果您在运行下面的块时遇到问题，请在继续操作之前安装缺少的 dep。

```
docker -v # Docker version 18.09.1, build 4c52b90
git --version # git version 2.17.2 (Apple Git-113)
node -v #…
```