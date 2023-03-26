# 翡翠发球运动员

> 原文：<https://medium.com/hackernoon/jade-service-runner-bd5ca222b7fc>

## 分散服务开发的桥梁

![](img/112052ec781787a679d761cca991aa92.png)

# TLDR 版本

以太坊经典实验室核心(ECLC)团队[最近创建了 Jade Service Runner](https://github.com/etclabscore/jade-service-runner) ，一个基于 JSON-RPC 服务的服务管理器。基于最近 [OpenRPC 规范](https://open-rpc.org)的成功，Jade-Service-Runner 是该团队新的 DApp 开发工具 Jade 套件的第一个版本。

核心开发者赞·斯塔尔说，*“Jade Service Runner 是一个软件开发工具，它允许开发者支持用户运行他们自己的基础设施。Service Runner 旨在减轻用户的认知负荷，同时为开发人员提供良好的特性，使连接到服务变得容易。*

**这不仅仅是以太坊经典；以太坊也将受益于这个新工具，因为它是 JSON-RPC 交互的完美接口。**

> Service Runner 是一个自以为是的 JSON-RPC 服务管理器，它为基于 JSON-RPC 的服务提供后台化、安装和发现。
> 
> Service Runner 帮助开发人员访问用户在本地运行的服务。它为用户提供了一个管理和安装工具，允许开发人员发现服务并可靠地请求访问这些服务。

# 翡翠套房

ECLC 团队显然专注于构建整个 p2p 格局。通过他们的第一个版本 OpenRPC，**该团队正在创建基础开发层**，这是区块链生态系统中非常缺少的。

> Jade suite 将使在以太坊和其他 p2p 技术上构建应用程序变得简单，就像使用 rails 等流行的 web 框架一样。**它不仅仅是一套工具，还是应用开发的范例**。范式的第一个基本原则是用户应该能够选择最适合他们的安全模型。我们通过将以太坊分解成具有特定角色的部分，定义各个组件的规范来实现这一点。应用程序开发人员不需要关心用户在哪里运行哪个版本/哪个供应商的 xyz。—扎克·贝尔福德，ECLC 开发商

哦，如果你没有跟上 OpenRPC **的步伐，这个规范现在支持 Golang。**

# 翡翠发球运动员

ECLC 的技术协调员 Stevan Lohja 解释道:“Jade-Service-Runner 是一个开发者可以用来在后台运行服务的工具。例如，DApp 开发人员需要 mainnet 或 testnet 来部署 dapps，这样他们就可以告诉服务运行程序运行 Geth 或他们整合到环境中的任何其他服务”。正是这种对简单性的关注，团队认为会引起开发社区的共鸣，使 Service Runner(以及整个 Jade 套件)成为向前发展的基本工具箱。

该团队对该工具的积极接受和使用非常有信心，他们已经在展望未来。

> 一个潜在的长期策略是允许 jade service runner 成为一个 p2p 节点，使用 open-rpc discover 创建一个人们可以连接的分散服务网络。然后，服务运行器将基本上充当网关，让人们使用彼此的服务，或者限制他们的交互，只使用他们的本地连接。—ECLC 开发商赞·斯塔尔

# 更好的 dApp 开发

Service Runner 缩短了 dApp 的开发周期，除了依赖本地运行的 JSON-RPC 服务之外，还减少了运行用户本地服务所需的步骤。

**为了有效地做到这一点，Jade Service Runner 支持以下功能:**

*   允许 dApp 开发者指定他们想要使用的服务
*   为要运行的服务提供默认值
*   为用户提供简单的安装路径
*   提供对服务运行程序运行的预先存在的服务的可靠发现
*   为服务运行器功能以及底层服务提供 OpenRPC 接口
*   允许 dApp 开发人员从服务中检索可靠的 JSON-RPC 连接信息
*   提供用于开发应用程序的类型化接口

# 入门指南

使用 npm 安装`jade-service-runner`

```
npm install -g @etclabscore/jade-service-runner
```

它还有一个 javascript 客户端:

```
npm install @etclabscore/jade-service-runner-client
```

然后要求它进入任何模块。

```
const { ServiceRunner } = require('@etclabscore/jade-service-runner-client');
const ERPC = require('@etclabscore/ethereum-json-rpc');
const serviceRunner = new ServiceRunner({ transport: { type: "http", port: 8002, host: "localhost" } });
const serviceName = 'multi-geth';
const successful = await serviceRunner.installService(serviceName);
if (successful === false) throw new Error('Service not installed')
const serviceConfig = serviceRunner.start(serviceName, 'kotti');
const erpc = new ERPC(serviceConfig);
erpc.getBalance("0x0DEADBEEF");
```

要运行服务运行程序:

```
jade-service-runner
```

# 支持的服务

目前它支持以下环境中的`multi-geth`:

*   `mainnet (ETC)`
*   `kotti`
*   `ethereum`
*   `goerli`
*   `rinkeby`

# Jade-Service-Runner 和 OpenRPC 的更多资源

[](/etclabscore/using-openrpc-mock-server-to-test-against-an-ethereum-json-rpc-api-50b86b6d02d6) [## 使用 OpenRPC 模拟服务器测试以太坊 JSON-RPC API

### open-rpc-mock-server 允许开发人员在本地和轻量级环境中运行和测试他们的 API。

medium.com](/etclabscore/using-openrpc-mock-server-to-test-against-an-ethereum-json-rpc-api-50b86b6d02d6) [](https://github.com/etclabscore/jade-service-runner) [## etclabscore/jade-service-runner

### 翡翠发球跑者。在 GitHub 上创建一个帐户，为 etclabscore/jade-service-runner 的开发做出贡献。

github.com](https://github.com/etclabscore/jade-service-runner) 

**想进一步了解 Jade Service Runner 和 OpenRPC 吗？**

![](img/31fa31b798d46924ba4c5db8073ce378.png)

[**以太坊经典实验室**](https://medium.com/u/b74e14157605?source=post_page-----bd5ca222b7fc--------------------------------) **正与 ETC 开发者一起主持一个工作坊，** [**【赞斯塔尔】**](https://twitter.com/zanecstarr) **。如果你在旧金山湾区，带上笔记本电脑和代码参加这个** [**必须参加的活动**](https://www.eventbrite.com/e/jade-service-runner-tickets-63457913327) **！**

[](https://www.eventbrite.com/e/jade-service-runner-tickets-63457913327) [## 翡翠发球运动员

### 工具描述:Service Runner 是一个固执己见的 JSON-RPC 服务管理器，它提供妖魔化、安装…

www.eventbrite.com](https://www.eventbrite.com/e/jade-service-runner-tickets-63457913327) 

有兴趣更多地参与 ETC 吗？我们致力于加速以太坊经典的开发，需要您的帮助！请联系我们，看看您今天如何能更多地参与进来！

**我们的团队链接:** [**关于**](https://etclabscore.github.io/etclabscore/)**[**Github**](https://github.com/etclabscore)**[**中**](https://medium.com/etclabscore)**[**推特**](https://twitter.com/etclabscore)******

******来和我们一起聊聊** [**不和**](https://discordapp.com/invite/NgzMPaj)****