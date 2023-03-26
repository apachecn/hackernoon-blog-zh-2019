# Ranorex GUI 测试自动化工具的优缺点

> 原文：<https://medium.com/hackernoon/the-good-and-the-bad-of-ranorex-gui-test-automation-tool-c65c70fac3ca>

![](img/8db4b5e3976ab0fd9b659bc9bc56cc05.png)

图形用户界面(GUI)——应用程序的外观和感觉——是首先吸引用户眼球的东西。因此，这就是应用程序的评判标准。因此，确保适当的 GUI 功能和无缝交互是至关重要的。这可以通过从用户的角度测试 GUI 来完成。

执行 GUI 测试是为了验证用户可见的功能，如菜单、按钮、图标、文本框、列表、对话框等。它还确保外观元素(如字体和图像)符合设计规范。GUI 测试发生在系统测试级别。要了解更多关于测试级别的信息，请访问我们的[软件测试白皮书](https://www.altexsoft.com/whitepapers/quality-assurance-quality-control-and-testing-the-basics-of-software-quality-management/?utm_source=MediumCom&utm_medium=referral)。

自动化 GUI 测试可能非常棘手，用户行为也很复杂。因此，使用现成的 GUI 测试自动化工具是有意义的。我们已经详细阐述了 [Selenium 测试自动化工具](https://www.altexsoft.com/blog/engineering/the-good-and-the-bad-of-selenium-test-automation-tool/?utm_source=MediumCom&utm_medium=referral)的优缺点。在本文中，我们将关注另一个流行的自动化框架， [Ranorex Studio](https://www.ranorex.com/) 。

# 什么是 Ranorex？

Ranorex Studio 于 2007 年由奥地利软件开发公司 Ranorex GmbH 推向市场，是一款商用 Windows GUI 测试自动化工具，为桌面、web 和移动应用程序提供测试。

Ranorex 不使用任何特定的脚本语言，它是建立在微软的基础上的。NET 平台。该框架支持标准编程语言 C#和 VB.NET 来编辑记录或创建自定义测试。

Ranorex 基于 [XPath](https://en.wikipedia.org/wiki/XPath) 查询语言，支持在基于 web 的应用程序中更容易、更高效地搜索组件。Ranorex 记录和回放接口允许通过记录 UI 动作来自动化 UI 测试。Ranorex 的无代码方法使测试变得更加简单，它绝对是初学者的首选框架。

Ranorex Studio 支持广泛的环境、规范和设置。其[广泛的技术支持](https://www.ranorex.com/supported-technologies/)包括:

*   Windows 操作系统 7 至 10 和 Windows Server 2008 至 2016
*   Windows 桌面技术，包括。NET、ActiveX、Delphi、Java、Telerik 和 Microsoft Office 应用程序
*   Android 和 iOS 上的移动测试
*   在所有主流浏览器上进行跨浏览器测试

Ranorex 是在[连续交付](https://www.altexsoft.com/blog/business/continuous-delivery-and-integration-rapid-updates-by-automating-quality-assurance/?utm_source=MediumCom&utm_medium=referral)和[开发](https://www.altexsoft.com/blog/engineering/devops-principles-practices-and-devops-engineer-role/?utm_source=MediumCom&utm_medium=referral)环境中实现自动化测试的一个很好的解决方案，这些环境需要连续测试和对测试结果的快速反馈。Ranorex 提供了广泛的测试工具来集成:最流行的持续集成(CI)工具、测试管理(TM)工具和任务调度工具。

![](img/db669c5f7c361cd62bb0fbcbec95e2ee.png)

*Ranorex integration with the leading testing tools; Source:* [*Ranorex Brochure*](https://www.ranorex.com/rx-media/files/Ranorex-Brochure-EU.pdf)

通过[Selenium web driver](https://www.seleniumhq.org/projects/webdriver/)integration Ranorex 允许内置对象映射、自动超时处理、动态 web 元素的智能识别、在其他操作系统上的测试:Linux 和 macOS。

![](img/9385415e6da752d6a9efc1669cf394c5.png)

*Main characteristics of Ranorex GUI test automation tool*

要全面了解 Ranorex 自动化工具的基础知识，请查看以下资源:

*   [下载全功能 30 天 Ranorex Studio 试用版](https://www.ranorex.com/free-trial/)
*   [Ranorex Studio 用户指南](https://www.ranorex.com/help/latest/)
*   [Ranorex 论坛](https://www.ranorex.com/forum/)
*   [GitHub 上的 Ranorex 自动化包](https://github.com/ranorex/Packages)

## 通用

Ranorex Studio 是市场上最全面的自动化工具之一，因为它为多种环境、设备和应用提供了解决方案，允许对任何桌面、web 或移动软件进行自动化测试。也就是说，Ranorex 被认为更适合基于网络的应用。

![](img/0a36575a8841e7571fe3766bc6d55632.png)

*Ranorex usage distribution among different platforms; Source:* [*Ranorex Studio survey*](https://www.ranorex.com/blog/customer-survey-2018/)

## 流行

数据驱动的研究公司 iDataLabs 的 Ranorex 使用数据可以追溯到近三年前，该公司已经确定了 954 家使用 Ranorex 的公司，占软件测试工具类别市场份额的 1.2%。

![](img/b345939bd6a1e70bec1120317ca43ede.png)

*Ranorex Market Share and Competitors in Software Testing Tools; Image source —* [*iDataLabs*](https://idatalabs.com/tech/products/ranorex)

同一项研究表明，使用 Ranorex 的大多数公司都位于美国的计算机软件行业。

![](img/bebdf799828507b21d89718f53cc4736.png)

*Forty-nine percent of Ranorex customers are in the United States and 10 percent are in the United Kingdom; Image source —* [*iDataLabs*](https://idatalabs.com/tech/products/ranorex)

与审查平台 G2 Crowd 上的其他流行测试自动化工具相比，Ranorex 在 7 个类别中的 4 个方面表现出色:易用性、支持质量、业务易开展性和产品方向。

![](img/5f3ebeaa646024b2aaa8c9186178e34f.png)

*Ranorex Studio, TestComplete, Selenium IDE, and Apache JMeter compared at* [*G2 Crowd*](https://www.g2crowd.com/compare/ranorex-studio-vs-testcomplete-vs-selenium-ide-vs-apache-jmeter)

要了解更多关于 Ranorex 与其他测试自动化框架的关系，请查看我们的文章[比较自动化测试工具。](https://www.altexsoft.com/blog/engineering/comparing-automated-testing-tools-selenium-testcomplete-ranorex-and-more/?utm_source=MediumCom&utm_medium=referral)

# 使用 Ranorex 的优点

## +多平台应用

使用 Ranorex，您可以对 web 和桌面应用以及移动应用进行自动化测试，而 Selenium 和 Katalon Studio 等竞争产品不支持桌面测试，Watir 只针对 web 测试。

## +无代码测试创建

Ranorex Recorder 及其拖放界面允许使用关键字驱动测试进行无脚本测试。一旦有技术经验的团队成员设置了关键字，就不需要任何编程背景来创建和维护使用这些关键字的自动化测试。因此，测试易于阅读和快速设计，因为关键字可以在整个测试用例中重用。

Ranorex 提供了两种关键字驱动测试的方法:

1.  带有[自动化模块](https://www.ranorex.com/#modules)的关键字驱动框架
2.  关键字驱动测试使用[动作表](https://www.ranorex.com/#action-table)。

虽然可以遵循前一个选项而无需编写任何代码，但后一个选项更高级，因为它需要基本的编程技能。

正如我们所看到的，Ranorex 无代码测试允许非程序员将测试自动化应用到他们的项目中。另一方面，Selenium 需要扎实的编程知识，这让非技术测试人员望尘莫及。

## +平滑的学习曲线

鉴于其在代码层之上的用户友好的 UI，Ranorex 被认为是市场上最容易接近的测试自动化工具之一。此外，Ranorex 帮助中心提供了各种资源来简化使用框架的第一步:

*   Ranorex 演示应用程序引导您创建、记录和分析自动化软件测试的过程。伴随着详细的说明，该应用程序可以帮助您熟悉 Ranorex Studio 用户界面。
*   培训使用三种语言——英语、法语和德语
*   指南有一步一步的教程
*   定期举行免费的测试自动化网络研讨会

与其他提供商相比，Ranorex Studio 在使用他们的工具促进工作流程方面做得很好。特别是，Selenium 课程和教程是由第三方提供的。

## +内置图像比较

自动化软件测试主要依赖于 UI 元素识别。尽管许多 UI 元素可以通过文本来识别，但通常情况下，基于图像的自动化是可行的。例如，当一个被测试的项目在 GUI 中改变它的屏幕位置时，基于文本的自动化可能仍然跟踪它的原始位置，这将导致最终不正确的结果。基于图像的自动化解决了这些问题。

Ranorex 智能对象识别技术能够自动检测用户界面的任何变化。它包括以下 [GUI 识别元素](https://www.ranorex.com/help/latest/ranorex-studio-advanced/):

*   [ranor expath](https://www.ranorex.com/help/latest/ranorex-studio-advanced/ranorexpath/introduction/)—XPath 的子集，用于描述、搜索、识别和查找应用程序中的动态 UI 元素
*   [路径编辑器](https://www.ranorex.com/help/latest/ranorex-studio-advanced/ranorex-spy/the-path-editor/) —一个为 RanoreXPath 指定和编辑被跟踪和识别的 UI 元素的工具
*   rano rex Spy——rano rex 区别于其他测试自动化工具的主要功能。它探索和分析被测试的应用程序，捕捉必要的元素。

Ranorex 提供基于图像的自动化，而 Selenium 则不提供这种功能，它要求您要么使用另一个库，要么手动完成。

## +高质量的客户支持

根据[用户评论](https://www.g2crowd.com/products/ranorex-studio/reviews?page=2)，Ranorex 客服专业、乐于助人、回复迅速。审查显示，48 个用户中有 10 个提到工作人员通过 Ranorex 论坛提供的及时支持和全面回应。

然而，一些用户声称该支持仅提供标准答案，解决复杂问题的唯一方法是允许 Ranorex 支持团队直接连接到用户的机器。这可能是公司数据保护政策所禁止的。

## +有效的团队协作

在许多方面，自动化测试开发需要被视为软件开发；因此，团队成员之间的协作应该相应地协调。当大多数竞争对手依赖源代码控制工具时，Ranorex 为跨职能团队提供了自己的解决方案。

基于一个开放的 XML 格式，统一功能测试(UFT)是另一个考虑到团队协作而设计的 QA 自动化工具。然而，它没有工作流分割，这使得 Ranorex 团队能够一致地将特定的技能集引入到项目中。

Ranorex 测试自动化项目包括以下几层:

1.  开发人员和技术测试人员在 Ranorex 核心自动化框架上编写灵活的自动化元素；
2.  同时，非技术测试人员能够创建无代码测试用例或重用现有的核心模块；
3.  然后，使用全面的基于 XML 的测试报告，项目所有者和管理者可以检查测试结果和项目进度。

这样，团队可以在测试自动化项目中有效地合作。

![](img/2f664ad9f73a8c203d72b78db2208854.png)

*Team collaboration within Ranorex test automation project; Source:* [*Ranorex Brochure*](https://www.ranorex.com/rx-media/files/Ranorex-Brochure-EU.pdf)

## +自动生成的报告

Ranorex Studio 中的每个测试运行都以一个自动报告结束，该报告提供了测试执行的细节，包括用于验证的可视截图。这使得工作流程比 Selenium 简单得多，在 Selenium 中，您必须构建一个单独的报告工具来插入到您的测试解决方案中。TestComplete 也提供测试报告，但是它们只包含当前测试运行的结果。Ranorex 报告更加全面，因为它们提供了当前和以前测试运行的饼图。

# 使用 Ranorex 的缺点

## –付费许可证

Ranorex 是一个许可工具，考虑到许多开源的竞争对手，如 Selenium、Katalon Studio 和 Watir，这可能是一个相当大的缺点。Ranorex 很贵，但从长远来看，它可以节省成本，因为它支持开箱即用的功能。

## –只有几种受支持的语言

虽然 Ranorex 在平台和浏览器方面提供了灵活性，但它只支持两种脚本语言:C#和 VB.NET，不像 Selenium 允许用十种流行的编程语言编码。

## –不支持 macOS

尽管 Ranorex 现在支持与 Selenium WebDriver 集成的 Mac 应用程序的 web 测试，但该框架仍然无法在 macOS 上启动。Ranorex 是建立在。NET，除非使用跨平台 [Mono](https://www.mono-project.com/) 项目，否则不会在 macOS 上运行。然而，Ranorex 不支持 Mono。此外，Ranorex 需要访问 Mac 不允许的操作系统的某些部分。锁定他们的系统，macOS 阻碍了许多在 Windows 系统上工作的 Ranorex 功能。

## –小社区

虽然 Selenium 和 Watir 等免费工具有庞大的社区支持，但 Ranorex 的用户群要小得多。iDataLabs 的研究表明，分别有 [24，725](https://idatalabs.com/tech/software-testing-tools) 和 [1，376 家公司](https://idatalabs.com/tech/products/watir)使用 Selenium 和 Watir，而只有 954 家公司在应用 Ranorex。因此，你可能很难在网上找到问题的解决方案和你正在寻求的建议。用户认为 Ranorex 团队应该致力于组织社区来分享更多的插件、问题解决等。

## –不稳定版本

作为最受欢迎的测试自动化提供商之一，Ranorex 经常发布新版本。这反过来又会花费时间来更新现有的自动化测试套件。虽然新版本并不是什么值得抱怨的东西，但事实证明，新特性仍然不稳定，并且包含一些 bug。这就是为什么在进行更改之前，可能需要等待新版本的第二次更新。

# Ranorex 是你项目的好选择吗？

**组织规模。**尽管 Ranorex 可能不是小型开发团队的首选，因为它有许多开源替代方案，但拥有大量预算的公司可以放心地投资该框架。也就是说，[Ranorex 2018 年客户调查](https://www.ranorex.com/blog/customer-survey-2018/)显示，使用 rano rex 的公司数量最多，员工超过 1000 人。

![](img/21ec7056f9eb7e53e3c18c493e92ffb6.png)

*Companies using Ranorex arranged according to the number of personnel; Source: Ranorex Customer Survey 2018*

**用户群体。鉴于 Ranorex 无脚本自动化测试，非程序员也可以使用这个框架。同时，Ranorex 的完整 IDE 使自动化专家能够增强他们的测试套件和记录。**

![](img/d654ff896f6a2fc4c6f98e932336f5a9.png)

*Ranorex user groups; Source:* [*Ranorex Customer Survey 2018*](https://www.ranorex.com/blog/customer-survey-2018/)

**用例。**许多知名品牌正在使用 Ranorex Studio 实现测试流程的自动化。

作为一家医疗保健技术供应商，[西门子](https://www.ranorex.com/customers/case-studies/case-study-siemens-healthcare/)在其西门子医疗保健计算机断层扫描项目中实施了自动化 UI 测试。Ranorex 的模块化方法使得位于德国和中国的测试团队能够在全球范围内共享测试自动化步骤和代码模块。存储 Ranorex 测试案例和结果。xml 文件简化了实现过程。结果，Ranorex 帮助减少了超过 40%的测试执行时间。

IP 管理软件 Unycom 选择了 Ranorex automation 工具为其提供网络和桌面支持，以及快速测试开发。有了 Ranorex 自动化测试，QA 人员现在能够构建多个测试并在一夜之间完成它们。

[交通、导航和地图产品公司 TomTom](https://www.ranorex.com/customers/case-studies/case-study-tomtom/) 应用 Ranorex 对其便携式导航设备功能进行移动测试自动化。在从 Calabash 工具切换到 Ranorex 之后，TomTom 受益于其友好的学习曲线、多种设备之间的交互以及集成到惠普测试用例管理工具中。该公司声称通过 Ranorex 自动化测试节省了超过 90%的时间。

# 一锤定音

Ranorex 是一个有效的 GUI 测试自动化工具，有助于应对您在转向[敏捷软件开发](https://www.altexsoft.com/whitepapers/agile-project-management-best-practices-and-methodologies/?utm_source=MediumCom&utm_medium=referral)时可能遇到的挑战。该框架适应不同的技能组合，确保高效的团队协作，同时具有可集成性和创新性。

也就是说，Ranorex 也有它的缺点，所以在为您的团队选择测试自动化工具的时候，定义您的优先级是有意义的。无论您的项目是否有预算限制或有许多初级测试人员，无论您主要在 macOS 设备上开发还是寻找强大的图像识别功能，所有这些和许多其他规范都将帮助您决定 Ranorex 是否是您应该选择的框架。

这篇文章是我们“好与坏”系列的一部分。有关最流行技术的优缺点的更多信息，请参阅本系列的其他文章:

[*Selenium 测试自动化工具的好与坏*](https://www.altexsoft.com/blog/engineering/the-good-and-the-bad-of-selenium-test-automation-tool/)

[*好坏之分。NET 框架编程*](https://www.altexsoft.com/blog/engineering/the-good-and-the-bad-of-net-framework-programming/)

[*Android 应用开发的好与坏*](https://www.altexsoft.com/blog/engineering/pros-and-cons-of-android-app-development/)

[*Java 编程的好与坏*](https://www.altexsoft.com/blog/engineering/pros-and-cons-of-java-programming/)

*原载于 AltexSoft Tech 博客**[***Ranorex GUI 测试自动化工具的优劣***](https://www.altexsoft.com/blog/engineering/the-good-and-the-bad-of-ranorex-gui-test-automation-tool/?utm_source=MediumCom&utm_medium=referral)*