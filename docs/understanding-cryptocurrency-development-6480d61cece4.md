# 了解加密货币的发展

> 原文：<https://medium.com/hackernoon/understanding-cryptocurrency-development-6480d61cece4>

## 一个基本的令牌只是一个表和四个方法。

![](img/412a0b7242cbcbe6c5bcd4bbfecc9f53.png)

Photo by [David McBee](https://www.pexels.com/@davidmcbee?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/ethereum-and-bitcoin-emblems-730569/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

> “你读得越多，你知道的事情就越多。你学得越多，去的地方就越多。”—苏斯博士

大多数商业领袖不知道的一个事实是，在以太坊开发一种加密货币出奇的简单。开发人员发现，一旦他们开始编写智能合约。对于那些没有信心编码的人来说，加密货币仍然神秘而复杂。

作为 TechHQ 的首席架构师，我的部分工作是用 CEO 们理解的术语向他们传授区块链技术，这样我们就可以根据他们的业务流程设计技术。我经常看到他们认为加密货币太复杂，他们无法理解。

在本文中，我将向商业受众解释什么是加密货币和代币。我也将向您展示代码，因为大多数非开发人员一旦看到加密货币的代码适合一个页面，就会感到不那么害怕了。

> 大多数商业领袖不知道的一个事实是，在以太坊开发一种加密货币出奇的简单。

请注意，本文指的是以太坊中的加密货币和代币，在一定程度上也指其他分布式计算机，如 Hyperledger。比特币等运行在专用区块链上的加密货币开发起来要复杂得多。

你准备好了吗？牵着我的手，走进兔子洞…

# 加密货币

在研究加密货币时，每个人都会第一次听说 ERC20 这个术语。ERC 是[以太坊 GitHub 知识库](https://github.com/ethereum/EIPs/labels/ERC)中的一个标签，意思是某人想要对某事的正式反馈。在以太坊的早期，有人希望得到加密货币规范的反馈，并得到了 [ERC20](https://en.wikipedia.org/wiki/ERC-20) 标签。历史往往就是这样被创造的，没有经过太多思考。

请看一下来自 OpenZeppelin 的 [ERC20 实现。该合约是大多数加密货币的基础。如果您不理解太多的代码，也不要担心。请注意，它只有 228 行代码，其中三分之二是注释。没那么复杂。](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/token/ERC20/ERC20.sol)

您可以按原样部署 [ERC20](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/token/ERC20/ERC20.sol) 合同，并且您将拥有一种加密货币。很可能你想添加一些功能，如[令牌符号和小数](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/token/ERC20/ERC20Detailed.sol)，或[转账限制](https://github.com/ethereum/EIPs/issues/1404)，或[停止向无效账户转账](https://github.com/Dexaran/ERC223-token-standard)。所有这些变化仅仅是向一个 [ERC20](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/token/ERC20/ERC20.sol) 契约添加更多方法的规范。他们之间没有什么不同。

> 你可以按原样部署 RC20 合约，你将拥有一种加密货币。

如果你需要给加密货币添加功能，你不需要在互联网上搜寻一些你想做的 ERC 代币。加密货币是软件应用，而且非常简单。例如，如果你需要跟踪谁是你的令牌的持有者，你只需[向你的加密货币添加一个数据结构](https://hackernoon.com/implementing-staking-in-solidity-1687302a82cf)来存储这些信息，并在需要时更新它。你可以编造一些东西。狂野一点。

概括地说，以太坊中的加密货币是:

*   简单的软件应用程序
*   这可能遵循一些标准
*   可能包括 [ERC20](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/token/ERC20/ERC20.sol)
*   可能还带有一些自定义代码。

在下一节中，我们将深入代码，进一步分解它。我们将去掉更多的复杂性，来看看什么是令牌这个非常简单的核心。

# 城里最简单的加密货币

OpenZeppelin 的 [ERC20 实现有三个契约变量和 13 个方法。协定变量是存储在协定中的数据。这些方法是协定可以对协定变量和从外部传递的数据做什么。](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/token/ERC20/ERC20.sol)

将契约视为运行在云中的应用程序，您可以激活它的方法，但不能激活其他任何东西。契约坐在那里，等待有人提出要求，然后返回一个值或更新其契约变量。

为了编写一个非常简单的加密货币，我将从 [ERC20](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/token/ERC20/ERC20.sol) 中取出一些东西，并丢弃其余的。

我将在我的合同中保留一个余额分类账。在我的加密货币中，每个地址都与某个值相关。换句话说，每个账户持有一些代币。

我将只实现四个方法来检查余额、创建令牌、销毁令牌和转移令牌。有了这个功能，我就有了一种可以用来支付的加密货币。整个合同如下:

```
contract Cryptocurrency {
  mapping (address => uint256) private _balances; // Return the amount of tokens held by an account.
  function balanceOf(address account) 
    public view returns (uint256) 
  {
    return _balances[account];
  } // Moves an amount of tokens from a sender to a recipient.
  function transfer(address send, address recv, uint256 amount)
    internal
  {
    _balances[send] = _balances[send] — amount;
    _balances[recv] = _balances[recv] + amount;
  } // Creates an amount of tokens and assigns them to an account.
  function mint(address account, uint256 amount) 
    internal
  {
    _balances[account] = _balances[account] + amount;
  } // Destroys an amount of tokens from an account.
  function burn(address account, uint256 amount) 
    internal
  {
    _balances[account] = _balances[account] — amount;
  }
}
```

以太坊中的加密货币可以是简单的数据结构，包含每个账户的余额和一些管理这些余额的函数。

既然您已经知道了什么是加密货币，那么您可以根据您的业务模型开始添加更多功能。至少你要执行 [ERC20](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/token/ERC20/ERC20.sol) 标准，使用 [SafeMath](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/math/SafeMath.sol) ，以上合同为教育目的。

# 代币

我喜欢用*加密货币*和*令牌*作为两个不同的术语。我喜欢说代币是你可以拥有的东西，加密货币是你可以拥有并可以用来支付东西的东西。

不是加密货币的令牌的一个例子是用 [ERC721](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/token/ERC721/ERC721.sol) 标准实现的合同。当人们谈论标记化时，你会听到这个。 [ERC721](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/token/ERC721/ERC721.sol) 标准的目的是让它的每一个令牌都代表区块链中的一项实物资产。

用技术术语来说，它用一个记录每个单独令牌的持有者的表来代替 ERC20 的余额分类账:

```
mapping (uint256 => address) private _tokenOwner;
```

有了这个令牌，我不会说 Bob 有 100 个令牌，我会说 Bob 有令牌#100、#42 和#1337，例如。针对 ERC20 描述的四种方法略有不同，但本质上这就是 ERC20 加密货币合约和 [ERC721](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/token/ERC721/ERC721.sol) 令牌合约之间的区别。

到目前为止，您已经看到加密货币和令牌是一个表，其中包含一些操作数据的方法。令牌的描述非常灵活，有许多东西可以描述为令牌。

当我设计一个解决方案时，我可能会养成创建许多不同令牌的习惯。这很容易做到，因为它们只是将用户链接到他们所拥有的东西的表格。当 ERC 标准非常合适时，我会使用它们，但通常我会从头开始编写我的令牌合同。

> 令牌的描述非常灵活，有许多东西可以描述为令牌。

一个象征付款，另一个象征投票权，另一个象征某种资产等等。营销团队总是抗议——包含四个令牌的解决方案太复杂，难以销售！。他们是对的。我让他们的生活变得艰难。

修复真的很简单。如果一个软件设计有太多的标记，我会从描述中删除单词*标记*。我将拥有投票权、资产键、用户句柄，或者任何我能想到的不使用单词 token 并且不会引起注意的名称。大家都很开心。令牌只是一个表和一些方法。

# 结论

让加密货币成为可能的技术是复杂的。用这些工具创建加密货币的代码不是。区块链领域的商业领袖应该努力理解加密货币和代币的基础知识。突破表面的复杂性将帮助他们[将他们的商业想法传达给开发团队](https://hackernoon.com/i-was-the-blockchain-architect-for-a-dozen-projects-and-this-is-what-i-learnt-7a17b36bad02)。

在本文中，我将加密货币和代币的概念分解到最低限度，甚至低于大多数区块链应用程序背后众所周知的 ERC20 标准。从这个最小的起点开始，我已经展示了在添加业务功能之前，加密货币和令牌的核心非常简单。

> 你有区块链的解决方案吗？你知道你的业务，但却很难找到一个会说你的语言的技术专家。请随时[联系我](http://linkedin.com/in/albertocuesta)，我喜欢把商业和技术结合在一起。