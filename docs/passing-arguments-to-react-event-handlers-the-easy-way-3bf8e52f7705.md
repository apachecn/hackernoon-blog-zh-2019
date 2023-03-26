# 在 React 中将参数传递给事件处理程序——最简单的方法

> 原文：<https://medium.com/hackernoon/passing-arguments-to-react-event-handlers-the-easy-way-3bf8e52f7705>

在上一期文章中，我们探讨了事件处理程序、状态和变化状态。如果您需要更新这些概念，请随意重读下面链接的文章！

[](https://hackernoon.com/the-road-to-react-mastery-handling-events-9da8bb1c6f1d) [## 反应精通之路——处理事件

### 如何利用 React 组件最强大的功能之一！

hackernoon.com](https://hackernoon.com/the-road-to-react-mastery-handling-events-9da8bb1c6f1d) [](https://hackernoon.com/the-road-to-react-mastery-understanding-state-29ef20572bc9) [## 反应掌握之路——理解状态

### React 中的状态是基于类的组件的一个非常强大的特性，有点类似于带有一些…

hackernoon.com](https://hackernoon.com/the-road-to-react-mastery-understanding-state-29ef20572bc9) ![](img/a3bdb64bfb091c51a440d3aac9e274dc.png)

使用 React 时，您会发现自己不断地为组件构建事件处理程序，因为这是 React 的动态特性。在许多情况下，您需要向该事件处理程序传递一个参数。当我最初学习 React 时，我发现这个任务令人困惑，不太明白什么需要完成，什么不需要完成。我不想让你经历和我一样的挫败感，所以在这里我将向你展示一种简单、有效和可行的方法来传递参数给 React 事件处理程序。

## 背景知识

如果您正在阅读本文，并且不知道什么是事件处理程序或参数，我强烈建议您回溯到我以前的一些文章，并继续学习。无论哪种方式，为了彻底起见，我将给出一些 TL；博士对其主要概念的解释。

事件处理程序——基本上是 react 组件的私有函数，在 React 组件的类声明中声明。

参数—您提供给函数(事件处理程序)的信息，该函数随后对其进行操作。

都赶上了？太好了！让我们开始吧。

## 设置我们的类

```
class RandomFunction extends React.Component {
  render() {
    return <button>Press to hear your message.</button>;
  }
}
```

到目前为止，非常简单。一个基本的 React 组件，有一个按钮。就是这样。没什么复杂的。现在让我们添加一个事件处理程序！

## 添加事件处理程序

```
class RandomFunction extends React.Component {
  render() {
    return <button>Press to hear your message.</button>;
  }

  displayMessage(message) {
    console.log(message)
  }
}
```

还是挺简单的。我们添加了一个事件处理程序`displayMessage`，它接受一个参数`message`，然后将其记录到控制台。现在，我们需要做的就是将按钮绑定到事件处理程序，并向它提供我们自己的小消息。

## 把所有的放在一起

```
let message = "Hey there!";
class RandomFunction extends React.Component {
  render() {
    return <button onClick={this.displayMessage.bind(this,
    message)}>Press to hear your message.</button>;
  }

  displayMessage(message) {
    console.log(message)
  }
}
```

这看起来可能要复杂得多，但事实并非如此。让我给你分析一下。

*   首先，我们在`message`创造了我们的信息(我知道，非常有创意！)并将其存储在全局范围内。显然，这不太实际，然而，对于本教程来说，这就足够了。只要变量可以被组件引用，它就可以工作。
*   然后我们在按钮上添加了`onClick`功能，只要点击它就会激活(非常简单明了)。
*   然后，为了引用我们的函数并将我们的消息作为参数传入，我们使用了`this.eventhandler.bind(this, parameters)`，我们的事件处理程序名称，在本例中是`displayMessage`。这将`this`(组件)的实例绑定到函数，因为它不是由 React 自动完成的，因此调用`this.props`或`this.state`将导致 null。

你有它！向事件处理程序传递参数的简单易行的方法！如果你有任何问题，请随时提问！如果你喜欢这篇文章，请关注我的每日更新！

感谢阅读！