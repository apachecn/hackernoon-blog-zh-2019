# 设置 Airbnb 的 Linter(反应和节点)

> 原文：<https://medium.com/hackernoon/a-simple-linter-setup-finally-d908877fa09>

![](img/9df54709044eaf5e4abf7fecd1edb6db.png)

let’s get your code squeaky clean

linter 会标记代码中任何违反其预定义规则的内容(“样式指南”)。对于什么是“好的”JavaScript 代码，每个风格指南都有不同的看法。这个帖子用的是 [Airbnb](https://github.com/airbnb/javascript) 的风格指南；谷歌的也很受欢迎。

尽管一些开发者在全球范围内安装他们的 linter，这篇文章旨在通过为每个项目创建一个定制的配置来更加精确。

首先，让我们设置 ESLint(最流行的 linter 工具之一)。幸运的是，ESLint 内置了一个神奇的工具！🎩

```
$ cd PROJECT
$ npx eslint --init
```

这将询问一系列关于您想要的设置的问题。我的选择:

```
"To check syntax, find problems, and enforce code style"
(if React, "JavaScript, if Node "CommonJS"
(if React, "React")
(if React, "Browser", if Node "Node")
Use a popular style guide
Airbnb
JavaScript
Yes
```

## 我用的是 yarn，为什么它创建了一个`package-lock.json`文件！？😡

```
$ rm package-lock.json
$ yarn install
```

> 如果您使用了 Create React 应用程序，请跳到⚛️

## 我该怎么操作这个东西？🤔

将`eslint . &&`添加到`test`脚本的开头(即`package.json`中的`eslint . && mocha)`)。这将在您每次运行`npm test`时运行`eslint`(在命令行上)。或者，如果你想将`eslint .`与`test`分开，只需编写一个运行`eslint .`的`lint`脚本。您应该在根目录下运行这两个脚本，这样它就链接到了您的整个项目。

## 我的测试文件有这么多错误！😕

只需将`mocha`(或者另一个测试框架)添加到您的`.eslintrc.js`中:

```
{
  "env": {
    "mocha": true
  }
}
```

# ⚛️创建 React 应用

CRA 内置了 ESLint，但它是有意宽容的(不警告`console.log`、分号等)。).但是我们什么也不怕，让我们把这些“垃圾”从我们的项目中清理出来。💪

## 我的 React 应用程序中有这么多错误！？😰

1.告诉`eslint`忽略`serviceWorker.js`

```
$ echo src/serviceWorker.js > .eslintignore
```

2.将`jest`和`babel-eslint`添加到您的。`eslintrc.js`:

```
{
  parser: 'babel-eslint',
  env: {
    jest: true,
  }   
}
```

3.将所有`jsx`文件更改为扩展名为`.jsx`的文件

4.给你的`package.json`添加一个`lint`脚本，告诉`eslint`除了查看`.js`文件之外，还要查看`.jsx`文件:

```
"lint": "eslint --ext .js,.jsx ."
```

现在你可以运行`eslint`的自动修复了:

```
$ yarn lint --fix
```

从命令行(在根目录中)使用`$ yarn lint`或`$ npm run lint`来`lint`你的应用程序。CRA 的内置 linter 功能仍然相同，在运行应用程序时会出现在控制台和终端中。

> 相关:[在 Atom 中设置 ESLint 的说明](https://hackernoon.com/set-up-eslint-in-atom-83dfb3d34fdf)，这样你就可以直接在你的文本编辑器中看到警告

这有用吗？你被卡住了吗？让我知道！✌️