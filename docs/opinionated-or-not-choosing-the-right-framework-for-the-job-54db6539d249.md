# 固执己见与否:为工作选择正确的框架

> 原文：<https://medium.com/hackernoon/opinionated-or-not-choosing-the-right-framework-for-the-job-54db6539d249>

## 每个项目都有一个基本问题:接受推出自己的解决方案的自由以及随之而来的负担，或者抓住机会使用明智的默认设置，让你快速行动，但隐藏大量的决策，让你走上一条规定的道路。

![](img/a7ff2e386d207bd9f4cd24e9351f8d88.png)

决定是否在你的下一个项目中使用固执己见的框架将会有重大的影响，无论是从产品按时上市的短期来看还是从长期来看。它会影响您的应用程序维护和修改的难易程度，以满足不断变化的需求。答案可能并不总是清晰或简单的，但是我们将帮助您了解何时选择大的固执己见的框架。

在这篇文章中，我们将看看一些固执己见和不固执己见的框架，以及哪些用例对选择哪种方法有意义。我们还会将这种思想应用到更适合固执己见的平台即服务(PaaS)或不那么固执己见的基础设施即服务(IaaS)的用例中。这将帮助您在构建自己的应用程序时做出最佳选择。

# 什么是“固执己见”的框架？

根据定义，框架对架构、最佳实践和约定做出一些假设。这通常对开发人员和架构师很有帮助，因为它为未来的架构决策提供了一条常用的路径，并且它还可以帮助您最终获得一个可预测的、一致的、易于其他开发人员理解的代码库。

但是，即使两个不同的框架是用同一种语言编写的，它们也可能会根据使用的程度而有很大的不同。在一个极端，一个框架被称为“固执己见”这意味着框架设计者已经建立了一条“快乐之路”,让使用他们框架的人更容易、更快地进行开发——只要他们遵循特定的假设。当您的需求与框架的目的相匹配或一致时，选择自以为是的框架可以极大地促进应用程序的开发。强制的或严格的约定也可以使新的开发者更容易加入并立即开始提供价值。

然而，这有时也意味着那些假设是有意难以违背的。在实践中，这可能意味着很多事情，从严格或强制的约定到缺乏可扩展性，或者可能是对单个工具集的限制。最终，固执己见意味着框架按照最初的意图工作得最好，改变它有时会非常痛苦。

决定使用固执己见的框架既是商业决策，也是技术决策。虽然您可能受益于启动的便利性，但是如果您扩展应用程序所需的东西超出了框架的能力，那么使用固执己见的框架的决定可能会在未来付出代价。

为了增加深度，让我们看一些前端和后端框架的具体例子。我们将看看 Angular vs React 和 Django vs Flask，但这种思路也适用于其他框架，如 Vue、Ruby on Rails 等等。

# 角度与反应

为了便于讨论，我们考虑 React，一个非自以为是的框架，Angular 是一个非常自以为是的框架。这两个框架都提供了创建全功能单页面 web 应用程序的能力，并提供了丰富的组件库和庞大的用户群。

为了便于比较，让我们考虑一下每个框架是如何处理数据绑定的。Angular 提供现成的双向数据绑定，而 React 提供单向数据绑定。为了更深入地研究这个问题，Angular 假设了一个 MVVM 或 MVC 类型的架构，并提供了模型(数据所在的位置)和视图(页面上呈现的内容)之间的双向链接。这意味着当用户更改页面上的内容时，您的模型会自动更新。另一方面，React 迫使您自己管理这个更新。

作为一个非常固执己见的框架，Angular 假设您将如何向组件提供数据并使状态可用。它提供了所有必要的链接，因此您所要做的就是声明一个变量并在模型中使用它，而不用担心视图是如何更新的。这被称为数据绑定，是 Angular 开箱即用的东西。

另一方面，React 不提供这种功能，因为您必须显式地处理组件中的更改事件。在模型变更不是由 UI 发起的情况下，您必须通过调用 setState 或使用状态钩子来显式地更新状态，这将自动确保视图被更新。这可能会导致每个组件中有更多的代码，甚至使用 Redux 等其他库来帮助管理跨组件的状态——但它也提供了更高程度的定制。React 也比 Angular 更轻量级，所以如果你不需要 Angular 的所有集成功能，那么 React 可以减少最终用户的数据使用和加载时间，以及开发你的应用程序的开发人员的认知负担。

例如，Angular 在网络连接、语言选择和构建工具链方面做出了类似的假设。当您想要一个完全集成的环境，而不必单独考虑每个组件时，这将节省时间。

归根结底，这是一个偏好问题。为您做出选择可以简化开发，让您走上一条规定的道路，这可以在快速构建应用程序时节省您大量的时间。

