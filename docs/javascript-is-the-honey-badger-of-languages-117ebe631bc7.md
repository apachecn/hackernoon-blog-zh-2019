# JavaScript 是语言中的蜜獾

> 原文：<https://medium.com/hackernoon/javascript-is-the-honey-badger-of-languages-117ebe631bc7>

## 我终于发现了如何解释 JavaScript。

疯狂的 JavaScript 根本不在乎。向它扔任何东西，它都会处理的。它根本不在乎什么编译错误。反正它只会运行程序。那么如果它给出了错误的结果呢？至少它给出了一些结果。如果结果可能是错的，就用 [jest](https://jestjs.io/) 或 [mocha](https://mochajs.org/) 来测试一下。

JavaScript 测试框架根本不在乎。您可以用 JavaScript 测试任何复杂的东西。不需要依赖注入体操，或过度一般化的接口，或反射魔术。用 [sinon](https://sinonjs.org/) 存根、间谍、嘲笑任何东西。用 [webdriverio](https://webdriver.io/) 进行端到端测试，或者用我个人最喜欢的 [lodash-match-pattern](https://www.npmjs.com/package/lodash-match-pattern) 测试 API😉

JavaScript 又臭又臭又丑，也不在乎。这就是为什么你用 [eslint](https://eslint.org/) 、[标准化](https://standardjs.com/)，用[更漂亮](https://prettier.io/)清理。

蜜獾 JavaScript 不在乎你看到它的内脏。没有*私有*或*受保护的*变量。您可以随时检查和更改任何对象和任何功能。蜜獾不会隐藏任何东西。他们为什么要这么做？他们太坏了。

JavaScript 不会等待。为什么会这样？等待是为了植物，不是蜜獾。JavaScript 不会等待数据库访问、API 调用或其他任何事情发生。它不会为错过的*等待*关键词而遗憾，也不会为未兑现的*承诺而内疚。*

你想要静态类型？JavaScript 的类型有 [TypeScript](https://www.typescriptlang.org/) 或 [Flow](https://flow.org/) 。但你问我这有点像给蜜獾装上翅膀。它们不合适，最终，它们会被*“any”*类型声明分解。(除此之外还有谁认为“类型安全”其实是个东西？😜)

![](img/6ab77ad8f405d5c841116adcaa861f63.png)

蜜獾喜欢使用工具

JavaScript 也是如此。

在[的最后一次统计](http://www.modulecounts.com/)，NPM 的包仓库拥有超过 800，000 个模块——几乎是 Maven (Java)的 3 倍。所以，如果这些模块中的大部分是丑陋的，像泥球一样可用，仍然有足够的价值来解释每月超过[180 亿次下载](https://www.linux.com/news/event/Nodejs/2016/state-union-npm)。

蜜獾几乎什么都吃——毒蛇、猛禽蛋、蝎子，以及它们最喜欢的——蜜蜂幼虫。他们不在乎它叫什么，对他们来说它只是“食物”。JavaScript 也会消化任何东西，它不在乎它叫什么，对 JavaScript 来说它只是“对象”。此外，[析构](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)和[展开语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)允许 JavaScript 将对象分解成最美味、最有营养的部分。

JavaScript 不关心它如何与其他语言接口:protobufs、swagger 定义、RESTish、graphQL。这就是它的工作，接受所有被搞砸了的 API 扔给它的垃圾，并使它有一些意义。

# **JavaScript 的蜜獾后代**

[*Lodash*](https://lodash.com/docs) 就是这么牛逼。如果您认为您需要在 JavaScript 中使用 *for* 循环或 *while* 循环，那么您对 Lodash 的了解还不够。如果你用 JavaScript 的原生 *foreach、*或者 *map、*或者 *filter* ，你对Lodash 的了解还不够。如果您需要编写定制代码来执行常见的字符串操作、原始类型检查或任何对象集合的整形，那么您对 Lodash 的了解还不够。

看看 [momentjs](https://momentjs.com/) 对日期做了什么。它不关心时区或节约时间，甚至不关心你在哪里。它会吃掉几乎所有愚蠢的日期格式，进行日期运算，并以本地时区和本地格式显示日期。

谁他妈的需要 ORM？JavaScript 不会。关系数据库表不是自然的 OOP 对象，假装它们只是制造了精神上的繁忙工作。通过 [knexjs](https://knexjs.org) JavaScript 避免了[DB/对象阻抗不匹配](https://www.geeksforgeeks.org/dbms-impedance-mismatch/)、[泄漏 ORM 抽象](https://martinfowler.com/bliki/OrmHate.html)，并且经常可以跳过冗余的 OOP 模型定义，因为 SQL 数据模型本身就很好。

你到底是谁，你在这里做什么？ [*passport*](http://www.passportjs.org/) 模块可以算出来。

忘了你神圣的前端 MVC 模式吧。 [*反应*](https://reactjs.org/) (还有朋友 [*redux*](https://redux.js.org/) ， [*vue*](https://vuejs.org/) 等。)永久地摧毁了那个被误用的、过于一般化的教条。

然后还有 [*【蓝鸟】*](http://bluebirdjs.com/docs/getting-started.html)[*axios*](https://github.com/axios/axios)[*babel*](https://babeljs.io/)以及其他许许多多让 JavaScript 成为这个星球上最无畏的语言的语言。

![](img/ffbcbbf59b85cf40edb52de7fd682d4c.png)

经过多年的使用和滥用，JavaScript 已经进化出了一层厚厚的松松垮垮的皮。它经受住了多年来来自编程实践各个角落的叮咬。但是 Github 上每年有 230 万的拉取请求，它根本不在乎人们怎么想。