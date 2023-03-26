# 为什么 Angular 应用程序中的类/组件继承可能不好

> 原文：<https://medium.com/hackernoon/why-class-component-inheritance-in-your-angular-app-might-not-be-good-a5d88fdd855b>

在开发您的应用程序时，您不可避免地会遇到这样的情况，您需要创建相似的功能/特性，甚至是跨多个组件甚至模块的功能。这意味着您必须找到一个代码可重用性的解决方案来保持代码的可维护性。在一般的 [Angular](https://hackernoon.com/tagged/angular) 和 [Javascript](https://hackernoon.com/tagged/javascript) 应用程序中，有许多模式和解决方案来设置这一点。由于 Angular 完全基于类，继承可能是一个解决方案。这就是为什么这可能不是最好的解决方案。

![](img/82d32bd567d800650ddb7772b2c22485.png)

Photo by [Kalen Emsley](https://unsplash.com/photos/Bkci_8qcdvQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/mountains?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# Angular 中的继承问题

对于不知道什么是继承的你来说。继承是用其他代码的功能扩展您的代码的一种方式。基本上和父母带着孩子一样。所有的孩子都从父母那里继承了一些特征，并在他们的生活中运用这些特征。

传承本身还不错。正如我前面提到的，您这样做可能是为了增加代码的可重用性。代码可重用性很好，因为这也增加了可维护性。那为什么这是个问题呢？嗯…

> 继承导致抽象出实现细节

这使得你的代码更难理解。对于作为作者的你来说，一切可能都非常清楚，但几个月后还会清楚吗？如果另一个同事，或者更糟的是，一个新同事拿起代码怎么办。我敢打赌，要看到所有东西是如何工作的会困难得多，因为方法可能会被调用多个层次，你真的必须深入了解所有东西在哪里以及它们是如何相互关联的。

在下面的例子中，我们有一种情况，我们需要显示带有不同的不可映射数据集的多个模型。这意味着每个模态都将是一个独立的组件，因为模板差别太大，不能把所有东西都放在一个组件中。

The parent class

The child class which extends from modal

模态组件使用继承来封装可重用的模态逻辑。你可以考虑过滤、分类、管理选择、保存等等。如果你打开`modal-companies.component.ts`，你不会立即看到所有这些逻辑。由于继承只有一层深度，这是一个简单的例子，它仍然是可管理的。但是当你进入多层次的继承和逻辑变得更加复杂时，事情就变得非常困难了。虽然事情可能变得非常灵活和可重用，但你必须浏览多个文件才能了解实际发生了什么。

如果你回头参考我的例子，关于从父母那里继承特征的孩子，这是完全一样的。如果你认识这个孩子，但不知道他的父母，你可能无法找到他的一些特征。你必须深入研究，看看或见见家长，才能理解为什么这个孩子会这样。

对于代码，你不希望这样。在理想的世界里，你想要快速地看到/扫描它，理解它，修复你的东西，然后继续前进。这就是为什么我更喜欢声明式的，所见即所得的解决方案，而不是类继承。

# 所见即所得

你所看到的就是你所能得到的，这可以通过将你的代码放在一个服务或实用程序文件中来实现。然后，您可以从为您工作的组件中调用服务。好的一面是，当你打开你的组件时，你会立即看到注入了哪些服务，调用了这些服务的哪些函数，以及这些函数返回了什么。

The helper service

The modal implementation

如你所见，我们已经移除了继承，并将`modal.ts`类中的代码移到了`modal-helper.service.ts`服务中。现在，我真正喜欢这种方法的是，当你打开一个组件文件时，你可以立即看到该组件有什么功能，并且你可以找到它。

# 缺点是

就像类继承有一个缺点一样，上面的解决方案也有一个缺点，那就是你必须写更多的代码。然而，更多的代码不一定是坏的，特别是在这种情况下，因为上述解决方案有助于使您的代码更容易理解。在我看来，可理解的代码是好代码。

# 结论

虽然这是一个有点固执己见的帖子。要点应该是，代码应该以具有最高形式的可重用性、可维护性和可理解性的方式编写。你几乎总是要在那里找到一个中间立场。在这方面，我描述了一个不同的解决方案，可能适合您的需求。无论如何，永远记住:

![](img/45be67f4ba741736ddb83aa111a1f8d0.png)

也查看我的其他文章:

[](/@luukgruijs/understanding-creating-and-subscribing-to-observables-in-angular-426dbf0b04a3) [## 在 Angular 中了解、创建和订阅可观测量

### 当 Angular 的第二版问世时，它向我们介绍了可观测量。可观察的不是角度的具体特征…

medium.com](/@luukgruijs/understanding-creating-and-subscribing-to-observables-in-angular-426dbf0b04a3) [](/@luukgruijs/understanding-hot-vs-cold-observables-62d04cf92e03) [## 了解热可观测量与冷可观测量

### 当我开始掌握观察的窍门时，我花了很长时间才明白什么是热或冷…

medium.com](/@luukgruijs/understanding-hot-vs-cold-observables-62d04cf92e03) [](/@luukgruijs/understanding-rxjs-map-mergemap-switchmap-and-concatmap-833fc1fb09ff) [## 了解 RxJS 映射、mergeMap、switchMap 和 concatMap

### 编写程序时，映射数据是一种常见的操作。当您在代码中使用 RxJS 来生成数据时…

medium.com](/@luukgruijs/understanding-rxjs-map-mergemap-switchmap-and-concatmap-833fc1fb09ff) [](/@luukgruijs/understanding-rxjs-subjects-339428a1815b) [## 了解 rxjs 主题

### 使用主题的主要原因是多播。

medium.com](/@luukgruijs/understanding-rxjs-subjects-339428a1815b)