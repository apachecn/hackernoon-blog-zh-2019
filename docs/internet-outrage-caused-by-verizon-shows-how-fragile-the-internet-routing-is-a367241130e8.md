# 威瑞森引发的互联网愤怒表明互联网路由是多么脆弱

> 原文：<https://medium.com/hackernoon/internet-outrage-caused-by-verizon-shows-how-fragile-the-internet-routing-is-a367241130e8>

## **为什么互联网基础设施易受 BGP 路由泄露的攻击**

![](img/0de7e6651dbf70f2d3e746f97b1209b3.png)

Source: [https://www.reddit.com/r/pics/comments/6f4725/helping_your_parents_with_a_tech_problem/](https://www.reddit.com/r/pics/comments/6f4725/helping_your_parents_with_a_tech_problem/)

6 月 24 日星期一，由于威瑞森错误，发生了 BGP 路由泄漏事件，引起了全球互联网的愤怒。BGP 的失败影响了 Cloudflare、亚马逊、脸书和其他公司。Cloudflare 在及时的[博客文章](https://blog.cloudflare.com/how-verizon-and-a-bgp-optimizer-knocked-large-parts-of-the-internet-offline-today/)中写道，宾夕法尼亚州的互联网服务提供商 DQE 在他们的网络中使用了 BGP 优化器，成为了许多通过威瑞森的互联网路由的首选路径。

“泄密应该在威瑞森停止，”[解释道](https://blog.cloudflare.com/how-verizon-and-a-bgp-optimizer-knocked-large-parts-of-the-internet-offline-today/) Cloudflare 网络软件工程师 Tom Strickx，“威瑞森缺乏过滤导致这成为一个重大事件，影响了许多互联网服务，如[亚马逊、Linode 和 Cloudflare](https://twitter.com/atoonk/status/1143143943531454464) 。”

就在上个月，甲骨文互联网分析总监 Doug Madory 在一篇博客文章[中详细描述了发生在 6 月 6 日星期四的 BGP 路由泄露事件。在大约两个小时的时间里，大量的欧洲互联网流量通过中国电信自己的服务器进行了重新路由。在瑞士数据中心托管公司 Safe Host 发生 BGP 路由泄漏后，互联网流量被重新路由。其内部路由表中超过 70，000 条路由(包括来自欧洲的大约 3.68 亿个 IP 地址)被泄露，随后被重新路由到中国互联网服务提供商(ISP)。](https://blogs.oracle.com/internetintelligence/large-european-routing-leak-sends-traffic-through-china-telecom)

发生的事情是，本来是给欧洲移动网络的互联网流量被中国电信的网络重新路由了。虽然有些人可能会认为，也许是因为他们使用的是中国品牌的手机；然而，安全问题的关键不在于你使用哪种手机，而在于运营商如何处理来自任何移动设备的大量互联网流量的路由。

在将来自欧洲的互联网流量重新路由到中国服务器的两个小时里，受影响的 3.68 亿个经过中国的 IP 地址可能会被中国监控。这意味着中国能够读取这段时间内所有的互联网流量。

五月份，发生了 BGP 劫持的[事件](https://www.manrs.org/2019/05/public-dns-in-taiwan-the-latest-victim-to-bgp-hijack/)，通过台湾网络信息中心(TWNIC)运行的公共 DNS 的流量被重新路由到巴西的一个实体，持续了大约三分半钟。

当前互联网路由基础设施的安全保护状况非常糟糕，如果我们继续忽视当今互联网安全的现实，BGP 路由泄露和劫持攻击将会一再发生。

## **什么是 BGP？**

上述事件被称为[边界网关协议(BGP)泄露或劫持攻击](https://en.wikipedia.org/wiki/BGP_hijacking)。为了理解什么是 BGP 泄漏，我们需要了解一些 BGP 的基础知识。[边界网关协议(BGP)](https://en.wikipedia.org/wiki/Border_Gateway_Protocol) 是用于整个互联网的协议，用于在互联网上的[自治系统(AS)](https://en.wikipedia.org/wiki/Autonomous_system_(Internet)) 之间交换路由和可达性信息。它是互联网上的路由器使用的一种独特的语言，用来决定数据包应该如何在网络中从一个路由器发送到另一个路由器。

根据边界网关协议(BGP-4)RFC 4271 的定义，自治系统(As)是“在单一技术管理下的一组路由器，使用内部网关协议(IGP)和公共度量来确定如何在自治系统内路由数据包，并使用自治系统间路由协议来确定如何将数据包路由到其他自治系统”。

它是一组在路由策略下运行的网络，并且拥有 IP 前缀。前缀是组合在一起的单个 IP 地址。安全主机、中国电信、美国电话电报公司和威瑞森各自是一个自治系统。自治系统之间的路由表使用 BGP 来维护。如果自治系统也是互联网服务提供商(ISP)，如上述欧洲流量重新路由案例，其中中国电信既是 as 也是 ISP，它可以将其管理的一些个人 IP 地址分配给客户。

## **什么是 BGP 路由泄漏？BGP 劫持？**

BGP 路由泄漏是指 BGP 被意外用于在 ISP 层重新路由网络流量。根据互联网工程任务组(IETF)的定义， [BGP 路由泄漏](https://www.rfc-editor.org/rfc/rfc7908.txt)是“路由公告超出其预期范围的传播。也就是说，从自治系统(AS)到另一个 AS 的学习到的 BGP 路由的通告违反了接收者、发送者和/或沿着前面 AS 路径的 AS 之一的预定策略

[BGP 劫持](https://en.wikipedia.org/wiki/BGP_hijacking)是一种网络安全事件，本质上是针对不同互联网路由实体(通常指自治系统号(ASN ))之间的互连进行的攻击。当自治系统宣布一个到它不拥有的 IP 前缀的路由时，它可以被添加到因特网上 BGP 路由器的路由表中。

简而言之，BGP 劫持是指攻击者在他们不拥有的网络中宣布一条路由。就是互联网流量被恶意改道的时候。攻击者有能力故意宣布他们不拥有的 ip 前缀的所有权。在大多数情况下，配置错误是 BGP 劫持的主要原因。一个关键的 BGP 保护机制是路由前缀过滤。它允许网络管理员过滤掉路由表中不需要的或非法的前缀。BGP 劫持很像是有人改变了交通标志并改变了交通路线。

## **区块链作为去中心化的互联网基础设施**

BGP 面临的主要挑战之一是它天生就建立在信任的基础上。换句话说，BGP 天生容易受到非法路由传播的攻击，因为它在设计时没有内置安全性。路由通告通常受到 ISP 的信任。因此，互联网很容易传播各种不正确的路由。因为没有内置机制来确保 as 确实拥有它们所通告的前缀。

该协议不包括安全机制，而且很大程度上基于网络运营商之间的信任，这一事实让很多人担忧。攻击者能够以很高的成功率影响 BGP 使用的路由表。毫不夸张地说，世界还不能完全防止 BGP 劫持的发生。许多安全研究人员越来越关注 BGP 漏洞以及当前全球路由系统缺乏改进。

此外，由于当前互联网路由基础设施的集中特性，它容易受到 BGP 劫持攻击和许多其他类型的攻击。这就是为什么我们想回到创建一个新的互联网基础设施，由开放、透明、分散的点对点网络来管理。多亏了区块链的技术，这才成为可能。

与传统的具有中央服务器的集中式路由网络基础设施不同，充当分散式 web 基础设施的区块链可以消除对中央服务器的需要，消除作为第三方中介的认证机构，并且通过其植根于系统的密码设计来提供强信任保证。

构建新的分散式 DNS 模型的一些参与者，如[握手](https://handshake.org/)、[以太坊名称服务(ENS)](https://ens.domains/) 和[二极管](https://diode.io/)建议我们从分散的角度来看待网络基础设施。例如，Diode 正在为资源受限的设备开发基于区块链的[分散式 PKI](https://hackernoon.com/decentralized-public-key-infrastructure-dpki-what-is-it-and-why-does-it-matter-babee9d88579) 。他们的系统是用 [BlockQuick](https://diode.io/burning-platform-pki/blockquick-super-light-blockchain-client-for-trustless-time-19144/) 实现的，这是一种新开发的以太坊超轻客户端协议。

## 结论

本文研究了最近发生的几起 BGP 事件，解释了 BGP 的一些基础知识，什么是 BGP 劫持，并描述了区块链模型如何成为一个完整的、去中心化的互联网基础设施。我很高兴你发现[我的黑客午间故事](https://hackernoon.com/@yahsinhuangtw)很有趣。让我们保持联系！在推特上关注我:[https://twitter.com/YahsinHuang](https://twitter.com/YahsinHuang)