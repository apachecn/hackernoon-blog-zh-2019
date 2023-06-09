# 零知识证明:外行介绍

> 原文：<https://medium.com/hackernoon/zero-knowledge-proofs-a-laymans-introduction-e9c1f50208e8>

![](img/05d4142c468bcaeff830ea55ea870249.png)

Alex 花了更多的时间写了一篇文章，介绍什么是 ZK-斯纳克。如果你觉得有用，我们鼓励你阅读并留下评论。

在过去的几个月里，我每天都沉浸在一个激动人心的前沿世界中:零知识(ZK)证明。这是一个具有挑战性的领域，正因为如此，我非常喜欢探索这个领域。

今天，我将更多地谈谈我的这个追求。最终目标是实现一个实用的原型， [*Artos*](https://www.artos.io/) (我的雇主)可以在他们的 Aventus 协议票务解决方案中使用。我现在不关心最终用途，只关心 ZK 技术本身:它是什么，我们能用它做什么，以及一些可用的工具。

# 零知识证明

以我的经验，人们听到这个名字都会微笑。也许我也是，我不记得了，那是很久以前的事了。但是听到类似“我是零知识专家，我对此一无所知”的嘲笑并不罕见。

零知识证明是一个令人惊讶的反直觉的密码概念，首先由 Goldwasser、Micali 和 Rackoff 在一篇介绍了 [*交互式证明系统*](https://people.csail.mit.edu/silvio/Selected%20Scientific%20Papers/Proof%20Systems/The_Knowledge_Complexity_Of_Interactive_Proof_Systems.pdf) 的论文中提出。文献是广泛的，如果你想了解更多现代和实用的参考和实现，你会发现 [*ZKP 科学*](https://zkp.science/) 非常有用。在这个帖子里，我会停留在一个很高的层次，忽略大部分。虽然我之前已经写了两篇关于以太坊中 SNARKs 的文章，但是我觉得我需要恰当地介绍这个概念。

# 那么什么是 ZK 证明，你能用它们做什么呢？

零知识证明是一种不用展示就能让另一个人相信你知道某事的方法。不仅如此，这样的人除了知道你有秘密之外，对你的秘密一无所知。给它一个世俗的眼光，假设你肯定知道某个特定配方的胡萝卜汁会阻止脱发。用一个零知识证明(如果在这种情况下*可以*应用的话)你绝对会*让*其他人相信你知道那是一个事实，但是那个人对如何制作这样一个公式一无所知。

公平地说，你可能也不知道，你所能证明知道的就是这种关系的存在，而且你可以让其他人相信这一点。因为你是以零知识的方式进行证明的，下一个人*不能*利用相同的证明来说服其他人(这在非交互式证明中可能是不同的，在非交互式证明中，证明的能力可能被转移给验证者):你不给出任何关于证明的知识，只给出被证明的陈述是真实的。这被称为**零知识证明**(一个语句)。

你可以更进一步。你可以向任何人证明你真的知道这个公式并且它有效，而不用透露任何有关它的信息。现在，不仅那个人相信你的陈述是真实的，她/他也相信你知道为什么，而且如果你愿意，你可能可以用它赚很多钱，但是他们仍然不知道关于那个神奇的秘密配方的任何事情。这将是一个**零知识*知识*** 的证明。

# 用例

这有几个用例，例如证明知道一个秘密密码；或者拥有你不想透露或给任何人的秘密物品(如泄露机密的照片、秘密法律文件)。在加密货币中，ZCash 使用它们来证明 ZEC 的私人交易已经正确发生，而没有透露任何细节(涉及的地址和数量)。这是一个相当强大的概念，但当然，你不能用它来证明什么。有一类精确的陈述，你可以为其开发一个 ZK 证明，在数学上用复杂性类 [*NP*](https://complexityzoo.uwaterloo.ca/Complexity_Zoo:N#np) 来描述。我不会在这篇文章中解释这是什么意思，但我可以推荐一些关于计算复杂性的[](https://www.amazon.co.uk/Computational-Complexity-A-Modern-Approach/dp/1316612155/ref=as_li_ss_tl?_encoding=UTF8&qid=1550787791&sr=8-1-fkmr1&linkCode=ll1&tag=alexpinto-21&linkId=afc30cbf7ca0930f11e41255281ba636&language=en_GB)*[*好书*](https://www.amazon.co.uk/Computational-Complexity-Perspective-Oded-Goldreich/dp/052188473X/ref=as_li_ss_tl?ie=UTF8&qid=1550787970&sr=8-2&keywords=computational+complexity&linkCode=ll1&tag=alexpinto-21&linkId=3ae4a333afbf65a9e315c8a86bb1e4ba&language=en_GB)[](https://www.amazon.co.uk/Computational-Complexity-Christos-H-Papadimitriou/dp/0201530821/ref=as_li_ss_tl?ie=UTF8&qid=1550787970&sr=8-12&keywords=computational+complexity&linkCode=ll1&tag=alexpinto-21&linkId=41c8983a8de48c2bc0af7b0eb6d2227d&language=en_GB)*。**

# **它是如何工作的？**

**在零知识系统中，用户可以扮演三种角色。我将称他们为**创造者**、**证明者**和**检验者**。某些类型的 ZK 系统不需要可信的创建者。ZCash 有，因为这个角色有重要的安全考虑，所以我将使用这些系统作为例子。**

# **创造者**

**创建者是建立系统的实体(例如，可以是一个人或一群人)。它首先决定了系统要证明什么。零知识系统的单个实例只能提供这种特定类型的证明。例如，在 ZCash 中，这相当于定义了确定单个交易已经正确形成的标准。ZCash 证明只证明一个交易是正确的。**

**这些系统的重要之处在于，在创建这个系统的过程中，会产生一些随机性，这些随机性应该永远保密。如果一个行为不端的人抓住了这种随机性，他们可以伪造证据，破坏整个系统的有效性。这就是为什么 ZCash 特别强调要创造一个非常公开的、[](https://cryptoslate.com/zcash-powers-of-tau-privacy-ceremony/)*[*仪式*](https://www.zfnd.org/blog/powers-of-tau/) ，让全世界相信他们已经摧毁了用来建立 ZCash 的 ZK 体系的随意性。系统设置的结果是创建两个密钥:证明密钥和验证密钥，根据用户的角色将它们分发给用户。***

# **证明人和验证人**

**日常生活中的两个主要角色是另外两个。证明者接收证明密钥，并且能够创建在设置中定义的类型的特定证明。验证者接收验证密钥，并且可以验证某个证明是正确的。如果一个用户用不同的密钥使用这些角色中的每一个，你可以让他同时成为证明者和验证者:毕竟，ZK 的目的是让证明者相信证明者知道而验证者不知道的事情。注意，所有证明者都知道相同的证明密钥，所有验证者都知道相同的验证密钥。系统的安全性不受此影响。**

**ZCash，以及我们在 Artos 中探索的用例，实际上使用了知识的 ZK 证明(从技术上讲，是论证而不是证明，但这与我们的讨论无关)。证明者证明拥有一些秘密信息。同样典型的是，每个证明都与一些公开已知的信息(验证者当然知道)相关，这些信息在不同的证明之间是不同的。因此，单个证明是由这个公共信息和只有证明者知道的私有信息定义的。引言中给出的例子是一个证明的例子，也许支持这一证明的更一般的陈述是“天然成分的某种配方可以治愈这种给定的疾病”。**

# **证据和验证**

**现在魔术开始了:证明者使用证明密钥及其实例参数(公共和私有输入)创建一个证明。这个过程是随机的:用同样的输入重复它会产生不同的证明。**

**然后，验证者获取该证明，验证密钥添加公共输入，并将所有内容提供给验证者算法，以决定该证明对于该输入是否有效。在这个过程中没有随机性:结果总是相同的(或者像加密证明中的典型情况一样，它可能是随机的，但失败的概率可以忽略不计)。**

# **ZK-斯纳克景观**

**这些系统是由椭圆曲线对支持的密码结构。有很多种不同的椭圆曲线，有些可以与配对一起使用，有些则不能，每种类型的曲线都可以以不同的方式参数化，从而导致特定的选择获得自己的名称。**

**ZK 系统也有不同的构造，特别是我们想在阿托斯使用的类型:ZK-斯纳克。最后，还有几个工具正在开发中，用于在不同的环境中生产这些系统。**

**自从我开始这项研究以来，我接触到了以下内容(要获得广泛的列表，请查看 ZKP 科学或参考资料 [*此处*](https://github.com/scipr-lab/libsnark#references) 或 [*此处*](https://github.com/scipr-lab/dizk#references) ):**

# **讽刺:**

*   **[*PGHR13*](https://eprint.iacr.org/2013/279.pdf) :最早也是最基本的 SNARKs 之一，启发了几个变体**
*   **[*bctv 14a*](https://eprint.iacr.org/2013/879.pdf):pghr 13 的改进，效率优化。最初在 ZCash 中实现的版本。本月发现了一个漏洞，但去年 6 月被 ZCash 发现。这篇论文已经被改正以避免它。**
*   **[*Gro16*](https://eprint.iacr.org/2016/260.pdf) :不受 PGHR13 启发的 SNARK 的别样设计。实现更小的证明和更快的验证。**
*   **[*GM17*](https://eprint.iacr.org/2017/540.pdf):gro 16
    Sonic 的一个进化(参考各自):SNARK 的一个新的不同的方法，避免了最近在 BCTV14a 中发现的漏洞。**
*   **[*Sonic*](https://eprint.iacr.org/2019/099.pdf)(mbkm 19):一个非常新的不需要可信设置的 SNARK。**

# **配对友好的椭圆曲线:**

*   **BN254 / BN_128:以太坊联盟选择的支持 ZK-斯纳克的椭圆曲线。这是一条 Barreto-Naehrig 曲线，长期以来是最有效的配对友好曲线。名字中的 128 是指这条曲线的名义安全水平。最近在攻击方面的改进已经将其降低到大约 100 位的安全性，但这仍然足够好了。另一个名称中的 254 是指表示该曲线中每个点的坐标所需的位数。**
*   **bls 12–381:z cash 团队针对 128 位安全级别设计的新曲线，以回应 BN254 的降级。**

# **工具:**

*   **[*Libsnark*](https://github.com/scipr-lab/libsnark) :一个 C++库，可以产生一个几乎端到端的 ZK 之旅，从编码要证明的语句到生成密钥和证明。**
*   **[*Libff*](https://github.com/scipr-lab/libff) :一个 C++库，用于大型代数群和代数域以及特定类型的配对友好椭圆曲线的快速计算。由 libsnark 使用。**
*   **[*py_ecc*](https://github.com/ethereum/py_ecc) :以太坊基金会的 Python 库，支持 SNARKs 的 BN128 和 bls 12–381，以及加密签名的 secp256k1。**
*   **[*Milagro*](https://github.com/apache/incubator-milagro-crypto) :涵盖多个平台的完整密码库，包括 JavaScript、Go、Swift、C 和 Java。不是专门针对 SNARKs 的。**
*   **[*DIZK*](https://github.com/scipr-lab/dizk):SNARKs 的 Java 库，镜像 libsnark 但只支持 Gro16。**
*   **[*zok rates*](https://github.com/Zokrates/ZoKrates):Rust/c++工具，提供了产生 SNARKs 的最佳端到端体验，因为它提供了一种简单的高级语言来指定要证明的语句。目前，仅支持 BCTV14a 和 GM17 Snarks。更多的人来了。**

**我们在 Artos 的目标是研究我们能为 Aventus 协议用例使用什么。我们的目标是几种类型的平台，包括链上和后端的验证，以及 Android 和 iOS DApps 中的验证生成服务器端。这带来了一系列的挑战，涵盖了各种各样的语言和技术。我们的目标是创建一个端到端的产品，可以很容易地允许选择不同的 SNARKs，不同的 curves，可能还有不同的库。这是一个雄心勃勃的项目，并不是所有的要求都是一成不变的，但它也非常激励人和令人满意。和这样尖端的技术一起工作是一种享受。**

**这是一个长系列的开始，在这个系列中，我将告诉你我们团队的成功，“血、汗和泪”，但也包括所有的欢呼。请继续关注更多信息。**

**如果你正在经历同样的困难，如果你有洞察力，即使你从这些帖子中学到了什么，请在评论中分享。如果你喜欢这个，点击下面的按钮，与你的网络分享。谢谢大家！**

**![](img/9c2558d79b8a408e9c5771eb58a52244.png)**

**Alexandre Pinto — Blockchain developer at Artos (Aventus Ecosystem Party)**

**Alex 是我们生态系统合作伙伴 Artos 的软件工程师，在区块链工程团队工作。他拥有 20 年的技术工作经验，完成了计算机科学博士学位和密码学博士后学位。作为研究的一部分，Alex [发表了关于 Kolmogorov 复杂性、密码学、数据库匿名化和代码混淆的论文](https://www.researchgate.net/profile/Alexandre_Pinto2)。**

**Pinto 还花了七年时间在 Maia 大学学院讲课，包括指导计算机科学和信息系统与软件学士学位课程。**

**这篇文章最初发表在[的博客](http://coders-errand.com/zero-knowledge-proofs-a-laymans-introduction/)上。另外，你可以关注亚历克斯的推特账户——[@ alexMirPinto](https://twitter.com/alexMirPinto)。**

**既然你在这里，我们希望你能在 [**电报**](http://bit.ly/2A343qG) **，**[**Reddit**](http://bit.ly/2MExLop)**，** [**推特**](http://bit.ly/2Rlv5Nf) **，** [**【脸书**](http://bit.ly/2WrJLhH) **，**[**Youtube**](http://bit.ly/2HE6fZw)**，****

**我们还为开源开发人员/票务开发人员和票务专业人员创建了一个小组，并邀请您在此加入— [区块链上的票务](http://bit.ly/2G65ohO)。**