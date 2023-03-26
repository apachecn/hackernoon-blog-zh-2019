# 使用 RxJS 的 DIY Redux:第 2 部分

> 原文：<https://medium.com/hackernoon/diy-redux-with-rxjs-part-2-f9d4c53fa230>

![](img/8404a87ec09016c896904ead515ef2e0.png)

Photo by [Steve Halama](https://unsplash.com/photos/iVGevPcaJzk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/code?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

万一你没有读第一部分，我建议你读一读。第一部分介绍如何实现 Redux 的核心功能。

[](https://hackernoon.com/diy-redux-with-rxjs-rxdx-23163a87ade1) [## 带 RxJS 的 DIY Redux with RxJS

### 一个结合 RxJS 和所有 Redux helper 库的实验，以创建一个“具有相似反应的状态管理…

hackernoon.com](https://hackernoon.com/diy-redux-with-rxjs-rxdx-23163a87ade1) 

在 DIY Redux with RxJS => RxDx Part 2 上，我将揭示一些最流行和广泛使用的 **Redux** 中间件的内部。正如我在文章的第一部分提到的——你可以在上面找到——我的目标是做一个实验，将 RxJS 和所有广泛使用的 Redux 中间件组合成一个库，可以在这里找到。

在进入主题之前，回顾一下 redux 中间件所需的签名总是有好处的。

```
(storeAPI) => (next) => (action) => state
```

*在我开始之前，我需要承认，当我完成 createStore 函数时，我满怀信心地认为 redux 的所有可用中间件* *都将适用于我的版本。实际上，它工作了，但不是对大多数来说，不是对广泛使用的来说:(所以我必须通过重新实现它们或者至少实现类似的来提供这些库/中间件。我们走吧。*

# 降低可观测性的另一种方法

正常情况下， **Redux** 不实现/无意实现 [TC39 Observable](https://github.com/tc39/proposal-observable) ，因此默认不兼容 RxJS。

*这里不得不提一下可观测量的用例；如果您需要对事件流进行操作，那么使用可观察值可能更好。我说的是哪种手术？将两个流合并为一个流，过滤、映射或重定向一个流。更多技术细节请查看* [*此处*](https://rxjs-dev.firebaseapp.com/guide/observable) *。*

当实现一个 **Redux** store 时，你肯定会注意到你必须创建许多动作。我不得不承认，动作和事件非常相似，在运行时，它们会被触发数百次。所以这一定是 **redux-observable** 捕捉 **Redux** 作为流事件源的点。通过这样做，它为那些愿意在每个选择的动作上创建一些异步副作用的开发人员创造了一个好机会。关于 **redux-observable** 的详细文档，你可以在这里查看[。TL；神奇的事情发生在一个名为 **createEppicsMiddleware** 的函数中，它的签名如下:](https://redux-observable.js.org/)

```
const createEpicsMiddleware = (rootEpic) => (storeAPI) => (next) => (action) => state
```

老实说，当我查看文档时，我不喜欢我们使用这个库的方式。对我来说，NgRx 的 [**Effect**](https://ngrx.io/guide/effects) 的用法更经典:)所以我决定不模仿 redux-observable 的 API，但功能应该在那里。所以我最后做了如下的事情。

正如我之前提到的，我非常喜欢 NgRx 风格的用法，我借用了 **Effect** decorator 和**type**tap table 函数。

效果装饰器就像在原始变量上添加一个类型一样简单。如果您的环境支持实验性装饰器，用法如下:

```
@Effect()
const someCoolAsyncSideEffect = action$
   .pipe(ofType('SomeCoolAction', 'OrEvenCoolerAction'))
   .pipe(map(() => {type: 'SomeOtherCoolAction'}));
```

不幸的是，如果您的环境不支持实验性装饰，这段代码看起来就不会那么漂亮。

# 重新选

*当我在这个故事的* [*第一部分*](https://hackernoon.com/diy-redux-with-rxjs-rxdx-23163a87ade1) *中实现 createStore 函数时，我相信自己的直觉，在暴露的商店对象中实现了 select 函数。这一决定显示出它在实现* ***重选****RxDx****的*** *库时非常有用。坦率地说，****reselect****与****RxDx****完全兼容，然而我正在试验，我需要挑战自己将所有的库合并到其中* ***。***

**重选**是做什么的？它接受状态或状态部分，并返回它的一部分。那交易是什么？没有它，我们也能实现同样的功能，如下例所示:

```
const reselect = (state, key) => state[key];
```

上面的例子完全破坏了这个简单却非常有用的库的功能。交易是，**重新选择**提供了一个简单的可重复的函数，其内部有**记忆**来加速我们对状态的过滤——可能很复杂。

当我们使用 react 组件中的 **RxDx** 商店时，该函数将具有关键功能。选择器的用法简单如下:

# Redux 开发工具中间件

我认为缺少对 redux 开发工具的集成将是 **RxDx** 的一大缺失。然后开始看 dev tools 的 API 文档，找到了我需要的 __REDUX_DEVTOOLS_EXTENSION__。连接

从这一点来看，将所有东西组合成一个中间件是非常容易的:

**最后，我可以创建一个商店，添加一些 reducers，应用内置的可选中间件，并选择一个特定的带记忆的子状态:**

# 最后的话，下次再说

我强烈建议那些想了解更多关于任何库或任何类型的设计模式或编码风格的人做这样的实验。这将导致你质疑你在日常编码工作中所做的许多事情。

下一次我将继续我的实验，如何将一个类似 redux 的库与 react 组件结合起来，它将包含许多关于 **HOCs** 和**decorator**的信息。

如果你也像我一样是一个自学者，并且对它有点不耐烦，那么在我发布 RxDx 文章的第三部分和最后一部分之前，让我们自己尝试一下。

这是我的回购开始你自己的实验:

[](https://github.com/onerzafer/rxdx) [## onerzafer/rxdx

### 基于 rxjs 的 react 类 redux 库。在 GitHub 上创建帐户，为 onerzafer/rxdx 开发做出贡献。

github.com](https://github.com/onerzafer/rxdx) 

如果你对 RxDx 感兴趣，并且喜欢到目前为止的文章，请不要忘记点击拍手(你最多可以拍手 50 次)。如果你在下面分享你的想法和反馈，我会很高兴的！

感谢阅读！

# 第三部分:

[](https://link.medium.com/o15qrM8VuT) [## 使用 RxJS 的 DIY Redux:第 3 部分

### 在一个真实的例子中解释了高阶组件和 Javascript 装饰器。

link.medium.com](https://link.medium.com/o15qrM8VuT)