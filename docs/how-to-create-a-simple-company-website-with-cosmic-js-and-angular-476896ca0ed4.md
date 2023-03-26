# 如何用 Cosmic JS 和 Angular 创建一个简单的公司网站

> 原文：<https://medium.com/hackernoon/how-to-create-a-simple-company-website-with-cosmic-js-and-angular-476896ca0ed4>

![](img/ba71f11ec162d73d9272e5c1ad7140a1.png)

**本文将假设 Angular 的一些基本知识，因此它可以专注于从 Cosmic JS 请求和显示数据的特定任务*

# TL；速度三角形定位法(dead reckoning)

看看[的资源库](https://github.com/cosmicjs/angular-company-website)和[安装的 app](https://cosmicjs.com/apps/angular-company-website)

这个应用程序是关于什么的？

一个典型的公司网站仅仅是一组或多或少简单导航的页面。我们希望创建一个网站，允许用户创建任意数量的页面，并决定导航的结构，而不需要等待代码发布。为此，我们将依靠 Cosmic JS 并创建结构来支持这个项目。在本例中，我们的 bucket 将具有以下对象类型:

*   页数。这是一个简单的对象，没有额外的元字段。
*   导航。它将保存页面集合。
*   预设。这是一个带有属性列表的配置对象，可以帮助我们启动应用程序。它将保存主页和主导航的值，我们将使用它作为我们站点的菜单。

**使用 API**

既然我们已经定义了数据的结构，我们需要创建我们的 Angular 应用程序(我推荐使用 Angular CLI)并创建一个服务来调用 Cosmic JS API。看看从 Cosmic JS 请求一个页面对象有多简单:

*get a single page and save it to the stream to keep it cached for the session*

请注意，对于嵌套对象，Cosmic JS 不会返回完整的树，因此有时您需要连接调用来充分利用我们的服务，例如:当获取主预置时，您可能希望包含导航页面。

*this call will combine the result of getNavigation into the result of getPreset*

我们的 api 调用需要用一个 read 键进行认证，我们可以在每个方法中包含这一点，但是最简单的方法可能是创建一个拦截器。这将拦截所有 http 请求，所以我们检查它是否是一个 Cosmic JS 请求，并在必要的地方附加 read 键。

*intercept and append the read or write parameters*

**路由到页面**

在这一点上，我们的网站什么都不做，它需要做的第一件事就是知道如何加载它的第一页。我们需要为页面创建一个路由模块，并将其设置为在根上加载。一旦进入模块，在它自己的路由上，我们将为参数:slug 设置一个路由来加载页面组件。当没有指定路由时会发生什么？这将是我们最常见的场景，为此，我们设置了一条有警卫的空路线。

在匹配路线和加载组件之前，将执行一个守卫，所以我们将使用它从 Cosmic JS 中获取主要的预设，并将应用程序重定向到我们建立的主页。

*from Angular 7.1, canActivate can return either a boolean or an UrlTree*

**组件**

现在我们有了一个将加载页面组件的路由配置，这是使用参数列表中的 slug 加载页面的方法:

*now we can use the page object on our template*

这样，我们就有了通过 url 导航的方法，但是我们也应该加载一个菜单，这样用户就可以看到可用的页面。为此，我们将在核心模块上添加一个菜单组件，因为它将在整个应用程序中共享。我们的菜单将由导航列表、徽标和公司名称组成；所有这些都是在预置上定义的，所以菜单会有这样一个调用:

*note that we specifically ask to include the navigation by adding “true” as a parameter to the method*

所有这些将允许你使用 Angular 和 Cosmic JS 快速创建一个网站，剩下要做的就是创建模板并扩展配置以满足你的需求。