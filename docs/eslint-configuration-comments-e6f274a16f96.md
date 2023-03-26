# ESLint 配置注释

> 原文：<https://medium.com/hackernoon/eslint-configuration-comments-e6f274a16f96>

![](img/2b788ab10bb2b7a7016bb452c9ba50d2.png)

ESLint 文档详细描述了配置注释和配置文件，但不是分开的。两者的格式有所不同，但是文档将它们结合在一起，使得注释格式很难单独找到。这里我将只关注 ESLint 注释的类型，它们的格式，以及如何在 JavaScript 中使用它们。

# 为什么使用 ESLint 注释？

一般来说，最好使用一个[配置文件](https://eslint.org/docs/user-guide/configuring#specifying-parser-options)，比如. eslintrc。指定文件的配置将应用于该文件夹及其子文件夹*中的所有 JavaScript，除非*被文件夹层次结构中更低的另一个配置文件覆盖。配置文件功能强大，应该成为实施 ESLint 规则的首选。

看，这就是为什么有规则，明白吗？所以在你打破它们之前你要好好想想。” ― **特里·普拉切特**

你可以打破它们，但不是经常。在文件注释中的什么地方打破它们。

## 典型的注释配置是什么样子的

```
const getFinalContext = (context) => {
        **/* eslint-disable-next-line no-unused-vars */**
        const { calls, ...finalContext } = context;
        return finalContext;
    }
```

上面的代码从传入的**上下文**参数中进行析构赋值。该函数仅对 [rest 赋值](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Rest_in_Object_Destructuring)到 **finalContext** 感兴趣，并“丢弃”第一个赋值，**调用**。我的。eslintrc 规则通常配置为将此标记为未使用的变量赋值。为了避免恼人的 ESLint 错误，我可以使用注释**/* ESLint-disable-next-line no-unused-vars */**禁用该规则。不过，这只适用于下一行代码，该规则在文件的其余部分仍然有效。

# ESLint 配置注释的类型

有三种主要类型:

*   [eslint-disable/eslint-enable](https://eslint.org/docs/user-guide/configuring#disabling-rules-with-inline-comments)用于禁用代码块的标志。如果要禁用多个标志，请使用逗号分隔的列表:
    `*/* eslint-disable no-alert, no-console */
    <reams of code here>
    /* eslint-enable no-alert, no-console */*`
*   eslint-disable-next-line/eslint-disable-line
    这个禁用标志只适用于 JavaScript 代码的下一行或当前行。`*/* eslint-disable-next-line no-alert */*
    alert('foo');
    alert('foo'); *// eslint-disable-line no-alert*`
*   这是一个全局设置，通常出现在 JavaScript 文件的顶部。它设置“环境”，该环境应用一系列针对特定环境定制的规则。例如， **eslint-env 浏览器**将关闭**窗口**和**控制台**的无未声明变量警告，并且 **eslint-env 节点**将对**进程执行相同的操作。**可以启用多个环境:
    `*/* eslint-env node, mocha */*`

# 这很容易学，很难记

基本就是这样！没什么复杂的，但是我很少使用这些注释标志，以至于我发现自己忘记了确切的语法。如果你发现自己遭受同样的记忆缺失，考虑将这篇文章加入书签吧！