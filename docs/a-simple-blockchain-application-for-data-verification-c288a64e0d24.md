# 一个简单的数据验证的区块链应用。

> 原文：<https://medium.com/hackernoon/a-simple-blockchain-application-for-data-verification-c288a64e0d24>

## 了解如何用几行代码为以太坊编写一个可行的应用程序。

![](img/575bd76a771a810c92d9fe823ed17bd0.png)

Photo from Pixabay

> 所有伟大的事情都很简单，许多都可以用一个词来表达:自由、正义、荣誉、责任、仁慈、希望。——温斯顿·丘吉尔

# **简介**

在这篇文章中，我将描述我所知道的最简单的区块链应用程序，并给出代码示例。这对于那些从区块链领域起步的人来说很有用，对于更有经验的架构师来说也是一种设计模式。

想象下面的用例。你收到了一份经过几手的文件，你想确定它是真实的。该文档可能是您正在购买的汽车的服务手册，也可能是声明您正在购买的房子确实属于卖方的文档。什么都有可能。

数字地[签署文件](http://blogs.mdaemon.com/index.php/2018/05/29/encrypting-vs-signing-with-openpgp-whats-the-difference-2/)以确保它们没有被篡改并不是什么新鲜事。您可以获取文档的内容，并用它们生成一个加密签名，该签名与文档一起发送。

> 在这篇文章中，我将描述我所知道的最简单的区块链应用程序，并给出代码示例。

文档的接收者可以再次生成签名，并验证它是否与所提供的签名相匹配。匹配意味着文档没有改变。

这是你用 [MD5 校验和](https://en.wikipedia.org/wiki/MD5)得到的，使用起来非常方便。它的缺点是需要接收签名来验证文件的真实性。如果有人从中作梗，把文件和签名都改了，你也不会知道。

由于从个人那里获取数据并不容易被信任，有时第三方会介入提供记录服务以获取利润。这种利润动机是保持记录者诚实的原因。

可行，但不是完美的解决方案。记录保管者会自然地工作到一个权力的位置，在没有竞争的情况下榨取高额租金。更糟糕的是，如果经济激励发生变化，记录者可能会腐败。谁看着守望者？

# **区块链能为你做什么**

区块链数据存储是分散的、健壮的和不可变的。

*   分散意味着数据存储在几个不同人的硬件上集体运行。
*   健壮意味着即使一些参与者离开或停止合作，数据存储也将继续运行。
*   不可变意味着数据一旦存储在区块链中，就不能被更改。

区块链以优雅的方式解决了文档注册问题。一旦我们在区块链注册中心输入签名，我们就不需要担心签名会被篡改以匹配被篡改的文档。要做到这一点，大多数网络参与者将不得不同意这一改变，这使得它很难保密。

与此同时，没有人有权收取运营服务的租金。网络参与者自己提供服务。

在这种情况下，文档可以是任何数据集。同样的模式将用于证明任何商业交易、物联网数据集或用户身份等的真实性。

> 区块链以优雅的方式解决了文档注册问题。一旦我们记录了签名，就不能被篡改。

如果我在 2016 年写这篇文章，我可以实现大约一百行代码，然后是一份 24 页的白皮书，[在一个 ICO](https://vinchain.io/) 中筹集几百万美元。这种模式的应用是无止境的，很多人抓住了这个机会。

如今，知道如何使用这个构建模块来设计更复杂的解决方案是很有用的。所有区块链解决方案都依赖于存储用户生成的数据，这些数据可以不依赖任何人而被信任。

# **实施**

这一次我没有从头开始编写合同代码。我正试图停止重新发明轮子，区块链注册中心已经实现了打。快速的 google 搜索把我带到了 GitHub repo，我将用它来展示这个模式是如何工作的。即使它使用旧版本的 solidity，它也能很好地工作，你可以测试它。)。

[契约](https://github.com/nakov/Ethereum-Web3-Document-Registry-Demo/blob/master/DocumentRegistry.sol)非常简单，只有一个相关的契约变量和两个函数。

*   一个`documents`映射将为一个文档计算的散列与它被添加到的块链接起来。
*   一个`add`方法获取一个散列并将其存储在映射中。
*   一个`verifiy`方法返回散列的时间戳。

```
pragma solidity ^0.4.18;contract DocumentRegistry {
  mapping (string => uint256) documents;
  address contractOwner = msg.sender; function add(string hash) 
    public 
    returns (uint256 dateAdded) 
  {
    require (msg.sender == contractOwner);
    var timeAdded = block.timestamp;
    documents[hash] = timeAdded;
    return timeAdded;
  } function verify(string hash) 
    constant 
    public 
    returns (uint256 dateAdded) 
  {
    return documents[hash];
  }
}
```

前端允许您用`contract.add`上传文件，并计算一个签名作为文件内容的 sha256。

```
async function uploadDocument() {
  …
  let fileReader = new FileReader();
  fileReader.onload = function() {
    let documentHash = sha256(fileReader.result);
    …
    contract.add(documentHash, function(err, result) {…});
    …
  }
}
```

前端还允许你上传一个带有`contract.verify`的文件，如果它是在之前上传的，它会告诉你大概的时间。

```
function verifyDocument() {
  …
  let fileReader = new FileReader();
  fileReader.onload = function() {
    let documentHash = sha256(fileReader.result);
    …
    contract.verify(documentHash, function(err, result) {
    …
    let contractPublishDate = result.toNumber();
    if (contractPublishDate > 0) {
      let displayDate = new Date(
        contractPublishDate * 1000
      ).toLocaleString();
      showInfo(
        `Document ${documentHash} is <b>valid<b>, 
        date published: ${displayDate}`
      );
    }
    else
      return showError(
        `Document ${documentHash} is <b>invalid</b>: 
        not found in the registry.`
      );
    });
  }
  …
}
```

这就是实现分散式文档注册中心所需的全部内容，它允许做两件事:

1.  签署文件
2.  验证文档自上次记录签名后是否已更改。

这是可行的，因为两个不同的文档具有相同签名的可能性非常接近于零。如果您获得了时间戳，您就可以确定您提供的文档是在那个时候被引入到注册中心的。

契约代码当然可以更新和改进，但是这 17 行代码中的核心行为仍然是正确的。

# **结论**

文档注册中心是具有商业价值的区块链应用程序的最简单的实现。在 2016 年，这篇文章中的几行代码足以在 ICO 中筹集数百万美元。今天，它们仍然是相关的构建块之一，您可以在更复杂的解决方案中一次又一次地重用它们。

文档注册中心有效地利用了区块链的分散性和不变性来消除信任任何人所提供的数据是真实的需要。这个想法很简单，但却是革命性的。