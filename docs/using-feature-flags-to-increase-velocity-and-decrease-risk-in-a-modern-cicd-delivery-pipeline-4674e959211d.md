# 在现代 CI/CD 交付渠道中使用特征标志来提高速度和降低风险

> 原文：<https://medium.com/hackernoon/using-feature-flags-to-increase-velocity-and-decrease-risk-in-a-modern-cicd-delivery-pipeline-4674e959211d>

![](img/eb5e042a03f51fc7d4331e8ff9b42bd8.png)

凯尔萨斯的克里斯·希克曼和乔恩·克里斯滕森以及 Secret Stache 的里奇·斯塔茨描述了在持续集成和持续部署(CI/CD)管道中使用特征标志来提高速度，但降低风险。

该节目的一些亮点包括:

*   Kelsus: CI/CD 管道和项目痛苦导致一个不完美的世界
*   集成:编写、合并、集成和测试代码
*   GetFlow:源代码控制系统中的分支管理和合并不同环境的代码——生产、登台和开发
*   CD 标准:
    -作为一个系统的有效和自动化测试
    -回滚能力(全有或全无版本，蓝绿色部署与特性标志)
*   出现问题并投入生产；特征标志降低了风险并限制了损害
*   什么是特征标志？启用代码时的临时或永久条件语句
*   特性标志的好处:通过将部署与发布分离来降低风险，构建反映生产的环境，并直接在生产中执行测试
*   特征标志的缺点:需要大量的工作、代码和设计；并且难以在遗留和正在进行的系统中采用
*   根据具体情况关闭对某些功能的访问

**链接和资源**

