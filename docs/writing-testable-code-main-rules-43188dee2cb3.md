# 编写可测试的代码。主要规则

> 原文：<https://medium.com/hackernoon/writing-testable-code-main-rules-43188dee2cb3>

![](img/ebaeaa432a8d24976ac615839312b31f.png)

每个开发人员都知道可测试的代码可以让生活变得更简单。有很多关于单元测试的书籍和文章。测试驱动开发(TDD)作为高科技产品开发的最佳过程受到了特别的关注。在我的日常工作中，我面临着大量不可测试代码的问题。这甚至可能发生在那些 100%测试覆盖率是主要验收标准的项目中。

我承认“好的代码”和“单元可测试的代码”并不总是等同的术语。您的代码可以是可理解的、自我记录的，但同时也是不可测试的。编写单元可测试代码有一个通用的技巧。你应该只使用干燥，亲吻和固体的原则，就像我在 Dashdevs 中做的那样。

不幸的是，开发人员可能会因为节省时间或者缺乏能力而忽略它们。无论如何，当你需要为一个根本不可测试的代码编写一个测试时，这种情况会比你希望的更频繁。

作为一名拥有 6 年以上经验的开发人员，我愿意分享我的生活经验和获得的可测试代码知识。在本文中，我将尝试列出最常见的问题和解决方法。进一步说，你可能会发现我的个人经验和 C#、MsTest 和 Moq 的源代码例子的挤压。经验是最好的老师，我们 Dashdevs 喜欢分享如何在实践中学习。让我们为更好的代码而奔跑吧！

# 操作员新建

在我看来，操作符 new 是你在等待单元测试的代码中可能遇到的最可怕的事情。这个操作符的值是可疑的，因为它的使用违反了 SOLID 中的依赖反转原则。

让我们参考一个简单的例子:

```
**public class** *CertificateManager*
{
    **public void** CreateCertificate(CertificateDto certificateDto)
    { ... **var** certificate = **new** Certificate(certificateDto.Name, certificateDto.Url, certificateDto.Content); certificate.Upload();
    }
}
```

这种方法之所以不好，只有一个原因:它无法通过任何方式进行测试。

它的存在只是为了调用 Upload()。然而，我们不能创建模拟对象(mock)证书；因此，我们无法检查它是否真的被“调用”

这是不可能的。有三种方法可以解决这个问题。

1.  **直白。**我们可以将证书实例化委托给工厂方法。

```
**public class** CertificateManager
{
    **private** CertificateCreator _certificateCreator;

    **public** CertificateManager(CertificateCreator certificateCreator)
    {
         _certificateCreator = certificateCreator;
    }

    **public void** CreateCertificate(CertificateDto certificateDto)
    { ...

        **var** certificate = _certificateCreator.Create(certificateDto.Name, certificateDto.Url, certificateDto.Content);

        сertificate.Upload();
    }
}
```

现在我们可以创建 _certificateCreator 和证书本身的模拟。

***优点:*** *它不改变代码结构。*

***缺点:*** *这种方法需要创建两个新类型。为了创建单元测试，我们将代码复杂化。很难理解使用 fabric 方法的原因。它可能导致违反单一责任原则(固体)。*

**2。将实例化移动到上层**。

```
**public class** CertificateManager
{
    **public void** CreateCertificate(Certificate certificate)
    { ... certificate.Upload();
    }
}
```

在这个例子中，我们将 Certificate 作为一个参数发送，并可以模拟它。

***利弊*** *:我们简化一下方法。*

***缺点*** *:需要改变代码的内部结构。这可能会导致问题，并要求为调用方法重写测试。这种重构需要更多的时间来实现。无论如何，这种方法是最好的，因为它总体上改进了应用程序的架构。*

**3。将功能移至较低级别**。

```
**public class** CertificateManagerpublic class CertificateManager
{
    **private** CertificateUploader _certificateUploader;

    **public** CertificateManager(CertificateUploader certificateUploader)
    {
        _certificateUploader = certificateUploader;
    }

    **public void** CreateCertificate(CertificateDto certificateDto)
    { ...

        **var** certificate = new Certificate(certificateDto.Name, certificateDto.Url, certificateDto.Content);

        _certificateUploader.Upload(certificate);
    }
}
```

