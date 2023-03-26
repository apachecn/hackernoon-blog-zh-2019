# JavaScript 中异步编程的发展

> 原文：<https://medium.com/hackernoon/the-evolution-of-asynchronous-programming-in-javascript-50c85859473d>

我想谈谈我最近建立的一些东西，但首先我想提供背景，以便它可以被更广泛的观众理解。我将讲述多年的背景知识，并尝试解释一些术语，以便 JavaScript 开发新手也能理解本文。

# 回调地狱

JavaScript 本质上是异步的，但这意味着什么呢？一个**异步**函数只是一个当函数返回时结果还没有准备好的函数。你可以把调用一个异步函数想象成调度一些要完成的工作，这些工作在将来可能最终会产生价值，也可能不会。我们如何获得这个值？一般的机制是传递一个函数，该函数将接收它作为一个参数，即一个**回调**。

链接异步函数，这样通过从一个回调到另一个异步函数调用一个异步函数，一个 feed 的结果进入另一个 feed，可以导致所谓的 [**回调 hell**](http://callbackhell.com/) :

# 承诺

第一个提升是作为库解来的: [**承诺**](http://exploringjs.com/es6/ch_promises.html) 。异步函数不接受回调，而是返回一个 promise 对象。最终，承诺要么**用一个值解析**，要么**用一个错误拒绝**。无论哪种方式，它都完成了。要知道这是什么时候发生的，可以分别用方法`then`和`catch`附加成功和/或失败回调。这些方法返回一个新的承诺，可以附加进一步的回调。如果一个回调函数返回值，它将被传递给链中的下一个`then`回调函数。如果它抛出一个异常，它将被传递给下一个`catch`回调。如果它返回一个承诺，它就被插入到链中的那个点。Promises 通过用链式回调替换嵌套回调提供了缓解，链式回调通常被认为更容易阅读:

(我将示例切换为使用与承诺同时到达语言标准的[箭头函数](http://exploringjs.com/es6/ch_arrow-functions.html)。我想用这些例子来展示艺术发展的现状。)

此外，promises 让我们可以整合多个异步函数的错误处理程序，就像一个 try-catch 语句可以处理多个函数调用一样。

承诺伴随着他们自己的问题。我们仍然在使用回调，它们只是被链接而不是嵌套。在多个回调之间共享一个异步结果的最流行的方法是将它保存到回调之外的变量中。异常用回调来处理，而不是我们熟悉的 try-catch 结构。与同步代码相比，它的可读性仍然较差。

# 发电机

[**生成器函数**](http://exploringjs.com/es6/ch_generators.html) 是 JavaScript 函数，在最终返回之前**可能会产生**多个值。它们是用特殊的`function*`语法在 JavaScript 中定义的:

调用生成器函数不会开始执行该函数。相反，它返回一个代表函数调用的**生成器对象**。您可以通过调用函数的`next`方法来单步执行该函数。第一次调用`next`时，它从头至尾执行函数，返回产生的值。 [](#b524) 第二次调用`next`时，它会在产生的地方重新进入函数，用你传递给`next`的任何参数替换产生的表达式，并继续下一个`yield`。每次调用`next`都重复这个过程，直到函数最终返回或抛出( [REPL](https://repl.it/repls/TatteredPlasticProject) ):

代替`next`，你可以调用`throw`从 yield 表达式中抛出一个异常。生成器函数中的 try-catch 块可以捕获它，但是如果它未被捕获，它会将调用堆栈向上传递到您调用`throw` ( [REPL](https://repl.it/repls/MutedFatherlySuperuser) )的作用域:

在学习发电机时，您可能会遇到几个相关的术语。步进代码(通过调用方法`next`和`throw`)和发生器(通过`yield`、`throw`、`return`)之间来回传递控制的方式称为[协同多任务](https://en.wikipedia.org/wiki/Cooperative_multitasking)。这种“弹跳”就是为什么踏码被称为 [**蹦床**](https://en.wikipedia.org/wiki/Trampoline_(computing)) 的原因。最后，生成器是一种协程。

想象一下，如果我们写一个能产生承诺的生成器。我们可以将其与一个特殊的蹦床配对，该蹦床拦截每个产生的承诺，并添加成功和失败回调，通过分别调用`next`或`throw`将它们的参数传递回生成器。trampoline 本身返回一个承诺，该承诺用生成器的返回值解决(或者用它唯一未捕获的异常拒绝)。这让我们能够以同步的方式编写异步代码:

# 异步函数

这种模式被证明是如此受欢迎，以至于它被包含在带有本机语法的语言中，称为 [**异步函数**](https://developers.google.com/web/fundamentals/primers/async-functions) 。唯一的区别是`async(function* (...) {...})`变成了`async function (...) {...}`并且`yield`变成了`await`:

## 脚注

[](#c9cf)实际上，`then`接受成功和失败两种回调，但两者都是可选的。`catch`只接受失败回调，与调用`then`回调不成功相同。

[](#877a)实际上，`next`返回的是类似`{ value: any, done: bool }`的结构。`done`字段指示该值是来自返回语句(`true`)还是来自产出表达式(`false`)。