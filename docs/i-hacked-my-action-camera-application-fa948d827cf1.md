# 我黑了我的动作相机应用程序

> 原文：<https://medium.com/hackernoon/i-hacked-my-action-camera-application-fa948d827cf1>

![](img/0bec8477d77a391b2dd71ebde4b53a0a.png)

Photo by [Jakob Owens](https://unsplash.com/@jakobowens1?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# **背景故事**

我最近买了一个动作相机来拍摄一些很酷的水下东西。你可以通过 WIFI 连接到它，浏览 SD 卡的内容，或将当前视频传输到你的手机。相当酷的功能，应用程序不是最好的，但在我的手机上运行良好。我想在我的平板电脑上试试，这就是问题所在。该应用程序需要定位来连接相机，但我的平板电脑没有 GPS，所以它无法做到这一点。

为什么位置对连接如此重要，为什么它还需要 GPS？我打赌没有它也能工作，让我们弄清楚。

# 黑客时间

目前我没有任何安卓设备，所以我决定从 [**APKpure**](https://apkpure.com) 下载。现在我有了 apk，该怎么办呢？让我们反编译它！

为了简单起见，我使用了一个在线 apk 反编译器。它像一个魔咒一样工作，但是现在我有一堆混乱的代码要处理。

![](img/033efdc9459f43b609389601a81d3e47.png)

我搜索了错误消息，发现了 *open_wifi_Permission* 字符串资源名称，它引导我找到了下面的代码片段。

如你所见，如果设备的 android api 版本大于或等于 23 (Android 6.0)，它将请求使用 GPS 的许可。如果我把数字 **23** 改成更大的整数**就好了。MAX_VALUE，**暂时就够了。该程序将跳过位置权限请求，但正如你可以在官方的 [Android 文档](https://developer.android.com/reference/android/net/wifi/WifiManager#getConfiguredNetworks())中看到的，WifiManager 需要**manifest . permission . access _ FINE _ LOCATION***权限，否则它将返回一个空列表。在逻辑的更深处，有*

```
*if(GlobalApp.m5984d(this.f5603e)) {
   //Checking permissions logic code
}*
```

*哪个实现看起来像这样？*

*它将总是返回 false，因为正如我所说的，我的平板电脑上没有 GPS。只需编辑它以返回常量 true，工作就完成了。*

*几周前，我读了一篇非常酷的关于 android 应用程序逆向工程的文章，作者描述了他如何反编译和重建一个应用程序，所以我从我的书签中找到了它*

*[](https://hackernoon.com/how-i-hacked-modern-vending-machines-43f4ae8decec) [## 我是如何入侵现代自动售货机的

### “拳打脚踢”他们最广泛的欧洲发行公司的捆绑应用程序。

hackernoon.com](https://hackernoon.com/how-i-hacked-modern-vending-machines-43f4ae8decec) 

我使用相同的步骤再次生成签名的 apk，但是设备无法安装该应用程序。

在几次失败的尝试后，我决定用 [**apktool**](https://ibotpeaches.github.io/Apktool/) 本地反编译 apk，而不是在线反编译。输出是没有 Java 源文件的小源代码。

> smali/baksmali 是 Android 的 Java VM 实现 dalvik 使用的 dex 格式的汇编器/反汇编器

如果我编辑这些 smali 源文件会怎么样？重建后可以让 app 运行吗？让我们试一试！我重建了 apk，签署并执行了 zipalign，它成功了！现在我必须在 smali 文件中找到"*m 5984d "***Java***方法。正如代码片段中的注释所说，该方法是由" *d "* **smali** *方法重命名的。有了这些信息，我可以在 GlobalApp.smali 文件中找到这段代码**

*看起来对开发者不太友好。在谷歌搜索了一下 smali 是如何工作的之后，我想出了这个解决方案，只是在 java 文件中返回 true。*

*我重复了 **apktool** rebuild、 **jarsign** 和 **zipalign** 命令，它终于生成了一个可运行的应用程序，可以在我的平板电脑上使用了。从现在起，我的平板电脑可以毫无问题地连接到摄像机了。**