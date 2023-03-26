# 在 AWS 中配置多个节点池

> 原文：<https://medium.com/hackernoon/configuring-multiple-multiple-node-pools-b2509641ac82>

> 根据您的部署需求定制 EC2

想象一下这个场景。你有一些小而简单的微服务。它们都可以在 200mb 内存上完美运行，并且可以立即扩展。工程幸福。但是，等待…在这蓝色的天空中，风景中的一道伤疤，隐藏着一个预兆。一块巨石形状的风暴云。您需要让这个庞然大物在您的 Kubernetes 集群中运行，但是它比您的其他应用程序要大得多。用互联网的话说，“wut do？”

![](img/884cd1e054af6359fcab8a9d250c0a0c.png)

Your cluster is gonna need some guns of steel.

## 你的微服务 EC2 实例类型够吗？

你的微服务可以装进小盒子里，但你的整体需要一些果汁。在单个实例中根本没有足够的能力将您的应用程序作为一个 pod 来运行。纯粹主义者会告诉你“那么，将其重构为微服务”。现实并不总是非黑即白的。

## 所以你把所有东西都放在大箱子里吗？

您的第一个目标可能是重新评估一切—耗尽现有节点并部署到更大的机器上。这样，您就不需要运行多个节点池。这能行！但这是有代价的。集群中浪费资源的可能性更大。更大的盒子意味着你更有可能有未被充分利用的实例，从而浪费你的钱。

## 好的，那么我可以有多种节点类型吗？

你可以的，你这个幸运的家伙。这很简单，只需要几个步骤。出于本教程的考虑，我将假设您熟悉 Kubernetes 的概念，如`pod`或`node`。如果你不是，你应该通读一下 [Kubernetes 文档](https://kubernetes.io/docs/concepts/workloads/pods/pod/)。此外，我假设您熟悉 AWS 术语，如 Autoscaling Group (ASG)和 EC2。如果你不是，[请先通读 AWS 文档](https://aws.amazon.com/ec2/)。为了简洁和清晰起见，我将不包括对 IAM 角色和安全组的更改。官方文件包含了这些细节。

## 好吧！集群自动缩放器

你首先需要连接的是[集群自动缩放器](https://github.com/kubernetes/autoscaler)。这有一个特定于 AWS 的安装模式。它将检测 EC2 实例中何时没有空闲空间来部署 pod，此时它将增加 ASG 中 EC2 实例的数量。它的工作原理是调整 ASG 中的“所需”字段，直到达到您设置的预定义最大值。我不会在这里详细讨论安装，因为文档非常好。当您按照这些说明操作时，请确保将其设置为“自动发现”。这样，当您添加新的 ASG 时，它会检测到它们，而您不需要重新配置您的集群。

## 接下来，你的自动缩放组

旋转 ASG 是非常简单的。为了举例，我们将使用 Terraform。在下面的代码片段中，我们将创建一个自动缩放组和一个运行配置。运行配置是 ASG 在构建新的 EC2 实例时应该使用的模板。这里有一些关键的东西需要注意。

1.  我们将忽略对`desired_capacity`的更改。这是因为群集自动缩放器将使用该值来缩放群集。我们不想每次做一个`terraform apply`就把它重置回 2。
2.  有两个突出显示的标记键。`k8s.io/cluster-autoscaler/enabled`**是强制的**以确保集群自动缩放器可以检测你的自动缩放组。第二，`k8s.io/cluster-autoscaler/cluster_name`不是强制性的，但是如果您正在运行多个集群，这是一个好主意。这将防止两个集群获得相同的 ASG。

```
resource "aws_autoscaling_group" "worker_node_asg" {
    desired_capacity = 2
    max_size         = 10
    min_size         = 1
    name_prefix      = "worker-node-"
    lifecycle {
         ignore_changes = ["desired_capacity"]
    }
    tag {
        key = "Name"
        value = "worker-node"
        propagate_at_launch = true
    }
    tag {
        key = "k8s.io/cluster-autoscaler/enabled"
        value = "true"
        propagate_at_launch = true
    }
    tag {
        key = "k8s.io/cluster-autoscaler/cluster_name" 
        value = "true"
        propagate_at_launch = true
    }
}
```

以及您的 ASG 的启动配置(模板)。注意以粗体突出显示的`instance_type`字段。

```
resource "aws_launch_configuration" "worker_launch_configuration" {
    image_id             = "${data.aws_ami.worker_ami.id}"
    instance_type        = "t2.medium"
    name_prefix          = "worker-node-"
    security_groups      = ["${some_security_group_ID}"]
    create_before_destroy = true
}
```

为您想要创建的每个 EC2 实例类型创建这个基础设施。我们假设，从现在开始，您已经为两种不同的实例类型做了两次。

## 让我们让豆荚更挑剔

你的豆荚此刻相当悠闲——当它们被扔向 k8s api 时，它们叹息着说“哦，把我放在任何地方都行”。我们需要给他们一点选择的余地，我们可以用**节点选择器**做到这一点。这将允许 pod 指定它想要运行的节点类型。

## 找到你的标签

如果您运行`kubectl get node -o yaml`，您将看到一些 EC2 实例。寻找`beta.kubernetes.io/instance-type`标签，这将让您知道 EC2 实例类型是什么。您可以在您的资源 yaml 中使用它来选择您希望您的应用程序部署到哪个盒子。还有其他方法可以做到这一点，不会用 AWS 特有的东西污染您的 yaml 一如既往，要小心。一个简单的例子 yaml 可能是这样的:

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
  nodeSelector:
    **beta.kubernetes.io/instance-type: t2.medium**
```

使用`nodeSelector`旗给了这个豆荚一个偏好——它很挑剔。一旦您运行`kubectl apply -f myfile.yaml`并将其推送到 k8s API，kubernetes 将找到一个具有正确实例类型的盒子并部署它。如果它不存在，它将触发自动缩放组为您创建一个实例。

## 现在你知道了！

您已经有了一个支持多种 EC2 实例类型的集群。您可以添加任意数量的 ASG。去占领这个世界吧，你这个小淘气！

如果你喜欢这个教程，我会定期在 twitter 上写一些关于 T4 的文章。