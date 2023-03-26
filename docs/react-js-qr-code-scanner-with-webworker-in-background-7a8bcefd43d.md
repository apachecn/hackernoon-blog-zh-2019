# WebWorker 如何挥动我们的 React.js 二维码扫描仪组件

> 原文：<https://medium.com/hackernoon/react-js-qr-code-scanner-with-webworker-in-background-7a8bcefd43d>

![](img/0cba3cedd758e61b4f256246d6367558.png)

JavaScript QR code scanner

大约一年前，我们开始开发基于 web 的移动应用程序，目标是在移动 Web 浏览器上运行。简而言之，移动应用是通过扫描二维码来定位特定仓库库存的工具。

这是我们应用程序的主要流程

1.  从请求浏览器 HTML5 权限打开设备本机摄像头