在这个解决方案中，Certificate 变成了一个平面模型，我们不需要模仿它。相反，我们模仿 CertificateUploader。

***优点*** *:不改变代码的结构，而是将证书转换为平面模型。*

***缺点*** *:测试创建过程变得更加复杂，因为我们需要检查 _certificateUploader 中的一个参数。按照模板上传。它看起来是这样的:*

```
certificateUploader.Verify(x => x. Upload(
    It.Is<Certificate>
    (arg =>
        arg.Name == certificateDto.Name
            && arg.Url == certificateDto.Url
            && arg.Content == certificateDto.Content
    )
), Times.Once);
```

在上面的例子中，这个类只有三个字段，这就是为什么这个类与一个模板的比较不是一个复杂的任务。但是，在实际项目中，一个类中可以有十个以上的属性。一些开发人员编写一些“半测试”来简化过程:

```
certificateUploader.Verify(x => x. Upload(It.IsAny<Certificate>), Times.Once);
```

这样的做法带来的利润很小。它检查调用的事实，但忽略参数的实例化。此外，模板的比较要求与模板相关的所有测试对象属性都是公共的。否则，我们需要拒绝完整的测试或者公开类属性的一部分，这会干扰封装。这就是为什么我不推荐使用这种方法，除非它的使用是非常必要的。

# 对对象属性的访问

这个问题几乎是看不见的，但它会引起一堆问题。

```
**public void** ProcessCertificate(Certificate certificate)
{ ... **var** ownerName = certificate.Owner.Name; ...}
```

你可能会想“几乎没有”。是的，这是一个微小的细微差别。除非这个小问题把证书嘲讽的过程转化为令人难以置信的体验。为了解决 NullReferenceException 失败的问题，我们还需要模仿 Owner。一般来说，设置对类属性或方法的多级访问是一种不好的做法。但是，如果您需要用这样的结构测试代码，您可以使用这些解决方案:

1.  将取回财产的责任移交给上级。

```
**public void** ProcessCertificate(Certificate certificate, string ownerName)
{ ...}
```

***优点*** *:要求代码结构变化最小。*

***缺点*** *:通话看起来如下:*

```
_certificateManager.ProcessCertificate(certificate, certificate.Owner.Name);
```

我们可能很难称之为一个好的解决方案。

**2。类的扩展**。

给**证书**添加一个属性或方法，负责多级访问。

```
**public** **class** Certificate
{ ... **public** **virtual string** OwnerName => **this**.Owner.Name; ...}
```

或者

```
**public class** Certificate
{ ... **public virtual string** GetOwnerName()
    {
        return **this**.Owner.Name;
    } ...}
```

该方法将变成这样:

```
**public void** ProcessCertificate(Certificate certificate)
{ ... **var** ownerName = certificate.OwnerName; ...}
```

或者

```
**public void** ProcessCertificate(Certificate certificate)
{ ... **var** ownerName = certificate.GetOwnerName(); ...}
```

相应地。

***优点*** *:不需要改变代码的结构。*

***缺点*** *:这种做法没有明显的缺点。但是，我们不应该在代码中添加一个与多级访问无关的属性或方法、逻辑。如果有逻辑，我们必须编写额外的测试来检查它。如果该类没有接口，重要的是不要忘记将新的属性或方法设置为虚拟的。*

# 静态

静态的使用使你沉迷于模仿库(模仿者)。一些模仿者可以为静态类创建模仿(AFAIK 大多数是付费的，但是你可以试着找一个免费的)。最好的解决方案是在代码中完全避免静态类和方法。但是这并不总是可能的，因为现在很多著名的框架仍然在使用它。如果您需要测试一个静态类，您可以用常规类包装它，并以通常的方式模拟它。

```
**public static class** StaticLegacy
{
    **public static void** StaticDo()
    { ... }
}

**public interface** ILegacyAdapter
{
    **void** Do();
}

**public class** LegacyAdapter : ILegacyAdapter
{
    **public void** Do()
    {
        StaticLegacy.StaticDo();
    }
}
```

***优点*** *:你很难找到一个，但是这种方法允许用静态调用来测试方法。*

***缺点*** *:我们将不得不为单元测试创建两个额外的类型。*

# 私有方法

