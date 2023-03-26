# Clojure 不可变和持久数据结构

> 原文：<https://medium.com/hackernoon/clojure-immutable-and-persistent-data-structures-3978118f6805>

![](img/baf82c31713a22ff1124d6e11aa888eb.png)

## Clojure 是什么？

Clojure 是老式编程语言 LISP 的现代方言。Clojure 坚信代码即数据，数据即代码哲学，就像 LISP 一样。Clojure 是一种运行在 Java 虚拟机上的动态通用编程语言。Clojure 强调

## 安装 Clojure

Clojure 需要 JVM，因为它运行在 JVM 之上。确保您的系统上安装了最小 Java 1.7

```
mkdir -p ~/bin && cd ~/bin curl -O https://raw.githubusercontent.com/technomancy/leiningen/stable/bin/lein chmod a+x leinexport PATH="$PATH":~/bin
```

或者

```
curl -O https://download.clojure.org/install/linux-install-1.10.0.403.sh
chmod +x linux-install-1.10.0.403.sh
sudo ./linux-install-1.10.0.403.sh
```

## 数据结构

4 clo jure 的基本数据结构。

*   列表()
*   向量[]
*   设置# { }幅图像
*   地图{}

通过不可变的方式，当从集合中添加、更新、移除元素时，集合的值在任何时间点都不会改变，而是输出将是结构的新版本。

在法律上，这是基本原则。Clojure 中的列表是链接列表，列表中的第一个元素总是作为函数调用进行计算。Clojure 中两个大括号之间的内容是 list。当我们想从列表的顶部获取项目时，列表很有用。

V ectors []是 Clojure 中创建序列的默认方式。载体是异质的。我们可以从它们中检索项目，还可以将它们添加到末尾和开头。当我们想在列表中添加新元素时，最新的元素总是被添加到向量的末尾。

Clojure 向量中的检索非常快。

M aps {}非常有用，在 Clojure 中广泛使用，以{:key value}格式存储结构化数据。为了从映射中检索值，我们可以使用`get`并且我们也可以使用`keys`作为从映射中检索值的函数，这是从映射中检索的更惯用的方法。

一些有用的函数可用于地图操作

*   获取-从地图中检索值
*   关联-更新地图
*   disoc—从映射中删除键/值
*   键-获取地图中的所有键
*   vals 获取地图中的所有键
*   合并-将多个地图合并为一个地图

集合#{}是唯一值的集合。不允许重复。