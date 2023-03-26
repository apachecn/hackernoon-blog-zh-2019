# 如何为 LXD 集装箱建立一个网桥

> 原文：<https://medium.com/hackernoon/how-to-set-up-a-network-bridge-for-lxd-containers-98e3e7d1f273>

![](img/f1654f40323f80a5f21d33d35e33907f.png)

我们的大多数 web 应用程序都运行在 LXD 容器中。对我来说，LXD 是 Ubuntu 服务器最重要的特性之一。有许多方法可以从外部访问 LXD 容器中的 web 应用程序。例如，您可以使用反向代理来控制对容器的访问。另一种可能性是建立一个网桥，这样容器就和容器主机(Ubuntu 服务器)在同一个网络中。在这篇文章中，我将描述如何为 LXD 容器建立一个网桥。

# LXD 集装箱的网桥

要在 Ubuntu 下建立一个网桥，你需要安装*网桥工具*:

```
$ apt install bridge-utils
```

然后就可以设置网桥了。

# Ubuntu 16.04

直到 Ubuntu 16.04 Ubuntu 使用 *ifupdown* 来设置网络连接设置。配置在 **/etc/network/** 下的文件中完成。一个简单的网桥(用于将容器接入主机网络)可能如下所示:

```
$ cat /etc/network/interfaces
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).source /etc/network/interfaces.d/*# The loopback network interface
auto lo
iface lo inet loopback# The main Bridge
auto br0
iface br0 inet dhcp
    bridge-ifaces enp4s0
    bridge-ports enp4s0
    up ip link set enp4s0 up# The primary network interface
iface enp4s0 inet manual
```

在本例中，网桥从 DHCP 服务器获取其地址。真实网卡 *enp4s0* 设置为手动模式，分配给网桥。

# Ubuntu 18.04

从 Ubuntu 18.04 [开始，网络计划](https://netplan.io/)用于配置网络连接。配置文件可以在 **/etc/netplan/** 下找到。桥的定义可能如下所示:

```
$ cat /etc/netplan/50-cloud-init.yaml 
# This file is generated from information provided by
# the datasource.  Changes to it will not persist across an instance.
# To disable cloud-init's network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    ethernets:
        enp3s0:
            dhcp4: no
    version: 2
    bridges:
        br0:
            dhcp4: no
            addresses:
            - 10.10.10.5/24
            gateway4: 10.10.10.254
            nameservers:
                addresses:
                - 10.10.10.254
            interfaces:
            - enp3s0
```

在上部，您配置了真正的网卡( *enp3s0* )，但没有为其分配地址。接下来是网桥的定义。它的设置类似于静态网络连接，并且还包含关键的*接口*。在那里你定义哪个真正的网卡应该被“桥接”。你会在官网上找到[更进一步(更复杂)的网桥示例](https://netplan.io/examples#configuring-network-bridges)。

现在，以下命令将更改应用于网络设置:

```
$ netplan apply --debug
```

# 分配网桥

一旦您完成了网桥的设置，并且它获得了正确的 IP 地址，您必须告诉 LXD 容器从网桥获得它的 IP 地址。这可以通过以下命令完成:

```
$ lxc config device add containername eth0 nictype=bridged parent=br0 name=eth0
```

用 *name=eth0* 你定义在哪个名字下可以在容器中找到网卡。现在，您可以在容器中随意配置 eth0。从现在开始，容器将从主机网络获得一个 IP 地址。

# 结论

你可以设置一个简单的网桥，并将其分配给一个容器。这允许网络上的其他用户访问 web 应用程序，而不需要在容器主机上设置反向代理。更复杂的情况也是可能的(VLANs、将容器放入不同网络的多个网桥等。)，但这超出了这篇短文的范围。

*最初发表于*[*【openschoolsolutions.org】*](https://openschoolsolutions.org/set-up-network-bridge-lxd/)*。*