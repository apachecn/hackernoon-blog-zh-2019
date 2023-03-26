# 使用 Scala 多态类型扩展 MapR 数据库查询

> 原文：<https://medium.com/hackernoon/extending-mapr-database-queries-using-scala-polymorphic-types-ee7a1c7ac2a7>

在使用 MapR 数据库时，我们在以前的帖子中探索过与它交互的无限方式，例如[然而，当在 JVM 上运行时，我们使用 Scala，因为这种函数式语言提供了所有的优势。此外，当使用 Apache Spark 时，我们已经看到人们倾向于使用 Scala，并且他们最终对其生态系统的其他部分使用相同的语言。](https://hackernoon.com/interacting-with-mapr-db-58c4f482efa1#、Go 等。</p><p id=)

[OJAI API 的主要构造之一叫做`QueryCondition`。这就是我们如何定义将要对 MapR 数据库表执行的条件。这些条件是由 MapR 数据库翻译和执行的各种过滤器组合而成的。](https://hackernoon.com/interacting-with-mapr-db-58c4f482efa1#、Go 等。</p><p id=)

[OJAI API 函数围绕着`QueryCondition` use 方法，覆盖了不同的数据类型。换句话说，为了构建一个查询，我们需要明确地知道我们正在使用的确切类型，虽然这看起来是一个好主意，但在大多数情况下这并不方便。](https://hackernoon.com/interacting-with-mapr-db-58c4f482efa1#、Go 等。</p><p id=)

[为了解决这些问题，我们创建了一个图书馆。`ojai-generics`项目在 OJAI 之上呈现了一个非常薄的层，通过添加惯用的构造，简化了从 Scala 使用 OJAI 的工作。](https://hackernoon.com/interacting-with-mapr-db-58c4f482efa1#、Go 等。</p><p id=)

# [使用`ojai-generics`](https://hackernoon.com/interacting-with-mapr-db-58c4f482efa1#、Go 等。</p><p id=)

[通过使用多态和惯用的 Scala，我们可以使用`ojai-generics`来减少我们在使用 OJAI 类 Java API 时被迫编写的样板代码。](https://hackernoon.com/interacting-with-mapr-db-58c4f482efa1#、Go 等。</p><p id=)

[让我们从头构建一个例子来更详细地展示使用`ojai-generics`的优势。](https://hackernoon.com/interacting-with-mapr-db-58c4f482efa1#、Go 等。</p><p id=)

[假设我们想要为来自 Spark 数据帧的值创建一个`QueryCondition`，通常是作为`Any`出现。](https://hackernoon.com/interacting-with-mapr-db-58c4f482efa1#、Go 等。</p><p id=)

[这里的问题是，我们需要找出`value,`的真实类型，这样我们就可以将`QueryCondition.is`传递给它。如果我们有许多类型和许多操作，这是一个问题。我们刚刚对上述类型所做的，必须对我们的应用程序所需要的操作和类型的所有组合进行。然后，重复和代码复制开始到处出现，那些我们应该不惜一切代价避免的。](https://hackernoon.com/interacting-with-mapr-db-58c4f482efa1#、Go 等。</p><p id=)

[使用`ojai-generics`，我们可以这样做:](https://hackernoon.com/interacting-with-mapr-db-58c4f482efa1#、Go 等。</p><p id=)

[我们通过删除类型的模式匹配减少了前面显示的代码。这意味着我们正在使用一个能够接受所有可能类型的多态 API。此外，`ojai-generics`为我们处理选角和转换事宜。此外，它还添加了一些运算符(`===`、`=!=`、`<`、`<=`、`>`、`>=`)，因此我们可以自然地思考这些比较。](https://hackernoon.com/interacting-with-mapr-db-58c4f482efa1#、Go 等。</p><p id=)

[如果我们喜欢更像 Java 的 API，我们仍然可以在不失去类型安全性或泛型的情况下完成以下工作:](https://hackernoon.com/interacting-with-mapr-db-58c4f482efa1#、Go 等。</p><p id=)

[这些函数是别名，产生与以前相同的结果，但是使用了更详细的方法。](https://hackernoon.com/interacting-with-mapr-db-58c4f482efa1#、Go 等。</p><p id=)

[我们可以通过运行以下测试来验证我们的查询:](https://hackernoon.com/interacting-with-mapr-db-58c4f482efa1#、Go 等。</p><p id=)

[使用这些测试，我们可以验证`ojai-generics`输出与 OJAI Java 相同的查询。这是意料之中的，因为最终我们只是在 OJAI 之上放置了一些语法和类型转换，同时公开了一个多态接口。](https://hackernoon.com/interacting-with-mapr-db-58c4f482efa1#、Go 等。</p><p id=)

[`ojai-generics`的代码可以在](https://hackernoon.com/interacting-with-mapr-db-58c4f482efa1#、Go 等。</p><p id=)[***this GitHub repo***](https://github.com/anicolaspp/ojai-generics)中找到，或者我们可以通过以下方式直接从 Maven Central 获取二进制文件:

mvn:

**Sbt:**

# 重要通知

需要注意的是，我们只是在现有的 OJAI API 上增加了一个薄层。在使用我们的库时，那里的所有功能都将正常工作。我们只添加扩展功能。我们不会以任何方式修改现有的功能。我们的库`ojai-generics`要求您链接相应的 OJAI 依赖项，因为它们没有打包在一起。除了 OJAI 库之外，还应该使用`ojai-generics`库。