# 复杂的模式并不总是那么复杂。通常是那些“简单的”咬你。

> 原文：<https://medium.com/hackernoon/complicated-patterns-arent-always-that-complicated-usually-it-s-the-simple-ones-that-bite-you-caf870f2bf03>

![](img/c0d8d48ada7958cc1d0ed9419782884e.png)

盯着微服务系统错综复杂的通道，我立刻意识到了问题所在。

我正和一个新客户坐在一起审查他们的系统。这是他们第一次向我展示被描述为“非常有趣”和“绝对是我工作过的最复杂的代码之一”的代码兴奋地。

我关闭了一点。

我想到了我那件讽刺地被错误引用的印有阿尔伯特·爱因斯坦照片的 t 恤。

> 任何聪明的傻瓜都能把事情变得更大、更复杂...向相反的方向前进需要一点天赋和很大的勇气。E. F .舒马赫。

**复杂从来都不是好兆头。**

首先，我想了解业务，更具体地说，是在代码中的什么地方。

这就是大多数人问的:“名词是什么？”

我的第一个问题是:“事件是什么？”

在我们的世界里，每天都有数量惊人的事件发生。当汽车爆炸时，这个男人穿过了街道，扔出了一盏刚刚变成红色的灯。

如此之多，以至于不可能的事情很可能发生，因为数十亿个因素在不断变化，并影响着其他行为者和实体。当人们惊呼“真巧！”我想到巧合在统计学上的可能性有多大，因为其数量之大。

我们消费世界以回应事件。我们读到了关于另一个演员的行为引起的反应的新闻。面对威胁，我们逃跑了。我们为了应对饥饿而进食。等等。

除了消费发生在我们周围的事件，我们还影响和作用于世界和其他人。你有你的专长——你让事情发生，而对于其他事情，你请求他人来获得你想要的结果。

通过我们自己的行动或我们命令的效果，在某种意义上，我们正在改变世界的状态。

如果你知道世界上发生的一切，你就可以重建世界的现状。

在构建自动化解决世界问题的软件时，我们需要对世界的有用子集进行建模。我们创建了一组对解决手头任务有用的抽象。你总是听说名词是多么重要，但我想强调的是，和名词一样，在我们的上下文中发生的命令和事件也是这个故事中非常重要和明确的部分。

当对我们世界的一小部分建模时，我们应该总是努力使概念明确定义并易于理解。

> **专业提示**:这也是重构中一个有用的提示。如果你发现一个概念被谈论和暗示，但没有在代码中明确定义，这是一个错误的迹象。一股“代码味”。把含蓄的，明确的。

这些名词由事件和它们在时间中的位置联系在一起，这是我们现实中唯一不变的构造。

世界很大。我们不能模拟整个世界。

正如计算机科学中任何复杂的问题领域一样，为了使它更容易解决，我们可以引入**约束**。在建模术语中，我们必须将我们正在建模的上下文或焦点缩小到仅涉及的实体。实际上，甚至更狭隘地说，将这些实体的属性仅限于那些在解决特定问题时真正有用的属性。

我们有时会从对话中容易而明显地知道一个领域的一些重要实体是什么，并且我们可以开始将它们分成更小的子系统，有时称为“**有界上下文**”。

例如，如果您正在构建一个电子商务平台，您可能需要一个产品模型来描述产品的属性，因为它适合在各种媒体上显示，例如互联网或印刷目录、用于跟踪可用性的库存模型、用于跟踪和管理客户订单的订单分类帐、电子商务店面，或者一些营销渠道。

各有各的语境。

在这些上下文中，从建模的角度来看，由于被称为“**耦合**”和“**内聚**”的概念，有一些更微妙的东西。

内聚性是对概念交织紧密程度的一种度量。*高内聚意味着实体紧密耦合。*这意味着它们可能需要在任何时候都是引用完整的——在单个事务中更新，或者当需要即时一致性时。

当事情非常不相关时，它们是松散耦合的。

回到电子商务商店的例子——在库存系统中，需要知道哪些商品有多少。然而，每个产品的细节对于满足库存环境/系统的要求不是必需的。

哪些事件与库存相关联？这里有几个:`inventory.product.listed`、`inventory.product.quantity.decreased`、`inventory.product.outOfStock`。

你会注意到的第一件事是语言的清晰。这和你讨论业务时使用的语言是一样的。

> **专业提示:**小心不要无意中把这种语言强加给别人——作为工程师，我们经常要给事物命名。错误的名字会产生深远的影响。接下来会有更多的报道。

通常每个事件都有一个有效负载，即关于事件的元信息，比如上面一些例子中的产品 id。它回答了一些问题，例如哪种产品，也提出了一些问题:一种产品如何上市，其库存如何减少？

