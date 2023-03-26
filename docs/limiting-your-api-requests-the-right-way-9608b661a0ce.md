# 限制您的 API 请求:正确的方法

> 原文：<https://medium.com/hackernoon/limiting-your-api-requests-the-right-way-9608b661a0ce>

![](img/dd4a4f5c2607cb07d8b5efa3a0a7f60e.png)

Image from @STR/AFP/GettyImages

大家好。我叫亚历山大，是 Javascript 开发人员。今天，我想告诉你一个故事，关于我试图在构建服务器应用程序中找到禅，以满足世界上每一个 API。

# 序言

这开始于 2015 年 6 月，当时 Telegram 在 API 中宣布了他们的新[机器人平台](https://core.telegram.org/bots)。我是一个在小型网络工作室工作的全栈 Javascript 和 PHP 开发人员，我所有的工作就是基于这个栈快速开发登陆页面。让你的工作机器人就在你的信使中的想法令人兴奋，类似的东西我在 2010 年为 ICQ 开发的。唯一剩下的问题是未来机器人的想法。它会做什么？我将如何开发它？所以我决定写 [baneksbot](https://t.me/baneksbot) 。

我开始每天阅读 bot API，试图理解我应该如何使用它。我决定用 MySQL 数据库写一个小的 PHP 机器人，它应该只要求 VK 集团的新职位，广播给所有订阅用户，并可以显示每天/每周/每月/以往最喜欢的职位。2015 年 7 月发布。

2016 年，我决定将这个机器人完全重写为 Node.JS，而不是 MySQL，我选择 MongoDB 进行帖子存储，选择 ElsaticSearch 进行快速搜索。代替长轮询，我开始使用 webhooks。所以 baneksbot v2.0 已经发布了。我有大约 20 000 名用户订阅新帖子，所以我很快遇到了 Bot API 限制，并开始得到`HTTP 429`错误，而不是电报响应。那是 2016 年 10 月，然后我意识到我需要以某种方式限制我的机器人请求，让它变慢。

# 简单地说，费率/限额

你可能知道，很多 REST API 兼容服务都有所谓的[速率限制](https://en.wikipedia.org/wiki/Rate_limiting)来防止 DoS 攻击和服务器过载。有些人有软规则，你可以在短时间内越过他们的限制，而有些人有严格的规则，你会立即得到回应和超时，然后你可以再试一次。

在我的情况下，Telegram 有软的，但硬的速率限制规则，我一再忽略这些规则，我的许多信息从未发送给用户。所以我决定寻找解决方案，并尝试寻找如何遵循这些规则的方法。

最简单的方法是设置超时，然后发送消息:

```
const delay = interval => new Promise(resolve => setTimeout(resolve, interval));const sendMessage = async params => {
  await delay(1000);

  return axios(params);
};
```

## 优点:

*   非常简单
*   非常管用

## 缺点:

*   难以管理
*   无法单独配置

所以这基本上是刚从 PHP 转移到 Node 的人的主要方法。JS，并试图写一些不会工作得那么快的东西*。*但是，很明显，节点。在异步的情况下，JS 更强大，所以我们需要一些优雅的东西。为了让它工作，我们需要一个**队列**。

# 请求队列

实际上，在[npmjs.com](https://npmjs.com)中有很多请求队列。有些相当不错，有些不怎么样。我开始尝试它们，看看它们能否在我的用例中正常工作。我使用了一个名为`request`的库来轻松地发出 HTTP 请求。在`require('http').request`之后，就像呼吸了一口新鲜空气:你有承诺，你可以使用流，你可以给它用户友好的网址等等。所以我的第一选择是[请求速率限制器](https://www.npmjs.com/package/request-rate-limiter)。它可以很容易地配置和使用在任何地方。实际上，对于 95%的用例来说，它是一个完美的库。

```
const RateLimiter = require('request-rate-limiter');const limiter = new RateLimiter(120); // 120 requests per minuteconst sendMessage = params => limiter.request(params);sendMessage('/sendMessage?text=hi')
  .then(response => {
    console.log('hello!', response);
  }).catch(err => {
    console.log('oh my', err);
  });
```

## 优点:

*   它不会发送比 API 允许的更多的请求
*   它有内置队列，所以您可以轻松地将请求放在那里，等待响应

## 缺点:

*   每个实例只能配置一个规则
*   所以你不能用*整体*队列来让你的应用程序全局遵循这些规则

如您所见，这是一个几乎完美的请求队列库。但是让我们更仔细地阅读 Telegram Bot API。有几条规则:

*   你**可以**每秒发送 **1 条消息**给个人聊天
*   您**可以每分钟**向群组/频道发送 **20 条消息**
*   但是那时你**不能**每秒发送超过 **30 条信息**总计**。**

你可能会想，如果我将速率/限制设置为 1/3(每 60 秒 20 条消息)，每个人都会很高兴。但是让我们再一次考虑这些数字。

# 在下一个请求前等待 3 秒钟

听起来很可怕，是吧？但那是真的。如果你想容易地遵循这些规则，你不能发送它，而不是这个时期。我很失望。我想尽可能快地从 VK 发送这些帖子，以便在这些帖子出现在 VK 后立即为我的用户提供顶级内容。我在我的机器人里也承诺过，在这篇文章发表后的一分钟内，我会给你发新的文章。所以我决定发展我自己的队列:用二十一点和…规则。

# 智能队列

我从 2017 年 1 月开始开发这个队列。创建基本概念，一周后写出第一个原型。这个队列的主要概念是:

*   应该有一个队列来存储请求并以正确的顺序执行它们
*   应该有能力为不同的请求设置多个规则
*   规则应该有一个优先级，这样优先级较低的请求可以保留一点，为更重要的请求让路
*   即使我完美地编写了这些规则，也应该有一个 B 计划来重新尝试这个请求，没有额外的痛苦，并在相同的承诺中得到响应

这就是我如何创建我的第一个名为[智能请求平衡器](https://www.npmjs.com/package/smart-request-balancer)的公共 npm 库。它可以很容易地遵循这些规则，并使我的 bot API 在近两年内保持安全。让我解释一下它是如何工作的:

首先，您需要创建一个队列，

```
const SmartQueue = require('smart-request-balancer');
```

然后你初始化它

```
const queue = new SmartQueue(config);
```

并利用它。没什么特别的！

```
const sendMessage = params => queue.request(retry => axios(params)
  .then(response => response.data)
  .catch(error => {
    if (error.response.status === 429) {
      return retry(error.response.data.parameters.retry_after);
    }
    throw error;
  }), user_id, rule);sendMessage('/sendMessage?text=hi')
  .then(response => {
    console.log('hello!', response);
  }).catch(err => {
    console.log('oh my', err);
  });
```

你可能会问，`error.response.data.parameters.rerty_after`是什么？`user_id`？`rule`？？？让我解释一下。

对于这个特殊的例子，我们创建了一个`sendMessage`函数，它基本上发出`axios`请求，但也有另外两个参数:`user_id`和`rule`。这里的`user_id`只是用户的唯一键，基于它我们可以将这些请求存储在一个队列中。例如，如果您想为用户 1 发送 30 条消息，为用户 2 发送 50 条消息，那么将会有两个队列，它们不应该相互等待并独立工作。用户每秒 1 条消息，记得吗？但与此同时，它不应该达到电报的总体规则，全球每秒不超过 30 条消息！

还不明白？让我向您展示如何配置这个队列:

```
const config = {
  rules: {
    individual: {
      rate: 1,
      limit: 1,
      priority: 1
    },
    group: {
      rate: 20,
      limit: 60,
      priority: 1
    },
    broadcast: {
      rate: 30,
      limit: 1,
      priority: 2
    }
  },
  overall: {
    rate: 30,
    limit: 1
  }
}
```

如你所见，我们有 3 条规则:针对个人、针对团体/频道和针对广播。我们还有总体规则。此外，个人和群体规则具有比广播更高的优先级，这意味着当我的机器人空闲并想要广播消息时，它会很容易地广播它们，但只要有人发送命令，它就会立即响应，然后继续广播。此外，机器人永远不会达到每秒 30 条消息的总限制，不会被 API 忽略。

让我们回到我们的例子。什么是`retry`？`retry`是一个特殊的函数，如果您不小心达到了极限(在我们的例子中是 429)或者出于某种原因想要重复您的请求，您应该调用它。当你在这个区间内调用这个函数时，这个请求将在这个区间后被添加到队列中，并在这个区间后解析这个承诺。很整洁，是吧？

但是等等，什么是`rule`？这正是我们在配置中提供的`rule`。

# 它是如何工作的

让我们总结一下这个算法:

1.  你用`queue.request`发出请求
2.  `SmartQueue`将此请求添加到队列`<key, rule>`
3.  如果整个队列中有第一个元素，则执行请求，否则等待轮到它
4.  解决请求，加热它的队列和整个队列
5.  移除空队列
6.  返回步骤 3，直到队列中没有请求

简单而强大。仅此而已。

# 收场白

这些天以来，这个队列在我的多个机器人中成功地工作，我几乎忘记了广播的事情，开始享受我的生活。这两年来，我一直在润色这个图书馆，准备出版。现在它准备好为人们带来和平的队列。

总之，以下是我写这篇文章时使用的有用链接:

*   [维基百科中的限速](https://en.wikipedia.org/wiki/Rate_limiting)
*   [链接到 npm 上的智能请求平衡器库](https://www.npmjs.com/package/smart-request-balancer)
*   [链接到 github 上的智能请求平衡器库](https://github.com/energizer91/smart-request-balancer)
*   [链接到 github 上的 baneksbot 电报机器人](https://github.com/energizer91/baneksbot)

欢迎你方的所有贡献。感谢您的耐心和关注。愿原力与你同在！再见。