私有方法是封装的核心。然而，在单元测试创建期间，它们会成为一个真正令人头痛的问题。我完全不呼吁拒绝私有方法的使用，但是你在创建它们之前应该三思。如果私有方法只是公共方法的一部分，一切都很好。我们可以在公共方法的上下文中测试它。当几个公共方法共享一个私有方法时，问题就出现了。

```
**public class** SomeClass: ISomeClass
{
    **public void** Do1()
    { ... PrivateDo(); ... }

    **public void** Do2()
    { ... PrivateDo(); ... }

    **private void** PrivateDo()
    { ... }
}
```

我们可以在两个公共方法的上下文中测试 PrivateDo，但是这会导致重复的测试，因此，当改变 PrivateDo 时，工作会加倍(作为一种替代方法——单元测试结构的复杂化，这是不希望的)。您可以只在一个公共方法的上下文中测试 PrivateDo，但是在这种情况下，另一个方法的单元测试失去了它的信息性:因为不清楚测试方法是自身出错还是只有它的私有部分出错。

1.  **把私有方法变成公有**。

这个解决方案允许我们直接测试这个方法。这种方法的严重缺点是违反了封装。我们面临的另一个问题是来自 PrivateDo 的 Do1 和 Do2 之间的依赖性。一些模仿器允许通过部分模仿(例如，nSubstitude)来解决这个问题，而另一些则没有部分模仿功能。

然而，我们可以通过不向接口添加新打开的方法来减少负面影响。例如:

```
**public interface** ISomeClass
{
    **public void** Do1();

    **public void** Do2();
}

**public class** SomeClass: ISomeClass
{
    **public virtual void** Do1()
    { ... PrivateDo(); ... }

    **public virtual void** Do2()
    { ... PrivateDo(); ... }

    **public virtual void** PrivateDo()
    { ... }
}
```

在这种情况下，ISomeClass 客户端不会知道 PrivateDo。它允许我们避免非法使用。测试不会模拟接口 ISomeClass，而是直接模拟某个类(因此，它的方法已经变成了虚拟的)。

**2。倒影**。

反射允许我们在不违反封装规则的情况下测试私有方法。然而，测试变得更加复杂，并且其执行速度降低。一些测试框架包括简化反射测试的适配器。例如，MsTest 有一个特殊的类 PrivateObject，它允许我们以这样的方式调用私有方法:

```
**var** someClass = new SomeClass();
**var** obj = new PrivateObject(someClass);
obj.Invoke(“PrivateDo”);
```

但是 PrivateDo 的 Do1 和 Do2 之间的依赖关系问题不能用所选的方法解决。

**3。搬出责任**。

对于私有方法，更明智的解决方案是将其作为另一个类的公共方法。例如:

```
**public class** SomeClass: ISomeClass
{
    **private** IHelper _helper;

    **public** SomeClass(IHelper helper)
    {
        _helper = helper;
    }

    **public void** Do1()
    { ... _helper.Do(); ... }

    **public void** Do2()
    { ... _helper.Do(); ... }
}
```

***优点*** *:解决了私有方法测试的所有问题，减少了代码连接。*

***缺点*** *:如果私有方法需要私有字段，将其移出到单独的类型会破坏封装。这是因为我们需要将私有字段作为参数发送。*

如果私有方法改变了内部状态，这种解决方案是不可接受的。例如:

```
**public class** SomeClass: ISomeClass
{
    **private string** _privateString1;

    **private string** _privateString2;

    **public void** Do1()
    { ... PrivateDo(); ... }

    **public void** Do2()
    { ... PrivateDo(); ... }

    **private void** PrivateDo()
    { ... privateString1 = "qwe";
        privateString2 = "asd";

    }
}
```

在这种特殊情况下，我们可以做一个小把戏。如果我们只保留状态变化的逻辑，但将所有其他部分移到单独的类中，我们可以在类中保存一个私有方法:

```
**public class** SomeClass: ISomeClass
{
    **private string** _privateString1;

    **private string** _privateString2;

    **private** IHelper _helper;

    **public** SomeClass(IHelper helper)
    {
        _helper = helper;
    }

    **public void** Do1()
    { ... PrivateDo(); ... }

    **public void** Do2()
    { ... PrivateDo(); ...
    }

    **private void** PrivateDo()
    { ... **var** result = _helper.Do();

        privateString1 = result.String1;
        privateString1 = result.String2;
    }
}
```

# 没有接口的类

