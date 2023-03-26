# 为您的 EOS DApp 找到最佳的 API 端点

> 原文：<https://medium.com/hackernoon/find-the-best-api-endpoint-for-your-eos-dapp-7b7489cb6449>

## 任何 DApp 的关键部分是与 EOS 节点的 HTTP API 端点的交互。

所以，如果你想开发一个基于 EOS 区块链的 DApp，而不想用 HTTP API 端点运行你自己的节点，因为这是一个相当复杂的任务，需要大量的资源。你可以尝试使用一些像 eosinfra 或 dfuse 这样的服务，它们为你提供现成的 API，但是在某些时候这些服务并不便宜。

EOS 区块链的核心是你投票支持的块生产者，但他们不是网络上唯一提供公共 API 端点的，许多其他 BP 候选人也提供它们，所以为什么不使用它们呢？

我的想法是收集一份在 EOS 区块链注册的所有 BP 的列表，并找出它们所有的 API 端点，以便进一步测试。

我为这个任务写了一些脚本，但我还没准备好展示(如果你感兴趣，我会清理它并在某一天发布=)，所以我只向你展示收集的结果:

*   [HTTP API 端点](https://gist.github.com/akme/89a4e596587cb605b530bd825994a0db)
*   [HTTPS API 终点](https://gist.github.com/akme/fe1e2797aeac06480a1e287445109bc0)

现在我们需要找出具有最低延迟的端点，这样我们就可以有更好的性能(如果 BP 有好的节点)，但 find 主机不是一个选项，因为一些端点隐藏在 CDN 或负载平衡器后面。

API 是动态服务，缓存它是个坏主意，所以我们需要测量 HTTP 请求-响应时间来找出最近的(就网络而言)主机。

对于这个任务，我将使用[变得更近](https://github.com/akme/get-closer)(这是我为此类任务编写的)。

测试的逻辑非常简单，只需测量 HTTP 请求时间并与其他时间进行比较。

对于 HTTP，您只需运行:

```
$ get-closer http -f [https://gist.githubusercontent.com/akme/89a4e596587cb605b530bd825994a0db/raw/db790fa2ffc1f9aef63a6b4b83b16a6badc71276/eos.http.txt](https://gist.githubusercontent.com/akme/89a4e596587cb605b530bd825994a0db/raw/db790fa2ffc1f9aef63a6b4b83b16a6badc71276/eos.http.txt)
```

你会得到这样的结果:

对于 HTTPS:

```
$ get-closer http --ssl -f [https://gist.github.com/akme/fe1e2797aeac06480a1e287445109bc0](https://gist.github.com/akme/fe1e2797aeac06480a1e287445109bc0)
```

结果:[https://gist . github . com/akme/73c 6826436968 bb 34 a 6 EDF 2468 da 7d 86](https://gist.github.com/akme/73c6826436968bb34a6edf2468da7d86)

# 结论

这是一个非常原始的想法，但是我对你的反馈很感兴趣。你认为基于这样的测量来选择终点怎么样？