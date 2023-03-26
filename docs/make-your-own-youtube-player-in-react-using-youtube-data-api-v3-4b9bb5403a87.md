# 使用 youtube 数据 API v3 在 React 中制作自己的 Youtube 播放器

> 原文：<https://medium.com/hackernoon/make-your-own-youtube-player-in-react-using-youtube-data-api-v3-4b9bb5403a87>

![](img/8eeecd9963fcfc9ebf8a2999e5555073.png)

Photo by [Rachit Tank](https://unsplash.com/@rachitank?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 那计划是什么？🤔

所以，在这个故事中，我将带你走过如何制作你自己的 Youtube 播放器的整个旅程。这很简单，我保证你会在一个小时内准备好你自己的播放器。太神奇了，不是吗？我们的最终结果将看起来像[这个](http://ytsearch.surge.sh/)。如果你浏览了这个链接，你可能会注意到一些事情。这些视频将通过他们提供的 API 直接从 youtube 数据库中获取。你们中的一些人可能已经看到了像超出每日限额这样的错误信息。哎呀，这意味着谷歌已经给了这个 API 有限的访问权限。因此，在创建项目时，请确保不要过多地使用 API，否则您将不得不等待第二天😃为你的配额充值。说够了。让我们制定一个循序渐进的计划。这里是 GitHub 库的[链接](https://github.com/dsc712/Youtube-Player-In-React)，如果你想边看边看代码的话。

## 项目的逐步规划

1.  首先，我们将从[谷歌开发者控制台](https://console.developers.google.com/)获取 API 密钥。
2.  我们将使用 facebook 的 [create-react-app](https://facebook.github.io/create-react-app/) 开始我们的项目。
3.  我们将安装 npm 包' [youtube-api-search](https://www.npmjs.com/package/youtube-api-search) '。
4.  对于 UI，我们会使用阿里巴巴著名的 React UI 库[蚂蚁设计](https://ant.design/)。
5.  对于响应式 iframe，我们将使用一点自举。
6.  最后，我们将对 youtube 播放器进行编码。
7.  最后，我们将使用 [surge](https://surge.sh/) 托管我们的项目。面向前端 web 开发人员的静态 web 发布工具。

# 让我们开始一些真正的行动💯

## 1.获取 API 密钥🔑

首先，让我们获得 API 密钥。访问[这个](https://console.developers.google.com/)并打开你的谷歌开发者仪表板，搜索 **Youtube 数据 API V3。**启用 API 并移至凭证部分，您可以在此处看到您的 API 密钥。您可以在项目中随时使用它。所以我们已经准备好了。

## 2.使用 create-react-app 启动项目😑

```
npx create-react-app myapp
cd myapp
yarn start
```

就是这样。我们有一些项目的启动代码。只要我们在终端点击`yarn start`。一个开发服务器启动了，我们的项目在`localhost:3000/`托管

## 3.让我们在项目中安装 youtube-api-search

```
$ yarn add youtube-api-search 
```

## 4.在项目中添加 antd React UI 库

```
$ yarn add antd
```

现在在`src/App.css`的顶部加上`antd/dist/antd.css`

```
@import ‘~antd/dist/antd.css’;.App { 
   text-align: center; 
 } 
```

现在我们准备在我们的 create-react-app 中使用 ant 设计。

## 5.添加自举 CDN 在(`public/index.html )` <头></头>

```
<head>
<link *rel=*"stylesheet" *href=*"https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css" />
</head>
```

酷，现在我们准备开始写一些 react 代码。

## 6.让我们最后编码 Youtube 播放器😃

现在让我们首先在**中添加 **API 键**。env** 文件。通常我们使用 npm 的 **dotenv** 包来完成。但是你可能想知道，我还没有告诉你安装 dotenv 包。实际上，这个包预装了 create-react-app。现在有一种在 create-react-app 中添加环境变量的特殊方法。您必须为您的环境变量加上前缀`REACT_APP_`。不然就不行了。此外，在本地主机上工作时，您需要`.env.development`文件，而生产构建`.env`文件将完成这项工作。

所以，让我们把这两个文件放在项目的根目录下。并添加变量，如

```
*REACT_APP_API_KEY*='your_api_key_jdfe39u3cmijsd'
```

## 为我们的应用程序规划组件

使用 App 作为根组件，我们将需要另外 4 个组件 **SearchBar** 、 **VideoDetail** 、 **VideoList** 和 **VideoListItem** 。因此，在项目的 **src** 目录中创建一个目录名 **Components** 。

```
└── src
 └── Components
      ├── SearchBar.js
      ├── VideoDetail.js 
      ├── VideoList.js
      ├── VideoListItem.js
  ├──App.js
   ...
```

让我们用一些生活❤ ️.来填充这些组件

## App.js(我们的根组件)

在这个组件中有一些事情需要注意。首先， **welcome()方法**。它用于向用户发出**欢迎通知**。为此，我使用了 antd 的**通知组件。第二个**，handleChange()方法**应用于 **SearchBar 组件**的 onChange 属性。一旦我们开始在 SearchBar 中输入内容，handleChange()方法就会触发。那就是一个**受控组件**是什么。从 handleChange()方法内部，我们调用了 **videoSearch()方法。**现在来看‘YouTube-API-search’包的使用。这个包返回一个函数，我们可以用它向 youtube API 发出请求。这个函数接受一个对象作为第一个参数，它有一个名为 key 的键，在这里您需要提供您的 API 键作为一个值。另一个关键字名为 term，它将搜索项作为值。第二个参数是回调。请求一完成，这个 API 就以 5 个对象的数组的形式返回数据，这些对象包含所获取的视频的细节。如果超过了每日限制，它会返回一个不同类型的对象，我已经在 try/catch 块中处理过了。这里我维护了两个状态变量，一个是包含 5 个视频数组的视频，它将显示在视频列表组件中，另一个是选择的视频，它将被播放。此外，我们还使用了一个布尔型的状态变量搜索。在 handleChange()方法中，我使用了几个 setTimeouts 来控制对 API 的请求数量。也可以通过[反作用去抖](https://www.npmjs.com/package/react-debounce-input)来完成。但是也很好。这就是我们的根组件。**

## SearchBar.js

这里，我使用了来自 antd 的[自动完成](https://ant.design/components/auto-complete/)组件。并且，我用 5 个视频填充这个自动完成组件，这些视频来自基于搜索词的 API。酷，我们已经准备好了一个带有自动完成功能的搜索栏。

## **视频列表. js**

在这里，我们的视频列表有两种状态，一种是当列表为空时，另一种是当列表充满视频建议时。你可以在上面的代码中看到，我是如何处理的。

## VideoListItem.js

## 视频细节. js

这里，我们简单地使用了一个 iframe 来播放 youtube 视频。并使用了 bootstrap 中的几个类来提高它的响应能力。

这就是我们准备好的 Youtube 播放器。

## 现在托管与激增

如果你以前没有使用过 surge，让我告诉你这是部署你的前端应用程序最简单的方法。检查此[出](https://surge.sh/help/getting-started-with-surge)开始与浪涌。

现在如何使用 surge 托管 create-react-app。首先，您必须创建一个生产版本。

```
$ myapp/ yarn build
```

这将为您创建一个优化的构建。一个构建目录将进入你的项目的根目录。现在，您可以使用 surge 托管您的应用程序。

```
$ myapp/ cd build
$ myapp/build/ surge
```

加油🎉 🎉 🎉 🎉。您已经部署了自己的 youtube 播放器。

如果你真的喜欢这个故事，请鼓掌几次(最多 50 次😃 ).更多类似的故事，请跟我来。