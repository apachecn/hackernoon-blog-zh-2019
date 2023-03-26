# 你需要对你的代码进行自动化测试的四个原因

> 原文：<https://medium.com/hackernoon/four-reasons-you-need-automated-tests-for-your-code-3fd4f6f4e720>

## 由 Verv 的区块链开发者 Vikash Tank 撰写

![](img/d9d8c4e470eaeee91b299dd6fc1492c1.png)

Photo by [Med Badr Chemmaoui](https://unsplash.com/photos/ZSPBhokqDMc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我们都应该在项目中使用自动化测试。它可以被视为一个无用的过程，如果代码有效，它不会产生任何影响，但我在这里说服您为什么这是一个好主意，以及它可以带来什么好处。

这篇文章将带你了解什么是自动化测试，你为什么要这样做，好处和缺点，以及在你写完测试之后要做什么。我们还将介绍测试驱动开发(TDD)的优点，以及它如何进一步改进您的工作流程。

什么是自动化测试？

编写代码的过程是测试系统的每个部分，以确保它们都按预期运行。自动化测试可以分为两种主要类型；单元测试是测试每一个单独的功能，而集成测试是测试你的系统中所有连接在一起的许多部分。知道这两者之间的区别是很好的，但是这篇文章将关注自动化测试作为一个整体的好处。

![](img/36ebc041df93e03275a3072c75244633.png)

Photo by [Temple Cerulean](https://unsplash.com/photos/tP8ZwlCF8og?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**为什么要写测试？4 注意事项**

在决定是否编写测试时，您可能需要考虑的一些事情是:

*   **你是否经常写一段第一次就无法运行的代码？**
*   你是否花费超过 5 分钟的时间来手动测试这段代码？
*   **您是否经常发现自己在最初创建代码后进一步开发代码？**
*   **其他人也在做这个代码吗？**

如果以上任何一个问题的答案是**是**的话，你或许应该考虑写测试了。测试的创建迫使你考虑代码的输入和输出需要是什么。有了这些，您就可以快速地对代码进行修改，并且知道一旦测试通过，它就会做您所需要的事情。测试运行起来通常比手工测试代码要快得多，尤其是当您每次添加或更改代码时都必须手工测试的时候。最初花费在设置测试上的时间最终被非手工测试所节省的时间所抵消。

也就是说，编写代码测试最初可能会很耗时，尤其是如果您知道自己可以快速编写代码，并且第一次就能正确完成的话。学习那些允许你进行单元测试的库和它背后的技巧意味着一开始可能会很慢，但是以后会提供很多好处。不浪费时间学习这种技能似乎更好，但是当您的系统增长并且在不引入其他人的情况下修复 bug 变得更加困难时，您可能会将时间收回。

反对测试的论点是如果我的测试是错的会发生什么？如果我的代码中可以有 bug，为什么我的测试中不能有呢？理想情况下，测试应该是没有非常复杂的逻辑的干净的代码片段，这意味着你不太可能引入错误。考虑到这一点以及您必须编写错误的测试和代码来创建 bug 的事实，您的代码不太可能出现问题。尽管编写测试是对错误的一种保护，但是如果你的测试和代码以完全相同的方式出错，你的错误将会被忽视。而这正是 QA 可以帮忙的地方。

![](img/4572495914b596551b02098b6e0dc0f8.png)

Photo by [Mason Panos](https://unsplash.com/photos/eUfZgWw0JrY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/adventure?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果你在一个敏捷团队中工作，它受到了瘟疫的打击(最近发生在这里的 [Verv](http://www.verv.energy) ！)，你可能是唯一的幸存者，完成你可能不擅长的任务。假设没有自动化测试，代码库相当大。您按照自己的方式处理代码，添加您需要的功能(以及对新部件的测试！).因为你不是代码方面的专家，而且在你所拥有的时间内，你不可能学会整个系统是如何工作的，你怎么知道系统的旧部分需要什么样的手工测试呢？可能会有一个巨大的文档详细概述每个手工测试……**但是运行自动化测试不是更快吗？**几秒钟之内，你就知道你可能破坏了已经存在的功能。

在这种情况下，你不需要花很多时间去了解整个系统，尤其是当周围没有人帮助的时候，你可以继续为你的项目添加更有价值的功能。测试通常比文档提供更清晰、更符合逻辑的功能描述。当添加新功能时，您可以准确地看到哪些测试失败，并更好地理解您编写的部分如何影响整体代码。测试的存在允许你试错新的代码，只要测试通过，并且你确信你已经很好地覆盖了代码。在这种情况下，您可以快速工作，并针对每个变更频繁地运行测试，直到您迭代地达到所有测试都成功的点。

如果你在几个星期后回到一段代码，并意识到它肯定没有达到应有的架构，会发生什么？你可能不得不重构你的代码，这是生活中最大的乐趣之一！但是当您已经实现了自动化测试时，这就不那么痛苦了。因为您只是重构代码，而不是改变它的功能，所以测试可以保持不变。您可能不希望更改现有的函数名，因为这些函数名可能会被用户/开发人员使用，所以您的测试可以在重新架构后不加更改地运行。如果他们通过了，你肯定知道你没有破坏你的团队成员的生活(假设你在测试中覆盖了你代码的所有功能！).

我会鼓励为你参与的所有项目编写测试。一段简单的代码本应该只使用一次，然后不得不集成或开发成一个更大的项目，这种情况并不少见。但最终这取决于你的判断。

![](img/2c58d9ed93d3db9432853bd62b5cad98.png)

Photo by [Med Badr Chemmaoui](https://unsplash.com/photos/ZSPBhokqDMc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/collections/4322820/the-code-venture-beyond-technology?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**为什么首先编写测试很重要**

首先编写测试(又名 TDD)比事后编写测试有更多的好处。首先编写测试，抵制添加适合您编写的代码的测试的诱惑，而不是添加适合您的测试的代码，这意味着您不太可能忽略不正确的响应。

回想起来写测试也是一件艰难的工作，尤其是如果你已经很长时间没有看代码了。了解在编写代码时确定的规范，并记住代码是如何工作的，以便涵盖所有可能出现错误的情况是不太可能的。

**测试后会发生什么？**

多写测试！一旦你写了一组测试并且你的代码是完整的，当你添加更多的功能或者让你的同事在你的拉请求中检查你的测试是有用的。覆盖工具可以检查你是否已经用测试覆盖了系统的所有代码，但是它们不会覆盖所有可能的问题。奇怪的“边界情况”,你的代码运行在它能够运行的边缘，但仍然应该工作，很容易从缝隙中溜走。

![](img/c92ccc0afe8f123d4ed1bb12a03e37de.png)

Photo by [rawpixel](https://unsplash.com/photos/nMVK0udN0-o?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/search/photos/bug?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

调试可能是一项巨大的任务，有时很难记住代码的哪一部分可以防止每个错误。在编写了初始测试并发现了一个 bug 之后，添加一个重现问题的测试是很有帮助的。这个测试不是一段临时代码，而是将永远保留在您的测试文件中的东西，以确保错误一旦被修复，就不会再出现。bug 返回的唯一方式是如果你破坏了你的测试，忽略它并发布。唯一比 bug 更烦人的事情是，在假定的修复之后，几个星期后发现同一个 bug 又出现了。这对那些讨厌多次查找 bug 的用户来说是非常有害的。任何从事代码工作的人都可能意外地恢复一个看似不重要的细微变化，并带回 bug。因此，最好是编写一个测试来查找任何 bug。

最后，编写测试会有很大的帮助。练习技能并度过最初的学习时间可以提供我所讨论的所有好处。它可能并不总是直截了当的，它可能看起来很复杂，或者是一个一开始会让你慢下来的过程，但最终你会更有经验，并且像学习任何其他技能一样，变得更快。