# 通过可信执行保护拜占庭容错

> 原文：<https://medium.com/hackernoon/protecting-byzantine-fault-tolerance-with-trusted-execution-743ab505cb2b>

![](img/6c2fb6a00032fdff655d97e73717daa4.png)

你可能熟悉非 BFT 共识协议，比如 Paxos 和 T2 Raft。这些协议可以容忍多达 1/2 的节点发生崩溃故障，但这些故障不包括恶意行为。Zookeeper、Consul 和 etcd 就是使用 Paxos 和 Raft 的应用程序的例子。

拜占庭容错(BFT)是分布式协议的一个属性，它保证诚实的各方“在相同的页面上”(看到相同的状态)，尽管在对等(P2P)网络中存在攻击者。

与非 BFT 共识协议相比，当少于 1/3 的节点不诚实或恶意时，现有的 BFT 协议是安全的。
[Tendermint](https://tendermint.com/) 是一个状态复制软件，使用基于 PBFT 的拜占庭容错(BFT)复制协议。它通过应用程序区块链接口(ABCI)与需要拜占庭容错状态复制的应用程序集成。

鉴于过去加密货币和区块链协议的经验，我们已经看到单个矿工获得了超过一半的哈希能力——这是一个非常可靠的假设。

当网络没有很多节点时，关于攻击者控制超过 1/3 的节点的可能性的担忧更加严重，这在引导新的 P2P 网络时很可能发生。

确保每个节点的完整性和真实性非常重要。 [Anjuna Runtime](http://www.anjuna.io) 支持在英特尔 SGX 或 AMD SEV 等可信执行环境中运行 Tendermint 节点，使得攻击者极难接管验证器节点来提议、预投票、预提交或提交非法事务。

# 发球多样化

虽然 Tendermint 可用于在用不同语言编写的应用程序之间复制状态，但它也可用于同步使用不同可信执行环境执行的应用程序。

它是 Tendermint 将不同编程语言之间的桥梁这一愿景向可信执行环境多样化的自然延伸。

# 与 Anjuna 整合

Anjuna 将证明集成到标准 TLS 认证中，无需修改 Tendermint 源代码即可运行节点网络。它可以很容易地应用于现有的 Tendermint 部署，以提高安全性。