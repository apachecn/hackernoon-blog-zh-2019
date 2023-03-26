# 使用 VuePress 建立网站

> 原文：<https://medium.com/hackernoon/building-a-website-with-vuepress-9766fcd2830e>

VuePress 是一个简单的静态站点生成器，最初是为了支持大型项目的文档而创建的。然而，它可以做更多的事情，正如你将要学习的，我个人用它来生成[我的网站和博客](https://willwillems.com/)。

![](img/a92547aed0a311337dfa1e89b5c4bf98.png)

**为什么是 VuePress:**

*   让你的网站运行速度更快，搜索引擎更友好。
*   开发是超级简单和绝对的乐趣。
*   将你的内容与网站的其他部分非常清晰地分离开来。
*   提供了一个成熟的 Vue 实例，与普通开发没有什么不同。你仍然可以创建自定义路线，使用 Vue 插件，任何你可以正常使用的东西。
*   灵活的插件 API，例如，我用它在每次更新内容时自动为我的博客生成一个 RSS 提要。
*   使用 Markdown 和 markdown-it 及其广泛的生态系统。
*   能够在你的 markdown 中使用 Vue 组件。

# 这本指南是给谁的

*   你想创建一个由 Vue
    ❌驱动的静态网站→ *你为什么会在这里？*✅→*继续* ⤵️
*   你的网站是以内容为中心的(例如，不是一个网络应用程序)
    ❌ → *静态对你来说一般会更难，但你可能会检查*[*nuxt*](https://nuxtjs.org/)
    ✅→*继续* ⤵️
*   你的内容正在或将要被降价出售
    ❌ → *如果你已经在使用任何一种 jam stack*[*nuxt*](https://nuxtjs.org/)*现在可能是更好的选择*
    ✅ → *继续* ⤵️
*   你想为你的项目创建除文档之外的东西吗
    ❌ → *创建文档？继续看*[*vue press docs*](https://vuepress.vuejs.org/)*，他们对这个很在行。*✅
    继续*⤵️*

# 这将如何工作

简单描述一下这将如何结束工作:

*   您将拥有一个 git repo，其中包含您的网站内容(降价文件)和您的 VuePress 资产(主题、配置、图像等)。)放在名为`.vuepress`的单独文件夹中。
*   每次运行`npm run build`时，您创建的 markdown 文件将使用您的主题中可用的 VuePress 布局文件转换为 HTML 文件。另外定义的路线也将被预渲染。
*   一个文件夹`/dist`将被生成，里面装满了静态资产，可以由任何普通的 web 服务器提供服务，比如 NGINX 或 Apache。

# 设置

好了，我们开始吧，首先我们要做一些平常的事情:

*   创建项目文件夹`mkdir my-project && cd my-project`
*   用`npm init`创建一个有效的`package.json`文件
*   用`git init`初始化 Git

如果你不想复制所有这些，并想在 markdown 中演示 Vue 组件的使用，你可以在这里填写你的项目名称，复制下面的命令就可以了！

[![](img/2016d18819f65b2bd14137820a796740.png)](https://willwillems.com/posts/building-a-website-with-vuepress.html#command-generator)

现在让我们来看看 VuePress 的具体产品:

首先，用`npm i -D vuepress`添加 VuePress 包本身

> 警告
> 
> 在本指南中，我们使用的是 VuePress v1，因此根据您阅读本指南的时间，您可能需要安装`vuepress@next`来获得正确的版本。
> 
> 您还可以使用`npm install -g vuepress`全局添加 VuePress，以便能够在终端中使用`vuepress dev`之类的命令，但我们会将这些命令作为 NPM 脚本添加，因此这取决于您。

接下来，我们将把以下脚本添加到我们的`package.json`文件中:

```
"scripts": {
  "dev": "vuepress dev",
  "build": "vuepress build"
},
```

这使得我们*和其他人*能够以一种更通用的方式与项目进行交互，而不需要对`npm run dev`和`npm run build`进行任何设置或全局打包。

好了，创建项目文件的时间到了，我们首先在我们的项目文件夹根目录下创建一个`.vuepress`目录，并在那里放一个`config.js`文件。我们的项目结构现在应该是这样的:

```
my-project
|--.vuepress
|  |--config.js
|--node_modules
|--package.json
```

目录`.vuepress`将是这个项目的主干，包含除了实际内容本身之外的所有关于你的网站的文件，它将被放在项目文件夹的根目录下。

> 等等，那个奇怪的文件夹里是什么？
> 
> 基本上`.vuepress`目录将包含任何与 Vue 相关的内容:
> 
> -如果您想使用自定义主题，它会在`theme`文件夹中
> 
> -如果您想在 markdown 中使用 Vue 组件，您可以将它们放在`components`文件夹中
> 
> -如果您想添加自定义路线，您可以使用`enhanceApp.js`文件进行添加。
> 
> -如果您想调整您的降价设置，它们将在`config.js`文件中提供。
> 
> 这为你的 Vue 相关文件和它之外的所有东西(你的降价内容)创建了一个非常清晰的分离。

关于这个文件夹的更多信息以及它将包含的内容请查看。

# config.js 文件

正如你可能已经猜到的，这个文件将包含很多关于你的网站的配置和选项，包括你的主题的配置。最小的配置文件如下所示:

```
module.exports = {
  title: 'My Site',
  description: 'Welcome to my site.'
}
```

但是我们将通过指定编译后的资产应该结束的文件夹来稍微直接地扩展它:

```
module.exports = {
  title: 'My Site',
  description: 'Welcome to my site.',
  dest: "dist"
}
```

现在，您的静态资产将在您的根目录下的`/dist`文件夹中结束。默认情况下，这些文件会放在`.vuepress/dist`文件夹中，这个文件夹有点乱，不太通用。

# 创建我们的拳头内容

因为这个项目最初是面向文档创建的，所以唯一合乎逻辑的是，`index.html`文件的源将是项目根目录下的`README.md`文件。创建该文件，并在其中做一些简单的标记，如下所示:

```
# Use a title

And some more content

- A list
- for example
```

现在是收获我们劳动成果的时候了，如果你已经做了所有正确的事情，你现在应该可以执行`npm run dev`并检查你的第一个网页了！

如果一切正常，您可以使用`npm run build`生成新网页。

我的减价内容最终会出现在哪里？

VuePress 发送你编译的 markdown 文件的方式非常简单，除了`[README.md](http://readme.md)`会变成一个`index.html`文件，其余的内容可以在项目中它们各自的位置找到。例如:

`/about.md` → `/about.html`

`/posts/vuepress-rocks.md` → `/posts/vuepress-rocks.md`

`/posts/README.md` → `/posts`

# 使用静态资产

我明白 VuePress 团队为什么这样做，但我真的希望有一个选项可以改变当前位置，就像构建的输出目录一样。目前，您的静态资产应该放在`.vuepress/public`中，它们将在您的项目的根目录下可用。

# 就是这样！

干得好！您可能已经建立并运行了您的第一个 VuePress 网站，但是您可能想要创建一个自定义主题。没问题，[在这里阅读更多相关内容](https://willwillems.com/posts/write-a-custom-vuepress-theme.html)。

*原载于*[*willwillems.com*](https://willwillems.com/posts/building-a-website-with-vuepress.html)*。*