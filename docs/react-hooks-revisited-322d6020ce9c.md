# 重温 React 挂钩

> 原文：<https://medium.com/hackernoon/react-hooks-revisited-322d6020ce9c>

*最初发表于* [*米哈伊尔-加贝洛夫. eu*](https://mihail-gaberov.eu/react-hooks-revisited) *。*

![](img/6fd9922dd9b9e1beb89eec08336a8b2e.png)

Photo by [Etienne Girardet](https://unsplash.com/photos/Xh6BpT-1tXo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/hooks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

前段时间，在↪️ [反应过来钩子](https://reactjs.org/docs/hooks-intro.html) ↩️ ️️were 正式发布之前，我已经写了[另一篇关于它们的文章](https://mihail-gaberov.eu/react-hooks-a-new-way-of-working-with-react-state/)。这是一次学习和解释这个新特性的尝试。现在，当它们正式成为上一个 React [版本](https://github.com/facebook/react/blob/master/CHANGELOG.md#1680-february-6-2019)的一部分时，我决定重温它们，并尝试展示它们用法的更好例子。

# 为什么

就在我第一次[演讲](https://mihailgaberov.github.io/react-hooks/#0)之后，我收到了一位同事的一些非常有用的评论。关于演示和示例的评论，以及我如何能做得更好的建议。我把这些评论在我的电子邮件里保存了一段时间。今天我将试着遵循它们，这样我们可以更好地理解这个特性，它现在可以用在任何新的或现有的项目中。

# 秀场

因为我们已经在上次讨论了钩子[背后的理论(现在有很多关于这个主题的很好的解释)，我不打算在这里重复。我的计划是直接从例子开始，但是要尽可能保持清晰和重点突出。🐽](https://mihail-gaberov.eu/react-hooks-a-new-way-of-working-with-react-state/)

## 示例 1:

在这个例子中，我们将仔细看看两个最重要的内置钩子——use state 和 useEffect。

📌**状态钩子—使用状态()**

📚理论——这只是提醒我们这个钩子是用来做什么的，然后我们将在下面看到如何使用它。 [React 文档](https://reactjs.org/docs/hooks-state.html)说的是:“***React 提供的其中一个钩子叫做*** `***useState***` ***。我们有时也会称之为“状态挂钩”。它让我们添加本地状态来反应功能组件*** ”。这意味着每当我们需要操作组件状态时，我们可以使用`***useState***`钩子。

👉实践——你可以在这里看到[的真实例子，然后用它来玩](https://codesandbox.io/s/9y96vvwwq4)。

**📌效果挂钩— useEffect()**

📚理论——这只是提醒我们这个钩子是用来做什么的，然后我们将在下面看到如何使用它。 [React](https://hackernoon.com/tagged/react) 文档说的是:“ ***效果钩子让你在函数组件中执行副作用。获取数据、设置订阅、手动更改 React 组件中的 DOM 都是副作用的例子。这意味着每当我们需要执行一些副作用或使用从这些副作用返回的结果时，我们应该使用`**useEffect**` hook。***

下面是一个例子，我们将看到当我们使用这个带有一个****参数和两个**参数的钩子时有什么不同。我们还将看到如果我们为第一个参数提供一个**返回值会发生什么。****

**👉实践——你可以在这里看到[实例，并使用它](https://codesandbox.io/s/9y96vvwwq4)。**

***如果你想更深入地了解这一点，这里丹·阿布拉莫夫贴出了* [*使用效果*](https://overreacted.io/a-complete-guide-to-useeffect/) *的完整指南。文章真的很长，但是很值得。***

# **示例 2**

**在这个例子中，我们将仔细看看**定制挂钩**以及如何使用它们。**

****📌自定义挂钩(如使用图片)****

**📚理论 **→** 我们从上次[的](https://mihail-gaberov.eu/react-hooks-a-new-way-of-working-with-react-state/)中还记得，自定义挂钩顾名思义。我们可以自己制作挂钩，也可以使用内置挂钩。*或者简单的说——我们自己的函数可以从 React 内置钩子中受益。*在下面的例子中，我们将看到如何创建这样的钩子，并结合前面提到的钩子使用它。例如，我们如何获取第三方 REST API 并对结果做一些事情。💻**

**👉实践——你可以在这里看到[的实例，在这里](https://hooks-revisited.herokuapp.com/)看到它的代码[。](https://github.com/mihailgaberov/react-hooks/blob/hooks-revisited/src/App.js)**

# **结论**

**在这个简短回顾的最后，我们可以通过提及一些在使用 React 钩子时需要记住的关键事情来刷新我们的记忆。**

**钩子不会移除类——如果我们想要或者需要，我们仍然可以使用类。**

****钩子规则——使用钩子时必须遵守:****

**☙ [只调用顶层的钩子](https://reactjs.org/docs/hooks-rules.html#explanation)——这基本上意味着我们不能在循环、条件和嵌套函数中调用钩子(是的，记住，钩子是函数，我们可以调用它们)。我们必须遵循钩子被调用的顺序。**注意，如果使用** [**提供的 lint 规则**](https://www.npmjs.com/package/eslint-plugin-react-hooks) **就不需要担心这个问题。****

**☙ [只从 React 函数](https://reactjs.org/docs/hooks-rules.html#only-call-hooks-from-react-functions)中调用钩子——这意味着我们不应该从常规的 [JavaScript](https://hackernoon.com/tagged/javascript) 函数中调用钩子。相反，我们应该只从 React 函数组件或定制钩子中调用它们。**

***我希望这篇文章能帮助你在使用 React 钩子时感到更加自信。*😏**

**🔥感谢阅读！🔥**