我们可以追踪领域内发生的事件的这些不变事实，追溯到执行命令的参与者，并再次追溯到该命令是在哪里以及在什么情况下发出的。

我们也可以反过来思考和搜索——当一种产品缺货时，什么系统会受到影响？产品描述？没有。这意味着在这种情况下内聚性很低，需要松散耦合。

我们能够从我们所知的事物中给它下一个外在的定义，来理解事件是如何影响实体的，以及这些事件是如何产生的。

一个事件在一个非常小的空间里传达了很多关于一个领域的信息。它有助于定义领域的范围、上下文的大小以及内聚性和耦合性的级别。

跟随事件和命令的踪迹将带领我们跨越界限来定义心智模型，并看到模式在哪里被遵循，规则在哪里被打破。我们可以识别出不正确耦合或不正确结合的部分，并开始评估复杂性。

回到代码审查…

我希望在这篇评论中看到的是明确定义的命令和事件。

就个人而言，我喜欢每个事件处理程序或命令处理程序都有一个文件。它强有力地传达了每个服务做什么，而不需要看代码。

当您可以浏览代码库并通过搜索以发生的事情或应该发生的事情命名的文件来找到某个功能的入口点时，您可以非常有效地浏览，因为这些概念被清晰地组织、定义和交流。

不幸的是，这不是我找到的。平心而论，通常不是。

我们只能说，这更像是一种混杂的服务，在房间的另一端互相叫嚷，有点像电影中描述的 90 年代的纽约证券交易所。

复杂性已经失去了控制。

“你听说过一种叫做 CQRS 和事件采购的模式吗？”我问道。

“是的”客户回答。"我们之前讨论过这个问题，但是决定保持简单，只用 REST 代替."

…“那是怎么回事？”

提示:不太好。

在努力避免复杂性的过程中，存在着比用模式和规则处理问题更复杂的情况。

这是一个陷阱，我看到真正伟大的工程师总是陷入其中，我认为这部分是语言的失败，部分是复杂性的谬误。

**首先——语言的失败——这个项目被描述为 MVP。**

这是大型团队密集开发的第 8 个月。

这绝不是一个 MVP。

MVP 是你能证明一个假设的最小可能的东西。对开发者来说，它传达了“快速和肮脏”。

几年前，当我创办第一家公司时，我犯了一个非常严重的错误。我花了几个月的时间构建了一个带有自定义 CSS 动画和实时更新的史诗 MVP，可以推送到任何数量的我们在酒店房间连接的平板电脑上，为客人提供在城市中的酷体验。事实证明，虽然人们可能想要它，但这并不重要，因为酒店不需要。

