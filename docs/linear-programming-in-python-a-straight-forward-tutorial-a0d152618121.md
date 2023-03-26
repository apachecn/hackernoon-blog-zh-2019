# Python 中的线性编程:简明教程

> 原文：<https://medium.com/hackernoon/linear-programming-in-python-a-straight-forward-tutorial-a0d152618121>

![](img/729846c5b8365eb6e2d91f760ececc67.png)

线性规划是最常见的优化技术之一。它的应用范围很广，经常被用于运筹学、工业设计、规划等等。唉，它不像机器学习那样被大肆宣传(这当然是优化本身的一种形式)，但它是可以通过具有线性关系的决策变量来制定的问题的首选方法。这是一个快速实用的教程，我可能会在后面的帖子中介绍单纯形算法和理论。

# **问题陈述**

为了得到一个概念，我们将解决一个关于生产调度的简单问题。假设你在一家制造电脑的公司工作。电脑是一种相当复杂的产品，有几家工厂组装电脑，公司按每台电脑支付一定的费用。市场上这种电脑型号的成本固定在 500 美元，不同的工厂组装电脑的速度和成本不同。工厂 **f0** 以每单位 450 美元每天生产 2000 辆，工厂 **f1** 以每单位 420 美元每天生产 1500 辆， **f2** 以每单位 400 美元每天生产 1000 辆。我们有一个月的时间来装配 80 000 件产品，但条件是任何工厂的产量都不能超过其他工厂的两倍。问题是，**工厂**之间的最优产量分配是什么，使得我们**在这些约束条件下最大化** **销售计算机所获得的利润**？

在本教程中，我们将使用 Python 和线性编程优化包 [PuLP](https://pythonhosted.org/PuLP/) ，使用 pip 复制粘贴安装:

```
pip install pulp
```

现在，为了用线性规划解决计算机生产问题，我们需要以下东西:

1.  决策变量集
2.  这些变量的线性约束集
3.  最大化或最小化的线性目标函数

所以，打开你最喜欢的编辑器，让我们开始吧。在定义 PuLP 中的变量之前，我们需要用下面的代码创建一个问题对象:

```
from pulp import *
problem = LpProblem("problemName", LpMaximize)
```

我们将为这个对象添加约束和目标函数。请注意，问题构造函数收到一个问题名称和 **LpMaximize** ，这意味着我们希望最大化我们的目标函数。在我们的例子中，这是销售一定数量的计算机的利润。此外，我们还定义了从问题陈述中收到的常数:

```
# factory cost per day
cf0 = 450
cf1 = 420
cf2 = 400# factory throughput per day
f0 = 2000
f1 = 1500
f2 = 1000# production goal
goal = 80000# time limit
max_num_days = 30num_factories = 3
```

在下一节中，我们将定义决策变量。

# 决策变量

在 PuLP 中，决策变量的定义如下:

```
variable = LpVariable("variableName")
```

有时我们需要为变量提供界限(默认情况下是没有界限)，在这种情况下，我们将编写以下代码:

```
var = LpVariable("boundedVariableName",lowerBound,upperBound)
```

还有一种定义纸浆变量的有用方法是使用 dicts 函数。这在我们需要定义大量相同类型和范围的变量时非常有用， **variableNames** 将是字典的键列表:

```
varDict = LpVariable.dicts("varDict", variableNames, lowBound, upBound)
```

因此，根据前面的定义，计算机生产问题的决策变量是我们为每个工厂生产的天数:

```
# factories
num_factories = 3
factory_days = LpVariable.dicts("factoryDays", list(range(num_factories)), 0, 30, cat="Continuous")
```

# 限制

既然我们已经定义了我们的决策变量，我们可以转移到定义问题的约束。注意，在线性编程设置中，约束必须是线性的。我们关心的约束条件是装配的单位数量应大于或等于目标数量，以及生产约束条件，即任何工厂的产量都不应超过其他工厂的两倍:

```
# goal constraint
c1 = factory_days[0]*f1 + factory_days[1]*f2 + factory_days[2] * f3 >= goal# production constraints
c2 = factory_days[0]*f0 <= 2*factory_days[1]*f1
c3 = factory_days[0]*f0 <= 2*factory_days[2]*f2
c4 = factory_days[1]*f1 <= 2*factory_days[2]*f2
c5 = factory_days[1]*f1 <= 2*factory_days[0]*f0
c6 = factory_days[2]*f2 <= 2*factory_days[1]*f1
c7 = factory_days[2]*f2 <= 2*factory_days[0]*f0# adding the constraints to the problem
problem += c1
problem += c2
problem += c3
problem += c4
problem += c5
problem += c6
problem += c7
```

计算机组装问题的目标函数基本上是最小化组装所有这些计算机的成本。这可以简单地写成负成本最大化:

```
# objective function
problem += -factory_days[0]*cf0*f0 - factory_days[1]*cf1*f1 - factory_days[2]*cf2*f2
```

让我们来看一下问题的定义，这可以简单地通过调用:

```
print(problem)
```

这导致了下面的输出，我认为这是不言自明的，它列出了问题中的目标函数、约束和各种决策变量:

```
computerAssembly:
MAXIMIZE
-900000*factoryDays_0 + -630000*factoryDays_1 + -400000*factoryDays_2 + 0
SUBJECT TO
_C1: 1500 factoryDays_0 + 1000 factoryDays_1 + 1000 factoryDays_2 >= 80000

_C2: 2000 factoryDays_0 - 3000 factoryDays_1 <= 0

_C3: 2000 factoryDays_0 - 2000 factoryDays_2 <= 0

_C4: 1500 factoryDays_1 - 2000 factoryDays_2 <= 0

_C5: - 4000 factoryDays_0 + 1500 factoryDays_1 <= 0

_C6: - 3000 factoryDays_1 + 1000 factoryDays_2 <= 0

_C7: - 4000 factoryDays_0 + 1000 factoryDays_2 <= 0

VARIABLES
factoryDays_0 <= 30 Continuous
factoryDays_1 <= 30 Continuous
factoryDays_2 <= 30 Continuous
```

# 解决

在我们定义了线性规划问题中的所有必要内容之后，我们可以调用 solve 方法，如果问题得到解决，则输出 1，如果问题不可行，则输出-1，这是一个简单的单行程序:

```
# solving
problem.solve()
```

问题的解决方案可以通过访问每个变量的 varValue 属性来获得:

```
for i in range(3):
 print(f"Factory {i}: {factory_days[i].varValue}")
```

这会产生以下输出:

```
Factory 0: 23.076923
Factory 1: 15.384615
Factory 2: 30.0
```

在线性规划中，我们假设变量之间的关系是线性的，并且变量本身是连续的。作为本教程的后续，我将介绍混合整数编程，其中变量可以是整数，这将证明是一件非常有用的事情，因为它可以用来模拟布尔逻辑。