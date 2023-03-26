# 亚马逊 AWS S3 桶数据复制和下载

> 原文：<https://medium.com/hackernoon/amazon-aws-s3-bucket-data-copy-and-download-ff75556af11e>

![](img/e52d0e785a6ecc8db4583ddfbfe11711.png)

简单的教程如何复制和下载数据从任何其他 S3 桶到你的。首先你需要一个 AWS 账户。没有帐户，您无法访问 AWS 的任何功能。

创建帐户后，您需要安装 AWS CLI。 **AWS CLI** 是一个将所有 AWS 服务集中在一个中央控制台的工具，让您可以用一个工具轻松控制多个 AWS 服务。首字母缩写代表 Amazon Web Services 命令行接口，因为顾名思义，用户从命令行操作它。

命令- pip 安装 awscli

一旦在您的电脑上安装了 AWS CLI，您还需要一个步骤才能连接到您的 AWS 帐户。

对于 Windows- aws 配置

该命令是设置 AWS CLI 安装的最快方式。

通过 *aws 配置，*AWS CLI 将提示您四条信息。前两项是必需的。这是您的 AWS 访问密钥 ID 和 AWS 秘密访问密钥，它们充当您的帐户凭证。(您可以生成新的凭证)您需要的其他信息是区域和输出格式，您可以暂时将它们保留为默认值。

![](img/84c1a1bd94950ac84cc0d3278be373ea.png)

```
aws configure
AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [None]: us-west-2
Default output format [None]: json
```

显示所有存储桶- aws s3 ls

要下载数据，首先您需要将其他存储桶中的数据复制到您的本地驱动器，它会自动下载。

![](img/668ebfe8d48776566d9ba9a55ee2a017.png)

现在轮到你了！…谢谢你。