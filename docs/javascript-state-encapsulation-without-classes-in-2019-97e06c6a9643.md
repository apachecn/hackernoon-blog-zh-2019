# 2019 年无类的 Javascript 状态封装(带私有字段！)

> 原文：<https://medium.com/hackernoon/javascript-state-encapsulation-without-classes-in-2019-97e06c6a9643>

![](img/52f6a5f025006a26eaa064e5ea107c17.png)

A state container in the wild.

在 Javascript 中，我倾向于尽可能避免使用类，因为比起面向对象的风格，我更喜欢函数式风格。但是对象做得非常好的一件事是状态封装。

在本文中，我将讨论使用不带`this`的函数构造器模式作为封装状态的首选方法。这是受到 Reddit 上对私有领域 TC39 提案的广泛批评的启发。

我将讨论如何封装状态:

1.  使用 ES2015 `class`关键字，
2.  对于简单的函数和对象文字，
3.  没有使用`this`和拥有私有领域

文末我有 ***加成*** 等着你，用这个图案示范构图。如果你喜欢 [React Hooks](https://reactjs.org/docs/hooks-intro.html) 你可能会喜欢这个！

# 使用 ES2015 类

这可能是 2019 年最受欢迎的选择，我认为以下内容应该很容易理解:

```
class MyObj1 {
  constructor(initVal) {
    this.myVal = initVal
  } set(x) {
    this.myVal = x
  }
}
```

一旦实例化:

```
const x = new MyObj1(0)
```

您可以通过以下任一方式设置`myVal`的值:

```
x.set(2)
```

或者:

```
x.myVal = 2
```

同样，您可以直接访问该字段:

```
console.log(x.myVal) // output: 2
```

这一切看起来很棒，语法看起来对熟悉其他 OOP 语言的人很有吸引力，但是这种方法有很大的缺陷。

## 没有私有字段

> 下一次`your object field` 是你没有预料到的事情，要弄清楚它在哪里、何时以及如何被改变就不那么容易了。

这相对来说比较直接，但是缺少私有字段使得这个实现有潜在的危险。状态封装的全部意义在于，您想要控制什么可以改变状态，什么不可以改变状态(以及它们如何改变状态)。

但在这种情况下，任何人(或程序的任何部分)都可以将`x.myVal`设置为任何值。下一次`myVal`是你没有预料到的事情，就不那么容易弄清楚它在哪里、什么时候以及如何被改变了。

然而，如果你有私有字段，你可以通过对象上的`set`方法控制设置变量的行为。

## 额外的金属开销和 API 表面积

对开发人员来说，理解 ES2015 类语法是额外的精神负担。有正在进行的提议，用[公共和私有字段](https://github.com/tc39/proposal-class-fields)来扩充它(其语法仍然[有争议](https://www.reddit.com/r/javascript/comments/acrwqt/an_article_about_private_fields/)和不确定)。

这些提议使得初学者甚至经验丰富的开发人员(他们不会主动跟踪 TC39 提议)在实践中使用类变得更加困难。

## 经典继承

最重要的是，语法的本质(通过`extends`关键字)鼓励继承而不是组合，并且错误地暗示了传统的经典继承模式。

关于 Javascript 的类和经典继承问题的一些现有技术:

*   每个 Javascript 开发人员都应该知道的 10 个面试问题
*   [通过 FunFunFunction 的复合遗传](https://www.youtube.com/watch?v=wfMtDGfHWpA)
*   参见本文底部链接的道格拉斯·克洛克福特的演讲

# 使用简单的函数和对象

> *即“无类”面向对象编程*

那么如果我们不使用`class`关键字，我们应该使用什么呢？

简单的函数和对象！这些是即使是最初级的开发人员也熟悉的基本构件。

> 我们所做的就是编写一个返回对象文字的函数；不完全是火箭科学。

这是我们声明函数的方式:

```
const MyObj2 = initVal => {
  return {
    myVal: initVal,
    set: function(x) {
      this.myVal = x
    }
  }
}
```

注意，你根本不需要了解`class`或`constructor`。它只是一个返回对象的函数。我们创造了这样一个:

```
const x = MyObj2(0)
```

我们甚至不需要使用`new`关键字，因为我们不会修改任何原型(这有点像 2019 年的代码味道，我觉得)。它只是一个返回对象的函数。

与 ES2015 类方法类似，我们可以使用以下方法设置该值:

```
x.set(2)
```

或者:

```
x.myVal = 2
```

这种方法仍然存在没有私有字段的问题，但至少我们不再处于不同的范式中。我认为这本身就是对 ES2015 课堂方法的一大改进，因为这里真的没有什么新东西可学。

> 这只是基本的 Javascript:函数和对象文字

# 不使用“this”关键字

Javascript 中的关键字`this`让新老程序员都感到困惑。在旧代码中，你可能会看到`this`被分配给`self`或`that`。快速的谷歌搜索会发现大量试图解释`this`关键字在 Javascript 中如何工作的文章。

我不打算讨论为什么避免使用`this`关键字更好，这是留给读者的练习，因为已经有很多关于它的文章了。相反，请允许我解释另一种方法的好处。

如果我们简单地使用闭包，我们可以像这样定义一个对象创建函数:

```
const MyObj3 = initVal => {
  let myVal = initVal
  return {
    get: function() {
      return myVal
    },
    set: function(val) {
      myVal = val
    }
  }
}
```

就像上面的方法一样，我们可以用它来创建这样一个对象:

```
const x = MyObj3(0)
```

`myVal`变量本质上是私有的，这意味着我们不能像上面的其他实现那样使用`x.myVal`来访问它。相反，我们必须调用 getter 函数:

```
x.get()
```

在某些方面，这是这种方法的一个缺点，但是当您考虑到它给你的力量和明确性时，它也可以被认为是一个优点。

## 闭包支持一个强大的契约

由于 Javascript 闭包，变量在概念上“保存”在函数返回的对象中。这意味着我们可以用 setter 方法(即`x.set(2)`)设置值，但是试图直接改变字段(即`x.myVal = 2`)不会有任何作用。

拥有这些显式设置器和获取器的最大好处是，对象现在与外部世界有了一个强有力的契约/接口。状态是完全封装的，进出对象的唯一方式是通过 setters 和 getters。

## 原型呢？

注意，在这次谈话中，我完全跳过了使用原型和原型继承。这是故意的，因为修改原型作为一种模式在最近几年已经越来越少见了。

甚至道格拉斯·克洛克福特(以前是原型继承的支持者)现在也推荐“无类”的面向对象编程(即上面讨论的模式)。

## 性能呢？

使用闭包的一个缺点是性能问题。当创建一个对象时，似乎没有任何差别，使用闭包的方法调用要慢 80%。

不可否认，我没有深入研究这个问题，但是由于不同的结果数量级相同，所以过于关注这个问题可能是一个过早优化的例子。

如果您每秒钟进行数百万次方法调用，而性能对您来说确实很重要，那么您就需要判断这种牺牲对于您的特定用例来说是否值得。

# 简单很重要

就个人而言，我认为更小的 API 表面积对社区来说是一件好事，因为它意味着初学者更少的困惑和更标准化的做事模式。

> 你不必使用一门语言的所有特性，这样做也不会给你加分。有时候，少即是多。

简单的函数和普通的 Javascript 对象就可以了，为什么还要添加类呢？当您使用 Javascript 中的函数时，已经需要理解闭包。有趣的是，Redux 在他们的`createStore` [函数](https://github.com/reduxjs/redux/blob/master/src/createStore.js)中使用了这种模式。

关于这种方法的更多信息，请点击这里查看道格拉斯·克洛克福特的演讲:

See discussion on inheritance and the function pattern at 41:55.

为了更加安全，他增加了`Object.freeze`，这也是我推荐的。整个演讲值得一看，但要特别注意他在 41:55 开始谈论课程的部分。

随着我们在 2019 年向前迈进，让我们致力于批判性地思考我们如何编码以及我们正在使用的语言功能。有时候，少用比多用更有益。

欲了解完整代码，请点击此处查看回购协议:

[](https://github.com/adrianmcli/js-state-encapsulation) [## adrianm CLI/js-状态-封装

### Javascript 中状态封装的例子。通过创建…为 adrianm CLI/js-state-encapsulation 开发做出贡献

github.com](https://github.com/adrianmcli/js-state-encapsulation) 

# 奖金:有构图的反例！

警告！！！这可能会让你想起 [React Hooks](https://reactjs.org/docs/hooks-intro.html) :)