# 如何用托管服务实现 Azure 和 AWS 之间的 VPN 连接？

> 原文：<https://medium.com/hackernoon/how-to-connect-between-azure-and-aws-with-managed-services-4b03ec334e8a>

![](img/4ee3029ae05b49a22b6169f25e42fad8.png)

# 介绍

2019/2/6 IKEv2 兼容新闻在 AWS 与站点到站点 VPN 上出现。

AWS 站对站 VPN 现支持 IKEv2
[https://AWS . Amazon . com/about-AWS/whats-new/2019/02/AWS-Site-to-Site-VPN-Now-Supports-ike v2/](https://aws.amazon.com/about-aws/whats-new/2019/02/aws-site-to-site-vpn-now-supports-ikev2/)

连接 Azure 和 AWS 的要点是 AWS 只支持 IKEv1。
这一次，通过支持 IKEv2，实现双向连接成为可能。

但是，有注意事项。
BGP 无法使用(根据设置可能会使用)。

连接配置图如下。

![](img/37d3e63ab25f4dfd5329e9b47c880fc0.png)

程序如下。
Azure 侧
1、创建虚拟网络
2、创建网关子网
3、创建公共 IP
4、创建虚拟网络网关

AWS 端
5、创建 VPC
6、创建子网
7、创建互联网网关(可选)
8、静态创建客户网关
9、创建虚拟专用网关
10、静态创建 VPN 连接
11、下载配置文件

Azure 端
12、创建本地网络网关
13、创建连接

AWS 端
14、添加一个虚拟专用网关到路由表

选项
蔚蓝这边
15 设置两个连接

下面，我们将一步步讲解。

# 1、创建虚拟网络

创建虚拟网络。
天蓝色一侧的线段为 10.0.0.0/16。

![](img/afab3686188d11ce401bf2f9772ab3c8.png)

# 2、创建网关子网

创建网关子网。
从虚拟网络屏幕打开子网，并单击网关子网以创建它。

![](img/c93ba02002a7f1c98e274d0733fd349f.png)

使用另一个 CIDR 创建在 1 中创建的子网。

![](img/06d2467ff09465caa11140b8710285ab.png)

确认它已创建。

![](img/a9c01fb077cb7b5e0be6e58eb263557c.png)

# 3、公共知识产权的创造

创建一个公共 IP。

![](img/1ec4014cb346d3431717e18a5064a524.png)

# 4、创建虚拟网络网关

创建虚拟网络网关。

![](img/e3ac9c95ffba3857376573cecdf5194c.png)

# 5、VPC 的创作

创造 VPC。
AWS 侧的段是 192.168.0.0/16。

![](img/f3c2d15a5885e8b3bb9a81aafba59b8b.png)

# 6、创建子网

创建一个子网。

![](img/3902f56032b816824f7102667a34efef.png)

# 7、创建互联网网关(可选)

创建互联网网关(可选)。

![](img/119d7fa008241129142e4e271e52a607.png)

将互联网网关连接到 VPC。

![](img/8f227688d151b9d14ac85ab5b7fafc18.png)

在路由表中将 Internet 网关指定为 0.0.0.0/0。

![](img/877e49df1a2b81b21438223d3174a743.png)

# 8、静态创建客户网关

从 Azure 的虚拟网络网关检查 IP 地址。

![](img/b07cf9da91de9f52eb4719f2baa6d2e0.png)

静态创建客户网关。在 IP 地址中输入上一部分确认的 IP 地址。

![](img/4d41d338310e08b4f973fa1189e7d524.png)

# 9、创建虚拟专用网关

创建虚拟专用网关。

![](img/566f69d33b2236ed97f693fca6906bb0.png)

# 10，静态创建一个 VPN 连接

静态创建 VPN 连接。将 Azure 端的子网添加到前缀中。

![](img/7ee9eaa2180f374aa40ebfd81f7466cb.png)

# 11、下载配置文件

下载配置文件。选择是通用的。

![](img/cacdfaab29fbca34f392431feb8a45af.png)

检查以下内容。
IPSec 隧道# 1
预共享密钥
外部 IP 地址:
-虚拟专用网关

# 12、创建本地网络网关

创建本地网络网关。
对于 IP 地址，设置上述识别的 IP 地址(虚拟专用网关)。
在地址空间中，输入 AWS 端的 VPC 段。

![](img/7c9ca48a61dcbbbd9a65539027c68108.png)

# 13、创建连接

从虚拟网络网关连接添加。

![](img/1a8e4de22120fbfa2dc716a8d0f8133c.png)

输入确认的公共密钥(预共享密钥)。

![](img/662721c35a27006d959f973daf279da1.png)

确认它已连接。

![](img/3931b8db42e458b200dbe1e27568f7a7.png)

# 14、将虚拟专用网关添加到路由表中

向路由表添加虚拟专用网关。

![](img/bc53eae389dd0413c93c196d2a5da674.png)

# 15 设置两个连接

单独创建一个本地网络网关。
此时，检查下载文件的 IPSec 隧道# 2 并输入。
创建后，指定您创建的本地网关并创建连接。

通过建立两个连接，即使一个连接过期到某个时间签名，通信也会继续。

# 沟通确认

Azure，AWS 来确认你们可以互相交流。

![](img/e74eac7692c5bc74a2720f5ee3037f9f.png)

所有的工作都用这个完成了。

# 吞吐量

我使用 iperf 粗略测量了吞吐量。

Azure:标准 D2s v3 (2 个 vcpu、8 GB 内存)
AWS:m5.large (2 个 vcpu、8 GB 内存)

Azure→AWS

```
------------------------------------------------------------
Client connecting to 192.168.0.5, TCP port 5001
TCP window size: 45.0 KByte (default)
------------------------------------------------------------
[  3] local 10.0.0.4 port 51160 connected with 192.168.0.5 port 5001
[ ID] Interval       Transfer     Bandwidth
[  3]  0.0-10.0 sec   659 MBytes   553 Mbits/sec
```

AWS→Azure

```
------------------------------------------------------------
Client connecting to 10.0.0.4, TCP port 5001
TCP window size: 45.0 KByte (default)
------------------------------------------------------------
[  3] local 192.168.0.5 port 50116 connected with 10.0.0.4 port 5001
[ ID] Interval       Transfer     Bandwidth
[  3]  0.0-10.0 sec   759 MBytes   636 Mbits/sec
```

我想知道它是否通常比租用线路服务更快。

# 摘要

我能够确认 AWS 可以通过支持 IKEv2 直接连接 Azure。
到目前为止，需要准备 Windows 服务器等。到 VPN Azure 和 AWS。
通过使 VPN 仅由被管理的服务建立，没有必要建立虚拟机，
减少了管理的需要，因此我们几乎不关心操作。我相信 Azure 和 AWS 可以在一个相互管理的环境中连接起来，以便将来可以用于各种目的。
我认为 Azure 擅长 Azure，AWS 擅长使用 AWS，DR 适合使用并且我认为有很多有用的价值。
另外，多云将继续进行！！

我很高兴，因为这是我几年前就想要的功能！

原文内容(日文):[http://level69.net/archives/26362](http://level69.net/archives/26362)