现在，在精益创业机器、精益创业会议和由精益分析作者 [Ben Yoskovitz](https://medium.com/u/773af9e5f1ee?source=post_page-----caf870f2bf03--------------------------------) 与百威英博(Anheuser Busch InBev)合作领导的令人敬畏的加速器之后，我是一家机器学习创业公司的首席技术官，该公司筹集了一轮后续资金，并被合并到 ABI 的电子商务部门，我可以自信地告诉你，这是*而不是*它是什么。

例如，在一个精益创业机器实验中，我构建了一个单页应用程序，其中包含一个收集电子邮件地址的表单，以了解人们是否有兴趣共享一辆出租车(在 uber pool 之前),因为上东区太远了！我花了大约 30 分钟来制作。那是 MVP。甚至是复杂的一个！

这足以说服一个陌生人和我一起坐出租车。它需要一个特定的假设，并证明或否定它。

评委告诉我们，“拼车”不是一个好主意，因为我们只赚了 7 美元，大多数人都对与陌生人共乘一辆车感到害怕。回想起来，即使这是真的，但显然不是，因为微生态位，一小部分没有被吓跑的人可能是一个足够大的市场！这完全取决于你的目标。

无论如何，即使是一封电子邮件也可能成为 MVP。它只需要证明一个假设。

这就引出了我的观点:**语言非常重要。**

没有合适的语言，你就没有合适的模型。

这就引出了我在代码中发现的第二个主要问题——语言并不那么重要。

听着。

模型👏需要👏到👏是👏显然地👏清晰的👏。

这意味着不要混合到 REST API 中。

但是当你在打造一个 MVP 的时候，你会怎么做呢？还不如扔进一个 REST API！

API 的目的是成为一个接口，而不是一个模型。此外，每个上下文最多应该有一个 API 服务。

> 如果你的每一项服务都是 REST 服务，那你还不如打开你的电脑，往里面倒些意大利面，因为 8 个月后，它会变得如此困难，你还不如拥有一台坏掉的电脑。

前端界的人*都知道*前端是复杂的。想想你认识的任何前端开发人员，问“前端复杂吗”，他们会滔滔不绝地谈论流量、单向数据流和功能组件。

当涉及到后端时，流动和单向的工作流就过时了。

> 专业提示:事件源基本上是 Redux。

**前端人员**:你知道这个东西。你知道流动之前的生活是什么样的。您希望后端也是这样吗？你知道吗？

**还有后端的人:**连你们前端的 dev 都知道这玩意！走吧。😜

如果我所做的只是咆哮而不是给出一个简单的例子，这将会是一篇什么样的文章！

让我们开始为库存建模，因为它的核心是一个非常简单的系统，但足够复杂，可以成为实际项目的一部分。

首先是 POJO，它代表“普通的旧 Java(脚本)对象”:

```
export default class Inventory {
  constructor() {
    this.products = []
  } init(id) {
    this.id = id
  }

  catalogProduct(product) {
    this.products.push(product)
  }
}
```

到目前为止，对吗？

> 现在让我们添加一些神奇的酱！🧙‍♂️

```
**import { SourcedEntity } from 'sourced'**export default class Inventory **extends SourcedEntity** {
  constructor(**snapshot, events**) {
    **super()**
    this.products = []

    **this.rehydrate(snapshot, events)**
  }

  init(id) {
    this.id = id
    **this.digest('init', id)** 
  }

  catalogProduct(product) {
    this.products.push(product)
 **this.digest('catalogProduct', product) // same as command name** }
}
```

你是活动采购！

我们来分解一下。

首先，我们从 SourcedEntity 扩展我们的类，因此需要调用`super()`。能够用以前的事件**再水合**我们的实例是很重要的。这意味着通过重放事件来重新创建最新的状态。快照是一种优化，因为它被用作一个起点，所以你不需要总是回到零。

最后，在我们执行命令之后，我们调用一个名为`digest`的函数。一个命令进入模型并被消化。

这是一个我见过人们在精神上犯错的领域。看起来好像我们是在“命令采购”，但是我希望你这样想:命令已经被消化了。每个命令都会被消化。

这意味着你的实体的属性`newEvents`有一个新的对象，包含你刚刚理解的命令名和与之相关的数据。

在我们继续讨论持久性之前，还有一件更重要的事情。在我们消化了一个命令之后，我们可以选择**发出**一个关于发生了什么的事件，这样我们服务的其他部分就可以决定用这个信息做些什么。

```
import { SourcedEntity } from 'sourced'export default class Inventory extends SourcedEntity {
  constructor(snapshot, events) {
    super()
    this.products = []

    this.rehydrate(snapshot, events)
  }

  init(id) {
    this.id = id
    this.digest('init', id) **this.emit('inventory.initialized')** 
  }

  catalogProduct(product) {
    this.products.push(product)
this.digest('catalogProduct', product) // same as command name
    **this.emit('inventory.product.cataloged')**
 }
}
```

然而，这有点棘手，所以让我们慢下来。

如果在事件完全提交并保存到数据库之前发出事件，会发生什么情况？

我们的系统会像鲁布·戈德堡机器一样启动:

![](img/36f1844bd50e33e486da251e58e12e84.png)

Dog Rube Goldberg Machine ([Giphy](https://giphy.com/gifs/DnxW82rvQubQc))

你可以想象，在我们准备好之前，我们不希望这种情况发生。

这是一个简单的修复，我们将使用一个名为“ **enqueue** ”的特殊版本的 emit。

我们来看看更新后的功能。

```
init(id) {
  this.id = id
  this.digest('init', id) this**.enqueue**('inventory.initialized') 
}catalogProduct(product) {
  this.products.push(product)
this.digest('catalogProduct', product) // same as command name
  this.**enqueue**('inventory.product.cataloged')
}
```

现在，在事件成功提交到我们的存储库之前，不会发出事件！

“什么储存库？”你可能想知道。

很高兴你问了。

如果你已经注意到了，我们的模型很好，很简单，什么都有…但是…难道它不需要**坚持**吗？

是的。是的，它是。

这就是我们将使用“**存储库模式**的目的。存储库模式将允许我们通过将持久性问题移到模型之外来保持我们的模型简洁明了。

Sourced 的构建考虑到了这一点，并为此提供了一个 MongoDB 实现。

但是，在我们创建存储库之前，让我们使用 Jest 测试我们的模型，因为测试很重要！

**_ _ tests _ _/models/inventory . mjs**

```
import Inventory from 'models/Inventory.mjs'describe('Inventory', () => {
  it('has a quick test for this article', () => {
    let inventory = new Inventory()
    let id = 'test-store-1'
    inventory.init(id)
    expect(inventory.id).toEqual(id) let product = { id: 1, quantity: 100 }
    inventory.catalogProduct(product)
    expect(inventory.products[0]).toEqual(product)
  })
})
```

对于我们的模型来说，这应该接近 100%的覆盖率。再告诉我一遍获得 100%的覆盖率有多难，我会告诉你写更简单的代码！

好了，让我们创建存储库吧！让我们创建一个新文件:

**repositories/inventory repository . mjs**

```
import SourcedRepoMongo from 'sourced-repo-mongo'
import Inventory from '../models/Inventory.mjs'const { Repository } = SourcedRepoMongoexport const inventoryRepository = new Repository(Inventory)
```

在我们继续之前，有一些额外的复杂性可以通过使用这个模式来解决。

例如，假设我们的服务批量接收事件，我们希望尽可能快地处理它们，但是某个实例的所有事件都需要按顺序处理，否则结果将是不正确的。

此外，我想使用`async / await`而不是存储库回调风格的实现，所以让我们用蓝鸟的`promisifyAll`来解决这个问题。

```
import SourcedRepoMongo from 'sourced-repo-mongo'
**import Queued from 'sourced-queued-repo'
import Bluebird from 'bluebird'** import Inventory from '../models/Inventory.mjs'const { Repository } = SourcedRepoMongo**const repository** = new Repository(Inventory)
**const queuedRepository = Queued(repository)****export const inventoryRepository = Bluebird.promisifyAll(queuedRepository)**
```

现在，事件仍将尽可能快地处理，但是，当从数据库中检索到资源时，存储库将使用给定的`id`锁定资源，并在更改完成并提交时移除它，确保事件按顺序处理。

让我们来看看如何使用我们的清单——有许多方法可以将事件传递到流程中，而本文已经很长了，所以让我们只关注在收到命令后做什么，而不是如何接收它。我们将实现函数`listen`，它将在收到命令时被调用。

**commands/inventory . initialize . mjs**

```
import Inventory from '../models/Inventory'
import { inventoryRepository } from '../repos/inventoryRepository.mjs'// This would be a good place for validation
export const listen = async function (data) {  
  const { id } = data
  let inventory = new Inventory()
  inventory.initialize(id) try {
    await inventoryRepository.commitAsync(inventory)
  } catch (error) {
    throw new Error(`Error while committing repository - ${error.message}`)
  }}
```

当提交成功时，所有排队的事件都将触发。

这一次，让我们检索并修改一个现有的清单。

**命令/库存.目录产品. mjs**

```
import Inventory from '../models/Inventory'
import { inventoryRepository } from '../repos/inventoryRepository.mjs'

export const listen = async function ({ inventoryId, product }) {
  let inventory

  try {
    inventory = await inventoryRepository.getAsync(id)  
  } catch (error) {
    throw new Error('Error while getting data from repository')
  }

  if (!inventory) {
      throw new Error('Inventory does not exist - cannot add product. Initialize inventory first, or ensure you are using the correct id'))
  }

  inventory.catalogProduct(product) try {
    await inventoryRepository.commitAsync(inventory)
  } catch (error) {
    throw new Error(`Error while committing repository - ${error.message}`)
  }}
```

搞定了。

# **结论**

“复杂模式”有时比它们应得的名声更坏。在这种情况下，避免它们会导致更复杂的解决方案，因为“MVP”已经超出了它最初的范围。

我希望您已经了解了如何使用普通的旧 JavaScript 对象和一些设计模式、事件源和存储库模式来极大地简化代码库最重要的部分——您的业务逻辑，以及为什么使用有时被称为“复杂”的模式可能并不复杂。

上面的代码为微服务打下了坚实的基础，你只需要弄清楚如何在服务之间发布消息，这可以通过各种方式完成，可能还需要添加一些验证。通信位我个人推荐 [servicebus](https://github.com/mateodelnorte/servicebus) 。

要获得更完整的源代码示例，请查看来自`sourced-repo-mongo`的[测试套件](https://github.com/mateodelnorte/sourced-repo-mongo/blob/master/index.test.js)。

关于使用 sourced with servicebus 的示例，请查看我的示例存储库:[https://github.com/patrickleet/servicebus-microservice](https://github.com/patrickleet/servicebus-microservice)

如果你想了解更多，我有一个即将推出的名为“微服务驱动”的微服务课程。[在此注册，以便在发布时收到通知！](https://www.microservicedriven.com/get-access)

> 当人们想学习微服务时，我总是建议学习一些 DevOps 和容器化，以及简化开发和生产过程的先决条件。[点击这里查看我的 DevOps 之旅](https://hackernoon.com/my-journey-to-achieving-devops-bliss-without-useless-aws-certifications-a7cbf7c539d1)！我个人认为未来包括容器和无服务器——以及在大多数企业场景中在 Kubernetes 上运行的无服务器工作负载。

像往常一样，如果你发现这很有帮助，那么帮助我的最好方法就是给我 50 个掌声，在 Medium 上跟随我，并与其他人分享！

✌️