[Mobycast 04:CI/CD 管道](https://mobycast.fm/episode/the-ci-cd-pipeline/)

[获取流程](https://www.getflow.com/)

[谷歌云](https://cloud.google.com/)

[亚马逊网络服务(AWS)](https://aws.amazon.com/)

微软 Azure

[黑暗发射](https://launchdarkly.com/)

[亚特兰蒂斯人](https://www.atlassian.com/)

GlueCon

[反应器](https://projectreactor.io/)

[凯尔苏斯](https://kelsus.com)

[秘密 Stache 媒体](https://www.secretstache.com/)

转录:

Rich:在 Mobycast 的第 66 集，Jon 和 Chris 讨论了如何使用特性标志来提高速度并降低 CI/CD 管道中的风险。欢迎来到 Mobycast，这是一个关于云原生开发、AWS 和构建分布式系统的每周对话。让我们直接开始吧。

乔恩:欢迎，克里斯和里奇。又是一集 Mobycast。

克里斯:嘿。

嘿，伙计们，回来真好。

乔恩:今天我们将直接进入它。我们开始讨论热门的特性标志。每个人都已经开始致力于他们的 CI/CD 渠道，这不算什么。我会说，人们至少在做一些事情，在新项目中全面考虑 CI/CD 渠道。现在就像，让我们在 CI/CD 管道的 CD 部分上工作，让我们实际上开始考虑在新项目和现有项目上的持续部署。我认为，许多在公共云、谷歌云、AWS、Azure 中工作的开发人员会说，“让我们利用这些功能，开始考虑我们是否可以自动部署，或者至少快速按一下按钮。”

我想今天，我们要谈谈 Kelsus 在我们的一个或多个项目中遇到的一些困难，以及我们想为此做些什么。具体来说，我们觉得其中一些问题可以通过特性标志来解决。我们将描述这些是什么，它们与我们当前的 CI/CD 流程有什么关系，以及它们可以如何帮助我们。克里斯，也许你可以把我说的更具体一些。

Chris:我们谈到了现代 CI/CD 管道，这意味着什么？它实际上有两个部分。有持续集成部分，CI，然后是 CD 部分，即持续部署。这是集成和部署。集成意味着我们正在编写代码。团队中的许多人都在编写代码。我们需要得到所有的代码，合并、集成和测试。这就是持续集成背后的理念，即持续不断地发生。

团队必须决定他们将使用什么样的工作流程。其中一个流行的是 GetFlow，它规定了一种在你的源代码控制系统中使用分支的方法，当你合并并把东西放在一起时。对我们来说，就个人而言，我们肯定会使用 GetFlow velocity，我们最终会为我们要部署到的每个环境建立分支。如果我们有三个不同的环境—生产、试运行和开发—那么就有三个分支来管理它们。它工作得很好，但是当我们需要将代码片段从一个环境转移到另一个环境时，就变得棘手了。稍后我们将对此进行更深入的探讨。

乔恩:我想补充一点，我们在去年的另一集里确实谈了很多。我们刚刚讨论了我们的整体 CI/CD 渠道及其外观。我们将在节目笔记中链接到这一点。大部分情况下，没有太大变化。我想我们正在寻找一个新的面孔，这就是这一集的内容。

克里斯:说得好。毫无疑问，人们可能想参考前一集。我只想强调持续集成部分，分支，合并，以及你通常是如何做的。在一个完美的世界里，一切都很顺利，但在现实生活中，有时会有点棘手。

乔恩:如果你能想象，你在 dev 上做东西，然后你去舞台，然后可能是 prod，我就完了。然后我会快乐地一遍又一遍地这样做。

克里斯:事情总是这样，这是一个完美的世界。这就像一个传送带，它总是在流动。重要的是事实并非如此。你发现，这就像，“哦，不，我们确实部署了产品，而且有一个相当大的错误，我们现在必须赶快修复。我们首先需要测试它，所以我们需要在试运行中使用它，但是现在正在试运行的实际上是尚未完全测试的下一个大版本。我们绝对不希望这种情况发生在 prods 身上。现在，我们如何实际制作该修补程序，并仅将其部署到 prod，而不是所有现成的东西？”这是您开始了解情况的部分，并且像功能标志这样的东西将帮助我们解决问题。

Jon:我想您可以将转移恢复到 prod 所在的位置，然后在转移上修复它，然后将转移移动到 prod，但是这样会造成中断，对吗？

克里斯:这是干扰，很冒险。这取决于你在哪里。可能不太可能。想一想，您正在进行的工作实际上涉及到数据库迁移，因此您需要更改模式。

Jon:也许你在 prod 上需要的补丁不是一两个小时就能搞定的；它需要在接下来的几分钟内出现。

克里斯:你做别人做的事。您通过 SSH 进入您的 prods 服务器；git 拉。我没这么说。那是 CI 部分。CD 部分是，现在您正在集成，一切都很好，那么部署就意味着它只是自动地，它只是在生产中不断地部署。这肯定有它的问题。实际上没有很多人这样做，因为这确实需要相当多的复杂性。

对我来说，能够参与持续部署的标准意味着你必须有一个真正好的测试和自动化测试作为一个系统，其次，你还要有回滚的能力。这些真的是更典型的全有或全无版本，蓝绿色部署与如果你有类似功能标志的东西。这实际上为您提供了更多的灵活性，您可以在部署中采用更先进的方法，而不是全有或全无，而是一种增量式的方法。我们会深入探讨这一点。

我想强调的是，我们都在谈论 CI/CD，每个人都在做，每个人都在谈论它，但功能标志确实是其中的一个因素，可以帮助解决一些现实世界中的生产问题。它们能使你提高速度，同时降低相关风险。

Jon:我想尝试提取你描述的两个主要问题，我们将尝试用特性标志来解决。我听说的一个是，它不总是开发-阶段-生产开发-阶段-生产。我们必须能够将内容直接放入 prod，或者直接放入 dev 或 stage。我听到你说的另一个问题是，事情并不完美，即使经过测试也是如此。有时，特别是在他们已经被测试之后，尽管你努力工作来创建一个团队，这个团队对他们的工作非常自豪，并且真正地练习他们所做的一切，现实是问题发生了，并且他们进入了生产。我们怎样才能减少损失？这是两件事，对吗？

克里斯:这是两个主要的东西，两个要点。就是这些。当我们在速度方面遇到问题时，这与持续集成部分和合并有关。它并不完美。我们在那里遇到了问题，这导致我们放慢了速度。我们不得不做额外的工作或者只是做一些不自然的事情。第二部分是，如果我们正在开发一个大的功能发布，这是一个刺激，现在有一群用户，现在他们看到了这一点，我们有多大的信心，这是真正全面的测试，有什么影响？对某些事情来说，没什么大不了的。但是，如果它是一项核心功能，或者是业务真正依赖的东西，任务关键型的，风险就高得多，或者诸如此类。有了特性标志，我们可以降低这种风险。

乔恩:这是我们一直在等待的时刻。什么是特征标志？

克里斯:在一天结束的时候，它肯定不是太典型的令人兴奋。它本质上只是条件语句。你所做的是在你的代码中，你只是说，“如果这个特性被启用，那么继续执行这个代码路径，否则执行这个路径。”真的就这么简单。这只是一个条件句。这是你不会免费得到的东西。当你写代码的时候，你必须考虑这个问题，你必须考虑你的代码是否应该启用这个功能。如果没有启用，它会做什么？当你以那种方式写代码时，你需要考虑，然后实际上放入这些条件语句。本质上，他们就是这样。这是一个条件语句，它包装了一个特定的代码块，您可能希望也可能不希望根据一些条件来启用它。

乔恩:有道理。这很简单。我立刻想象他们到处都是。我应该启动或阻止多少代码发生？我怎么记得他们在哪里或者有多少人？你一说这个我就在想象意大利面。也许你可以更深入一点，你认为我们可以做些什么来保持它的可管理性、整洁性和可用性。你想去那里吗？我想也许在我们去那里之前，我们还应该谈谈，所以你去吧。现在你用它做什么？我们有这种可以开关的代码。我们该怎么办呢？

克里斯:那里有很多东西要打开。我将更多地介绍特性标志本身，并扩展特性标志定义和类型。之后，我们可以继续讨论，看看它能为我们带来什么。我们做所有这些工作。你提到了我们在特性标志方面面临的许多挑战。这不是免费的。这是一个挑战。

这就是为什么很多人没有一个包含特征标志的强大系统的原因。不容易。这是一件复杂的事情，很容易让汽车的轮子快速脱落。我们还会谈到挑战，比如实施速度是多少？你如何去做这件事？绝对有一种爬行-行走-奔跑的方式。然后，你又是如何做到的呢？

这些都是有趣而重要的话题，我们可以随心所欲地深入探讨。也许我们会多谈一点关于特性标志的事情。这是一个条件表达式。毫无疑问，最起码，你要用一个名字来定义这面旗帜，你必须给它一个名字，或者用某种方式来指代它。这是它的一个方面。

可选地，如果你是更强大和更复杂的东西，你也可以为此提供上下文。基于名称和上下文，这就是您在条件表达式中决定是否应该启用它的原因。这里我们不会考虑任何上下文。这真的只是一个开关。不是开着就是关着。你可能有些东西是硬编码的，或者是代码中的常量。

你可以想象一下，在你的配置中，你有 use_v1=true 这样的东西，你的特性标志最后是这样的，“如果 use_v1，对 v1 代码分支这样做，如果不是，那么它使用 V0 分支，”或者类似的东西，与上下文相对。这完全是武断的。如何决定是否启用特性标志取决于您自己。这可能就像，“这是我的用户上下文。”用户实际上是怎么称呼它的，所以它可能与您所处的环境有关，也可能与系统上正在进行的更多宏设置或元数据有关。不管你想用什么，说出你想用来做这些决定的信息。

Jon:我可以想象一个特性标志为一定数量的用户或指定用户打开一个特性，或者只在系统流量低的时候打开。各种很酷的东西。

克里斯:当然。可以超级厉害。我们有这些条件表达式。它们由一个标志名和一些可选的上下文定义。这些条件要么基本上是静态的，要么是动态的。我们讨论了静态部分。这是硬编码的，它基于一些常量或配置，并不真正改变。这基本上是一种切换，而动态就像，“我在运行时实时传递这个上下文，决定是否应该采用这个代码路径。”

需要指出的另一点是，特性标志可以被认为是临时的或永久的。这可能就像你正在做一个新版本，V2 的东西，你的意图是要贬低 V1。有了这个，你可能会有一面 V2 对 V1 的旗帜。在你完全迁移到 V2 之后的某个时候，这个标志就不再有意义了。你只想进去把它全部删除。这就是临时特征标志的一个例子。永久的可能是永远存在的东西，也可能是只为某一组用户提供的高级功能。那是一个特征标志。这将是一件可能会更长久的事情。

乔恩:完全有道理。

克里斯:这就是一般的特征标志。我们[…]有哪些优势？我们为什么要这么做？也许当我们经历和谈论这个问题时，我们也可以谈谈我们自己的经历——真实的生活，日常生活——我们正在进行的项目，我们正在部署的软件，以及我们面临的一些现实挑战。就我个人而言，我参与的一些项目，我真的希望我们在这方面有更多的能力。我们谈到的一些原因，我们在持续集成、合并以及持续部署方面面临的挑战，部署新功能的风险，这些功能标志对我们非常有益。

有了这些，至于你能用这些特征标志做什么，你能做的一件真正重要的事情是你能在这里降低你的风险。首先，现在发生的是你将部署和发布分离开来。这是一件大事。您可以部署产品和代码，但这并不意味着您一定要发布这些代码。也许还没有人看到那个代码，它仍然处于休眠状态，你还没有激活它。你在分离它。这给了你很大的灵活性和力量。

乔恩:我想在接下来的几件事情上这样做。我在考虑[…]我们即将发布的几个改变世界的功能。如果出了问题，那就不太好了。每个人都是这么看的。尽管需要重新测试，但在人们真正在生产中使用它之前，我不能 100%确信它是完美的。

克里斯:随着风险的降低，你也可以对你的用户进行细分。您可以部署它，也许您有数千个用户，或者 100，000 个用户，但是您可以设置它，以便它只是一个很小的子集，这些人将会看到它。这将是功能标志和条件中上下文的一部分，现在不是成千上万的人看到它，可能只是 20 或 50 或 100。通过能够以用户为基础进行控制，比如谁可以看到这个，你真的降低了风险。

有助于降低风险的另一件事是，我们只面临测试的挑战，尤其是围绕数据的测试。我们努力营造反映生产的环境。您甚至可以拍摄生产数据库的快照，并将它们恢复到其他环境中，但这有点问题。

Jon:在一些高合规性的环境中可能是不允许的。

克里斯:当然。能够在真正限制生产的试运行环境中进行测试是一个挑战。真正限制产量的只有产量。特性标志有助于降低这种风险，因为它允许我们在 prod 中进行实际测试，这听起来很疯狂，“什么？我们要在生产中测试？”但是，当你最小化这种情况的范围和谁看到它时，它就会出现在你的头上，给你很大的灵活性和权力，同时减少风险和影响。有一些实现细节和挑战，我们稍微掩饰了一下，但总的来说，这些原则是绝对可靠的，并且适用于那里。

乔恩:当然。这感觉像是要写很多软件。如果你在做这件事，而且做得很好，你就有了一个管理工具，它可以让你进去，告诉你谁能看到，谁不能，或者你可以做一些事情，比如控制谁获得任何功能的百分比。就像是，“嘿，等等，我们已经很忙了，要满足业务需求。我真的没有时间去写一个特征标志系统。”你对此有什么看法？

克里斯:这可能非常复杂，尤其是如果你想从中获得最大的动力。这肯定需要大量的工作和代码。很多设计。只是认真思考问题，比如什么是功能，什么是应该启用还是禁用的功能？你如何给这些东西命名？你是怎么管理这些东西的？它是一个配置文件吗？是数据库吗？它是成熟的功能管理服务吗？这些都是你要考虑的现实世界的事情。

又是爬行-行走-奔跑。我认为无论你在哪里，开始这样思考，从简单开始，都是有价值的。对于每个人来说，都有一个标准的发展，几乎每个人都是这样开始的，“我只是要有一个功能切换。它将基本上是硬编码的，如果我要更改它，那只意味着代码更改和重新部署。”你就是这样开始的。

如果你对某个特性不太确定，或者它是一个更大的特性，而你只是想在它上面运用这种技术，那么就继续下去，把它作为一个特性切换来做。要非常有策略，这样你就不必去创建一百个特性标志或特性切换。从一个开始，然后可能会到少数人或类似的人。这可能是你做了三、六个月的事情，只是有点想尝试一下。然后你可以从那里开始。

我认为在遗留系统和正在运行的系统中采用这些特性标志要困难得多。与从头开始相比，仅仅是重构和放置这些东西，适当地修饰你的代码就要做更多的工作。显然，这变得容易多了。当你从零开始的时候，我认为你有更多的灵活性和更多的余地来考虑这个更长远的问题，如果你决定这样做，“嘿，这是我真的想采用的设计哲学。”

乔恩:我想了解两件事。第一，我有点惊讶，当我谈到你实际上是如何构建一个完全不同的产品来管理你的功能标志，一旦它变得相当复杂，我有点惊讶，你没有提到有商业产品可以帮助你做到这一点。

克里斯:是的，已经有公司涌现出来处理这个问题。LaunchDarkly 就是这样一家初创公司；在 Atlassian 工作的一些人那里 Atlassian 大量使用了特征标志。

乔恩:[……]亚特兰蒂斯人大量使用了特征标志。

克里斯:显然，他们有自己的内部工具和服务，他们建立这样做，“嘿，我们应该把它变成一个产品。”我肯定这是思考的过程。

Jon:几年前，我在 Gluecon 上遇到了一些来自 LaunchDarkly 的人，他们都是好人。我认为没有任何工具能够解决决定代码的哪些部分应该运行，哪些部分不应该运行，以及理解这个决定的背景的问题。所有像 LaunchDarkly 这样的东西都可以让你很好地了解什么在运行，什么没有运行，并且能够打开旋钮。

克里斯:这是一个伟大的观点。也许更直截了当地说，艰苦的工作在你的背上。这些工具不能为你做艰苦的工作。你必须这么做。借用 AWS 的术语，他们只是让其他人来做一些无差别的繁重工作，但你必须再次做设计的艰苦工作。功能标志应该是什么？我该如何做决定？那是什么背景？它的过期策略是什么？我要怎么做细分呢？我如何管理谁可以更改这些特性标志？这是一个很大很大的领域，你也必须处理，只是围绕它的行政政策。如果你做了这样的事情，“我要改变这个特性标志，[…]对 100%的用户，而不是只对 2%的用户，”这可能会导致重大问题。

Jon:在 LaunchDarkly 的辩护中，有一个通用的模式，可能很多产品都会用到。如果你的上下文通常是用户或用户会话，那么你可以说，“嘿，黑暗启动，这就是你要去的地方。找出目前登录的用户。”这种集成被认为可能会更容易一些，但是它仍然不会出现在您的代码中，也不会打开或关闭特性。它不知道你的特点是什么[…]。

克里斯:仅仅因为你必须做艰苦的工作并不意味着像“黑暗发射”这样的事情不应该是其中的一部分。如果你真的达到了需要功能管理服务的复杂程度，那么你真的应该认真考虑是否要推出自己的功能，而不是类似于 LaunchDarkly 这样的功能。这就是他们存在的原因，他们已经筹集了近 8000 万美元的投资资金。它们从 2014 年就有了。您应该考虑利用现有的基础设施。但还是要意识到，他们不可能为你做所有的事情。没有什么能改变你必须去修改代码的事实。如果不修改代码，就不能使用 LaunchDarkly。

乔恩:我说过有两件事我仍然想深究。另一个只是我在对话开始时想到的事情。您想要关闭对某些功能的访问。很多时候，如果你的软件是面向用户的，而有些东西你还不想让用户使用，因为它是新的，你可以把你的特性标志放在 UI 中。我在想这在你看来是禁忌还是可以的。基本上，如果关闭了该功能，就不要显示这个 UI。然后，也许整个后端 API 是开放的，每个人都可以使用，谁在乎呢，因为只有拥有客户端的人才能看到激活后端的 UI，才能实际使用它。对此有什么想法吗？

克里斯:我认为你完全可以根据具体情况决定什么是有意义的。如果你有一个典型的微服务架构，或者你有像 Reactor 或类似的前端 JavaScript 客户端，你有一个后端 RESTful 微服务，它实现了这些客户端调用的 API。如果你只有这些 API 订阅者，那么你可能只需要在前端标记特性，这样 JavaScript 就不会进行这些调用，你也不用担心在后端标记特性是否处理这些调用。

按照设计，没有人会去做这些调用，除非是你的前端 UI 客户端。如果那样做很干净，你可能会省去很多麻烦。如果你有其他的客户，或者更多的是公共服务和公共 API，那就另当别论了。我觉得还是要具体情况具体分析。我宁愿选择对你来说最实际的方法。

乔恩:我正坐在扶手椅上，所以我想做一点扶手椅建筑。在我看来，如果你正在接近这一点，你只是在爬-走-跑，那么一个好的原则可能是试图限制发生的数量。保持小规模。尽量把它放在尽可能多的已知和预期的地方。如果有一个地方，你有你所有的前门代码，如果你可以限制它，就像，“功能标志打开和关闭。这些只是两三行代码，不是这个庞大的 500 行方法，它需要在开头和结尾或者在开头和中间。”

如果有一种方法可以组织你的代码，让特性标志非常清楚地关闭它的一小部分，打开它的一小部分，似乎每个人都会更容易理解。我这么说是因为人们可能会考虑如何在他们自己的项目中实现这一点，这似乎是有用的。

克里斯:我认为这适用于模块化和重构之类的东西。理想情况下，您应该在函数级别上这样做。如果标志启用，调用此函数。如果不是，那么调用这个函数类型的东西。同样，可能只是这些条件块主体上的函数调用。这就是你在这些功能中实现不同方面的地方。这些应该被重构，也许它们实际上最终使用了许多完全相同的代码。它们被进一步重构了，但差异也随之而来。代码最终读起来非常好。这就像，“这是实现新功能的地方，这是现有功能或后备功能的地方。”

Jon:你从中获得的另一个优势不仅仅是模块化。尽可能地，你不希望有多个巨大的不同逻辑的不同代码路径，所有这些都是活跃的，因为这样会有更多的地方出错。和你的代码一样多，应该是一样的。我不知道更好的方式来说，但只要可能，不要重复代码。

克里斯:是的。一定要考虑你的政策是什么，什么是可标记功能，什么不是。如果在特性标志中开始特性标志，事情会变得非常棘手。你肯定想考虑清楚。如果你开始发现自己处于最高水平，你有两个选择。每当你有另一个特征标志时，它就是一个二叉树。在你意识到之前，可能有 16 种不同的代码路径，很快就变得相当混乱。什么是原子单元特征标志的粒度级别是什么？

乔恩:好的，克里斯。这让我想起了一两年前你和我在丹佛与 Strava 的一次谈话，当时他们谈到了何时使用特征标志。那真的很酷也很有趣。我觉得这里值得重复一下。

当他们构建一个新功能时，他们不确定它对于这个核心产品是否足够好，他们只是想知道人们是否喜欢它，他们不想让该功能的开发受制于他们对 Strava 应用程序中所有其他内容的严格代码要求。他们只是想让人们制造出一些东西，很少经过测试。

我不记得这个词了。这本质上是一个精益过程，只是让一些人看到他们喜欢它，但在生产代码库中，真正的 Strava 而不是 Strava 的一些测试版本。他们会为此使用特性标志，只为少数人打开它们，获得反馈，关闭它，然后使用他们的开发和工程指南重写代码。扔掉所有的代码，重新开始。这是他们控制技术债务的一种方式，但也仍然能够非常精简和快速。

Chris:这真的很类似于 AB 测试，审查这个功能是否得到了用户的反馈。这是一种低风险、简单易行的方式。这变得非常非常普遍。所以很多大公司都这么做。网飞经常这样做。所有大公司都是这样做的。他们有数百万用户。他们想知道，“这个新东西怎么样？人们会对此作何反应？”不要让每个人都可以使用这种改变，只让少数人使用，从实验中进行测量，然后在此基础上做出决定。

Jon:我希望在接下来的一两个月里，我们能找到解决没有特性标志给我们带来的一些问题的方法。

克里斯:是的。我们现在的情况是，我们有一个已经工作了三年多的系统，它非常复杂，有一个非常大的代码库，但是拥有功能标志真的有助于解决我们日常面临的一些挑战。对我们来说，这将是一个有趣的问题，我们能更多地利用特性标志吗？

Jon:还有遗留代码库。很好。非常感谢你告诉我这些，谢谢你，瑞奇，让我们在一起。

理奇:当然。

克里斯:好吧。谢谢伙计们。

乔恩:下周再聊。

克里斯:再见。

亲爱的听众，你坚持到了最后。感谢您抽出时间，并邀请您继续与我们在线交流。这一集，以及显示笔记和其他宝贵的资源可在 mobycast.fm/66.如果你有任何问题或额外的见解，我们鼓励你给我们留下评论。谢谢你，我们下周再见。