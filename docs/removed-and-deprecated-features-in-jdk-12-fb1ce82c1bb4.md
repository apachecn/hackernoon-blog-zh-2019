# 在 JDK 12 中删除和弃用的功能

> 原文：<https://medium.com/hackernoon/removed-and-deprecated-features-in-jdk-12-fb1ce82c1bb4>

![](img/576624f33164d53dec12f99d315a6bce.png)

有几篇关于 Java 最新版本(JDK 12)的新特性的博文和文章。在这篇文章中，我将介绍 JDK 12 中最重要的被删除和弃用的 API 和特性。

虽然这个版本没有长期支持，但你应该记住，它会影响下一个长期支持版本。

除了已经添加的重要特性(例如[开关表达式](http://openjdk.java.net/jeps/325)或 [JVM 常量 API](http://openjdk.java.net/jeps/334) )，还有几个重要的特性、API 和选项已经被删除或弃用。

**这里是一个概述:**

*   [**删除了 javac 对 6/1.6 源代码、目标和发布值的支持**](http://bugs.java.com/view_bug.do?bug_id=JDK-8028563) **:** 删除了对 javac 的`-source`、`-target`和`--release`标志的 6/1.6 参数值的支持。
*   [**GTK+ 3.20 及更高版本不受 Swing 支持**](http://bugs.java.com/view_bug.do?bug_id=JDK-8218469) **:** 由于 GTK+ 3 库版本 3.20 及更高版本中的不兼容更改，Swing GTK 观感在使用该库时不呈现某些 UI 组件。
*   [**从 FileInputStream 和 FileOutputStream 中删除 finalize 方法**](http://bugs.java.com/view_bug.do?bug_id=JDK-8192939\)**:**`FileInputStream`和`FileOutputStream`的`finalize`方法在 JDK 9 中已被弃用。在此版本中，它们已被删除。关闭文件的推荐方法是显式调用`close`或使用`try-with-resources`。
*   [**user . time zone 系统属性的初始值已更改**](http://bugs.java.com/view_bug.do?bug_id=JDK-8185496) **:** 以前，初始值为空字符串。在 JDK 12 中，`System.getProperty("user.timezone")`可能返回 null。
*   [**增强弃用**](http://openjdk.java.net/jeps/277) **:** 对`@Deprecated`标注进行了改进，并提供了强化 API 生命周期的工具。
*   **java.util.Observer 和 java.util.Observable** **已弃用:** *对于*更丰富的事件模型，可以考虑使用`[java.beans](http://cr.openjdk.java.net/~iris/se/12/latestSpec/api/java.desktop/java/beans/package-summary.html)`包。为了线程间可靠有序的消息传递，考虑使用`[java.util.concurrent](http://cr.openjdk.java.net/~iris/se/12/latestSpec/api/java.base/java/util/concurrent/package-summary.html)`包中的一种并发数据结构。对于反应流风格的编程，参见`[Flow](http://cr.openjdk.java.net/~iris/se/12/latestSpec/api/java.base/java/util/concurrent/Flow.html)` API。
*   **java.applet.Applet 已弃用**:Applet API 已弃用，无替代。
*   **java.lang.Compiler 已被弃用:** JIT 编译器及其技术变化太大，无法通过标准化接口进行有效控制。因此，许多 JIT 编译器实现忽略了这个接口，而是由特定于实现的机制(如命令行选项)来控制。在 Java SE 的未来版本中，该类可能会被删除。

还有一些其他功能、API 和选项已被删除或弃用，您可以通过以下链接了解更多信息:

*   [删除了 JDK 12](https://www.oracle.com/technetwork/java/javase/12-relnote-issues-5211422.html#Removed) 的功能和选项
*   [JDK 12 中已弃用的功能和选项](https://www.oracle.com/technetwork/java/javase/12-relnote-issues-5211422.html#Deprecated)
*   [JDK 12 中被否决的 API 页面(API 规范)](http://cr.openjdk.java.net/~iris/se/12/latestSpec/api/deprecated-list.html)