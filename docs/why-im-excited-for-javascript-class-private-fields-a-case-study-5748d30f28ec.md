# 为什么我对 JavaScript 类私有字段感兴趣:一个案例研究

> 原文：<https://medium.com/hackernoon/why-im-excited-for-javascript-class-private-fields-a-case-study-5748d30f28ec>

![](img/bf785228afdd9fb46884a1ceb78a874a.png)

JavaScript Class Privacy — Or Lack Thereof

类在编程中并不新鲜，但是它们在 JavaScript 中的实现却很奇怪。对于任何习惯于其他语言的类的人来说，您可能会认为它们是复制新实例的蓝图。此外，您可能认为这些类会提供公共、私有和受保护的实例属性。在 JavaScript 中，这两种情况都不正确:对象的新实例与它们的父对象有原型继承关系，不存在私有或受保护的属性。在本文中，我将更深入地探讨后一点。

如果你喜欢这篇文章，请为它鼓掌👏(还是 50！)来帮助传播消息！

# 为什么 JavaScript 类中有私有字段？

我开始考虑类实例隐私的问题，因为我正在创建一个基于类的数据验证模块(想想表单或 JSON 数据验证)。其思想是，每当有新的东西需要验证时，您就可以创建一个新的验证器实例(也就是说，如果您的网站上有两个不同的表单，您可以为每个表单创建一个验证器)。完成此任务的公共 API 代码可能如下所示:

看起来很棒，对吧？当然，为什么不呢？因此，当我设计实际的验证器模块来实现这一愿景时，我很快意识到我希望在模块中有一些可用的方法，但不在公共 API 中公开。也就是说，对我来说显而易见的是，调用一个规则添加方法(例如，`mustBeValidEmail`或`minLength`)应该引用一个名为`addRule`的私有方法，该方法会将规则添加到一个名为`validationRules`的私有规则数组中。

> 我很快意识到我想让一些方法在模块中可用，但不在公共 API 中公开。

这并不是说`addRule`方法或者`validationRules`数组有什么超级秘密，而是因为*不需要*在类外调用/查看它们，所以为什么要暴露它们并潜在地导致错误呢？

# 尝试和未能实现财产隐私

这一节的标题完全是剧透警报:我尝试了几种方法，但最终未能(令人满意地)实现财产隐私。在这一节中，我将向您展示我尝试过的方法，以及它们为什么行不通。

**尝试 1:使用 IIFE 创建只有该类可以访问的变量。**

立即调用的函数表达式(IIFE)是创建闭包的好方法，本质上是在闭包内提供一些有状态性。这种工作方式的一个概念性示例如下:

啊，真漂亮。`validationRules`数组和`addRule`函数现在应该是私有的。**但这失败了！**为什么？因为只创建了一个`validationRules`数组，这意味着所有创建的验证器实例都将向同一个数组添加验证规则。当需要验证一个对象时，这将是一片混乱——来自 Validator 类的所有实例的所有验证规则都将被应用！

**尝试 2:在构造函数中创建新变量**

这是我在 StackOverflow 上发现的一个有趣的方法。它实际上是有效的。算是吧。让我们来看看。

这是怎么回事？好吧，每次我们创建一个新的验证器类的实例(例如，使用`new Validator()`，一个新的`validationRules`数组和`addRule`函数被创建。由于构造函数正在创建一个闭包，`this.prototype.mustBeValidEmail`可以访问这些限定了作用域的属性。所以这实际上会起作用！**但是这个失败了！**为什么？因为从面向对象编程的角度来看，在创建每个新实例时创建新的原型属性是低效的，而且是错误的。

**尝试 3:只需公开该死的属性**

所以这个是行得通的，但是绝对没有实现隐私。然而，我确实决定使用这种方法:不像其他方法，它并不复杂。它只需要确保这些方法和属性在生成的任何 API 文档中都不被记录为可用，当然，对那些因为决定乱用这些属性而出错的人要有一定的容忍度(我们都知道开发人员是怎样的)。

再说一遍，它是可读的，不粗糙，而且有效。

# 结论和新的希望

所以，最终我有了一个工作良好的模块，但是牺牲了一些属性和方法隐私。这并不理想，但它在 JavaScript 今天所提供的范围内是可行的。

但是……在我们的 JavaScript 类中仍然有隐私的希望！显然，我不是唯一认为 JavaScript 类可以使用私有属性的人。ECMAScript 技术委员会 39 正在[考虑私有类字段](https://github.com/tc39/proposal-class-fields)作为我的类型！很快，我们将能够在类中指定私有字段，只需在它们前面加上`#`符号。就我而言，我真的很兴奋，我希望你也是。

大家编码快乐！