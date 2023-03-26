# 为什么 Go 没有泛型

> 原文：<https://medium.com/hackernoon/why-go-doesnt-have-generics-b40ef9e69833>

![](img/1ff5c415fe162faee7c46fdc49833148.png)

这是一个老问题，很多开发人员仍然会问我。首先，我只想说，是的，我知道最终 Go 将拥有[仿制药](https://hackernoon.com/tagged/generics)，并且目前已经有一个关于这个主题的大讨论甚至草案，如果你正在寻找一些轻松的阅读材料，你可以在这里[阅读。](https://go.googlesource.com/proposal/+/master/design/go2draft-generics-overview.md)

首先，让我们参考关于这个主题的[golang.org 常见问题](https://golang.org/doc/faq#generics)。

> 泛型很方便，但是它们的代价是类型系统和运行时的复杂性。我们还没有找到一种设计能提供与复杂性相称的价值，尽管我们在继续思考它。与此同时，Go 内置的映射和切片，加上使用空接口构造容器的能力(通过显式拆箱),意味着在许多情况下可以编写代码来做泛型所能做的事情，即使不太流畅。

所以在这篇文章中，我想谈谈这一部分，我们如何用 Go 编写代码来做同样的事情，或者至少实现泛型会做的事情，即使不那么优雅。

# 那么，什么是泛型呢？

## 问题是

许多数据结构和算法适用于一系列不同的数据类型。例如，可以定义一个排序树来保存 int 类型、map[string]string 或某种 struct 类型的元素。排序算法能够对这些类型中的任何一种进行排序，只要它们可以相互比较。例如，如果你需要一个排序的字符串树，你可以很容易地坐下来写一个。

但是如果你需要许多不同类型的排序树，会发生什么呢？每种树数据类型都附带了一些方法，比如插入、更新和删除。如果你有 N 个树元素，M 个树方法，要实现树，你需要 N×M 个方法！可行，但是非常重复——祝你好运！

## **仿制药拯救世界**

为了解决这个问题，许多[编程](https://hackernoon.com/tagged/programming)语言都有*泛型的概念。*要点是只写一次代码，以抽象的形式，用一般类型占位符代替真实类型。这是一个用 Java 写的例子:

```
*// Java pseudo code*
**public** **class** **SortedTree<**E **implements** Comparable**<**E**>>** **{**
	**void** **insert(**E comparableSortTreeElement**)** **{**
		*//...* 	**}**
**}**
```

`E`是一个类型占位符；它可以出现在类或接口定义中，也可以出现在函数参数列表中。通过用实数类型替换占位符来使用`SortedTree`，如下所示:

```
*// Java pseudo code* SortedTree**<**String**>** sortedTreeOfStrings **=** **new** SortedTree**<**String**>();**
sortedTreeOfStrings**.**insert**(**"abcde"**);**
```

这几行用字符串元素实例化了一个排序树。然后，`insert` 方法只接受字符串，排序算法使用字符串比较方法。如果我们使用`Integer`来代替，就像在`SortedTree<Integer>`中一样，树将会是一个整数的排序树。像魔术一样工作！

总而言之，泛型允许我们用类型参数定义类和函数。类型参数可以被限制到某个子集(例如，参数 T 必须是可比较的，不要担心这种特定于 Java 的东西)，但是除此之外它们是不特定的。结果是一个代码模板，可以根据需要应用于不同的类型。

## 有什么不好的地方吗？

是的，事情并不都是美好的。虽然泛型可能会派上用场，但它们也有一些缺点。

**1。将通用代码模板转化为实际代码需要时间，无论是在编译时还是在运行时。正如 Russ Cox 在 2009 年所说:**

> 一般的困境是这样的:你想要缓慢的程序员、缓慢的编译器和臃肿的二进制文件，还是缓慢的执行时间？

**2。复杂性:**泛型不一定复杂，但是当与继承或嵌套泛型等其他语言特性集成时，它们可能会很复杂，比如:

```
*// C#* List<dictionary<**string**,IEnumerable<HttpRequest>>>
```

甚至递归继承，如:

```
*// Java* **public** **abstract** **class** **Enum<**E **extends** Enum**<**E**>>**
```

(摘自 Angelika Langer 的《Java 泛型常见问题解答》。

# Go 没有泛型。现在怎么办？

众所周知，Go 没有泛型，它的设计初衷是简单，而上面提到的泛型被认为增加了语言的复杂性。同样的道理也适用于继承、多态和其他一些顶级面向对象语言在 Go 诞生时表现出来的特性。

一切都没有失去！

## Go 确实有一些泛型，比如特性

Go 中有一些通用的构造。首先，有三种通用数据类型可供您使用，并且可能已经有了:

*   数组
*   部分
*   地图

所有这些都可以在任意元素类型上实例化。对于`map`类型，键和值都是如此。这使得地图非常通用。例如，`map[string]struct{}`可以用作集合类型，其中每个元素都是唯一的。

第二，一些内置函数对一系列数据类型进行操作，这使得它们几乎就像通用函数一样。例如，`len()`处理字符串、数组、切片和映射。

我知道这可能不完全是你想要的，但是有可能有内置的泛型已经覆盖了你的大部分需求。

## 但是如果它没有，并且你真的需要一些泛型呢？

有时候你会遇到一个问题，如果不是必不可少的话，泛型可能是最合适的。幸运的是，你可以做几件事。

## **1。审查要求**

规范真的需要泛型吗？考虑到虽然其他语言可能支持基于类型系统的设计，但 Go 有点不同。

> 如果说 C++和 Java 是关于类型层次和类型分类的，那么 Go 就是关于组合的。[(抢长枪)](https://commandcenter.blogspot.de/2012/06/less-is-exponentially-more.html)

所以不要试图用你用其他语言做事情的方式去做事情。做事该*走正道*。

想想 Go 的范例，尤其是组合和嵌入，并验证这些是否有助于以更自然的方式处理问题。

## **2。复制&粘贴**

我知道这听起来可能很愚蠢，它违背了你所相信的一切，比如“保持干燥(不要重复自己)”原则，如果应用不当，它可能会发生，但很快就完全抛弃它。

原因如下。

每当你认为你需要创建一个通用对象的时候，做一个快速的石蕊测试:问你自己，“我需要在我的应用程序或者库中实例化这个通用对象多少次？”换句话说，当一个泛型对象可能只有一两个实际的实现时，它值得被构造吗？在这种情况下，创建一个通用对象只是一种过度抽象。

如果你想要一个真实的例子，我们可以看看标准库。包*字符串*和*字节*具有几乎相同的 API，然而在创建这些包时没有使用泛型。

## **3。接口**

接口定义行为，不需要任何实现细节。当类型不太重要时，这是定义“一般”行为的理想方法:

*   找到一组您的通用算法或数据容器可以用来处理数据的基本操作。
*   定义一个包含这些操作的`interface`。
*   要在给定的数据类型上“实例化”您的泛型实体，请实现该类型的接口方法。

`sort`包就是这种技术的一个例子。它包含一个排序接口(名为`sort.Interface`，声明了三个方法:`Len()`、`Less()`和`Swap()`。

```
**type** Interface **interface** {
	**Len**() **int**
	**Less**(i, j **int**) **bool**
	**Swap**(i, j **int**)
}
```

通过为一个数据容器(例如，一个结构片)实现这三个方法，`sort.Sort()`可以应用于任何类型的数据，只要数据有一些“小于”的定义。

这里一个好的方面是,`sort.Sort()`的代码不知道任何关于它排序的数据的事情，实际上，它不需要知道。它仅仅依赖于三个接口方法`Len`、`Less`和`Swap`。

你可以在 [sort package 文档](https://golang.org/pkg/sort/)中找到几个例子，但是这里是一个简单的例子，展示了一个集合处理多种类型，仅仅是通过实现一个具有特定行为的接口。

```
type Integer16 int16 
type Integer32 int32 type Calculator **interface** { 
    DoSomething() 
} func (i Integer16) DoSomething() { /* implementation here */ } 
func (i Integer32) DoSomething() { /* implementation here */ }func **main**() { 
    container := []Calculator{Integer16(1),Integer32(2)}       fmt.Println(container) 
}
```

## **4。类型断言**

假设您想要一个通用容器，它不太关心内容的实际类型。因此该值可以作为“没有属性的类型”存储在容器中。为此我们可以将这个空接口声明为`interface{}`。这个接口没有特定的行为，因此任何行为的对象都满足这个接口。但是请记住

> “空{介}没有做声”，罗布举起长枪

尽管这种方法很有用，但它带来了一些开销，比如额外的代码，而且它不提供编译时检查，所以正如罗布·派克自己所说，你还不如用 Python 来编程([https://www.youtube.com/watch?v=PAAkCSZUG1c&t = 7m 36s](https://www.youtube.com/watch?v=PAAkCSZUG1c&t=7m36s))。

基于`interface{}`构建容器类型相当容易。正如我提到的，当从容器中读取元素时，我们需要一些额外的代码来恢复实际的数据类型。这就是类型断言的由来。下面是一个实现通用容器对象的示例。

是一个通用容器，接受任何东西。

```
**type** Container []**interface**{}
```

将一个元素添加到容器中。

```
**func** (c *****Container) **Put**(elem **interface**{}) {
	*****c = append(*****c, elem)
}
```

从容器中获取一个元素。

```
**func** (c *****Container) **Get**() **interface**{} {
	elem **:=** (*****c)[0]
	*****c = (*****c)[1:]
	**return** elem
}
```

调用代码在检索元素时进行类型断言。

```
**func** **assertExample**() {
	intContainer **:=** **&**Container{}
	intContainer.**Put**(7)
	intContainer.**Put**(42)
	elem, ok **:=** intContainer.**Get**().(**int**) *// assert that the actual type is int* 	**if** !ok {
		fmt.**Println**("Unable to read an int from intContainer")
	}
	fmt.**Printf**("assertExample: %d (%T)\n", elem, elem)
}
```

这看起来很简单，对吗？！是的，但是正如我上面提到的，我们不要忘记这种方法的缺点。这里我们放弃了编译时类型检查，这增加了应用程序在运行时出现类型相关故障的风险。此外，与接口之间的转换也是有代价的。最后，所有的类型断言都发生在我们的容器类型之外，在调用者的层次上。在理想情况下，您会想要一种对调用者隐藏类型转换机制的技术。

在 Go 中仍然有一些其他方法可以解决泛型问题，比如使用反射，但是我决定不谈论它，因为它带来了更多的麻烦，在我看来，解决方案增加了很多复杂性，也没有编译时检查，并且增加了相当大的运行时开销。

所以我把这些留给你们，希望对你们有用。

## 喜欢邮报吗？拍手怎么样？！