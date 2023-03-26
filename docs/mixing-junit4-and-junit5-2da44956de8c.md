# 混合 JUnit4 和 JUnit5

> 原文：<https://medium.com/hackernoon/mixing-junit4-and-junit5-2da44956de8c>

![](img/cfb70e54e04b2718597ce21779383229.png)

Photo by [Yomex Owo](https://unsplash.com/photos/zRDoTgjDvCc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/mix?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

JUnit 是 Java 编程语言最流行的测试框架之一。它在测试驱动开发(TDD)的发展中也很重要。2018 年，该框架的最新版本 JUnit5 发布，许多使用 JUnit 平台的程序员不得不决定是否要重构他们在以前版本的 JUnit 中编写的现有代码。

这篇文章是关于将 JUnit4 和 JUnit5 混合到您正在进行的项目中，以获得 JUnit5 的好处，而无需重构所有已经编写的 JUnit4 测试。目的不是深入描述 JUnit5，而是给出一个迁移 JUnit5 支持的指南。

JUnit5 有许多开发人员想要使用的好处。更好的异常处理，更好的参数化测试，多个*测试运行器*等等。如果您像我一样，希望尽快迁移到一个更新的平台，您可能也知道这样做有多痛苦。

幸运的是，JUnit5 很好地支持与版本 4 一起工作。我认为这在已经有数百个已写好的测试的项目中是很重要的。重构所有这些都是巨大的工作，放弃比继续采用新的东西要容易得多。

在下面的文章中，我分解了引入 JUtin5 测试支持，同时仍然支持 JUnit4 测试所需遵循的步骤。

让我们假设我们有 **gradle** 项目

```
plugins {
  id 'java'
  id 'com.adarshr.test-logger' version '1.6.0'
}

group 'rainoko'
version '1.0-SNAPSHOT'

sourceCompatibility = 11

repositories {
  mavenCentral()
}

dependencies {
  testCompile('junit:junit:4.12')
}

wrapper {
  gradleVersion = '5.2.1'
}
```

用一些简单的类*SimpleNaturalCalculator.java*

```
package ee.rainoko.junit5demo;

public class SimpleNaturalCalculator {
  public int addition(int arg1, int arg2) {
    return arg1 + arg2;
  }

  public int substraction(int arg1, int arg2) {
    return arg1 - arg2;
  }

  public int multiplication(int arg1, int arg2) {
    return arg1 * arg2;
  }

  public int division(int arg1, int arg2) {
    return arg1 / arg2;
  }
}
```

和盖有测试*的 SimpleNaturalCalculatorTest.java*

```
package ee.rainoko.junit5demo;

import org.junit.*;

import static org.junit.Assert.*;

public class SimpleNaturalCalculatorJunit4Test {
  @Test
  public void shouldAdd() {
    SimpleNaturalCalculator calculator = new SimpleNaturalCalculator();

    int result = calculator.addition(1, 2);

    *assertEquals*(3, result);
  }

  @Test
  public void shouldSubstract() {
    SimpleNaturalCalculator calculator = new SimpleNaturalCalculator();

    int result = calculator.substraction(1, 2);

    *assertEquals*(-1, result);
  }

  @Test
  public void shouldMultiply() {
    SimpleNaturalCalculator calculator = new SimpleNaturalCalculator();

    int result = calculator.multiplication(1, 2);

    *assertEquals*(2, result);
  }

  @Test
  public void shouldDivide() {
    SimpleNaturalCalculator calculator = new SimpleNaturalCalculator();

    int result = calculator.division(1, 2);

    *assertEquals*(0, result);
  }
}
```

正如我们看到的，这是一个 JUnit4 测试，使用了来自 *org.junit* 的注释

让我们用下面的命令来运行它:

```
gradlew test
```

我们得到了这个结果:

```
ee.rainoko.junit5demo.SimpleNaturalCalculatorTest Test shouldAdd PASSED
  Test shouldDivide PASSED
  Test shouldSubstract PASSED
  Test shouldMultiply PASSED
```

现在我们已经用 JUnit4 建立了一个标准项目。

让我们继续。我们的目标是引入 JUnit5 支持。与版本 4 不同，版本 5 有模块化的库结构:[https://junit.org/junit5/docs/current/user-guide/]

> **JUnit 5 = *JUnit 平台*+*JUnit Jupiter*+*JUnit Vintage***

JUnit 平台是[在 JVM 上发布测试框架](https://junit.org/junit5/docs/current/user-guide/#launcher-api)的基础。[https://junit.org/junit5/docs/current/user-guide/]

**JUnit Jupiter** 是 JUnit 5 中新的[编程模型](https://junit.org/junit5/docs/current/user-guide/#writing-tests)和[扩展模型](https://junit.org/junit5/docs/current/user-guide/#extensions)的组合，用于编写测试和扩展。[https://junit.org/junit5/docs/current/user-guide/]

**JUnit Vintage** 为在平台上运行基于 JUnit 3 和 JUnit 4 的测试提供了一个`TestEngine`。[https://junit.org/junit5/docs/current/user-guide/]

现在知道了这一点，让我们添加 JUnit5 支持。首先，为了避免混淆，让我们将 junit4 测试类重命名为 SimpleNaturalCalculatorJunit4Test.java

让我们将 JUnit5 依赖项添加到 *gradle*

```
// JUnit Jupiter
testImplementation('org.junit.jupiter:junit-jupiter:5.4.0')

// JUnit Vintage
testCompileOnly('junit:junit:4.12')
testRuntimeOnly('org.junit.vintage:junit-vintage-engine:5.4.0') {
  because 'allows JUnit 3 and JUnit 4 tests to run'
}
```

如您所见，我们添加了 junit-jupiter。也将 junit:4.12 标记为 testCompileOnly。如果我们想让我们的老 junit4 测试不做任何修改，这个库仍然需要编译。然后，我们还添加了 junit-vintage-engine，使 junit 平台能够同时运行 JUnit4 测试和 JUnit5 测试。

最后，我们还需要添加命令来使用 JUnit5 平台在 *gradle* 中运行测试

```
test {
  useJUnitPlatform()
}
```

不，让我们添加 6 月 5 日测试类*SimpleNaturalCalculatorJunit5Test.java*

```
package ee.rainoko.junit5demo;

import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*assertEquals*;

class SimpleNaturalCalculatorJunit5Test {
  @Test
  void shouldAdd() {
    SimpleNaturalCalculator calculator = new SimpleNaturalCalculator();

    int result = calculator.addition(1, 2);

    *assertEquals*(3, result);
  }

  @Test
  void shouldSubstract() {
    SimpleNaturalCalculator calculator = new SimpleNaturalCalculator();

    int result = calculator.substraction(1, 2);

    *assertEquals*(-1, result);
  }

  @Test
  void shouldMultiply() {
    SimpleNaturalCalculator calculator = new SimpleNaturalCalculator();

    int result = calculator.multiplication(1, 2);

    *assertEquals*(2, result);
  }

  @Test
  void shouldDivide() {
    SimpleNaturalCalculator calculator = new SimpleNaturalCalculator();

    int result = calculator.division(1, 2);

    *assertEquals*(0, result);
  }
}
```

正如你可以从进口确认，我们现在使用 junit 木星。请注意，测试类是受包保护的，方法也是。JUnit5 现在允许我们使用受包保护的修饰符，我认为这更好地揭示了 java 的访问修饰符的用途。

让我们运行测试

```
gradlew test
```

我们有产出

```
ee.rainoko.junit5demo.SimpleNaturalCalculatorJunit4Test Test shouldAdd PASSED
 Test shouldDivide PASSED
 Test shouldSubstract PASSED
 Test shouldMultiply PASSEDee.rainoko.junit5demo.SimpleNaturalCalculatorJunit5Test Test shouldAdd() PASSED
  Test shouldDivide() PASSED
  Test shouldSubstract() PASSED
  Test shouldMultiply() PASSED
```

我们甚至可以使用混合测试类。

```
package ee.rainoko.junit5demo;

import static org.junit.jupiter.api.Assertions.*assertEquals*;

public class SimpleNaturalCalculatorJunitMixedTest {
  @org.junit.Test
  public void shouldAddJunit4() {
    SimpleNaturalCalculator calculator = new SimpleNaturalCalculator();

    int result = calculator.addition(1, 2);

    org.junit.Assert.*assertEquals*(3, result);
    org.junit.jupiter.api.Assertions.*assertEquals*(3, result);
  }

  @org.junit.jupiter.api.Test
  void shouldAddJunit5() {
    SimpleNaturalCalculator calculator = new SimpleNaturalCalculator();

    int result = calculator.addition(1, 2);

    org.junit.Assert.*assertEquals*(3, result);
    org.junit.jupiter.api.Assertions.*assertEquals*(3, result);
  }

  @org.junit.Test
  public void shouldSubstractJunit4() {
    SimpleNaturalCalculator calculator = new SimpleNaturalCalculator();

    int result = calculator.substraction(1, 2);

    *assertEquals*(-1, result);
  }

  @org.junit.jupiter.api.Test
  void shouldSubstractJunit5() {
    SimpleNaturalCalculator calculator = new SimpleNaturalCalculator();

    int result = calculator.substraction(1, 2);

    *assertEquals*(-1, result);
  }

  @org.junit.Test
  public void shouldMultiplyJunit4() {
    SimpleNaturalCalculator calculator = new SimpleNaturalCalculator();

    int result = calculator.multiplication(1, 2);

    *assertEquals*(2, result);
  }

  @org.junit.jupiter.api.Test
  void shouldMultiplyJunit5() {
    SimpleNaturalCalculator calculator = new SimpleNaturalCalculator();

    int result = calculator.multiplication(1, 2);

    *assertEquals*(2, result);
  }

  @org.junit.Test
  public void shouldDivideJunit4() {
    SimpleNaturalCalculator calculator = new SimpleNaturalCalculator();

    int result = calculator.division(1, 2);

    *assertEquals*(0, result);
  }

  @org.junit.jupiter.api.Test
  void shouldDivideJunit5() {
    SimpleNaturalCalculator calculator = new SimpleNaturalCalculator();

    int result = calculator.division(1, 2);

    *assertEquals*(0, result);
  }
}
```

正如你现在看到的，我们有来自 org.junit 和 org.junit.jupiter.api 的注释。

您甚至可以在任一测试类型中混合旧 JUnit4 包和新 JUnit5 包中的断言。

```
gradlew test
```

我们得到输出

```
ee.rainoko.junit5demo.SimpleNaturalCalculatorJunit4Test Test shouldAdd PASSED
  Test shouldDivide PASSED
  Test shouldSubstract PASSED
  Test shouldMultiply PASSEDee.rainoko.junit5demo.SimpleNaturalCalculatorJunitMixedTest Test shouldAddJunit4 PASSED
  Test shouldDivideJunit4 PASSED
  Test shouldMultiplyJunit4 PASSED
  Test shouldSubstractJunit4 PASSEDee.rainoko.junit5demo.SimpleNaturalCalculatorJunit5Test Test shouldAdd() PASSED
  Test shouldDivide() PASSED
  Test shouldSubstract() PASSED
  Test shouldMultiply() PASSEDee.rainoko.junit5demo.SimpleNaturalCalculatorJunitMixedTest Test shouldAddJunit5() PASSED
  Test shouldDivideJunit5() PASSED
  Test shouldMultiplyJunit5() PASSED
  Test shouldSubstractJunit5() PASSEDSUCCESS: Executed 16 tests in 1s
```

**注意** JUnit4 和 JUnit5 有不同的@Before…注释。如果您需要混合使用它们，请将注释添加到您的 setUp 和 tearDown 方法中。

希望这篇文章有助于迁移到 JUnit5，即使您的项目大量使用 JUnit4 测试，并且**现在受益而不是明天**。

# 阅读更多

有很多关于 JUnit5 的好例子和文章。也读一读

*   [https://junit.org/junit5/docs/current/user-guide/](https://junit.org/junit5/docs/current/user-guide/)
*   [https://www.baeldung.com/junit-5](https://www.baeldung.com/junit-5)
*   [https://github.com/junit-team/junit5-samples](https://github.com/junit-team/junit5-samples)