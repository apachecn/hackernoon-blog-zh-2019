# 采访 GitLab (GTLB)

> 原文：<https://medium.com/hackernoon/interviewing-at-gitlab-66539aeb28c0>

![](img/a2ee0e42b839458a160921c93d58019f.png)

Lesson Learned: You Have to be Born Into It

***更新！*** *Gitlab 亲切地联系了我，并通过电话提供了团队的反馈。你可以在文章的结尾看到这个反馈。*

***更新 2！*** *尽管手册中承诺提供合理的面试、关于面试的反馈以及公平的面试，但下面所有这些困难的真正原因似乎是，Daniel Peric 可以用 Nicki Peric 填补他自己的职位，以便他可以继续担任技术客户经理的角色。你没看错——奇怪的是，团队选择“让他们成功”的是他们自己家庭的成员。GitLab 坚持汉隆剃刀(“不要把可以用错误解释的事情归因于恶意”)坚持认为，一系列方便的错误导致某人的家庭成员被雇用而不是更合格的候选人。*

GitLab 是我最喜欢的项目之一。我对产品和团队只有赞美。只要有可能，我就试着向同行技术人员甚至我工作的公司介绍 Gitlab 的设置、接受代码和启动 CI/CD 任务是多么容易。看着团队相互转换驾驶技巧，看着分布式跑步者之间的管道都显示为绿色，所有这些都是在一个下午建立起来的，令人上瘾。

不止一次，我负责 GitLab 的安装和所有*护理和喂养*。毫无疑问，他们基于 Chef 的安装和配置管理总是为我工作。升级是一件轻而易举的事，每个月的 22 号都会出现，没有人会因为一个不明显的错误而让我处于某种不可恢复的状态。在我的职业生涯中，我不得不参与的许多其他软件产品就不一样了。

当我看到下面的推文时，我抓住了这个机会。

我当然想成为一名实施工程师！

# 第一轮，早期评估

申请的第一步是通过电子邮件发送一份小问卷。大多数问题都是关于你将如何帮助客户、在什么情况下你可能推荐虚拟机或 Kubernetes 的文章式问题，以及一个关于 HA 的有点模糊的问题。问卷的最后一部分写道:“展示你在 AWS 或选择的云平台上安装 omnibus GL 的能力”。我打算让你在他们的 onboarding 平台上提交所有这些内容，用一个相当小的盒子来装我输入的冗长内容，但是除了……

我准备了我的答案，安装了 Omnibus，然后将我的答案签入到新的 GitLab 安装中。我在新的 GitLab 装置中提交了一个项目链接。:)

