# 测试覆盖率是测试或代码质量的好指标吗？

> 原文：<https://medium.com/hackernoon/is-test-coverage-a-good-metric-for-test-or-code-quality-92fef332c871>

> 让火焰战争开始吧。

![](img/269b886ff81344c95a619a93fb805ad7.png)

I bet a pilot doesn’t look at half of this stuff.

# 首先是定义。

和所有好的观点一样，我会明确我使用的术语和它们的含义。

## 代码覆盖率百分比

其中一个自动化测试运行时执行的代码行，以占整个代码库的百分比表示。例如，65%的代码覆盖率意味着测试执行 65%的代码。

## “良好的度量”

对于这个上下文中的“好”度量标准，它必须与高质量的代码有某种关系。质量被定义为更容易理解、改变和维护。

# 那么高的代码覆盖率告诉我们什么呢？

考虑下面这个异常简单的示例代码。它接受一个值并给出地图的输出。为了简单起见，该地图是硬编码的。

```
public class MyService {

    Map<String, String> myMap = new HashMap<>();

    public MyService() {
        myMap.put("1", "v1");
    }

    public String generateValue(String input) {
        String mapValue = parseValue(input);
        return myMap.get(mapValue);
    }

    private String parseValue(String input) {
        return input.substring(1);
    }
}
```

## 那么这里 100%的代码覆盖率是什么样的呢？

使用 spock 的简单单元测试可能如下所示。这将给我们 100%的测试覆盖率。

```
def "test my service does its thing properly"() {
    given: 'my service'
    MyService myService = new MyService()

    when: 'I run my service with a value ending in 1'
    String output = myService.generateValue('l1')

    then: 'I expect something back'
    output != null
}
```

我们知道些什么？

*   当我们有 100%的测试覆盖率时，我们知道测试已经运行了代码中的所有行。

## 酷，所以是经过充分测试的！

呃，不。让我们来看一下我的代码——如果我传入 null 会发生什么？如果有人篡改类的内部来改变输出怎么办？我们 100%的测试覆盖率能告诉我们什么吗？**地狱号**这是怎么回事？

## 代码覆盖率是一个愚蠢的指标

当你跟踪*你的测试运行了多少代码的时候，它的测量是可靠的*，但是它绝对不会告诉你那些测试的价值。可视化它本身是没有用的，因为它与代码或测试的质量没有可靠的、可预测的关系。阻止代码覆盖率轻微下降的发布的质量关卡会招致疯狂的狗屎——像这样的测试。

```
def "test my setter works"() {
    given: 'an instance of my domain object'
    MyDomainObject myDomainObject = new MyDomainObject()

    when: 'I set some value on my domain object'
    myDomainObject.setSomeValue('this is a value')

    then: 'i expect that value to be set'
    myDomainObject.getSomeValue() == 'this is a value'
}
```

当然，你可以测试它，但是**来吧**。你真的认为你的测试会被这种噪音污染吗？你应该测试你的逻辑，而不是域对象中的每一个 setter 这正是像 [Lombok](https://projectlombok.org/) 这样的工具存在的原因。这就是过分热衷于代码覆盖率的问题——开发人员会找到弥补数字的方法。你创造了一个他们不相信的系统，他们会玩这个系统，这是他们的工作。这就是浪费的定义。

# 我并不是说代码覆盖率是一个不好的度量标准，它只是很愚蠢

作为度量标准合唱的一部分，它可以帮助描绘出代码库中正在发生的事情。如果它是一个平坦的 0%，它可能会给你一些关于团队的编码标准和测试实践的洞察力，但是它不会告诉你**那些测试是否是好的或者整个测试策略是否被破坏了**。这才是您真正关心的——经过多个领域全面测试的高质量代码。安全性、性能、弹性等。

让我们带着它走一走。这里有一个小小的思想实验——你有两个选择。

*   测试运行了代码库中 100%的代码行。那些测试就像上面的实现——它们没有告诉你太多关于代码*应该*做什么。
*   您的代码库中有 50%的代码行是由测试执行的。这些测试彻底地测试了逻辑，并为它所覆盖的代码提供了优秀的活文档。然而，测试中有一些漏洞。有些课程没有涵盖。

我敢打赌，你的眼睛是在后者。这是因为这个思想实验有助于找出代码库中一组好的测试的真正价值。他们应该阻止不好的事情发生，并使代码更容易继续工作。前者不这样做，尽管执行了所有代码。后者有。

# 那么代码覆盖率本身是代码质量的一个好指标吗？

不，不是它自己。上面的例子显示了一个简单但常见的情况，代码覆盖率报告满分，但代码看起来像是由一个八岁的孩子写的。当你需要的是*质量*时，代码覆盖率会给你*数量*。记住，一周中的任何一天，10 个好的测试都会击败 100 个垃圾测试。

更多技术漫谈，请在 twitter 上关注我！