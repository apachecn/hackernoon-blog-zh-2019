# 编写正确用户故事的工程指南

> 原文：<https://medium.com/hackernoon/engineering-guide-to-writing-correct-user-stories-238bb2a2b6e0>

![](img/b99d409b6bcfac13e319d44111a16e10.png)

敏捷的人痴迷于编写用户故事。这确实是一个强有力的工具。但是，从我的实践来看，很多人都做错了。

让我们看一个例子:

```
As a user 
I want to receive issue webhooks from Gitlab 
So that I can list all current tasks
```

似乎是一个有效的用户故事，不是吗？事实上，这个小故事包含了多个问题。如果你找不到至少 8 个错误，那么这篇文章值得一读。

本文分为三个主要部分:

1.  默认用户故事格式越来越好
2.  用 BDD 重写用户故事，使其可验证
3.  将用户故事与测试、源代码和文档联系起来

虽然不同类别的读者可能对某些部分更感兴趣，但对每个人来说，理解完整的方法是很重要的。

# [定位和修复问题](https://sobolevn.me/2019/02/engineering-guide-to-user-stories#spotting-and-fixing-problems)

众所周知，我们所有的需求都必须[正确、明确、完整、一致、有序、可验证、可修改和可追踪](https://standards.ieee.org/standard/830-1998.html)，即使它们乍看起来不像需求。

用户故事往往不具备某些给定的特征。我们需要解决这个问题。

# [使用一致的语言](https://sobolevn.me/2019/02/engineering-guide-to-user-stories#using-consistent-language)

“接收问题网页挂钩”和“列出所有当前任务”有什么联系吗？“任务”和“问题”到底是不是一回事？这可能是完全不同的事情，或者只是措辞不当。我们怎么知道？

这就是词汇表的用途！每个项目都应该从定义特定的术语开始，这些术语将为未来构建无处不在的语言。首先，我们如何构建这个术语表？我们询问领域专家。当我们遇到一个新术语时:我们确保所有的领域专家都正确且相似地理解这个术语。我们还应该注意，同一术语在不同的情况和背景下可能会有不同的理解。

假设在我们的案例中，在咨询了领域专家之后，我们发现“任务”和“问题”是同一个东西。我们现在需要删除不正确的术语。

```
As a user 
I want to receive issue webhooks from Gitlab 
+++So that I can list all current issues 
---So that I can list all current tasks
```

非常好。对相同的实体使用相同的词语将使我们的需求更加清晰和一致。

# [用户不想要你的东西](https://sobolevn.me/2019/02/engineering-guide-to-user-stories#users-do-not-want-your-stuff)

当我们修改了最后一行时，我注意到用户的一个目标是“列出所有当前问题”。为什么这个可怜的用户要列出一些问题？做这件事有什么意义？没有用户希望这样。这个要求是不正确的。

这是写作要求中一个非常重要的问题的指标。我们倾向于混合我们和用户的目标。虽然我们的目标是取悦用户，但我们应该首先关注他们。让他们的需求比我们的更有价值。我们应该在需求中明确表达这一点。

我们如何知道用户想要什么？再说一次，我们没有。我们需要咨询真正的用户或他们的代表。或者你自己做一个假设，如果我们不能问任何人。

```
As a user 
I want to receive issue webhooks from Gitlab 
+++So that I can overview and track the issues' progress 
---So that I can list all current issues
```

在收集了更多的反馈后，我们知道，我们的用户需要知道项目的进展。不列出问题。这就是为什么我们需要从第三方服务接收和存储有关问题的信息。

# [删除技术细节](https://sobolevn.me/2019/02/engineering-guide-to-user-stories#removing-technical-details)

你有没有遇到过一个人真的想要“接收问题网页挂钩”？没有人想那样做。在这种情况下，我们还将两种不同的关注点混合在一起。

用户的目标和实现目标的技术方法之间有明显的区别。而“接收问题 webhooks”显然是一个实现细节。明天可以改成 WebSockets，推送通知等。而用户的目标也不会因此而改变。

```
As a user 
+++I want to have up-to-date information about Gitlab issues 
---I want to receive issue webhooks from Gitlab 
So that I can overview and track the issues' progress
```

看到了吗？只留下重要的信息，实现细节被剥离。

# [澄清角色](https://sobolevn.me/2019/02/engineering-guide-to-user-stories#clarifying-roles)

仅从上下文来看，很明显我们正在处理某种与开发人员相关的工具。我们使用 Gitlab 和问题管理。因此，不难猜测我们将有不同类型的用户:初级、中级和高级用户。也许项目经理和其他人也是如此。

所以，我们来看看角色的定义。所有项目都有不同类型的用户。即使你认为它们不是显式类型。这些角色的形成取决于你的产品被使用的方式或目标。这些角色必须用我们定义项目术语的方式来定义。

在这个特定的用户故事中，我们谈论的是什么样的用户？初级开发人员会像项目经理和架构师一样概述和跟踪进度吗？显然不是。

```
+++As an architect 
---As a user 
I want to have up-to-date information about Gitlab issues 
So that I can overview and track the issues' progress
```

在进行智能猜测之后，我们可以通过不同的用户角色来区分不同的用户故事。它让我们能够更好地控制我们发布的功能，以及由谁来发布这些功能。

# [扩展用户故事](https://sobolevn.me/2019/02/engineering-guide-to-user-stories#extending-user-stories)

这个简单的`As a <role or persona>, I want <goal/need> so that <why>`很棒，因为它既简洁又强大。它给了我们一个完美的交流方式。然而，下面的格式有几个我们应该知道的缺点。

# [让用户故事可验证](https://sobolevn.me/2019/02/engineering-guide-to-user-stories#making-user-stories-verifiable)

对于给定的用户故事，我们仍然存在的问题是它是不可验证的。我们如何确定这个故事(现在或仍然)对我们的用户有用？我们不能。

在这个用户故事和我们的测试之间没有清晰的映射。如果有人能写用户故事作为测试，那就太棒了…

等等，但是有可能！这就是为什么我们有了[行为驱动开发](https://en.wikipedia.org/wiki/Behavior-driven_development)和`gherkin`语言。这就是为什么最初 [BDD 被创建](https://cucumber.io/blog/2014/03/03/the-worlds-most-misunderstood-collaboration-tool)。这意味着我们可以用`gherkin`格式重写我们的用户故事，使其可验证。

```
Feature: Tracking issues' progress
  As an architect
  I want to have up-to-date information about Gitlab issues
  So that I can overview and track the issues' progress

  Scenario: new valid issue webhook is received
    Given issue webhook is valid
    When it is received
    Then a new issue is created
```

现在，这个用户故事是可以证实的。我们可以把它作为一个测试，跟踪它的状态。此外，我们现在有了高阶需求和实现细节之间的映射，这将允许我们理解我们将如何准确地满足这个需求。注意，我们没有用实现细节替换业务需求，但是我们*称赞*它。

# [发现不完整性](https://sobolevn.me/2019/02/engineering-guide-to-user-stories#spotting-the-incompleteness)

一旦我们使用`gherkin`来编写我们的用户故事，我们就开始为我们的用户故事编写场景。我们发现，同一个用户故事可能有几种场景。

让我们来看一下我们制作的第一个场景:“接收到新的有效问题 webhook”。等等，但是当我们收到一个无效的 webhook 时会发生什么呢？我们还应该保存这个问题吗？也许我们还需要做一些额外的工作？

让我们参考 [Gitlab 的文档](https://docs.gitlab.com/ee/user/project/integrations/webhooks.html),作为在这些情况下什么会出错以及如何做的信息来源。

原来我们有两个不同的无效案件，我们需要不同的处理。第一个:Gitlab 不小心给我们发了一些垃圾。第二:我们的认证令牌不匹配。

![](img/4c2ccbb06099a6e7658caa1cd807f307.png)

现在我们可以再添加两个场景来完成这个用户故事。

```
Feature: Tracking issues progress
  As an architect
  I want to have up-to-date information about Gitlab issues
  So that I can overview and track the issues' progress

  Scenario: new valid issue webhook is received
    Given issue webhook is valid
    And issue webhook is authenticated
    When it is received
    Then a new issue is created

  Scenario: new invalid issue webhook is received
    Given issue webhook is not valid
    When it is received
    Then no issue is created

  Scenario: new valid unauthenticated issue webhook is received
    Given issue webhook is valid
    And issue webhook is not authenticated
    When it is received
    Then no issue is created
    And webhook data is saved for future investigation
```

我喜欢这个简单的用户故事现在变得非常复杂的感觉。因为它向我们揭示了它内在的复杂性。我们可以调整我们的开发过程来适应日益增长的复杂性。

# [用户故事排名](https://sobolevn.me/2019/02/engineering-guide-to-user-stories#ranking-user-stories)

目前，还不清楚架构师“概述和跟踪问题的进展”有多重要。它比我们拥有的其他用户故事更重要吗？既然它看起来相当复杂，也许我们可以做些更简单、更重要的事情来代替？

排名和优先级对任何产品都至关重要，我们不能忽视它。即使我们将用户故事作为编写需求的唯一方式。有不同的方法来区分你的需求的优先级，但是我们推荐坚持使用[莫斯科方法](https://wemake.services/meta/rsdp/requirements-analysis/#requirements-prioritization)。这个简单的方法基于四个主要类别:`must`、`should`、`could`和`won't`。并且暗示我们将在文档的某个地方有一个项目中所有用户故事的单独的优先表。

同样，我们需要询问用户每个特性有多重要。

在与使用我们产品的不同建筑师进行了几次交谈后，我们发现这是一个绝对的`must`:

功能优先级经过身份验证的用户必须能够发送私人消息必须架构师必须跟踪问题的进展必须应该有一个关于传入私人消息的通知应该支持多个消息提供者应该不支持加密的私人消息不应该

因此，我们现在可以修改用户情景的名称，将其映射到优先特性:

```
Feature: Architects must track issues' progress
  As an architect
  I want to have up-to-date information about Gitlab issues
  So that I can overview and track the issues' progress

  ...
```

我们甚至可以把它们连在一起。只需使用从您的分级需求表到包含用户故事的特征文件的超链接。

通过这种方式，我们可以确定该功能将是最先开发的功能之一，因为它具有最高的优先级。

如果没有适当的关注，你很快就会得到一堆用户故事、测试、源代码和文档。随着你的项目的不断增长，将不可能区分应用程序的哪些部分负责哪些业务用例。为了克服这个问题，我们必须将所有东西联系在一起:需求、源代码、测试和文档。我们的目标是以这样的方式结束:

![](img/23ebb563a6e486178d8ab18ae5be975f.png)

我用`python`来说明原理。

我可以将用例定义为您的应用程序可以执行的一组独特的高级动作(它看起来非常类似于 [Clean Architecture](http://www.plainionist.net/Implementing-Clean-Architecture-UseCases/) 的观点)。

我通常定义一个名为`usecases`的包，把所有东西都放在里面，这样很容易一次忽略所有现有的用例。每个文件都包含一个简单的类(或函数)，如下所示:

```
class CreateNewIssueFromWebhook(object):
    """
    Creates new :term:`issue` from the incoming webhook payloads.

    .. literalinclude:: path/to/your/bdd/user-story/file
      :language: gherkin

    .. versionadded:: 0.2.0
    """

    def __call__(self, webhook_payload: 'WebhookPayload') -> 'Issue':
        # Do something ...
```

我使用`sphinx`和`[literalinclude](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-literalinclude)` [指令](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-literalinclude)来包含我们在测试中用来记录领域逻辑的同一个文件。我还使用术语表来表明`issue`不仅仅是一个随机的词:它是我们在这个项目中使用的一个特定的[术语](http://www.sphinx-doc.org/en/latest/usage/restructuredtext/roles.html?highlight=term#role-term)。

这样，我们的测试、代码和文档将尽可能地耦合。我们需要少担心他们。我们甚至可以自动化这个过程，并检查`usecases/`中的所有类在它们的文档中都有`.. literalinclude`指令。

您也可以使用这个类来测试我们的用户故事。这样，您将绑定需求、测试和实现这个用户故事的实际领域逻辑。

任务完成。

# [结论](https://sobolevn.me/2019/02/engineering-guide-to-user-stories#conclusion)

本指南将帮助你写出更好的用户故事，关注他们的需求，保持你的源代码整洁，并尽可能多地重用不同的(但相似的)目的。

*原载于 2019 年 2 月 23 日*[*sobolevn . me*](https://sobolevn.me/2019/02/engineering-guide-to-user-stories)*。*