在这一步，由于语言指定了 [*综合*](https://docs.gitlab.com/omnibus/) ，我假设这意味着我应该手动安装 GitLab。我已经这样做过无数次了:几分钟之内我就完成了我的装置。我真的很想使用谷歌管理的证书，我认为这也将涵盖调查问卷中的一个 HA 问题。配置了一个基本的 GCE 虚拟机来托管安装。

我的配置示例如下:

```
registry_nginx['listen_port'] = 80
registry_nginx['listen_https'] = falsegitlab_rails['db_adapter'] = "postgresql"
gitlab_rails['db_encoding'] = "unicode"
gitlab_rails['db_database'] = "gitlab"
gitlab_rails['db_username'] = "gitlab"
gitlab_rails['db_password'] = "supersecuregitlabpassword"
gitlab_rails['db_host'] = "10.26.241.3"
```

TLS 在 Google 负载平衡器处终止，带有 Google 托管证书。所有这些都包含在这个装置唯一包含的项目的 README.md 中——响应中链接的同一个项目。

我通常会用 Chef、Puppet 或 Ansible 来管理这样的东西，但是用 manual 来确保我没有提交不必要的东西。

旁注:以防读者以前没做过这个…综合安装者*是厨师！*更新配置非常简单，我想不出有哪一次 Omnibus Chef 内置管理让我失望了。

# 第二轮，面试

我完成了对人力资源代表的面试。对话令人愉快，引人入胜。我们和招聘经理安排了另一次面试。

对招聘经理的面试也很棒。与之前的采访相比，这次谈话更加专业，但是两位技术专家很快就开始谈论我们喜欢的东西——技术！

# 第 3 轮，技术演示(A 部分:准备)

不幸的是，这是一切分崩离析的地方。我之前收到过在云提供商上安装 GitLab 的指示(再次),只是这次明确指出它应该按照 IaC(基础设施即代码)原则安装。没问题！

我有一周的时间准备。在那一周，我决定看看将 GitLab 安装到 Kubernetes 的工作进展如何。我一直关注着 [Gitlab 云原生头盔图](https://docs.gitlab.com/charts/)，迫不及待地想使用它。老实说，我最初的几次尝试是对 [nginx 入口控制器](https://kubernetes.github.io/ingress-nginx/)配置的一知半解，但我很快发现这是不必要的——正如我应该预料的那样，GitLab Helm Chart 与 GitLab Omnibus 安装程序一样好，几乎可以通过非常方便的配置为您做任何事情。该文档广泛涵盖了使用外部数据库等用例。掌舵图确实很棒，但确实表明 Geo 和 Pages 不打算与它一起使用，因为团队正在解决一些技术挑战。

去年年底，谷歌宣布了云 SQL 数据库的[私有 IP 特性。与](https://cloud.google.com/blog/products/databases/introducing-private-networking-connection-for-cloud-sql)[谷歌共享服务 API](https://cloud.google.com/service-infrastructure/docs/service-networking/reference/rest/) 配合使用，现在自动将云 SQL 数据库添加到您的项目中变得极其容易。以前，通常会有一些使用[云 SQL 代理](https://cloud.google.com/sql/docs/mysql/sql-proxy)的工作——通常很麻烦，而且，由于您将*从项目中路由出*并将*返回到*，因此有理由假设存在更高的延迟、流量成本和安全性问题。我在我的安装中使用了这些特性，为 Gitlab 安装提供了一个高可用性 PostgreSQL Cloud SQL 实例(相当于一张嘴)。

所有这些都是由 HashiCorp 的 Terraform 驱动的。一个包含大量 README.md 的项目链接被发送给采访团队。实际上，*发送了两个链接*——镜像到 gitlab.com 的项目，以及签入由上述所有工作创建的装置的项目。

我使用了一些伟大的新功能，有机会与 GitLab 头盔图表，并有一个防弹部署。我都准备好了！

如果你有兴趣，你可以在这里找到这个项目:[https://gitlab.com/e_snyder/gitlab-setup](https://gitlab.com/e_snyder/gitlab-setup)

# 第 3 轮，技术演示(B 部分:事情偏离方向)

读者可能会认为这一部分的标题暗示我的平台不起作用。他们完全错了。

第一个问题是，实际的招聘经理突然无法参加技术演示。技术演示与一个没有招聘经理的面试团队重新协调。对我来说，这应该是第一个危险信号。

在技术演示的那天，我首先问是否有人有机会看这个项目。包括相当广泛的 README.md。在 README.md 里面，我概述了我所做的决定(私有 IP、服务网络、掌舵图等)。我还包含了一个示例*运行手册*，其中包含了所有必需的变量(在本地，我将这些放在一个‘terraform . TF vars’文件中——这是一种非常常见的模式)。

不幸的是，**我真的没有收到任何断言说团队在技术演示**之前已经阅读了 README.md。README.md 还包括一小部分，提到包含的 terraform 创建了一个全新的项目——这有时会使首次运行变得复杂，因为没有启用 API。这个后面很重要。

我等待我的屏幕共享，并在现有安装的用户界面周围戳来戳去，以证明它的工作。为了开始工作，我被要求破坏这个项目。没问题！我说过它很有可能不会在第一次尝试中被破坏，因为头盔提供者，庞大的 GitLab 安装，以及完整的项目破坏。

团队很惊讶。他们问“为什么行不通？”我尽力回答了销毁 Kubernetes 名称空间、禁用 API 和其他冗长的任务通常需要时间来完成，而 terraform 很可能会出错。我还解释了一个不对称的操作手册，它只破坏了项目，会立即破坏部署。或者，我们可以创建一个新项目(所有项目资源都有“昵称”terraform 资源用于随机命名，这意味着我们可以随时创建一个新项目，不会有冲突或重叠)。我不认为他们理解这些选项。

“好吧，无论如何，让我们运行销毁，”他们说，不出所料，我们遇到了一些错误，包括名称空间待定删除。我使用了 3 到 4 次`terraform state rm`来清除资源。毁灭在其他方面运行良好，我带领团队通过 GCP 控制台进行了一次快速浏览，以证明这一点并解释 terraform 命令。

# 第二个危险信号——谷歌 API

**这时我有了一个怀疑**。我问是否“大多数其他人为此使用 Ansible”，团队似乎同意。值得注意的是，使用 terraform 安装并移交给 Ansible 只需要关闭一个 GCE 虚拟机。没有 API(取消)供应或等待容器停止。

**对我来说，这将是危险信号# 2**——我选择了一种部署方法，很明显，团队并不熟悉这种方法。事后看来，如果我知道团队只熟悉单个 VM 安装，我会创建一个包含所有必要 API 的项目，并简单地从现有的 Kubernetes 集群中添加/删除 GitLab，而不是演示一个从零开始的“从零到 GitLab”方法。

随着项目被破坏，我们继续前进。我再次警告说，我们可能会遇到这样一种情况，我们有一个 API 供应缓慢。我们在安装过程中没有发生这种情况。安装按预期顺利进行。

对于那些不熟悉 terraform 和 GCP 的人来说，当 API 在项目尝试使用它之前没有完全上线时，它看起来是这样的:

```
* google_dns_managed_zone.gitlab_zone: 1 error(s) occurred:* google_dns_managed_zone.gitlab_zone: Error creating ManagedZone: googleapi: Error 403: Google Cloud DNS API has not been used in project PROJECTNUMBER before or it is disabled. Enable it by visiting [https://console.developers.google.com/apis/api/dns.googleapis.com/overview?project=P](https://console.developers.google.com/apis/api/dns.googleapis.com/overview?project=940210438455)ROJECTNUMBER then retry. If you enabled this API recently, wait a few minutes for the action to propagate to our systems and retry., accessNotConfigured
```

使用 terraform 和 GCP 的人很可能对这一小段非常熟悉。解决方案是完全按照错误消息告诉您的去做—等待几分钟让操作传播并重试。

terraform 的这些 API 操作很容易出现问题，terraform 团队做了大量工作，等待 Google 报告 API 可用。Google API 不会等到 API 在所有地区、区域和系统中 100%就绪。请再次参考 Google 在 API 报告准备就绪后给出的错误。

# 危险信号#3 和#4，地形语法

如前所述，我在本地有一个不在我的回购中的`terraform.tfvars`文件。通过我的 IDE，我被要求点击我的项目中的每个文件，当我到达 tfvars 文件时，我被问到，“那个文件是什么？这不是地形。”

“这些是我的变量，包括我的项目账单、数据库机密和其他我不想签入的项目，会被自动加载，”我回答道，并说出了我的答案。gitignore 显示它已被排除。

“你是怎么装的？你能在代码中告诉我你在哪里通过名字加载那个文件吗？”面试官问。

**红旗#3** 的形式是不得不解释几次`terraform.tfvars`是由 terraform 自动加载来覆盖变量的。我不能通过名字在代码中显示它是在哪里加载的— [它是自动的](https://www.terraform.io/docs/configuration/variables.html#variable-definitions-tfvars-files)。我完全不知道面试官在问什么，也不知道我是否回答了他的问题。

在项目部署期间，我们进行了一些其他的对话。“你熟悉 AWS 和 terraform 吗？”出现了。是的，我提到过*我觉得更多的 AWS 资源有嵌套块——这有点烦人，因为你不能迭代嵌套块。*

面试官突然叫住了我。“是的，你能”，他说。我打开了一个 Atom 窗口，准确地打出了我的意思。

```
resource "fakeresource" "myfakeresource" {
  config_block {
    ...
  } config_block {
    ...
  }
}
```

例如，在 GCP 创建了[防火墙规则的人应该熟悉这种模式。](https://www.terraform.io/docs/providers/google/r/compute_firewall.html)[谷歌计算安全政策](https://www.terraform.io/docs/providers/google/r/compute_security_policy.html)也广泛使用这些嵌套块。

“你不能迭代这些块”，我说。“你可以，”面试官坚持说。我解释了 terraform 的`count`只对资源有效。他认为你可以“传递一个数组”(不知道这是什么意思，它不是一个东西)。在 Google Compute 安全策略的情况下，该数组只能包含 5 个成员——如果有 10 个规则，则必须重复该块。“不，你不知道，”他说。

我不是说要把谁叫出来，而是面试官完全错了。在 terraform 0.11 中，没有办法像这样迭代嵌套块。你可以看到[这个特性正在 terraform 0.12 中实现](https://www.hashicorp.com/blog/hashicorp-terraform-0-12-preview-for-and-for-each)。

我尽我所能不显得粗鲁或僵持，但面试官坚持这是完全可能的。出于礼貌，我换了个话题，谈到了**的第四面红旗。**

# 红旗#5、#6、#7、证书

完成`terraform apply`是逃离上面嵌套的块对话的机会。有了 GCP Kubernetes UI 作为后台，我提到我们必须等待 certmanager 进程来获取一些证书。这是完全预料到的，也是安装的正常部分。

“现在就去参观吧”，面试官要求道。由于使用了 HSTS，我无法在没有证书错误的情况下访问该页面，这在 Chrome 中是无法绕过的。我试着解释这个。“只要按继续”是回应。HSTS 错误页面显示时没有继续按钮。这正是所发生的事情，因为我们没有等待证书完成。没有任何进展。同样，这完全是安装的正常部分。

“你可以进行了。“继续前进”是坚持不懈的指示。我坚持要我们等待，同时盯着一个没有继续按钮和 HSTS 错误的窗口。certmanager 过程不会花那么长时间来完成。如果他真的想让我这么做，我或许可以用 Firefox 绕过与 HSTS 证书相关的限制，因为我还没有用那个浏览器访问过支持 HSTS 的网站。采访者(在这一点上是多个)认为 Chrome 会运行良好。并没有。他们可以在屏幕共享上看到我的屏幕，并看到没有*继续*对话框。这是一个危险信号，因为当他们的目标是绕过 HSTS、GitLab、nginx 等的安全机制，并拒绝完全承认 HSTS 时，暗示我没有正确使用浏览器似乎很屈尊。

Chrome 包含以下信息:

```
You cannot visit <site> right now because the website uses HSTS. Network errors and attacks are usually temporary, so this page will probably work later.
```

“如果你不能这样做，那么就通过 IP 访问它”，丹尼尔·佩里克建议。我解释说 nginx 中没有路径，我们将在 nginx 的默认后端结束。“那应该行得通”，丹尼尔说。那不行。那不行。没有与 IP 匹配的入口。没有只匹配 IP 的 nginx 主机。**危险信号#6:** ***你将在 nginx 入口控制器的默认后端结束，这正是所发生的事情。***

“在端口 80 上按名称打开它”采访者然后陈述。在这一点上，我有点恳求我们只是等待证书完成——这不会花很长时间。我解释说，我们无法在 80 端口上访问它，因为 [HSTS 已启用](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security)，而且我已经通过 HTTPS 访问过该站点(在技术演示开始时)。“真是奇怪。你的安装有问题。这应该行得通。”

面试官错了。又来了。

**出现 7 号红旗。采访者提出的我的安装的问题是 GitLab 安装的一个特征。通过普通 IP，或端口 80，或 IP:80，或主机名:80 的任意组合进行访问…这是行不通的。那永远行不通。那不行。只要 Gitlab 采访者有几分钟的耐心，让 GitLab 获得 GitLab 工程师设计的 GitLab 证书，作为 GitLab 云原生头盔图的一部分，就可以让我们正常访问它。**

在所有这些对我的“坏掉的”装置不屑一顾的过程中，另一位采访者插话说“嘿，现在可以加载了”。

是的。是的，确实如此。:)

奇怪的是，对我的装置的成功挖掘并没有进一步深入。面试官没有要求我提供登录账户，也没有要求我访问任何状态页面。据他们所知，ssh 克隆被破坏了，或者登录屏幕之外的页面甚至无法工作。

当然，所有这些都在我的装置中工作。但是为什么不验证呢？

我借此机会问团队我做的事情是不是不成功或者看起来很奇怪。“大部分人用 Ansible”。

# 反馈

我实际上没有收到任何反馈。我收到了一封普通的拒绝邮件。

我礼貌地给和我一起工作的招聘人员发了电子邮件，请求反馈。我没有得到回答。我给招聘经理和招聘资源发了邮件。我没有得到回答。

我很有礼貌，并要求我接受有关技术演示的反馈。四天后，我收到了一封电子邮件回复:“机会似乎是围绕 GitLab 的安装，并提供与招聘团队讨论问题的能力”。

Gitlab 手册概述了一个完整的反馈流程，以及用于响应反馈请求的模板。当发现手册中的过程没有提供一些关于完美工作的安装是如何失败的结论时，看起来完全武断的决策缺乏透明度只会更加令人痛苦。

团队甚至没有登录到实例。据我所知，我失败了，因为 GitLab 自己的 certmanager 流程超出了面试官的耐心。

我被难倒了。我还是很惊讶。除了团队认为我没有部署 GitLab 或者我没有恰当地回答他们的任何问题之外，我还没有收到任何反馈。我们没有时间回答很多问题——我们太忙了，被 terraform 和 TLS 的工作方式分散了注意力。

我理解这个角色与可能有相同观点和顾虑的客户互动，但这是一个技术演示！

# 结论

我非常尊重 Gitlab 每个人的工作。我坚信我的面试官是伟大的技术专家。他们似乎对 Kubernetes 和 terraform 一点也不熟悉，他们已经演示得很清楚了。

不幸的是，想到我因为一个充满问题的完整安装工作演示而被解雇，这是非常痛苦的，这些问题不是关于安装，而是缺乏经验的团队成员对 terraform、TLS 和 Google APIs 如何工作的挑战。我怀疑，如果被问及，答案将是*我误解了*，但没有太多的回旋余地来争论 terraform 0.11 中不存在的功能或 Chrome 应该反对 HSTS，因为有人不耐烦了。

我认为招聘经理不在场是不公平的。

对于下一个有希望申请 GitLab 的人，不要忘记这篇文章中响亮的智慧:使用 Ansible！:)

# GitLab 的反馈！

在请求反馈的一周之后，有一个 GitLabber 通过电话联系我，以便更好地提供面试团队的反馈。有趣的是，在那一周，一位 GitLab 招聘经理告诉我，技术演示有问题，招聘经理很快回复说没有问题——这是一个竞争激烈的职位。

有 ***被*** 团队反馈为负面。招聘经理撒谎了。git lab 手册的意图就是这样…

由于我没有收到书面的，我会尽我所能转录它。

**git lab 团队不理解基础设施即代码(IaC)规程**

提供的反馈中有一部分是“技术演示的目标被误解了——已经安装了 GitLab 的一个现有实例”。

概括地说，IaC 是我们使用开发人员已经能够使用的相同规程来规划和实现基础设施的能力。我们有能力使用 terraform 这样的工具来描述任何基础设施的预期状态，部署、测试、迭代、更新，并根据需要“从头”重新部署。操作应该是可复制的和幂等的。

我在技术演示中带来了一个预先存在的 GitLab 安装，以考虑关于 HA、DR、云 SQL 的问题，以及关于如何安装 GitLab 的其他主题。我证明了我能够销毁它，根据需要迭代它，并重新部署它。

我不禁觉得我带来了超出目标的职业期望。很难想象一个 GitLab 这样的公司的实施工程师会对此感到困惑。难以想象的是，一个实现团队在头脑中没有这些明确的目标来交付给客户。

**git lab 团队不明白他们问的问题**

从上面的回忆来看，大多数问题都写得一模一样，我不记得有任何一个问题是有效的(“在代码中显示加载文件[terraform.tfvars]的位置”是无效的)。我很乐意花时间向客户解释“HSTS，或 HTTP 严格传输安全，通过明确防止从 HTTPS 降级到不受保护的 HTTP，防止浏览器和服务器之间的中间人攻击”。对于客户，如果有未回答的问题或顾虑，我们将有机会*提出这个*或做额外的研究。我们可以一起研究。GitLab 的实施工程师应该熟悉这一主题，尤其是曾经在 Kubernetes 中使用过 nginx 入口控制器的人，该控制器包含在 GitLab Cloud 原生 Helm 图表中。

对于采访者来说，宣布装置被破坏是因为 HSTS 没有机会推断采访者和 HSTS 之间完全不熟悉以外的任何事情。再说一次，这是我以前无法想象的。

技术演示并不适合即兴讲授 LetsEncrypt、ACME Challenges 和 HSTS，但如果我知道这是必要的话:如果有机会，我完全愿意教 Gitlab 工程师什么是 HSTS 以及它与 Gitlab 安装的关系。它对 Gitlab 的运行至关重要，任何为客户安装它的人都应该非常清楚地理解它。

没有人问我关于服务网络 API 或它如何有利于安装的问题。我没有被问到高可用性。没有人问我为什么选择云 SQL 实例和/或 Kubernetes，而不是使用 PostgreSQL 和 Omnibus 的虚拟机。我没有被要求解释 HSTS。

**git lab 团队对我问的问题撒了谎**

这一个非常令人惊讶。

我问的问题是，如果从云本地舵图表安装不是很常见。我问其他技术演示是什么样子的。我问我所做的是否有意义。“不太常见”、“是的”、“当然”、“嗯”、“可能”是答案。面试官没有提供关于技术演示或整体演示的对话。面试官不愿意讨论实际的技术演示…我想我们已经谈完了。

根据这个职位的列表，我应该有另一个不是技术演示的面试(如果我没记错，技术演示实际上是“可能”而不是“意愿”)。我认为这里更适合回答关于 GitLab 价值观、在那里工作有多“有趣”、那个人对手册做出了什么贡献以及其他话题的问题。

# 更新结论

我不得不假设我是在招聘经理不在场的情况下被一个经验不足的团队面试的。你不会让我相信 GitLab 专业服务不了解 HSTS、terraform 或基础设施即代码原则。

我的采访者与这些概念进行了斗争，以至于留下了与成功的 GitLab 安装截然相反的反馈。 ***你不会让我相信我不知何故没有抽到 B 队的运气，因为这不是 GitLab 的标准(嗯，更新——我们了解到这是为了让 Daniel Peric 可以雇佣 Nicki Peric)。***

我最初的建议仍然有效，只是稍微扩展了一下，适用于任何渴望得到我申请的职位的人:了解你的受众——只需使用 Ansible 将 GitLab Omnibus 放在一台虚拟机上，使用一个没有 TLS 的本地 postgresql 数据库……并询问是否每个人都喜欢早上冲浪。对于任何企业来说，这都是一个非常不合适的安装，并且与任何专业期望都非常不符合，但我相信，在采访我的团队眼中，这应该是一个成功的技术演示。

对于任何值得交付给客户的 GitLab 安装来说，这都是一个更合理的起点*——但是请确保您纵向扩展了数据库节点并关闭了抢先节点设置！*

*[https://gitlab.com/e_snyder/gitlab-setup](https://gitlab.com/e_snyder/gitlab-setup)*

*我真的很高兴我使用了一个微实例和抢占式节点……这堂课在超额完成方面的花费会少很多。*