我们可以模仿没有接口的类。但是我们需要记住两件事。首先，这些类的方法必须是虚拟的。否则，您的模拟的一部分将作为模拟，另一部分作为原始对象。在这种情况下，缺乏适当的关注会导致低可追溯性的 bug。修复这样一个错误可能会花费你额外的几个小时。一个例子:

```
**public class** SomeClass
{
    **public virtual void** Do1()
    { ... }

    **public virtual void** Do2()
    { ... }

    **public void** Do3()
    { ... }
}
```

模拟装置 **Do1** 和 **Do2** 将按照预期的方式工作。模拟方法 Do3 是无用的，因为它调用原始方法。

没有接口的类的第二个问题是构造函数。类的模拟需要存在默认构造函数(不带参数)。否则，您也需要创建参数模型。

在某些情况下，构造函数包含逻辑。所以这个逻辑在所有模拟类的测试中都被调用。总的来说，这不是一个好方法。解决方案非常简单——所有被模仿的类都必须包含接口。这条规则没有例外。

唯一的特例是没有接口的密封类。我们可以在一些框架中找到它们。默认情况下，它们是不可移除的。唯一的解决方案是创建一个包装器并模仿它。

# 构造函数中的逻辑

构造函数中的逻辑是一种众所周知的反模式。然而，很多开发人员使用它。这种方法有许多缺点。对我们来说最关键的是，每次测试都会反复调用构造函数逻辑。构造函数中的错误破坏了该类的所有测试。有两种方法可以避免这个问题。

1.  **方法初始化**。

它将初始化过程分为两个独立的部分:对象创建(构造函数)和附加逻辑(Init())。

```
**public class** SomeClass
{
    **public** SomeClass(params)
    { ... }

    **public void** Init()
    { ... }
}
```

***优点*** *:测试忽略 Init 方法以避免依赖于构造函数逻辑。*

***缺点*** *:在测试之外，这种方法非常糟糕。首先，因为它不明显。其次，它使 IoC 容器的设置复杂化，违反了 SOLID 的单一责任原则。*

我不推荐使用初始化器，即使它们解决了构造函数的核心问题。

**2。面料方法**。

实例化逻辑被移到一个单独的类中。

```
**public class** SomeClass
{
    **public** SomeClass(calculatedParams)
    { ... }
}

    **public interface** ISomeClassCreator
    {
        SomeClass Create(params);
    }

**public class** SomeClassCreator: ISomeClassCreator
{
    **public** SomeClass Create(params)
    { ... }
}
```

***优点*** *:可以摆脱繁重的构造函数，改善代码结构。*

***缺点*** *:需要为测试创建两个特殊类型。此外，繁重的构造函数不仅仅是单元测试的问题。这就是为什么这种复杂化是合理的。*

# 变量环境的使用

如果该方法使用某个全局资源(例如，app.config 中的设置),那么它的测试过程就变得非常重要。

```
**public class** SomeClass
{
    **public void** Do()
    { ... **var** mySetting = ConfigurationManager.AppSettings["MySetting"] ... }
}
```

模拟 ConfigurationManager 的正确操作是一项复杂的任务。我们不能嘲笑它。我们可以为不同的测试创建不同的文件，但这可能是一个相当复杂的解决方案。一般来说，这种情况与静态对象相同。这就是为什么解决方案是相同的——创建一个包含环境变量的类并通过它工作:

```
**public class** SomeClass
{
    **private** Settings _settings;

    **public** SomeClass(Settings setting)
    {
            _ settings = setting;
    }

    **public void** Do()
    { ... **var** mySetting = settings.MySetting; ... }
}
```

# 结论

单元测试的支持增加了设计测试工作流时需要考虑的几个因素，但是努力会有回报的。

有很多编写可测试代码的最佳实践。我们的主要目标是创建一个每个人都可以维护的健壮的解决方案。即使你不打算写单元测试，你也需要使用这个指南，因为它会帮助你提高代码和架构的质量。这需要额外的时间，但这是值得的。

*在*[*dash devs*](https://www.dashdevs.com/#expertise)*公司的经历中，我们有很多客户要求不要写单元测试的产品。毕竟，大多数客户意识到单元测试是稳定产品的保证。*

*让我们用*[*dash devs*](https://www.dashdevs.com/#contacts)*来讨论一下你的可靠产品开发的细微差别。*