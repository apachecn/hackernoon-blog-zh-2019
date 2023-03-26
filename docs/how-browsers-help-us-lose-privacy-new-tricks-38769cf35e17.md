# 浏览器如何帮助我们失去隐私(新招)

> 原文：<https://medium.com/hackernoon/how-browsers-help-us-lose-privacy-new-tricks-38769cf35e17>

![](img/209336990071f74f219e1e158616c28a.png)

您知道吗，我们在浏览器的帮助下从互联网下载的每个文件都在其扩展属性中(在 [inode](http://www.linfo.org/inode.html) 中，而不是在文件本身中)存储了一个完整的网络链接，该链接指向它的下载位置。刚发现的时候很激动，想立刻冲上去拯救世界。后来，研究了一下这个问题，阅读了“提交”和“注释”，我明白这个特性至少在 10 年前就被引入到 GNU / Linux 内核中了。它不仅被浏览器使用，还被非常流行的 [Wget](https://www.gnu.org/software/wget/) 实用程序使用，这个特性在 Linux 中几乎被认为是一个标准，后来证明在 MacOS 中也是如此。

所以，是的，在任何 Chromium 兼容浏览器的帮助下下载的每个文件都会在扩展文件属性(隐藏在文件系统的深处)中写入以下类型的信息:

【我@ ars:~ $ getfattr-d hackread-logo.png

*#文件:hackread-logo.png*

*user . xdg . origin . URL = " https://www。******.com/WP-content/uploads/2015/12/hack red-logo . png "*

*user . xdg . referrer . URL = " https://www。* ******。com/WP-content/uploads/2015/12/hack red-logo . png "*

同时，如果你在匿名/私人浏览模式下下载文件，所有浏览器诚实地不写任何东西。

至于火狐和苍月，这些家伙不做这种事情。我没有检查浏览互联网的所有程序，但我认为所有基于 Chromium 的程序都可以做到，其余的很可能做不到。

在 macOS 中，这个功能在 Chrome 和 Safari 中也可以使用。我没有在 FF 中检查它，但我猜想，在 Linux 中，为 macOS 设计的漂亮的 Firefox 也被剥夺了这种“特权”。据解释，该特征用于确定文件下载的位置，并且由于 macOS 中该属性的存在，可以向用户显示关于危险的警告。嗯…奇怪…

我们正在顺利地向微软转移。Windows 绝对是平庸的，用明目张胆的玩世不恭为所有人做间谍。使用 NTFS 的 Windows 有很多隐藏的漏洞，可以在文件的扩展属性中写入任何内容。它们被称为**流**。他们说，病毒喜欢在这些流中写入一些被文件系统隐藏的东西，因为所有其他程序很少使用它们。

我说不出任何关于 Windows 的确切信息，因为我已经有 10 年没有使用 Windows 了。当我第一次接触 Win 10 时，我没有注意到谷歌 Chrome 将链接写入扩展的 NTFS 属性，就像它在 Linux 和 MacOS 中一样。乍一看，它看起来不错，但也有人说，流有几层，并不是所有的都可以直接访问。

我请你提前拒绝类似这样的评论:“那又怎样，我没什么好隐瞒的”。总的来说，作为一条规则，我们都没有什么可隐瞒的。

总而言之，我可以说，在所有三种流行的操作系统中:Linux、MacOS 和 Windows——浏览器，主要是 Chrome 和基于 Chrome 的浏览器，写下下载文件的源的路径(在文件系统级完成)。

在 Linux 中，基于 Google Chrome 的浏览器在字段中写入原始文件位置的完整链接: *user.xdg.referrer.url* 和 *user.xdg.origin.url.* 这是使用 Linux 内核的特性完成的，这些特性早在 2002 年就以扩展文件属性的形式出现在 Linux 内核中，一般名称为 [xattr](https://en.wikipedia.org/wiki/Extended_file_attributes) ，几乎所有流行的文件系统中都有这些特性:ext2、ext3、ext4、ReiserFS、XFS、ZFS、BTT 您可以使用内核选项禁用对扩展属性的支持，但它用于容器化(名称空间的属性)，SELinux。

这些字段被一些程序使用，例如，文章中提到的 Chrome、Chromium、Opera 浏览器，在某些情况下被 Firefox 和 Wget 控制台实用程序使用，根据维基百科，还有:curl、Dropbox、Beagle、OpenStack、Swift、KDE。

最新版本的 Wget 1.20 不写任何东西(我有 1.19——正在写)。显然，开发人员之间会定期进行讨论——在下一个版本中包括或不包括这样的特性。

一般来说，在 Linux 中对扩展属性字段的创建和管理没有限制。您可以将任何想要的信息写入扩展属性。在内核级别，名称的限制是 255 字节，字段值的限制是 64KB。

MacOS 和 Windows 的情况与它们的 FS HFS +和 NTFS 类似。唯一的区别是这些属性被不同地调用、隐藏和显示。

在 Ubuntu 18.10 中，您将不会在默认的 Nautilus 文件管理器中看到它们，直到您安装了 attr 包，该包包括用于查看和设置 attr 属性的实用程序(分别为 getfattr 和 setfattr)。

在 Mac 电脑上，您可以在 Finder 中或使用内建的 xattr 来查看它们。

他们说，在 Windows 中，只有文件属性中隐藏链接的标志是可见的。

有许多选项可以避免为一个或另一个操作系统写入信息或删除指向文件的链接。例如，对于 Linux，我喜欢挂载到一个文件夹中的简单想法，浏览器文件将被下载到另一个文件系统——没有默认的属性写入标志。

一个好主意是在挂载阶段禁用/etc/fstab—*mount-o nouser _ xattr*中的属性

还有一种方法是这样清洁:

Linux:

*setfattr-hx user . xdg . origin . URL 文件名*

MacOS:

*xattr -c -r ~ /下载*

窗口:

*get-child item " D:\ Downloads \ " | unblock-file*

向文件中添加任何字段并用值填充它们的能力很久以前就出现了(在 2002 年的 Linux 中)，最近才开始用于编写下载源的完整路径。2015-2016 年后的某个时候。在 Windows 中，只有在 Windows XP 中“引入”了该功能。

我将重复一个简单的想法:这种功能的存在并不奇怪，奇怪的是它悄悄地出现了，而且无处不在。在我看来，这样的问题必须由我们——用户——来控制和决定。关心自己隐私的普通用户应该使用 [VPN 服务](http://www.vpngeeks.com)，并且总是在浏览器中开启匿名模式。