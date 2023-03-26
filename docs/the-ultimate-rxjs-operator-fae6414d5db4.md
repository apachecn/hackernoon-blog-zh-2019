# 终极 RxJS 操作员

> 原文：<https://medium.com/hackernoon/the-ultimate-rxjs-operator-fae6414d5db4>

## 给予 console.log 它真正应得的爱

![](img/adac27c1e535e1993687e68cdcf18746.png)

Photo by [Fauzan Saari](https://unsplash.com/photos/AmhdN68wjPc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/trophy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我有 99 件感到羞耻的事情，但用`console.log`调试不在其中。我的代码充满了它们，我甚至数不过来。我最近发现自己在 RxJS 管道上记录了太多东西，我还看到了一堆代码库在管道的不同步骤记录值。

> 我有 99 件应该感到羞耻的事情，但是用`console.log`调试并不是其中之一。

我相信你也面临过这种情况，你可能为此使用过`map`或`tap`:

现在，让我们拥抱我们的守护进程，接受我们的职业生活依赖于日志的事实。然后，我们将创建一个操作符，使记录 RxJS 变得轻而易举。一个主宰一切的操作员。**让我们构建终极 RxJS 运算符**🏆**。**

我所说的*微风*指的是这样的东西，它会记录“下一次”、“错误”和“完成”:

```
source.pipe(log()).subscribe(...)
```

# 构建日志运算符

我们将逐步构建`log`操作符。这实际上会帮助您理解如何创建自己的操作符。事实上，让我感到自豪，并在您的项目中创建一个`utils/operators.ts`文件。现在用尽可能多的运算符填充那个东西。**你的代码看起来将是 FP-abulous** 。

1.  RxJS 算子是高阶函数。这意味着*一个函数返回另一个函数*。

2.然而，返回函数的签名是不完整的。构建操作符的经验法则是:**RxJS 操作符必须接收并返回一个可观察值。**让我们开始吧:

3.太多垃圾了，我是来记录东西的！从小我们就被告知，我们需要订阅一个可观察值来获得它的值。因此，如果我们想要记录传入的值，我们需要订阅输入 observable 并调用 console.log。

*…不过看看 dat green console.log 吧。*

4.现在，请记住,`output`可观测值将是下一个运营商将订阅的值。因此，如果我们劫持源可观察对象来记录它的值，那么我们必须负责并将值传递给下一个操作者。可以把它想象成**一个只获取值的代理，console.logs 它并把它传递给**。我们可以通过做`observer.next(val)`来达到这个目的。

5.同样的原理也适用于`error`和`complete`，因此，无论何时我们的源出错或完成，我们的输出也会出错。

6.我们需要小心，因为我们正在创建输入源的内部订阅。在同行评审中，每个人都被告知“适当地处理 dat 订阅，你想要内存泄漏还是什么”。这种情况很容易通过遵循[官方运营商创建指南](https://github.com/ReactiveX/rxjs/blob/master/doc/operator-creation.md#guidelines)来解决，该指南建议只返回内部订阅并忘记它。**让我们这样做，看看我们的操作符最终是什么样子:**

# 让它 shine🕺

是时候使用我们全新的`log`操作员了。

上面的代码片段在映射前记录日志，然后在平方数字后记录日志([否，](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Exponentiation_(**))表示`[**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Exponentiation_(**))` [不是错别字](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators#Exponentiation_(**)))，最后在过滤后记录日志。多酷啊。我为你创造了一个闪电战，这样你就可以和操作员玩了。过会儿谢谢我。

我希望你和我一样喜欢记录东西！如果你有兴趣从我这里读到更多的东西，你可能想看看[这篇文章](https://hackernoon.com/angular-pro-tip-how-to-dynamically-create-components-in-body-ba200cc289e6)或者[这篇](https://hackernoon.com/deploying-frontend-applications-the-fun-way-bc3f69e15331)。我不时地写一些关于 web dev 的文章，所以考虑订阅或者用掌声来表达我的爱吧！👏👏

*PS:我在*[*@ caroso 1222*](https://twitter.com/caroso1222)*得到了 DMs 开放，以防你想跟进这个帖子或者问我什么。*