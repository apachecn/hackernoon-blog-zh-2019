# 混合整数编程:一个简单的教程

> 原文：<https://medium.com/hackernoon/mixed-integer-programming-a-straight-forward-tutorial-41cc50fb9c23>

![](img/01ff2f2c572f763f4ebda5977b40455b.png)

Photo by [Antoine Boissonot](https://unsplash.com/@antoineboissonot?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在之前的一篇文章([Python 中的线性编程:一个简单的教程](https://hackernoon.com/linear-programming-in-python-a-straight-forward-tutorial-a0d152618121?source=your_stories_page---------------------------))中，我介绍了线性编程，其中我们通过定义一组线性约束来解决工厂生产问题，并且变量是连续的。但是如果变量不是连续的会怎么样呢？如果要引入决策变量，应该怎么做？这就是混合整数编程的用武之地。

我们将在本教程中进一步使用[纸浆](https://pythonhosted.org/PuLP/)，如果你想要它的安装指南，你可以查看以前的文章来设置和定义基本功能。让我们先来看一下问题陈述，稍微调整一下，看看混合整数编程在哪里有用。

# 问题陈述

假设你在一家制造电脑的公司工作。电脑是一种相当复杂的产品，有几家工厂组装电脑，公司按每台电脑支付一定的费用。市场上这种电脑型号的成本固定在 500 美元，不同的工厂组装电脑的速度和成本不同。工厂 **f0** 以每单位 450 美元每天生产 2000 件，工厂 **f1** 以每单位 420 美元每天生产 1500 件，工厂 **f2** 以每单位 400 美元每天生产 1000 件。我们有一个月的时间来装配 80 000 件产品，但条件是任何工厂的产量都不能超过其他工厂的两倍。只有两个工厂可以同时工作。问题是，**工厂之间的最优产量分配**是什么，使得我们**在那些约束条件下最大化** **销售计算机所获得的利润**？

注意附加约束“**只有两个工厂可以同时工作**”。这不能用经典的线性规划来解决，因为我们需要决定哪两个工厂在给定的一天工作。虽然有多种方法可以解决这个问题，但为了更好地理解它，我选择了一种或许是直观的方法。

因此，我们知道我们有 30 天的时间来组装所有这些单元，我们如何定义我们的约束？嗯，这很简单。我们可以为每天定义 3 个二进制变量(每个工厂 1 个变量)，并设置约束条件，即它们的总和不应超过 2。二进制变量基本上是整数变量，限制在 0 和 1 之间，包括 0 和 1。最后，我们的混合整数程序看起来就像这样简单:

如果你现在想知道为什么它像一个线性程序，你已经理解了这一点，唯一的区别是我们引入了整数变量。我们可以看一下问题的定义来更好地理解它。基本上，我们可以看到最终的目标是我们在上述 for 循环中总结的所有变量的逻辑组合。以及所有整数约束。

```
computerAssembly:
MAXIMIZE
-450*f0eachDay_0 + -450*f0eachDay_1 + -450*f0eachDay_10 + -450*f0eachDay_11 + -450*f0eachDay_12 + -450*f0eachDay_13 + -450*f0eachDay_14 + -450*f0eachDay_15 + -450*f0eachDay_16 + -450*f0eachDay_17 + -450*f0eachDay_18 + -450*f0eachDay_19 + -450*f0eachDay_2 + -450*f0eachDay_20 + -450*f0eachDay_21 + -450*f0eachDay_22 + -450*f0eachDay_23 + -450*f0eachDay_3 + -450*f0eachDay_4 + -450*f0eachDay_5 + -450*f0eachDay_6 + -450*f0eachDay_7 + -450*f0eachDay_8 + -450*f0eachDay_9 + -420*f1eachDay_0 + -420*f1eachDay_1 + -420*f1eachDay_10 + -420*f1eachDay_11 + -420*f1eachDay_12 + -420*f1eachDay_13 + -420*f1eachDay_14 + -420*f1eachDay_15 + -420*f1eachDay_16 + -420*f1eachDay_17 + -420*f1eachDay_18 + -420*f1eachDay_19 + -420*f1eachDay_2 + -420*f1eachDay_20 + -420*f1eachDay_21 + -420*f1eachDay_22 + -420*f1eachDay_23 + -420*f1eachDay_3 + -420*f1eachDay_4 + -420*f1eachDay_5 + -420*f1eachDay_6 + -420*f1eachDay_7 + -420*f1eachDay_8 + -420*f1eachDay_9 + -400*f2eachDay_0 + -400*f2eachDay_1 + -400*f2eachDay_10 + -400*f2eachDay_11 + -400*f2eachDay_12 + -400*f2eachDay_13 + -400*f2eachDay_14 + -400*f2eachDay_15 + -400*f2eachDay_16 + -400*f2eachDay_17 + -400*f2eachDay_18 + -400*f2eachDay_19 + -400*f2eachDay_2 + -400*f2eachDay_20 + -400*f2eachDay_21 + -400*f2eachDay_22 + -400*f2eachDay_23 + -400*f2eachDay_3 + -400*f2eachDay_4 + -400*f2eachDay_5 + -400*f2eachDay_6 + -400*f2eachDay_7 + -400*f2eachDay_8 + -400*f2eachDay_9 + 0
SUBJECT TO
_C1: f0eachDay_0 + f1eachDay_0 + f2eachDay_0 <= 2

_C2: f0eachDay_1 + f1eachDay_1 + f2eachDay_1 <= 2

_C3: f0eachDay_2 + f1eachDay_2 + f2eachDay_2 <= 2

_C4: f0eachDay_3 + f1eachDay_3 + f2eachDay_3 <= 2

_C5: f0eachDay_4 + f1eachDay_4 + f2eachDay_4 <= 2

_C6: f0eachDay_5 + f1eachDay_5 + f2eachDay_5 <= 2

_C7: f0eachDay_6 + f1eachDay_6 + f2eachDay_6 <= 2

_C8: f0eachDay_7 + f1eachDay_7 + f2eachDay_7 <= 2

_C9: f0eachDay_8 + f1eachDay_8 + f2eachDay_8 <= 2

_C10: f0eachDay_9 + f1eachDay_9 + f2eachDay_9 <= 2

_C11: f0eachDay_10 + f1eachDay_10 + f2eachDay_10 <= 2

_C12: f0eachDay_11 + f1eachDay_11 + f2eachDay_11 <= 2

_C13: f0eachDay_12 + f1eachDay_12 + f2eachDay_12 <= 2

_C14: f0eachDay_13 + f1eachDay_13 + f2eachDay_13 <= 2

_C15: f0eachDay_14 + f1eachDay_14 + f2eachDay_14 <= 2

_C16: f0eachDay_15 + f1eachDay_15 + f2eachDay_15 <= 2

_C17: f0eachDay_16 + f1eachDay_16 + f2eachDay_16 <= 2

_C18: f0eachDay_17 + f1eachDay_17 + f2eachDay_17 <= 2

_C19: f0eachDay_18 + f1eachDay_18 + f2eachDay_18 <= 2

_C20: f0eachDay_19 + f1eachDay_19 + f2eachDay_19 <= 2

_C21: f0eachDay_20 + f1eachDay_20 + f2eachDay_20 <= 2

_C22: f0eachDay_21 + f1eachDay_21 + f2eachDay_21 <= 2

_C23: f0eachDay_22 + f1eachDay_22 + f2eachDay_22 <= 2

_C24: f0eachDay_23 + f1eachDay_23 + f2eachDay_23 <= 2

_C25: 2000 f0eachDay_0 + 2000 f0eachDay_1 + 2000 f0eachDay_10
 + 2000 f0eachDay_11 + 2000 f0eachDay_12 + 2000 f0eachDay_13
 + 2000 f0eachDay_14 + 2000 f0eachDay_15 + 2000 f0eachDay_16
 + 2000 f0eachDay_17 + 2000 f0eachDay_18 + 2000 f0eachDay_19
 + 2000 f0eachDay_2 + 2000 f0eachDay_20 + 2000 f0eachDay_21
 + 2000 f0eachDay_22 + 2000 f0eachDay_23 + 2000 f0eachDay_3 + 2000 f0eachDay_4
 + 2000 f0eachDay_5 + 2000 f0eachDay_6 + 2000 f0eachDay_7 + 2000 f0eachDay_8
 + 2000 f0eachDay_9 + 1500 f1eachDay_0 + 1500 f1eachDay_1 + 1500 f1eachDay_10
 + 1500 f1eachDay_11 + 1500 f1eachDay_12 + 1500 f1eachDay_13
 + 1500 f1eachDay_14 + 1500 f1eachDay_15 + 1500 f1eachDay_16
 + 1500 f1eachDay_17 + 1500 f1eachDay_18 + 1500 f1eachDay_19
 + 1500 f1eachDay_2 + 1500 f1eachDay_20 + 1500 f1eachDay_21
 + 1500 f1eachDay_22 + 1500 f1eachDay_23 + 1500 f1eachDay_3 + 1500 f1eachDay_4
 + 1500 f1eachDay_5 + 1500 f1eachDay_6 + 1500 f1eachDay_7 + 1500 f1eachDay_8
 + 1500 f1eachDay_9 + 1000 f2eachDay_0 + 1000 f2eachDay_1 + 1000 f2eachDay_10
 + 1000 f2eachDay_11 + 1000 f2eachDay_12 + 1000 f2eachDay_13
 + 1000 f2eachDay_14 + 1000 f2eachDay_15 + 1000 f2eachDay_16
 + 1000 f2eachDay_17 + 1000 f2eachDay_18 + 1000 f2eachDay_19
 + 1000 f2eachDay_2 + 1000 f2eachDay_20 + 1000 f2eachDay_21
 + 1000 f2eachDay_22 + 1000 f2eachDay_23 + 1000 f2eachDay_3 + 1000 f2eachDay_4
 + 1000 f2eachDay_5 + 1000 f2eachDay_6 + 1000 f2eachDay_7 + 1000 f2eachDay_8
 + 1000 f2eachDay_9 >= 80000

VARIABLES
0 <= f0eachDay_0 <= 1 Integer
0 <= f0eachDay_1 <= 1 Integer
0 <= f0eachDay_10 <= 1 Integer
0 <= f0eachDay_11 <= 1 Integer
0 <= f0eachDay_12 <= 1 Integer
0 <= f0eachDay_13 <= 1 Integer
0 <= f0eachDay_14 <= 1 Integer
0 <= f0eachDay_15 <= 1 Integer
0 <= f0eachDay_16 <= 1 Integer
0 <= f0eachDay_17 <= 1 Integer
0 <= f0eachDay_18 <= 1 Integer
0 <= f0eachDay_19 <= 1 Integer
0 <= f0eachDay_2 <= 1 Integer
0 <= f0eachDay_20 <= 1 Integer
0 <= f0eachDay_21 <= 1 Integer
0 <= f0eachDay_22 <= 1 Integer
0 <= f0eachDay_23 <= 1 Integer
0 <= f0eachDay_3 <= 1 Integer
0 <= f0eachDay_4 <= 1 Integer
0 <= f0eachDay_5 <= 1 Integer
0 <= f0eachDay_6 <= 1 Integer
0 <= f0eachDay_7 <= 1 Integer
0 <= f0eachDay_8 <= 1 Integer
0 <= f0eachDay_9 <= 1 Integer
0 <= f1eachDay_0 <= 1 Integer
0 <= f1eachDay_1 <= 1 Integer
0 <= f1eachDay_10 <= 1 Integer
0 <= f1eachDay_11 <= 1 Integer
0 <= f1eachDay_12 <= 1 Integer
0 <= f1eachDay_13 <= 1 Integer
0 <= f1eachDay_14 <= 1 Integer
0 <= f1eachDay_15 <= 1 Integer
0 <= f1eachDay_16 <= 1 Integer
0 <= f1eachDay_17 <= 1 Integer
0 <= f1eachDay_18 <= 1 Integer
0 <= f1eachDay_19 <= 1 Integer
0 <= f1eachDay_2 <= 1 Integer
0 <= f1eachDay_20 <= 1 Integer
0 <= f1eachDay_21 <= 1 Integer
0 <= f1eachDay_22 <= 1 Integer
0 <= f1eachDay_23 <= 1 Integer
0 <= f1eachDay_3 <= 1 Integer
0 <= f1eachDay_4 <= 1 Integer
0 <= f1eachDay_5 <= 1 Integer
0 <= f1eachDay_6 <= 1 Integer
0 <= f1eachDay_7 <= 1 Integer
0 <= f1eachDay_8 <= 1 Integer
0 <= f1eachDay_9 <= 1 Integer
0 <= f2eachDay_0 <= 1 Integer
0 <= f2eachDay_1 <= 1 Integer
0 <= f2eachDay_10 <= 1 Integer
0 <= f2eachDay_11 <= 1 Integer
0 <= f2eachDay_12 <= 1 Integer
0 <= f2eachDay_13 <= 1 Integer
0 <= f2eachDay_14 <= 1 Integer
0 <= f2eachDay_15 <= 1 Integer
0 <= f2eachDay_16 <= 1 Integer
0 <= f2eachDay_17 <= 1 Integer
0 <= f2eachDay_18 <= 1 Integer
0 <= f2eachDay_19 <= 1 Integer
0 <= f2eachDay_2 <= 1 Integer
0 <= f2eachDay_20 <= 1 Integer
0 <= f2eachDay_21 <= 1 Integer
0 <= f2eachDay_22 <= 1 Integer
0 <= f2eachDay_23 <= 1 Integer
0 <= f2eachDay_3 <= 1 Integer
0 <= f2eachDay_4 <= 1 Integer
0 <= f2eachDay_5 <= 1 Integer
0 <= f2eachDay_6 <= 1 Integer
0 <= f2eachDay_7 <= 1 Integer
0 <= f2eachDay_8 <= 1 Integer
0 <= f2eachDay_9 <= 1 Integer
```

很明显，混合整数程序会因为决策变量而变得很大。引入整数变量和约束也将非线性引入到优化问题中，这使得问题更加难以解决。事实上，对于一个相当大的混合整数程序，求解时间随着整数变量的数量成指数增长！想象一下，你有 n 个二元变量，这些二元变量有多少种构型？嗯， **2^n** 的配置。这就是为什么混合整数编程仍然是一个活跃的研究领域。这种程序的应用是巨大的，例如在组合优化或任何需要决策的问题中。

我希望这篇文章以简单明了的方式揭开了混合整数编程的神秘面纱，以便对您有用。继续优化！