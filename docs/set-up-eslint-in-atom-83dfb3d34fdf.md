# 在 Atom 中设置 ESLint

> 原文：<https://medium.com/hackernoon/set-up-eslint-in-atom-83dfb3d34fdf>

![](img/f33255d0510a2322fee998f035f12ca0.png)

Atom Logo

所以你[在全球或特定项目中设置了 ESLint](https://hackernoon.com/a-simple-linter-setup-finally-d908877fa09) ，但是你不能让警告直接出现在 Atom 中？你来对地方了。

# 安装 linter-eslint +对等依赖项

`$ apm i linter-eslint linter linter-ui-default intentions busy-signal`

# 自定义 ESLint 设置

打开 Atom 的*偏好设置*查看你的*包*。在*社区包*下，打开`linter-eslint`的设置并进行以下更改。

## **不要指向全局**

*   勾选“未找到 ESLint 配置时禁用”(禁用)
*   取消选中“使用全局 ESLint 安装”(全局 ESLint)

## **输入** *(可选)*时，将一些规则静音

*   选中“键入时忽略可修复的规则”(自动修复)

## **修罗上保存** *(可选)*

*   选中“保存时修复错误”(自动修复)

*注意:您必须禁用/卸载任何其他棉绒包*

试着保存一个有 linter 问题的文件，你会看到一些警告。如果您遇到问题，请重新加载 Atom。

**🔥提示 1** :当太吵的时候关掉你的棉绒！打开“命令调板”(可以在“视图”中找到或使用 cmd + shift + P)，键入“启用”或“禁用”，然后按两次 Enter。

⌨️ **提示 2** : 我喜欢关闭“保存时修复错误”，所以我添加了一个键盘快捷键(⌘+⌃+s)来自动修复它们。为此，将以下内容添加到 Atom 的`keymap.cson`(单击菜单栏中的“Atom”并选择“Keymap”):

```
'atom-text-editor:not([mini])':
  'cmd-ctrl-s': 'linter-eslint:fix-file'
```