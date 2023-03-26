# Google 云引擎+ cPanel:设置主机名(解决方法)

> 原文：<https://medium.com/hackernoon/google-cloud-engine-cpanel-set-hostname-workaround-f7834ec19c06>

![](img/998f36e4025c74065dfccd6363080274.png)

Photo by [Benjamin Dada](https://unsplash.com/@dadaben_?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我猜你们中的许多人已经在使用基于云的托管解决方案和 WHM (cPanel)设置来为客户提供虚拟主机解决方案，或者为自己的项目托管网站和应用程序。

在许多情况下，我使用 DigitalOcean 来满足我的主机需求，但我在使用 Google Cloud Engine 时注意到了一个奇怪的问题，这是我在使用 DigitalOcean droplets 时没有发现的(后来发现，这也是少数基于云的主机解决方案的常见问题)。也就是说，在命令行上配置的主机名在重新启动后不会保留。

# 详细的问题。

大多数云托管提供商在启动时通过调用 *dhclient* 脚本来配置自动部署，但是这会错误地生成服务器的主机名。为了解决这个问题，大多数主机提供商使用额外的脚本作为解决办法，如谷歌云引擎上的*谷歌主机名称*。这在很多情况下是可以的，但是当 WHM/cPanel 等一些服务正在使用 *dhclient* 脚本来设置机器的主机名 *(WHM > Home >网络设置>更改主机名)时，上述问题就会出现。在某些情况下，这可能会导致“无效”cPanel 许可错误。*

# 解决方案编号:一。

如果您正在使用:

```
sudo hostname hostname.example.com
```

请改用:

```
sudo hostnamectl set-hostname hostname.example.com
```

在许多情况下，这将解决问题。但是如果那没有帮助，我已经为你找到另一个解决方法。

# 解决方案编号:二。

如果第一个选项不适合您，您可以设置一个“退出”钩子脚本，它将在引导时正确设置主机名。**记住**用自己的主机名替换*hostname.example.com*。

```
mkdir -p /etc/dhcp/dhclient-exit-hooks.d/ && echo -ne '#!/bin/sh\nhostname hostname.example.com\n/scripts/fixetchosts\n' > /etc/dhcp/dhclient-exit-hooks.d/set-hostname.sh && chmod +x /etc/dhcp/dhclient-exit-hooks.d/set-hostname.sh
```

该脚本将在*/etc/DHCP/DHCP client-exit-hooks . d/*目录中创建一个名为 *set-hostname.sh* 的脚本，并具有运行所需的权限。

*跟我上:中* [*米奴万·德沙里雅*](https://medium.com/u/a11268193e26?source=post_page-----f7834ec19c06--------------------------------) *，推特*[*@ twit minu*](https://twitter.com/twitminu)*或脸书*[*@米奴万*](https://facebook.com/minuwan) *。*