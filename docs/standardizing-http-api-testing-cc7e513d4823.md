# 标准化 HTTP API 测试

> 原文：<https://medium.com/hackernoon/standardizing-http-api-testing-cc7e513d4823>

这篇博文实际上是我的软件开发和咨询业务的标准操作程序草案。在过去的几个月里，我遇到过三四次为 HTTP APIs 编写测试的任务。我已经尝试了多种编写自动化测试的方法，这是我最喜欢的方法。TypeScript、Jest 和 supertest 似乎配合得很好，足以实现简洁的测试。

![](img/548b80884936af682a6d516729c316f3.png)

# 背景和标准化的需要

几年前，我的大部分网络编程仅限于 PHP。使用 PHPUnit 和 Guzzle 对这些项目进行了测试。后来，我们还使用 [Behat](http://behat.org/en/latest/) 进行更具可读性的测试。Docker 流行起来后，PHP monolith 演变成了一个用多种编程语言构建的应用程序。现在一些小服务是用 Node.js 写的，新的数据分析代码大多是用 Python 写的。

起初，用与服务器相同的语言编写 API 级测试似乎是显而易见的。但是在比较了 Node.js、PHP 和 Python 的解决方案之后，很明显不同语言之间的质量和方法是不同的。

除了质量上的差异，问题还在于学习每个测试框架所浪费的时间。投入时间学习 Python 是值得的，因为我们现在可以使用 Pandas 和 Scipy 进行分析。Jupyter 在原型制作方面很棒。相比之下，学习用每种语言测试 HTTP APIs 没有任何价值。使用同一种语言来编写 web 服务和伴随的测试也没有技术上的理由。

对于像 HTTP API 测试这样的任务，拥有一个标准的过程有很多好处:你可以用同一个框架编写很多测试。在你学习了基础知识之后，你可以专注于更高层次的问题，比如组织测试，确定测试内容以及测试更复杂的 API。每当你创建一个新的 web 服务时，你不会浪费时间在谷歌上搜索诸如“测试 REST API 的最好方法是什么？”。因为决定标准化我工作流程的这一部分，我正在为其他决定节省大脑带宽。

# 我的标准测试堆栈

在过去的几个月里，我对 TypeScript 的体验非常积极。大部分时间我都是在前端用 Angular。但它在后端的表现同样出色。可用的工具和类型定义使 Express 的开发变得轻而易举。与普通 JavaScript 相比，像 VS 代码这样的 ide 中的自动完成功能是非常出色的。类型检查非常有用，尤其是在重构的时候。它支持 ES2017 的现代功能，如[异步/等待](https://hackernoon.com/6-reasons-why-javascripts-async-await-blows-promises-away-tutorial-c7ec10518dd9)。我曾经使用 Karma 作为测试框架，但是找到插件和编写配置的良好组合是一场噩梦。Jest 是一个令人愉快的改进。它开箱即用，并具有您可能已经熟悉的常见 API:[describe，It，expect](https://jestjs.io/docs/en/api) 。测试 HTTP APIs 的另一个重要因素是发出请求的方式。Superagent 有非常好的[文档](https://visionmedia.github.io/superagent/)，它与 TypeScript 配合得很好。我喜欢它的可链接语法。

# 测试简单 GET 请求的示例

```
import request = require('supertest');const BASE_URL = 'http://my-service:3000';describe('Hello world', () => {
    it('Should say "hello world"', async() => {
        const response = await request(BASE_URL)
            .get('/')
            .expect(200);
        expect(response.text).toBe("hello world");
    });
});
```

函数`describe`将特定特性的测试组合在一起。函数`it`定义了一个测试。在这种特殊情况下，测试是一个异步函数。它也可以是一个普通函数或返回承诺的函数。在接下来的四行中，我们看到了`async/await`语法的优雅。执行仍然是异步的，但是语法看起来非常线性。这使语法与我们连续步骤的心理模型一致。行`.expect(200)`断言响应代码确实是`200`。最后，我们还要检查响应的内容。

# 发布请求

使用 POST 和 PUT 请求发送数据如下所示:

```
import request = require('supertest');const BASE_URL = 'http://my-service:3000';describe('POST /news', () => {
    it('Should save and return back the news', async() => {
        const response = await request(BASE_URL)
            .post('/news')
            .send({'title': 'Breaking news!'})
            .expect(201); expect(response.body).toEqual({'id': 1, 'title': 'Breaking news!'});
    });
});
```

我们使用了`post(url)`而不是`get(url)`，并添加了对`send(data)`的调用来发送我们的 JSON。

# 授权和其他标题

可以使用`set('header-name', 'value')`设置标题，如下例所示:

```
import request = require('supertest');const BASE_URL = 'http://my-service:3000';describe('Authorization test', () => {
    it('Should respond with 401 if token invalid', async () => {
        await request(BASE_URL)
            .post('/admin/news')
            .set('Authorization', 'Bearer invalid')
            .send({})
            .expect(401)
            .expect('WWW-Authenticate', /^Bearer/);
    });
});
```

我希望这篇文章能帮助你更快地设置测试。完整的代码可在[https://gitlab.com/mdrolc/testing-http-apis](https://gitlab.com/mdrolc/testing-http-apis)那里你会发现一个简单的服务和相应的测试。

*原载于 2019 年 2 月 19 日*[*【https://drola.si/post/2019-02-19-testing-http-apis*](https://drola.si/post/2019-02-19-testing-http-apis)*。*