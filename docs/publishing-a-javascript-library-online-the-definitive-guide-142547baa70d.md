# 在线发布 JavaScript 库:权威指南

> 原文：<https://medium.com/hackernoon/publishing-a-javascript-library-online-the-definitive-guide-142547baa70d>

![](img/65c70b70d4f7e6073948f78806c0b421.png)

Background by Unsplash

这是我之前[指南](https://hackernoon.com/converting-your-js-boiler-plate-into-npm-modules-the-definitive-guide-3dfa0f9f0a9c)的延续。参考它以了解如何向 npm 发布代码

本文主要关注发布在**浏览器**中使用的代码。

本指南将分为两个部分:

1.  发布浏览器-本机代码🌐
2.  转换在浏览器中使用的 npm 模块🔃

对于这两种情况，我们最终都会将代码部署到 **npm** 来利用免费 CDN [unpkg](http://unpkg.com) 的力量

## 1.发布浏览器-本机代码🌐

这个容易些。由于代码已经是浏览器可用的格式，所以只需要通过`<script>`标签将它包含在 HTML 代码中。

这将使它的变量和函数自动对浏览器可用。

要将其发布到 CDN:

1.  使用`npm init -y`为存储库初始化一个`package.json`。
2.  通过此处给出的步骤将其发布到 NPM:[发布步骤](https://hackernoon.com/converting-your-js-boiler-plate-into-npm-modules-the-definitive-guide-3dfa0f9f0a9c)
3.  发布到`npm`时，根据您提供的库名导航到`unpkg.com/repository-name`。这是从 npm 自动生成的 CDN。
4.  如果您获得了想要的文件，您可以通过`<script>`标签在浏览器中使用它。如果您获得了一个文件目录结构，只需导航到所需的文件，并通过`<script>`标签将其包含在您的前端。
5.  通过名字直接访问你的变量和函数

## 2.转换在浏览器中使用的 npm 模块🔃

**browser fiy**将你的 js 文件的所有依赖项捆绑到一个文件中，并将一个**变量**导出到`window`以便在浏览器中使用。

1.  安装浏览器`npm add browserify -g`。
2.  要生成浏览器可用的文件，请执行以下操作

`browserify ./main.js -o ./bundle.js -s variableName`

这里，`main.js`是您想在浏览器中使用的文件，

`-o`标志用来定义输出文件的位置，并且

`-s`标志用于定义导出到浏览器的变量名

1.  发布到 npm，包括捆绑的 JS 文件。导航至`unpkg.com/repository-name`以访问为您的回购生成的 CDN。
2.  如果你得到了你想要的文件，你可以通过`<script>`标签在浏览器中使用它。如果您获得了一个文件目录结构，只需导航到想要的文件，并通过`<script>`标签将它包含在您的前端。
3.  通过您导出的变量名访问您的代码。

就这样，你成功发布了你的 JS 库🎉！

拍拍自己的背，这是你应得的😁！

取得联系！！

[Github](https://www.github.com/akash-joshi)LinkedInTwitter