# 用钩子实现 React 16.8

> 原文：<https://medium.com/hackernoon/react-16-8-with-hook-implementation-212220171685>

![](img/fb801a0bca18f4b1f03e277c7773acdc.png)

React 是一个用于构建完美用户界面的流行 JavaScript 库，于 2013 年首次发布。它由脸书和他们的开发者社区维护。现在，React 16.8 的稳定版本已经发布。它叫做“有钩子的那个”。

那么，React 16.8 中最受关注的 Hook 实现是什么呢？让我们在这篇博客中讨论这个问题，以及新稳定版本中提供的一些其他流行特性。

# 反应 16.8 &挂钩

React Hooks 这个功能自去年在 React 16.7 中首次出现以来，一直是人们最期待的一个。对于任何开发人员来说，挂钩都允许您使用 React 的状态和其他特性，而无需编写类。这并不意味着您需要从头开始重新学习 React 或者重写您现有的 React 应用程序。

钩子不会取代你现有的知识。相反，它提供了一种更舒适的 React 使用方式。它为现有的 React 概念(如 props、state、context、refs)和生命周期事件(如 ComponentDidUpdate、ComponentWillUpdate 等)提供了更直接的 API。

好消息是，React DOM、React DOM server、React Shallow Renderer、React Test Renderer 将支持 Hook 实现。它还有巨大的工具支持。React 钩子受 React DevTools 支持，也受最新版本的流和类型脚本定义支持。

此外，您还可以构建自己的钩子，用于在各种组件之间共享可重用的有状态逻辑。然而，React 引入钩子并不意味着彻底根除了类。另一方面，React 团队正在寻找更多创新的方式来减轻开发人员的负担。

# React 16.8 的其他功能

React 16.8 的特性并没有到此为止。让我们看看新稳定版本中提供的其他一些特性:

*   更好的 useReducer 钩子惰性初始化 API。
*   通过添加变量来同步支持 React.lazy()。
*   在严格模式下用钩子对组件进行双重渲染(仅适用于开发人员)。
*   在后续渲染中返回不同的钩子时，会向开发人员显示警告。
*   使用 Object.is 算法比较 useState 和 useReducer 值。
*   useImperativeMethods 挂钩的新名称。

# 一个很有前途的 JavaScript 来了！

与 Vue.js 等其他技术相比，React 更加成熟，为企业提供了大量机会。它由脸书专业人士支持，因此被认为比 Vue.js 更可靠

如果你是 JavaScript 爱好者，那么 React 是复杂的企业应用程序的选择和强烈推荐。虽然 Vue.js 是高性能的，但 React 是那些在速度、可伸缩性、灵活性和性能方面寻求竞争优势的企业的完美选择。