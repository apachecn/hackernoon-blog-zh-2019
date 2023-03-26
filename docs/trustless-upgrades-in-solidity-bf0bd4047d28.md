# 可靠性的可靠升级

> 原文：<https://medium.com/hackernoon/trustless-upgrades-in-solidity-bf0bd4047d28>

## 可升级性并不总是缺陷。

罗布·希钦斯和阿里·阿扎姆

![](img/bdff3633d2b05b1b807716b4d0613956.png)

Photo by [James Hammond](https://unsplash.com/@jamesmhammond?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

几周前，我们出版了[自毁是一个错误](https://blog.b9lab.com/selfdestruct-is-a-bug-9c312d1bb2a5)，它本身的灵感来自于[可升级性是一个错误](/consensys-diligence/upgradeability-is-a-bug-dba0203152ce)作者是 Consensys Diligence 的 Steve Marx。两篇文章的主旨都是，改变合约会破坏不变性。有人可能会问——如果特权用户可以随心所欲地修改代码，那么区块链有什么特别之处呢？

## 如果你没有失眠，你可能做错了

以太坊 Dapp 开发人员必须处理一种不熟悉的范式(区块链)和一种不熟悉的语言(稳固性)，以便为不熟悉的问题(博弈论、业务流程再造、市场等等)创建不熟悉的解决方案(治理、资产、权利)。难怪可升级性是一种受欢迎的可能性。

## 可升级的合同来拯救！

2016 年，Coloney 发表了[在 Solidity 中编写可升级合同](https://blog.colony.io/writing-upgradeable-contracts-in-solidity-6743f0eecc88/)。他们“永久存储”模式的要点是

> …将您的数据存储与代码的其余部分隔离开来，并使其尽可能灵活，这样就不太可能需要升级。

冒着过分简化的风险，你可以拥有一个如此简单、灵活、解决得如此好的数据存储`contract`，以至于你可以确信它永远不需要*修改。至少，是这样的想法。例如，您可能决定使用一个键/值存储，然后停止。*

考虑一些简单的事情:

```
contract EternalStorage is Ownable {   mapping(bytes32 => uint) UIntStorage; function setUintValue(bytes32 record, uint value) public {
     UintStorage[record] = value;
  } function getUIntValue(bytes32 record) constant returns (uint){
    return UIntStorage[record]; 
  }
} // the end
```

当然，我们可以像那样回答问题，称之为胜利。我们可能会庆幸自己至少应该永远这么做。

另一个`contract`、*逻辑*、*拥有*存储器。所有权可以转让给另一份*逻辑*合同。这种结构提供了用新逻辑替换旧逻辑的可能性。瞧啊。只要把数据层做好，你就能拥有一个可升级的`contracts`系统。看看 [RocketPool](/rocket-pool/upgradable-solidity-contract-design-54789205276d) 中一个在有多个合同的系统中采用这种方法的项目的例子。

对这种方法的一个经常性的批评是上升到逻辑层次的复杂性。所有持久化的东西都需要以存储`contract`能够理解的方式来描述。这可能会增加复杂性并降低可读性，因为将所有变量名、数组和映射元素减少到键/值存储的唯一键会在以这种方式存储所有内容的契约中增加额外的步骤。

例如，你不能说:

```
counter++
```

你可能会说:

```
counter = datastore.get(keyForCounter);
datastore.set(keyForCounter, counter++);
```

不恐怖，但是视觉噪音加起来。

## 委托调用、库和代理模式

通过 Homestead hard fork，EVM 获得了一个名为`DELEGATECALL`的操作码。顾名思义，一个契约将一个功能“委托”给另一个契约。Solidity 团队添加了使用它的`library`。

一个`library`看起来很像一个`contract`，但是有一个可能奇怪的限制。它们不能保存任何数据。那是因为它们运行在决定`DELEGATECALL`的`contract`的上下文中。因此，如果`contract I`决定将`DELEGATECALL`传递给一个名为`makeBed`的`library`函数，那么将生成 I 的床(我的)，而不是`library`的床。如果我想升级`makeBed`进程，我需要一个新的`library`并且我需要`contract`来开始委托对新库的调用。

你可以有一个 main `contract`，将所有重要的事情委托给一个或多个`libraries`。

```
pragma solidity 0.5.1;library DoStuff {

    struct DataStore {
        mapping(bytes32 => uint) value;
    }

    function setVal(DataStore storage self, bytes32 key, uint value)
        internal 
    {
        self.value[key]= value;
    }

    function getVal(DataStore storage self, bytes32 key) 
        internal view returns(uint) 
    {
        return self.value[key];
    }
}contract Switchboard {

    using DoStuff for DoStuff.DataStore;
    DoStuff.DataStore data;

    function set(bytes32 key, uint value) public {
        data.setVal(key,value);
    }

    function get(bytes32 key) public view returns(uint) {
        return data.getVal(key);
    }

}
```

这不是一个关于`libraries,`的完整教程，但这里有一些快速的提示:

*   实际值存储在`SwitchBoard`中，但是`library`定义了它理解的数据类型的布局。
*   一个存储指针给了`library`一些工作。`library` 直接写入`Switchboard's`存储器。你可能会说，`Switchboard`在它传递一个存储指针时授权它这么做。
*   这里有语法上的好处，这就是为什么 a)似乎缺少了一个参数(计算一下), b)函数`library`被神奇地调用为`data`的方法，而`data`在`library`中被定义为`struct`(仅用于布局)的一个实例。
*   `contract`的功能非常简单。他们只是接受输入，并把它们传递给一个应该知道它在做什么的库。

关于`libraries`的一个好处是，一旦你理解了整体思想，它们不需要对熟悉的`contract`编码风格做太多调整。简而言之，我们有一个`library`，它会“做”你扔给它的任何适当布局的`DataStore`。

## 功能也需要升级

简而言之，让我们把允许我们换入一个新的`library`的机制放在一边，只需多花一点努力就可以完成。这个我们就不细说了，因为还有一个更大的问题。如果我们想添加一个`SwitchBoard`没有的功能怎么办？如果我们想在现有的函数中添加一个参数，该怎么办？

假设函数签名可以在任何时候都预先计算出来是不完全合理的。未来是不可知的，这也是为什么我们会对升级感兴趣的部分原因。

“代理”契约可以通过将*任何东西*转发给实现契约，并返回*任何东西*来解决灵活的接口。汇编，就其本质而言，对作者来说有点过时了，但是汇编代码已经存在了几年了。这是一个非常流行和众所周知的模式。

```
function () external payable {
  assembly {
    let ptr := mload(0x40)
    calldatacopy(ptr, 0, calldatasize)
    let result := delegatecall(
      gas, 
      implementationAddress, 
      ptr, calldatasize, 0, 0)
    let size := returndatasize
    returndatacopy(ptr, 0, size)

    switch result
    case 0 { revert(ptr, size) }
    default { return(ptr, size) }
  }
}
```

*   首先，它是一个后备函数，所以当代理契约找不到匹配时运行。在这种情况下，每次都是故意的。它是`payable`因为它应该支持`payable`实现函数才是有用的。
*   它复制输入的`calldata`并将其传递给`delegatecall`，后者将它发送给`implementationAddress`，希望它知道该做什么。`implementationAddress`是一个`contract` `address`我们就目前而言，预先假定是成立的。
*   也许`implementationAddress`不知道该做什么，或者也许它恢复了。在这种情况下,`delegatecall`将返回`0/false`,所以这个实现检查它，如果`implementationAddress`想要恢复，就恢复
*   如果操作成功，该函数返回`implementationAddress` 响应。它不知道输入或输出意味着什么，但是您可以在代理契约的地址使用实现 `contract` *的 ABI。这种轻微的手工创建了一个令人信服的直接与实现契约一起工作的假象，而事实上，人们是通过代理传递一切。*

这种模式的一个非常好的特性是`implementationAddress`是一个`contract`，它不一定知道它是可升级的。它使用通常的语法，以通常的方式布置了状态变量，但是，事实上，所有东西都被写入代理`contract's`状态。

事实上，这非常受欢迎，以至于 Zeppelin 将其作为 ZeppelinOS 的透明代理的基础。你也会在 Gnosis Safe 中找到它。

您将发现的一个重要注意事项是，如果替换实现有缺陷或有恶意，很有可能会覆盖重要的状态数据。重要的是建立一个框架和/或实践来确保这样的事故是不可能的。

即使一切都像预期的那样工作，它似乎也没有解决由可升级性本身引起的一个重要问题。让我们回到更高层次的考虑，可升级性本身减少了不变性。让我们考虑一下我们可以做些什么来减轻这种担忧。

## 什么时候可升级性不是 Bug？

在这个概述中，我们已经触及了永久存储、开关板和透明代理模式，如果你愿意深入研究更多关于*如何*的内容，你可能会对其中任何一个感兴趣。

> 你还是会遇到一个令人困扰的问题。如果你的合同是可升级的，为什么有人要相信它呢？

毕竟，这种形式的软件不就是为了创造确定性吗？如果特权用户可以更改实现，确定性会发生什么？它消失了。你还不如用你最喜欢的语言编写软件，部署在你最喜欢的平台上，用传统的方法让你的用户相信，你自己不会做任何不利于他们的事情。很可能你会省去很多麻烦。

作者第一次听说 Ali Azam 解决这个问题的方法是在 DevCon IV 上。他的方法是需要用户的同意。请注意，不是所有的用户。这不是一个民主的过程，可能会让少数用户被迫接受他们不想要的升级。

> 每一个用户都应该自己决定是否继续他们已经签署的合同，或者自愿将*迁移到新版本。*

## 注册处

这是一个更细粒度的升级过程，每个用户都可能使用他们最喜欢的实现`contract.`。注册中心`contract`负责记录可用的实现`contracts`，以及每个用户的首选实现。总之，注册中心保存了每个用户想要使用的版本。

该设置如下所示:

```
mapping(address => address) userImplementationChoices;
```

前面介绍的透明代理通过查找用户的首选实现，然后继续使用前面描述的代理机制来适应这种情况:

```
address implementationAddress = userImplementation(msg.sender);
```

函数`userImplementation(address)`计算出一些细节并为用户返回一个实现`contract`地址*。是用户决定他们是否想要选择某个实现或选择加入来推送更新。*

> 可信升级意味着用户决定。

敏锐的读者可能会意识到，不同的支持契约可能意味着需要不同的客户端组件，比如 UI。是的，的确如此，要知道哪个 ABI 在玩，就去问注册处。

```
function userImplementation(address user) public view returns(address) {
```

## 通过设计，更加分散

让每个人使用他们喜欢的任何`contract` 可能是疯狂的，尤其是当他们在一个系统中共享公共存储时。不，我们想把这限制在开发人员和 QA 专业人员同意兼容的有效选择上。

注册中心允许特权用户批准用户可以选择的新合同，*如果他们愿意的话。*“特权用户”可以是一个治理合同，在该合同中，用户会考虑甚至应该接纳哪些申请人。

该流程可以轻松容纳明确的提议和批准。

*   有人部署了新的实现。
*   有人*提议*将`0x123...`处的`contract`添加到注册中心的有效实现列表中。
*   决定，添加到注册表:

```
function addImplementation(address implementationAddress) 
  public 
  onlyOwner 
```

重要的是，没有人可以被迫离开最初的合同，就像他们签约时那样。而且，用户不必相信可升级性特性不会对他们不利。它旨在促进与此相反的过程——一个透明的过程，在该过程中，审查代码和公共审议是规范，没有规避商定的治理策略和控制版本准入的可能性，并且用户自己决定他们更喜欢使用哪个被认可的版本。

## 默认实现和其他管理问题

除了基本的运作原则之外，剩下的问题主要是管理上的。用户可以:

*   选择加入，并接受出现的新版本，或
*   选择退出，不接受新版本，除非他们手动更改他们的首选项。
*   所有新用户的默认选择(选择加入或选择退出)是部署后不能修改的注册表设置，因为改变用户对他们所拥有的选择和他们可以依赖的流程的理解将是一个错误。
*   紧急召回功能允许特权用户否决一个版本。这将把*受影响的用户，*只有那些使用过时版本的用户，迁移到另一个推荐的选项。特权用户控制建议的版本，称为默认实现。召回是用户可以被强制迁移到其他地方的唯一情况。另一种方法是对受影响的用户禁用实现，直到他们明确选择另一个版本。我们为了用户体验的利益而设计了这一点，期望治理能够阻止恶意的实现出现在列表中。

## 版本样式指南

该模块包含一个可继承的`contract`，它解决了一个安全问题，即任何人都不应该直接调用实现契约(即不通过代理)。这毫无意义，因为数据驻留在代理中，而不是实现中。一个修饰符，`onlyProxy`阻止了不适当的调用。

一般来说，实现的后续版本应该继承以前的实现。这样做可以确保状态布局总是以累加的方式发展。覆盖函数是可以接受的。添加新函数和新参数是可以接受的。禁用功能是可以接受的(用`revert("deprecated");`覆盖)。

示例实现包括一个 Hello World `contract`和第二个版本`HelloUniverse`。上述问题都以最小的侵扰性得到解决:

```
contract HelloWorld is Upgradable { function doSomething() ... onlyProxy ...{
    ...contract HelloUniverse is HelloWorld {
  ...
```

就是这样。

等等。还有一件事。注册中心使用一个任意的叫做“componentId”的 T2。这将在部署代理时生成，代理本身部署相应的注册中心(每个代理一个注册中心，每个可升级代理一个注册中心`contract`)。

`componentId`很容易用`Registry.componentId()`检查，它的值必须传递到每个可升级的合同中。例如，在 Hello World 中:

```
constructor(bytes32 componentUid) Upgradable(componentUid) public {
```

这有助于进行基本的检查，以帮助捕捉部署时的错误，例如在生产中将错误组件的实现放入注册表中。注册中心将拒绝没有声明实现预期组件的实现`contracts`。

## 变体

示例代码将实现视为一个用户一个用户的问题。上下文感知代理的思想可以以其他方式应用。例如，考虑离线数据和在线验证`contracts`。有可能使用一个可升级的验证`contract`来获得来自链外资产的信号——一个文档说，大约*使用验证版本 3.2 来解析我。*

## 代码

试验性回购，[https://github.com/rob-Hitchens/TrustlessUpgrades](https://github.com/rob-Hitchens/TrustlessUpgrades)有时可能比社区回购，[https://github.com/ali2251/Upgradable-contracts](https://github.com/ali2251/Upgradable-contracts)提前几个提交

阿里的无信任升级文档和研讨会讲义可在 https://docs.upgradablecontracts.com/[在线获得。](https://docs.upgradablecontracts.com/)

[*Rob Hitchens*](https://ethereum.stackexchange.com/users/5549/rob-hitchens)*是加拿大智能合约设计顾问，以太坊智能合约审计师*[*solidified . io*](https://www.solidified.io)*和以太坊、Hyperledger Fabric、Hyperledger 锯齿湖、Corda、Quorum 和 Tezos bootcamps 的课件合著者和导师由*[*B9 lab*](https://www/b9lab.com)*。*

[*阿里·阿扎姆*](https://www.linkedin.com/in/ali-azam-01112b71/) *是*[*vault platform*](https://vaultplatform.com/)*的资深区块链开发者，之前在伦敦大学国王学院工作了两年多，在伦敦大学国王学院、Devcon4、Blockercon 等许多地方举办过各种研讨会和主题演讲。阿里是区块链每月区块链开发者训练营的高级导师和核心成员，并领导在伦敦举行的智能合同升级研讨会。*