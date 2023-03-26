# 使用选择器计算 JavaScript 中的派生状态

> 原文：<https://medium.com/hackernoon/calculating-derived-state-in-javascript-using-selectors-7f6ce06ac247>

![](img/17378074a381591cea67e0fd0a5d0f51.png)

Knowledge based on information based on data

状态管理具有挑战性。我们可以通过确保不在状态中存储任何冗余信息来降低难度。我什么意思？假设在我们的程序中，我们需要确定是否允许人们进入我们的酒吧。我们可以通过检查这个人的几个属性来确定这一点:我们可以看他或她的年龄(任何 21 岁或以上的人都可以进入酒吧)，或者我们可以看他或她是否是酒吧的员工(所有酒吧员工都可以进入，无论年龄大小)。现在，我们可以将所有这些信息存储在状态对象中:

```
const state = {
  name: "Joe",
  age: 15,
  employee: false,
  allowedIn: false
};
```

这里的问题是,`allowedIn`可以很容易地从`age`和`employee`道具中导出，这意味着从技术上讲，这些信息是多余的。这是最有问题的，因为它为我们的国家提供了自相矛盾的机会。

***

## 通过注册我的免费时事通讯，在您的收件箱中获得快速 JavaScript 技巧！

***

# 介绍选择器

我们可以使用选择器来解决这个问题。选择器是以`state`为属性并返回派生状态值的函数。让我们看看是否可以创建一个选择器来替换我们的`allowedIn`属性。

```
const state = {
  name: "Joe",
  age: 15,
  employee: false
};const allowedIn = state => state.age >= 21 || state.employee;
```

现在我们看到，如果我们需要确定这个人是否被允许进入我们的酒吧，我们可以简单地使用调用`allowedIn(state)`的布尔结果！

# 使用可组合选择器深入探索

如果我们有一些更复杂的需求呢？也许我们需要根据他们是否被允许进入酒吧*和*做出一个叫做`highFiveThem`的决定，他们是友好的。让我们首先假设我们有一个新的状态对象，其中包括他们是否友好。

```
const state = {
  name: "Judy",
  age: 22,
  employee: false,
  isFriendly: true
};
```

我们的决定不再仅仅基于我们的状态对象，还基于另一个选择器的结果。这是我们开始使用高阶函数从其他选择器合成选择器的地方。让我们看看这在实践中是如何工作的，然后我们可以在引擎盖下窥视一下。

```
const state = {
  name: "Judy",
  age: 22,
  employee: false,
  isFriendly: true
};const allowedIn = state => state.age >= 21 || state.employee;
const isFriendly = state => state.isFriendly;const highFiveThem = createSelector(
    allowedIn,
    isFriendly,
    (allowedIn, isFriendly) => allowedIn && isFriendly;
)highFiveThem(state);
// true
```

这实际上将计算`allowedIn(state)`和`isFriendly(state)`选择器的结果，并将这些输入传递给`createSelector`的最终函数。

理论上，让我们看看这个高阶函数是如何工作的。

```
const createSelector = (...funcs) => {
  const last = funcs.pop();
  return state => {
    const inputs = funcs.map(func => func(state));
    return last(...inputs);
  };
};
```

工作原理:

*   `createSelector`函数接受任意数量的`funcs`。
*   我们创建一个名为`last`的变量来存储传递给`createSelector`的最后一个函数，因为这个函数将使用所有先前函数的结果。
*   我们返回一个函数(我们的新选择器！).
*   每当执行该函数时，我们映射所有的输入函数，根据传递的`state`来确定它们的结果。
*   给定前面所有函数的结果，我们返回我们的`last`函数的值。

很漂亮吧？

# 关于效率的思考

许多选择器库(例如，为 Redux 重新选择)包括附加功能来记忆选择器结果。这是因为如果选择器的输入没有根本改变，那么重新计算选择器是低效的。在这里映射我们的记忆功能有点超出范围，但是请记住，由于这种优化，使用其中一个库可能是有益的(相对于使用您自己的选择器解决方案)。

# 谢谢！

喜欢这篇文章吗？请为它鼓掌👏(还是 50！)来帮助传播消息！另外，欢迎在 [Twitter](https://twitter.com/nas5w) 或 [Github](https://github.com/nas5w) 上关注我，了解更多(通常是 JavaScript 相关的)编程想法。