在所有条件相同的情况下，假设您同时拥有支持 Angular 和 React 的开发人员，如果您正在构建一个快速的应用程序原型，并且您不想经历构建 React 应用程序所需的令人头痛的设置和初始投资，Angular 是一个舒适而简单的选择。使用它可以帮助您的应用程序快速起步，因为为了得到一个可行的解决方案，需要做出的决策和集成的代码会更少。对于 Angular 来说，构建工具集也是非常标准化的，并且存在大量用于快速启动项目的脚本。

另一方面，如果你知道你正在开发一个非常定制化的 web/mobile 应用程序，并且你想要以一种渐进的方式灵活地做事情，以便创建一个高性能和强大的 web 应用程序，那么 React 无疑是一个更好的选择。

# 姜戈对弗拉斯克

围绕 Django 和 Flask 之间的选择也可以进行同样的讨论，但是使用不同的用例。姜戈非常固执己见；烧瓶不是。

Django 为您的所有应用程序提供了功能全面的体验，包括 ORM、管理面板和基于约定的目录结构。另一方面，Flask 提供了绝对的简单性、高度的可控制性，以及遵循几乎任何您喜欢的架构路径的能力。它也比 Django 更轻，因为它集成了更少的功能。

这两个框架都运行在 WSGI 上，并提供现成的模板。

当您构建一个相当标准的 web 应用程序或服务时，比如新闻网站、组织网站、电子商务网站或者博客，您会希望使用 Django 而不是 Flask。Django 提供了大量的例子、入门应用，以及一条简单而常用的路径供您遵循。

如果您正在创建一个需求非常少的产品，比如一个小型的内部 API，或者如果您需要选择特定的组件，比如自定义身份验证或数据层访问，那么 Flask 是一个更好的选择。它允许您在产品架构的每个阶段挑选组件。如果您正在构建完全定制的东西，并且没有共同的功能和架构路径可以遵循，那么这也是一个更好的选择。Flask 让你在做决定时更加灵活，不会强迫你走上一条预先定义好的道路。

# PaaS 对 IaaS

框架可以固执己见。基础设施的各个部分也是如此。如果你问自己，你的部署平台有多固执己见，PaaS 和 IaaS 之间的区别会变得更加明显。

PaaS 产品为您提供了一个构建应用的全包式环境，其中网络考虑因素、基础架构配置、计算能力和存储都由您管理。在我们的类比中，PaaS 是固执己见的框架，为您做出许多部署和运行时架构假设。Heroku 和 Elastic Beanstalk 就是 PaaS 平台的例子。您从 PaaS 获得的是一个经济高效、易于扩展的托管平台，让您专注于构建和部署您的应用程序。

相反，IaaS 框架是相对独立的，提供了一个灵活的基础设施，您可以轻松地定制、供应和部署。尽管数据中心和服务器基础架构是托管的，但您必须专门考虑和解决规模调整、容量规划、基础架构支持、服务集成和应用程序架构。微软 Azure、AWS 和 GCP 都提供 IaaS。当灵活性是必需的或者当应用程序需要:

*   特定的操作系统
*   专用的非共享计算环境
*   虚拟网络中部署的资源
*   传统平台即服务产品未涵盖的部署环境

简单来说，当你想要快速构建你的产品而不需要重新发明轮子时，使用 PaaS，当你有定制或非标准的需求时，选择 IaaS。

PaaS 很有可能适用于大多数用例，并且将使您不必浪费时间来管理和排除您希望能够正常工作的堆栈层。Heroku 是一个更加成熟的 PaaS 产品，它可以随您的应用程序扩展，并提供多种平台选项，因此您不会被早期的架构或语言决策所束缚。它使用像[十二要素应用](https://12factor.net/)这样的设计模式来帮助你构建一个可扩展和可维护的应用程序，并避免犯无意的错误。如果您遇到了平台的限制，它们可以让您轻松地迁移到一个不那么固执己见的 IaaS，在那里您将有自由(和责任)来管理基础架构中更复杂的部分。

# 结论

如果您有一个明确的目标，并且遵循一条成熟的开发或架构道路，那么固执己见的框架可以使应用程序更容易开发和部署，从而帮助您节省时间和金钱。一个固执己见的框架给了你护栏，大量的起始代码，并优化了你的路径。但是如果你知道事情会很快偏离轨道，那么你应该考虑使用一个非固执己见的框架。

例如，如果您想要构建一个快速的 UI，或者您知道您的 UI 将使用标准组件——并且您几乎不需要以非标准的方式定制或更改这些组件的行为——那么固执己见的框架 Angular 可能是比 React 更好的选择。然而，如果您需要 Python web 框架的灵活性——或者正在构建一个 Django 可能没有示例或开放源代码的定制产品——那么您可能需要考虑使用非固执己见的 Flask。

将固执己见与不固执己见的概念扩展到部署选项，为我们提供了一种看待 PaaS 与 IaaS 的简便方法。如果您想要一个交钥匙环境，您应该考虑固执己见的 PaaS 选项。对于高度定制的需求，请选择 IaaS 的灵活性。