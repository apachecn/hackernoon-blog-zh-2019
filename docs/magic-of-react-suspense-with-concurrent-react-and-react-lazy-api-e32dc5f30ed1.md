# react 悬念与并发 React 和 React.lazy API 的魔力

> 原文：<https://medium.com/hackernoon/magic-of-react-suspense-with-concurrent-react-and-react-lazy-api-e32dc5f30ed1>

![](img/42ffc281bd14a06eb3ecde3dbf3cf8df.png)

Magical land of React Suspense, Concurrent React and React.lazy API

[Dan Abramov](https://twitter.com/dan_abramov) 在 2018 年冰岛 JSConf 大会上的“[超越反应 16](https://www.youtube.com/watch?v=nLF0n9SACd4) ”演讲中说道:

> *我们已经建立了一种通用的方法来确保像用户输入这样的高优先级更新不会被呈现低优先级更新所阻塞。*

让我们了解这意味着什么，并了解即将推出的一些新功能，其中一些已经…