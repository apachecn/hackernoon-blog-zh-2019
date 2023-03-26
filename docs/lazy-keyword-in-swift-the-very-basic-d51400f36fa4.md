# Swift 中的懒惰关键字

> 原文：<https://medium.com/hackernoon/lazy-keyword-in-swift-the-very-basic-d51400f36fa4>

![](img/53e13db06274dceea2e080e3f37e9ac2.png)

Pic Credits : [www.infoworld.com](http://www.infoworld.com)

从我在 iOS 开发生涯中第一次遇到 *lazy 关键字和 lazy 初始化开始，我就对这个概念有点兴趣。我从一开始就不明白为什么我们要首先使用 lazy 关键字，因为它所做的只是，某种程度上，推迟你的对象/变量或者你声明的任何 lazy 的初始化，直到它被第一次调用。*

*出自* [*雨燕文档*](https://swift.org/documentation/) *:*

> *惰性存储属性*是直到第一次使用时才计算初始值的属性

好吧好吧。你一定在过去读过*这句话很多次，并且仍然在疑惑“是的，我明白了。但是对于懒惰关键字"*来说，什么是理想的情况呢

让我从一个例子开始:

考虑这样一个场景，我们有一辆车，我们可以开着它到处跑。) .无论是在度假、日常工作、杂货店，还是我无意中听到有人谈论的新俱乐部！还有，让我们假设随着技术的快速提升，每辆车都有内置 GPS(虽然，不是每辆车都有，只是假设)。

Without lazy keyword

我们的模型非常简单，GPS 和 Car 都有两个类。因为我们已经假设每辆车都有内置 GPS，所以类 **Car** 有一个属性 **gps** 。

现在，我们一到达那条线:

```
let myVeryExpensiveCar = Car()
```

我们将在控制台上看到以下输出:

```
**GPS Initialized****Car Initalized**
```

…老实说，这不是火箭科学！

它描述的是，即使我们只创建了汽车对象，GPS 也用它初始化。

比方说，我们每天都要去上班，每周都要去当地的杂货店，经常去祖父母家。在所有这些情况下，我们 ***不需要 GPS*** 因为我们记住了路线，甚至是到达那里的备用路线，但是使用上面的代码，即使我们不需要它，我们仍然会将内存分配给 GPS 变量。作为程序员，我们知道内存的价值

![](img/5f060d92b694a46b3d8cb7bcb76ead64.png)

Pic Credits: memegenerator.net

这就是 ***懒*** 的由来。它所做的是，直到它第一次被调用时才会被分配内存。要使一个属性懒惰，只需将 lazy 关键字放在该属性的 var 声明之前(是的，你没看错，var，**没有让**因为它的初始值可能直到实例初始化完成后才能被检索)

现在，输出将是:

```
**Car Initalized**
```

看，GPS 没有初始化，即使我们的车已经启动并运行。现在，只要我们尝试访问 myVeryExpensiveCar 实例的 ***gps*** 属性:

```
myVeryExpensiveCar.gps
```

并再次在操场上点击 run，我们会看到控制台中添加了以下内容:

```
**GPS Initialized**
```

![](img/3ef490895816b480bcc0bd9d66a2d8aa.png)

Pic Credits: makeameme.org

编译器一看到 ***lazy*** 关键字，就跳过初始化，不给那个变量分配内存。但是，当我们第一次尝试访问同一个变量的时候，它就被分配了内存，之后，它就可以作为普通变量使用了。

现在你可能会想，如果`lazy`很好，为什么我们不把所有的变量都默认为`lazy`？我现在能想到的原因有两个

1.  `lazy`变量总是被声明为`var`，所以**永远不能创建不可变的变量**。
2.  `lazy`T14 的内部需要一个分支，来检查*是否已经有值*，或者第一次*时*是否需要计算一个，这增加了开销，从而超过了`lazy`的好处。让我解释一下这个

一个简单惰性变量声明如下所示:

```
lazy var someValue = 10
```

然而，它的工作原理是这样的:

```
var _someValue: Int? = nilvar someValue: Int {mutating get {// When we are NOT accessing the value for fist time
if let someValueUnwrapped = _someValue { 
      return someValueUnwrapped
}// When we are accessing the value for first time
else { 
    let newValue = 10
    _someValue = newValue
    return newValue
  }
 }}
```

这个`if-else`就是我说的分支，增加了开销。

![](img/b289e201401e287aa2170b052e605caf.png)

最后，百万美元的问题，什么时候真正的编程世界需要`lazy`变量？

> 当属性的初始值需要复杂的计算时，它们很有用，除非真正需要，否则不应该执行复杂的计算。(就像我们的 GPS 一样，你知道 GPS 需要多复杂的计算，我也不知道😅)

你可以在 LinkedIn 上和我联系👱🏻或者可以通过[其他渠道](https://about.me/shubhambakshi)与我联系📬