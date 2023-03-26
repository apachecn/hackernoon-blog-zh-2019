# 在 webpack 中使用 HTML 组件

> 原文：<https://medium.com/hackernoon/using-html-components-with-webpack-f383797a5ca>

用 JavaScript 替换 PHP

PHP 的一个强大特性是它的`<?include?>`特性，它允许你将 HTML 文件导入到服务器本身的 DOM 中。然而，对于许多人来说，使用 PHP 并不是一个可行的解决方案——尤其是前端开发人员。GitHub Pages 不支持 PHP 这样的服务器端语言；因此，我们必须找到一个更好的解决方案。

我们希望在客户端使用 JavaScript 来在单页面应用程序中使用 HTML 模块。webpack 通过提供`html-loader`来拯救我们。HTML 加载器允许我们将 HTML 代码注入 JavaScript，然后 JavaScript 可以将 HTML 组件动态注入主 DOM。

![](img/d20d910b08cbf73fdd3a417fead1441b.png)

Photo by [Max Nelson](https://unsplash.com/@maxcodes?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我把这些 HTML 模块称为“静态组件”。它们没有内在状态，也不像 React 组件那样包含任何 JavaScript 代码。这使得它们和普通的 HTML+普通 JS 一样快，并且增加了模块化的便利。

# 安装 HTML 加载程序

假设您安装了 webpack，

```
npm install --save-dev html-loader
# or
yarn add -D html-loader
```

现在您必须将它添加到您的配置文件中:

```
// webpack.config.jsmodule.exports = {
  module: {
    rules: [
      {
        test: /\.html$/,
        use: [
          {
            loader: 'html-loader',
            options: {
              minimize: true,
              interpolation: false
            }
          }
        ]
      }
    ]
  }
};
```

## 向项目中添加静态组件

我喜欢为我的 HTML 模块创建一个单独的文件夹。

```
static-components/
  registration.html
  header.html
  footer.html
```

# 将 HTML 注入 JavaScript，然后注入 DOM

现在，您可以将 JavaScript 文件中的 HTML 模块作为字符串导入。这些字符串可以被设置为容器的`innerHTML`属性。

```
// main.html
<html><head>...</head>
<body>
  <div id="root"></div>
  <script src="index.js"></script>
</body>
</html>
```

现在，在您的代码中:

```
// index.jsimport header from './static-components/header.html'
import footer from './static-components/footer.html'document.onload = function() {
  document.getElementById("root").innerHTML = header + footer;
}
```

# 类比反应

HTML 模块类似于 React 组件，只是它们是“静态”的。它们不会随着时间而改变，也没有状态。

但是，您可以创建控制器，每当您在 DOM 中添加和删除 HTML 时，控制器就会被加载和卸载。这些控制器可以添加 DOM 事件处理程序来制作交互式应用程序。

```
// StaticController.jswindow.staticControllers = {};// HeaderController.jsimport header from 'path/to/header.html'
window.staticControllers.headerController = {
  initialRender: function() {
    return header;
  },
  onLoad: function() {
    document.getElementById('id').onclick = ...;
  }
};
```

然而，与 React 相比，静态组件几乎没有开销，因为它不再渲染。此外，您不需要在站点中加载数百千字节的数据。

## 结合 JSX 和 HTML 模块

一个非常好的特性能够将长 JSX 从实际的 JavaScript 文件中分离出来，对吧。唯一的障碍是在运行时评估 JSX 的对象符号。当然，通天塔开发者将不得不添加该功能。

然而，如果有一天我们做到了这一点，你可能会在单独的标记文件中编写 JSX！！！

延伸阅读:

*   [HTML 画布的完整概述](https://medium.freecodecamp.org/full-overview-of-the-html-canvas-6354216fba8d)
*   [非整数值如何存储在浮点型中(以及它为什么会浮点型)](https://medium.freecodecamp.org/how-non-integer-values-are-stored-in-a-float-and-why-it-floats-902effacbfb9)
*   [移除 JavaScript 中的循环依赖](/@sukantk3.4/circular-dependencies-in-javascript-34183fc2720)
*   [如何在多台设备上同步您的应用程序](https://medium.freecodecamp.org/how-to-synchronize-your-game-app-across-multiple-devices-88794d4c95a9) (Android)
*   [如何使用 Firebase 构建多人安卓游戏](https://medium.freecodecamp.org/match-making-with-firebase-hashnode-de9161e2b6a7)