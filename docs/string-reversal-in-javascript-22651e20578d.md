# Javascript 中的字符串反转

> 原文：<https://medium.com/hackernoon/string-reversal-in-javascript-22651e20578d>

![](img/7e2811a43e1f2a29be10f7a0bf35f8d9.png)

Photo by [Fancycrave](https://unsplash.com/photos/Eh1Qt6L_d68?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/blue-string?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

字符串反转是一个非常常见的面试问题，幸运的是它非常简单。有许多不同的方法可以做到这一点，但这里有四种方法，从最简单的到最复杂的。

# 使用数组

这是一种非常简单的方法，它利用现有的语言特性来完成任务。如果你尝试这样做，大多数面试官会让你用一种更“人工”的方式来完成同样的任务，但这展示了你对 Javascript 的了解，也是我在现实生活中会用到的。

Javascript 允许您对数组调用`reverse`方法。你可以在文档[中看到一些例子](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)。

因此，我们将创建一个执行以下操作的函数:

*   将字符串转换为数组
*   反转数组
*   将数组转换回字符串并返回它

```
reverse = str => { return str .split("") .reverse() .join(""); };
```

例如，调用`reverse('Ruairidh')`将返回`hdiriauR`。

# 使用循环

这更像是一个经典的面试答案，并且以一种更为人工的方式颠倒了字符串。你需要做的就是遍历字符串中的每个字符，并颠倒它的顺序。

看一下下面的代码:

```
reverse = str => { let reversed = ""; for (let character of str) { reversed = character + reversed; } return reversed; };
```

我们在这里所做的就是创建一个空的`reversed`字符串，我们将把我们的字符放入其中。

然后我们使用 ES6 `for...of`循环遍历收到的字符串([参见文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of))，并将每个字符推送到`reversed`字符串。

所以如果我们有了`hello`,那么我们将循环遍历并将`h`添加到一个空字符串中。然后我们的下一个字符是`e`，它将被添加到我们的字符串中，现在已经变成了`eh`。按顺序运行，您会得到:

这非常简单，时间复杂度为 O(n)。

# 使用 Javascript 助手

Reduce 是一个非常有用的 Javascript 数组方法。我会在采访中用这个来表明我了解 ES6 的特性，它有一些非常紧凑的语法的好处。

如果你没有见过`reduce()`的运行，那么它只是对数组的每一项运行一个函数，最后返回一个值。你可以在文档中了解更多[。](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)

至于代码示例，它看起来有点像这样:

```
reverse = str => { return str.split("").reduce((rev, char) => char + rev, ""); };
```

所以这里发生了一些事情。首先，我们使用`split`将字符串转换成数组。然后我们减少数组中的每一项，在回调中我们提供目前为止的累积结果(`rev`)，然后我们将数组中的下一个字符添加到`rev`。我们还通过箭头函数和隐式返回来缩短语法。

逻辑实际上与上面的循环完全相同，但是我们使用 javascript 来完成一些繁重的工作。

# 结论

正如你在上面看到的，一旦你看过一些例子，反转一个字符串实际上是非常简单的。以上任何一个都可以完成你的任务，尽管我推荐最后两个进行面试。

就我个人而言，我更喜欢第二个选项，因为它显示了你比最后一个选项更依赖于 ES6 的功能。

有别的方法来反转一个字符串吗？[发微博给我！](https://twitter.com/RuairidhWM)。或者尝试在 [LeetCode](https://leetcode.com/problems/reverse-string/) 上反转琴弦。

*原载于 2019 年 1 月 2 日*[*ruairidhwm . github . io*](https://ruairidhwm.github.io/2019/01/02/string-reversal.html)*。*