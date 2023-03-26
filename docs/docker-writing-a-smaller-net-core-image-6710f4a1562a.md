# Docker 使用多阶段构建编写较小的图像。网芯。

> 原文：<https://medium.com/hackernoon/docker-writing-a-smaller-net-core-image-6710f4a1562a>

![](img/e1c4fb5920d3ad9fba0566644c7f13d7.png)

Docker Logo

我一直用 docker 来玩我的小网站，但是 docker 文件/图像总是有点暴力。是时候探索一种更有效的 docker 文件了！

# Docker 概述

Docker 是一种在容器内构建应用/基础设施/代码的方法；一个容器是一个自我…