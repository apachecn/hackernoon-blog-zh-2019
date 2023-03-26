# 科特林-Spring Boot:明白了

> 原文：<https://medium.com/hackernoon/kotlin-spring-boot-gotchas-e267be7ec022>

![](img/d180e60cfd381659aa8d21eb22278f88.png)

自从我们在我目前工作的公司采用 Kotlin 作为我们的后端服务以来，已经有大约 6 个月了。我们一直在将现有的 java spring boot 服务迁移到 Kotlin。

我们在科特林的经历大多很棒。但有时确实感觉 Spring 是为了支持 Kotlin 而改造的。有一些基本问题，比如:

1.  Spring 要求打开带注释的类，默认情况下 Kotlin 中的所有类都是关闭的。这可以通过在 gradle 配置中添加`**kotlin-spring**`插件来解决。
2.  Hibernate 实体需要一个无参数的构造函数，Kotlin 提供了一个编译器插件`**kotlin-jpa**`来为类生成一个额外的零参数构造函数。

需要注意的是，这些问题已经被发现，项目是使用 [start.spring.io](http://start.spring.io/#!language=kotlin) 生成的，这些插件已经嵌入到您的配置中。在进一步阅读部分会有更多的细节。

## **Spring DTO @NotNull 验证**

让我们用一个简单的 DTO 来表示一个用于创建一个人的 HTTP 请求体，其中我们期望`name`和`age`为 ***而非空*** 。我们使用`*javax*` *的* `@NotNull`注释来验证请求字段，我们已经将其定义为不可空。

对于以下请求:

```
{
    "age": 20,
    "description": "person with no name"
}
```

我们预计在`name`字段上会有一个验证错误，但是它通过了 DTO 上指定的验证器。

## 为什么？

`javax`验证器需要一个对象的实例才能验证它。由于字段被声明为不可空的，`jackson-module-kotlin`根据字段的类型为其提供默认值。在`name(String)`的情况下，它将其设置为空字符串，或者在`age(Int)`的情况下设置为`0`。

## 可能的变通办法

1.  基于您的用例，考虑该值成为验证中的默认值的可能性。针对可能的违约进行验证。

这确保了名称字段至少有一个字符，并且在空字符串上验证失败。类似地，对于年龄，它可以被指定为至少具有值 1。

2.为`jackson-module-kotlin`启用`DeserializationFeature.FAIL_ON_NULL_FOR_PRIMITIVES`可以在反序列化期间构造对象时抛出错误。这些不同于验证错误，必须进行相应的处理。

在我看来，前一种方法更可取，因为除了它仍然会抛出验证错误之外，它还允许您为验证指定更具表达性的条件，从而使它更具可读性。

## Spring 不支持协程

在 Kotlin 版本 1.3 中，协程变得稳定了。然而，它们仍然不支持开箱即用。

## 为什么？

Spring framework 的调度程序不支持 Kotlin 编译器为**协程**生成的构造。Kotlin 编译器为`**suspended**`函数生成一个`**Continuation**`参数，Spring 没有内置的处理程序。

## 可能的解决方案

多亏了 [Konrad](https://github.com/konrad-kaminski) 的努力，通过使用 [spring-kotlin-coroutine](https://github.com/konrad-kaminski/spring-kotlin-coroutine) 可以在 Spring 中使用协程。该库提供了一些模块来支持 Spring 中不同应用程序/域的协同程序。

或者只是利用 Spring 自带的 [@Async executors](https://spring.io/guides/gs/async-method/) 。

Spring 正在努力为协程提供更多的本地支持。它的状态在这里被跟踪。[https://jira.spring.io/browse/SPR-15413](https://jira.spring.io/browse/SPR-15413)

# 进一步阅读

*   [https://github . com/faster XML/Jackson-module-kot Lin/issues/130](https://github.com/FasterXML/jackson-module-kotlin/issues/130)
*   [https://kotlinlang.org/docs/reference/compiler-plugins.html](https://kotlinlang.org/docs/reference/compiler-plugins.html)
*   [https://www.youtube.com/watch?v=WO6d6nWg6Zk](https://www.youtube.com/watch?v=WO6d6nWg6Zk)

希望你喜欢读这篇文章，就像我喜欢写这篇文章一样。
*你是否认为这样会对某人有所帮助？不要犹豫分享。如果你喜欢，点击下面的* ***拍拍*** *，这样其他人会在媒体上看到。别忘了在博客后用* ***来表达爱意！***