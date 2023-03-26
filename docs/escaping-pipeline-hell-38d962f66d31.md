# 逃离管道地狱

> 原文：<https://medium.com/hackernoon/escaping-pipeline-hell-38d962f66d31>

![](img/937c9dbc7f494c8831377f1ed806c70d.png)

之前，我从回调地狱开始，通过承诺，逐步介绍了 JavaScript 中异步编程的发展，并在异步函数(构建在生成器的底层技术之上)处停止了[大多数](https://www.wptutor.io/web/js/generators-coroutines-async-javascript) [覆盖](https://blog.hellojs.org/asynchronous-javascript-from-callback-hell-to-async-and-await-9b9ceb63c8e8)的地方。鉴于这段历史，我想谈谈 JavaScript 中带有[可观察对象](https://github.com/tc39/proposal-observable)的异步编程的状态。

# 看得见的

对于我的新手读者来说，你可以把可观察到的事物想象成可以解决不止一次的承诺，有效地产生多重价值。以同样的方式将成功和失败的回调附加到承诺上，您**为**订阅一个带有三种类型事件回调的可观察对象:零个、一个或多个值事件；零个或一个错误事件；和零个或一个完成事件。错误和完成事件是**终端事件**:它们中最多有一个，不会两个都有，一旦事件到达，就不会再看到任何类型的事件。这些回调通常分别称为**下一个**、**错误**和**完成**回调。合在一起，他们就形成了一个**观察者**。

可观测量可以从许多不同种类的源中构造，包括数组、生成器和承诺(它们只是最多产生一个值的可观测量)。在埃里克·梅耶尔对 observables 的精彩介绍中有一个著名的例子，名为[你的鼠标是一个数据库](https://queue.acm.org/detail.cfm?id=2169076)，他使用 observables 实现了一个简洁的自动完成(或“提前键入”)文本框。它已经被复制了[很多次](https://alligator.io/angular/real-time-search-angular-rxjs/)[超过](https://angular.io/guide/practical-observable-usage)，在最新的 JavaScript 中，它可能是这样的:

与承诺相比，可观察对象的另一个优势是**订阅可以取消**，这让可观察对象知道它可以停止工作。这对于取消不再需要响应的 AJAX 请求非常有用，例如在 autocomplete 示例中，当用户在最后的 autocomplete 结果返回之前编辑了文本框。曾经有一个[可取消承诺的提案](https://github.com/tc39/proposal-cancelable-promises)，但是被[撤回](/@benlesh/promise-cancellation-is-dead-long-live-promise-cancellation-c6601f1f5082)。在没有深入调查的情况下，我得到的印象是，它被大声疾呼，以支持现有的和更一般的可观察的抽象。

然而，正如你在上面的例子中所看到的，observables 遭受了与承诺在生成器到来之前相同的“回调链”风格。本着回调地狱的精神，我称这个**管道地狱**。我们缺少异步函数的可观察等价。

# RxJS

据我所知，RxJS 是最流行的 JavaScript observables 实现。巧合的是，RxJS 的第 4 版[有一个名为`spawn`的可观测生成生成器](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/spawn.md)的蹦床。它似乎已经毫不客气地从图书馆中掉出来了。我怀疑没有足够的人使用它。[有些人](https://github.com/ReactiveX/rxjs/issues/1497#issuecomment-199420304)实际上更喜欢可观测量上的操作符链，但是*如果*我的可观测量看起来像承诺(即它们只产生一个值或者抛出一个错误)但是可取消的，那么我更喜欢像异步函数这样的语法，因为它使承诺更容易使用:感觉更像熟悉的同步编程，更容易读写。

好消息是发电机给了我们一个机会。就像异步函数是从产生承诺的生成器和特殊的蹦床的组合中派生出来的一样，我们可以为产生可观察结果的生成器定义自己的蹦床。

退一步想想生成器函数。它接受一些参数并返回一个生成器对象。它有一个名字，当我准备好启动生成器时，我可以调用它，我可以从许多不同的地方调用它，甚至远离它被定义的地方。我想修改我的产生可观察值的生成器函数，使它们返回一个可观察值而不是一个生成器对象。为此，我需要一个函数，它接受生成函数并返回一个新函数，一个接受与生成函数相同的参数但返回一个可观察值的函数。此外，生成器不应该在调用这个新函数时启动，而是应该在订阅观察对象时启动。

我们必须定义自己的可观察对象来实现蹦床。最简单的方法就是把所有有趣的工作都塞进一个 [**订阅者**](http://reactivex.io/rxjs/class/es6/Subscriber.js~Subscriber.html) (图中没有)，然后和订阅者一起构造可观察对象:

订户需要在其构造函数中启动蹦床，并且需要一个能够停止蹦床的`unsubscribe`方法。我不打算在这里一步一步地介绍整个订阅者，所以我只是[链接它](https://gist.github.com/thejohnfreeman/239a7c0ef79005f536138cd50aee23c3)给有兴趣了解它如何工作的高级观众。使用`async`，我们可以在 observables 的框架中编写处理可取消请求的函数:

事实上，从 RxJS 4 实现`spawn`变得微不足道。它只是用`async`包装了一个零参数生成器函数，并立即调用它:

# MobX

现在我想谈谈另一种不同的可观察对象，即那些 MobX。(如果你不熟悉，我将不得不遵从他们的文档。)MobX 观察值*的突变应该*在 [**动作**](https://mobx.js.org/refguide/action.html) 内批量处理。异步函数不太适合这种实践，但是 MobX 提供了一个建立在生成器之上的
替代方案:`[flow](https://mobx.js.org/best/actions.html)`。问题是，flow 只对产生承诺的生成器起作用，而承诺是不可取消的。如果我们使用可取消的可观测量，那么我们需要一个`flow`替代品
来处理它们。

幸运的是，它可以很容易地构建在我们用于`async`的同一个订户的*之上。我们所要做的就是将每个生成器方法(`next`、`throw`和`return`)包装在一个动作中:*

# 警告

然而，异步函数并不是解决可观测量的灵丹妙药。一些更高级的操作符很难或者很难实现为异步函数，尤其是那些携带状态的操作符，例如`[debounceTime](https://rxjs-dev.firebaseapp.com/api/operators/debounceTime)`或者`[distinctUntilChanged](https://rxjs-dev.firebaseapp.com/api/operators/distinctUntilChanged)`。但是，根据我的经验，如果你有一连串的函数，比如`[map](https://rxjs-dev.firebaseapp.com/api/operators/map)`、`[filter](https://rxjs-dev.firebaseapp.com/api/operators/filter)`和`[catchError](https://rxjs-dev.firebaseapp.com/api/operators/catchError)`，那么它最好写成异步函数。