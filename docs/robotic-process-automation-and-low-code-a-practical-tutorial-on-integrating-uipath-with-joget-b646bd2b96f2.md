# 机器人流程自动化和低代码:集成 UiPath 和 Joget 的实用教程

> 原文：<https://medium.com/hackernoon/robotic-process-automation-and-low-code-a-practical-tutorial-on-integrating-uipath-with-joget-b646bd2b96f2>

![](img/bc581699b0a6e6a6942e1ecee9b2d570.png)

本实用教程介绍了领先的机器人过程自动化(RPA)平台 [UiPath](https://www.uipath.com/) ，并展示了它如何与 [Joget](https://www.joget.com) 开源无代码/低代码应用平台集成。有 3 个部分:

*   第 1 部分:UiPath 入门
*   第 2 部分:Joget 入门
*   第 3 部分:集成 UiPath 和 Joget

有关背景信息，请阅读[机器人过程自动化:快速介绍以及它如何补充低代码应用平台](/@jogetworkflow/robotic-process-automation-a-quick-introduction-and-how-it-complements-low-code-application-9be580f32511)。

# 第 1 部分:UiPath 入门

## UiPath 概述

[UiPath](https://www.uipath.com) 是领先的 RPA 供应商之一，在[《Forrester Wave:机器人流程自动化，Q2 2018》报告](https://www.forrester.com/report/The+Forrester+Wave+Robotic+Process+Automation+Q2+2018/-/E-RES142662)中被公认为领导者。

UiPath 由 3 个主要组件组成:

![](img/5a6e9bd8b627c65b01cac2cba5d595d7.png)

UiPath Component Architecture

1.  [UiPath Studio](https://studio.uipath.com/) :可视化设计流程以实现自动化的 UI 工具
2.  [UiPath Orchestrator](https://www.uipath.com/product/orchestrator) :管理所有机器人和流程的创建、监控和部署的 Web 应用程序
3.  [UiPath 机器人](https://robot.uipath.com/):运行在 UiPath Studio 中构建的流程。在实际机器上安装和执行的执行代理。

简而言之，它是这样工作的:

1.  在开发人员 PC 中使用 UiPath Studio 可视化地设计一个流程，
2.  将流程发布/部署到 UiPath Orchestrator 进行管理、计划和监控。
3.  实际过程本身由安装在相应机器上的 UiPath 机器人执行。

## UiPath 概念和术语

下面是 UiPath 中的一些基本概念和术语，您应该在后面的教程中熟悉它们:

*   [机器](https://orchestrator.uipath.com/docs/about-machines):代表机器人执行的实际机器
*   [环境](https://orchestrator.uipath.com/docs/about-environments):环境是一组机器人，用于部署流程
*   [包](https://orchestrator.uipath.com/docs/about-packages):发布的 UiPath Studio 项目
*   [进程](https://orchestrator.uipath.com/docs/about-processes):进程表示包和环境之间的关联。每次将一个包部署到一个环境中时，它会自动分发到属于该环境的所有机器上。
*   [作业](https://orchestrator.uipath.com/docs/about-jobs):作业是在一个或多个机器人上执行一个过程。

对于更高级的用法，还有一些概念在本教程中不会用到，例如:

*   库:过程库描述了一个可重用的共享活动系统
*   [调度](https://orchestrator.uipath.com/docs/about-schedules):使作业以预先计划的方式执行
*   [资产](https://orchestrator.uipath.com/docs/about-assets):通常表示可以在不同项目中使用的共享变量或凭证。
*   [队列](https://orchestrator.uipath.com/docs/about-queues-and-transactions):存储多种类型数据的地方，如发票信息或客户详细信息。

## 第一步:注册 UiPath 云平台

让我们在 https://www.uipath.com/platform-trial 注册一个账户。有一个社区计划可以免费开始，所以我们将**选择社区**进行注册。

![](img/b3f1e0fc4dbddca9d0fa97bd9329666a.png)

使用社交账户或电子邮件注册，注册后你会被带到 https://platform.uipath.com 的仪表板上。

![](img/c6b2b97606ab077ce48cc39e2a78c6a7.png)

一个**服务**代表公司中的一个部署。已经创建了默认服务，例如 DemoDefault。

![](img/0b4c10eaf9aecfc209b08dd538915090.png)

选择服务名，打开 **UiPath Orchestrator** web 应用程序。

![](img/f0118fb3db452c7fc2fcb0dfa59c6393.png)

## 步骤 2:安装 UiPath Studio 和 UiPath Robot

从[资源中心](https://platform.uipath.com/Demo/portal_/resourceCenter)下载 **UiPath Studio** 安装程序(UiPathStudioSetup.exe)，并安装在目标计算机上。有关 UiPath Studio 的更多信息，请参见 [UiPath Studio 指南](https://studio.uipath.com/)。

![](img/4d042feffba434396548170a7342249c.png)

安装 UiPathStudioSetup.exe 后，从 Windows 开始菜单启动 **UiPath Studio** 并激活它。您可以通过激活社区版免费开始。

![](img/958b9443d9c5fa0dcb59df317de761e9.png)

## 步骤 3:启动 UiPath 机器人并获取机器名称

在 Windows 开始菜单中，搜索 **UiPath 机器人**并启动它

![](img/319e54a385de3da2efe10b51509b97d8.png)

点击**齿轮**图标，打开**编制器设置**窗口，复制**机器名称**。

![](img/bf85411f96712de42e560e1f4577856b.png)

## 步骤 4:获取机器密钥

回到 **UiPath Orchestrator** ，在 Machines 菜单下创建一个新的**机器**。

**重要提示**:确保该名称与之前 **UiPath Orchestrator 设置**中显示的**机器名称**相匹配。

![](img/0afdc28ffd34828a5a47e59527ac2b3b.png)

查看选中的**机**并复制**机键**。

![](img/4e0a6d75a0e583070bd499454171fead.png)![](img/dbbc78385840ce753540904da15b46d8.png)

## 步骤 5:将 UiPath 机器人连接到机器

在 **UiPath 机器人中，**点击**齿轮**图标打开 **Orchestrator 设置**窗口。

填写**Orchestrator URL**(ui path 云平台为[https://platform.uipath.com](https://platform.uipath.com))和上一步复制的**机器密钥**，然后点击**连接**。

一旦连接，您应该在窗口底部看到“**状态:机器人不可用**”。

将鼠标悬停在该消息上，您应该会看到“**机器人不可用的原因。**“这个消息意味着机器连接是有效的，所以现在该创建机器人了。如果您看到不同的原因，那么您的配置可能有错误，因此您需要仔细检查您的设置。

![](img/687bece0309a3cf6e6bebaa3e689109b.png)

## 第六步:创建一个机器人

在 **UiPath Orchestrator** 的**机器人**菜单下，点击右上角的**加号**图标。

选择“在单个标准机器上工作的标准机器人”。

将机器人分配给之前创建的机器。

设置一个“名称”，在**域\用户名**和**密码**下填写目标机器(安装 **UiPath 机器人**的地方)的实际 Windows 登录。

![](img/da03f1bd202eacfa9d1def9f4d40748e.png)

## 步骤 7:将机器人添加到环境中

接下来，我们需要将创建的**机器人**添加到**环境**中。选择**机器人**菜单顶部的**环境**选项卡，并选择**管理**选项。

![](img/ad3e72b7b36abc370eb2b99cd96c4101.png)

选择之前创建的机器人，点击**更新**。

![](img/87ff381d4ffc3963d6519300356050c8.png)

此时，机器人应该成功连接到机器上。检查 **UiPath 机器人协调器设置**，状态应显示“**已连接，许可**

![](img/11ea726f6efbc1e7c71fe3397000fbb0.png)

## 第八步:开始工作

现在机器人已经部署好了，是时候开始测试它了。

在**作业**下，点击右上角的**播放**图标。

选择预先存在的**演示过程**和先前的机器人，然后点击**开始**。

![](img/d5bedcb39207fe3587d04e61300cf53e.png)![](img/7222f6d719f74e6fa2f06b170f41b57b.png)

在 **UiPath 机器人**中，会显示“**安装包…** ”。软件包安装完成后，会显示“**作业开始处理**”，表示处理已经开始。在此演示过程中，将会出现一个 Hello 弹出对话框。

![](img/3010c6c30dbaa76d4f2a1cd89a3c0b0d.png)

机器人已成功开始工作。您可以在 **UiPath Orchestrator** 的**监控**菜单中监控机器人和作业。

![](img/87613f8c970fe2b37517b4c3259f1dc3.png)

## 步骤 9:准备 UiPath 集成

2019 年 6 月，UiPath 为[消费其云 API](https://orchestrator.uipath.com/v2019/reference#consuming-cloud-api)引入了新的机制。检索集成所需的一些信息需要许多步骤:

**1。获取访问和 ID 令牌**

**1.1 生成代码质询和代码验证器**

在 web 浏览器中，访问[https://repl.it/languages/nodejs](https://repl.it/languages/nodejs)并运行以下代码:

```
function base64URLEncode(str) {
  return str.toString('base64')
    .replace(/\+/g, '-')
    .replace(/\//g, '_')
    .replace(/=/g, '');
}

function sha256(buffer) {
  return crypto.createHash('sha256').update(buffer).digest();
}

console.log("Generating challenge and Verifier");
var cryptoResult = crypto.randomBytes(32);
var verifier = base64URLEncode(cryptoResult);
var challenge = base64URLEncode(sha256(verifier));
console.log("Code challenge: " + challenge)
console.log("Code verifier: " + verifier);
```

注意输出中的**代码挑战**和**代码验证器**值，例如

```
Generating challenge and Verifier
Code challenge: RzYlHiiGzPGgOLaRQJYftZ1mmc3sCbeicZVRftTmC-A
Code verifier: YVMnLczXQgJ9dwzV7MlMWEjGyAia4nXvTZzU4UVrAPE
```

**1.2 获取授权码**

将**代码挑战**替换为下面的 URL，并在浏览器中访问更新后的 URL。

```
[https://account.uipath.com/authorize?response_type=code&nonce=b0f368cbc59c6b99ccc8e9b66a30b4a6&state=47441df4d0f0a89da08d43b6dfdc4be2&code_challenge=**[code_challenge]**&code_challenge_method=S256&scope=openid+profile+offline_access+email](https://account.uipath.com/authorize?response_type=code&nonce=b0f368cbc59c6b99ccc8e9b66a30b4a6&state=47441df4d0f0a89da08d43b6dfdc4be2&code_challenge=[code_challenge]&code_challenge_method=S256&scope=openid+profile+offline_access+email) &audience=https%3A%2F%2Forchestrator.cloud.uipath.com&client_id=5v7PmPJL6FOGu6RB8I1Y4adLBhIwovQN&redirect_uri=https%3A%2F%2Faccount.uipath.com%2Fmobile
```

浏览器应该重定向到

```
[https://account.uipath.com/mobile?code=**[authorization_code]**&state=47441df4d0f0a89da08d43b6dfdc4be2](https://account.uipath.com/mobile?code=[authorization_code]&state=47441df4d0f0a89da08d43b6dfdc4be2)
```

复制 URL 中的**授权 _ 代码**。

**1.3 获取刷新令牌**

使用任何 API 测试工具(例如 [Postman](https://www.getpostman.com/) ，提交 **POST** 请求至

[https://account.uipath.com/oauth/token](https://account.uipath.com/oauth/token)正文如下，相应替换**【授权 _ 代码】****【代码 _ 验证器】**的值。

```
{
    "grant_type": "authorization_code",
    "code": "**[authorization_code]**",
    "redirect_uri": "[https://account.uipath.com/mobile](https://account.uipath.com/mobile)",
    "code_verifier": "**[code_verifier]**",
    "client_id": "5v7PmPJL6FOGu6RB8I1Y4adLBhIwovQN"
}
```

从响应中复制**刷新令牌**的值:

```
{
    "access_token": "eyJ0eX...",
    "**refresh_token**": "**CBZcOD6vrP2FQ9qa8fuqDdfoEwnVPuR2Kp...**",
    "id_token": "eyJ0eX...",
    "scope": "openid profile email offline_access",
    "expires_in": 86400,
    "token_type": "Bearer"
}
```

**1.4 使用刷新令牌获取访问令牌和 ID**

使用任何 API 测试工具，提交一个 **POST** 请求到

【https://account.uipath.com/oauth/token】的用下面的正文，相应地替换了**【刷新 _ 令牌】**的值。

```
{
    "grant_type": "refresh_token",
    "client_id": "5v7PmPJL6FOGu6RB8I1Y4adLBhIwovQN",
    "refresh_token": "CBZcOD6vrP2FQ9qa8fuqDdfoEwnVPuR2Kpz-hitbTAIzG"
}
```

从响应中复制**访问令牌**和 **id 令牌**的值。

```
{
    "access_token": "eyJ0eX...",
    "id_token": "eyJ0eX...",
    "scope": "openid profile email offline_access",
    "expires_in": 86400,
    "token_type": "Bearer"
}
```

**2。获取 UiPath 帐户、服务和进程标识符**

**2.1 获取账户逻辑名**

使用任何 API 测试工具，提交一个 **GET** 请求到

[https://platform.uipath.com/cloudrpa/api/getAccountsForUser](https://platform.uipath.com/cloudrpa/api/getAccountsForUser)带请求头

```
Authorization: Bearer **[id_token]**
```

从响应中复制 **accountLogicalName** 的值:

```
{
    "userEmail": "[demo@domain.c](mailto:demo@joget.org)om",
    "accounts": [
        {
            "accountName": "Demo",
            "accountLogicalName": "Demo"
        }
    ]
}
```

**2.2 获取服务实例逻辑名**

使用任何 API 测试工具，提交一个 **GET** 请求到

[https://platform.uipath.com/cloudrpa/api/account/**【帐户逻辑名称】**/getAllServiceInstances](https://platform.uipath.com/cloudrpa/api/account/Demo/getAllServiceInstances)带请求头

```
Authorization: Bearer **[id_token]**
```

从响应中复制**serviceInstanceLogicalName**的值:

```
[
    {
        "serviceInstanceName": "DemoDefault",
        "**serviceInstanceLogicalName**": "**DemoDefaultzous50676**",
        "serviceType": "ORCHESTRATOR",
        "serviceUrl": "[https://platform-community.azurewebsites.net](https://platform-community.azurewebsites.net)"
    }
]
```

**2.3 获取所需过程的释放键**

我们现在需要检索 Release 键，这是一个期望过程的惟一标识符。使用任何 API 测试工具，提交一个 **GET** 请求到

[https://platform.uipath.com/odata/Releases](https://platform.uipath.com/odata/Releases)带请求头

```
Authorization: Bearer **[access_token]** X-UIPATH-TenantName: **[serviceInstanceLogicalName]**
```

从响应 **:** 中复制**键**的值

```
{
    "[@odata](http://twitter.com/odata).context": "[https://platform.uipath.com/odata/$metadata#Releases](https://platform.uipath.com/odata/$metadata#Releases)",
    "[@odata](http://twitter.com/odata).count": 1,
    "value": [
        {
            "**Key**": "**b27c7363-459c-4520-bae5-660d4a1d3813**",
            "ProcessKey": "Demo_Process",
            "ProcessVersion": "1.0.21",
            "IsLatestVersion": false,
            "IsProcessDeleted": false,
            "Description": "Demo Process",
            "Name": "Demo Process",
            "EnvironmentId": 98069,
            "EnvironmentName": "Demo Environment",
            "InputArguments": null,
            "QueueDefinitionId": null,
            "QueueDefinitionName": null,
            "Id": 120897,
            "Arguments": {
                "Input": null,
                "Output": null
            },
            "ProcessSettings": null
        }
    ]
}
```

**2.4 测试 Orchestrator API 调用以启动作业**

此时，请确保手头有以下重要信息:

*   **刷新 _ 令牌**(刷新令牌)
*   **访问令牌**(访问令牌)
*   **服务实例逻辑名称**(服务实例逻辑名称)
*   **键**(释放键)

让我们尝试进行一个 API 调用来为该流程启动一个作业。

使用任何 API 测试工具，提交一个 **POST** 请求到

[https://platform.uipath.com/odata/Jobs/UiPath.带有 2 个请求头的 server . configuration . odata . start jobs](https://platform.uipath.com/odata/Jobs/UiPath.Server.Configuration.OData.StartJobs)

```
Authorization: Bearer **[access_token]** X-UIPATH-TenantName: **[serviceInstanceLogicalName]**
```

和身体:

```
{ 
    "startInfo":
   { "ReleaseKey": "b27c7363-459c-4520-bae5-660d4a1d3813",
     "Strategy": "All",
     "RobotIds": [ ],
     "JobsCount": 0,
     "Source": "Manual" 
   } 
}
```

如果成功，响应将如下，作业的状态显示在**状态**属性中:

```
{
    "[@odata](http://twitter.com/odata).context": "[https://platform.uipath.com/odata/$metadata#Jobs](https://platform.uipath.com/odata/$metadata#Jobs)",
    "value": [
        {
            "Key": "e811bd29-26fb-4cc3-af91-7fd6308ca643",
            "StartTime": null,
            "EndTime": null,
            "**State**": "**Pending**",
            "Source": "Manual",
            "SourceType": "Manual",
            "BatchExecutionKey": "382e7a96-a5b9-4343-b258-26a80cf87f80",
            "Info": null,
            "CreationTime": "2019-06-12T08:02:03.2595408Z",
            "StartingScheduleId": null,
            "ReleaseName": "Demo Process",
            "Type": "Unattended",
            "InputArguments": null,
            "OutputArguments": null,
            "HostMachineName": null,
            "HasMediaRecorded": false,
            "Id": 10169582
        }
    ]
}
```

# 第 2 部分:Joget 入门

## Joget 概述

[Joget](https://www.joget.com) 是一个开源的无代码/低代码应用平台，用于更快、更简单的数字化转型。Joget 在一个简单、灵活和开放的平台上结合了业务流程自动化、工作流管理和快速应用程序开发的优点。它基于视觉和网络，使非编码人员能够随时随地即时构建和维护应用程序。

![](img/51de291fe96bed96ee1e18f1e6d121fc.png)

Joget Platform Architecture

## 按需注册 Joget 工作流

如果您已经安装了 Joget 平台，您可以使用它并跳过这一步。否则，你可以注册 [Joget Workflow On-Demand](https://cloud.joget.com) ，这是一个 Joget 平台的托管版本，可以让你快速上手。

![](img/fa0816ac3e02335c0c3003c590663196.png)

访问[https://cloud.joget.com](https://cloud.joget.com)，点击**注册免费**注册一个新账户。

![](img/0951a62d3d85e40b51d212ab0087cb23.png)

一旦您成功验证了您的电子邮件，您将链接到您自己正在运行的 Joget 工作流安装，您将能够在 30 分钟内[可视化地构建一个完整的应用程序，而无需编码](https://blog.joget.org/2017/06/simplifying-dev-in-devops-build-full.html)。

![](img/20d952c6b3f835bf5f434bc93fc334b0.png)

# 第 3 部分:集成 UiPath 和 Joget

## 如何与 UiPath 集成

UiPath 为集成提供了 [Orchestrator API](https://integrations.uipath.com/docs/integrating-with-uipath) 。

最常用的功能是[启动一个作业](https://postman.uipath.rocks/#738c3beb-1c19-4257-8474-841a054c4220)，这应该满足大多数用例。我们将在下面的集成教程中使用这个函数。

## 设计一个 Joget 流程来启动一个 UiPath 作业

由于 **UiPath Orchestrator API** 是一个带有 [JSON](https://www.json.org/) 格式数据的 [REST](https://searchmicroservices.techtarget.com/definition/RESTful-API) API，我们可以使用 Joget [JSON 工具](https://dev.joget.org/community/display/KBv6/JSON+Tool)来调用该 API。

在我们开始之前，确保您手头有以下关键的 UiPath 信息(从之前的**准备 UiPath 集成**教程中获得):

*   **刷新 _ 令牌**(刷新令牌)
*   **服务实例逻辑名称**(服务实例逻辑名称)
*   **键**(释放键)

## **第一步。设计新的应用程序**

首先，让我们通过点击 Joget [应用中心](https://dev.joget.org/community/display/KBv6/Apps+and+the+App+Center)中的**设计新应用**来设计一个新应用。

![](img/ed8320771526aeb654fdc04d49905810.png)

为**应用 ID** 和**应用名称**填入所需值，点击**保存**。

## **第二步。设计新流程**

点击**流程**菜单，然后点击**设计流程**按钮，启动[流程生成器](https://dev.joget.org/community/display/KBv6/Process+Builder)。

设计一个包含两个工具的简单流程，如下所示。

![](img/76ac7f77cf484026e01ea4a398e8d09b.png)

将鼠标悬停在流程名称上方后，点击**编辑**铅笔图标。输入合适的流程名称，并创建 3 个工作流变量来存储来自 UiPath API 调用的响应值:

*   **状态**
*   **id_token**
*   **访问令牌**

![](img/b71796f595715d0788f7a2b5e48e1d11.png)

点击**部署**。

## **第三步。配置第一个工具以获取 UiPath 访问令牌**

部署流程后，关闭流程构建器。在流程视图页面中，选择**映射工具到插件**选项卡。

![](img/4e95c3a7ea05d4d741b30c59ec39cede.png)

点击第一个工具的**配置插件**，选择 **JSON 工具**，然后输入以下配置:

*   JSON 网址:[https://account.uipath.com/oauth/token](https://account.uipath.com/oauth/token)
*   呼叫类型:POST
*   POST 方法:自定义 JSON 负载
*   自定义 JSON 负载:

```
{
    "grant_type": "refresh_token",
    "client_id": "5v7PmPJL6FOGu6RB8I1Y4adLBhIwovQN",
    "refresh_token": "**[refresh_token]**"
}
```

![](img/1a332a5466e6cca60e64919344d10ab0.png)

在**存储到工作流变量**下，映射变量以存储匹配工作流变量中的令牌，即

*   访问令牌:访问令牌
*   标识令牌:标识令牌

![](img/489c014d411a0ccf44d8e2745a6cdd9f.png)

点击**提交**进行保存。

## **第四步。使用访问令牌**配置第二台刀具启动作业

点击第二个工具的**配置插件**，选择 **JSON 工具**，然后输入以下配置:

*   JSON 网址:[https://platform.uipath.com/odata/Jobs/UiPath.server . configuration . odata . start jobs](https://platform.uipath.com/odata/Jobs/UiPath.Server.Configuration.OData.StartJobs)
*   呼叫类型:POST
*   POST 方法:自定义 JSON 负载
*   自定义 JSON 有效负载(用实际的释放键替换**【Key】**):

```
{ 
    "startInfo":
   { "ReleaseKey": "**[Key]**",
     "Strategy": "All",
     "RobotIds": [ ],
     "JobsCount": 0,
     "Source": "Manual" 
   } 
}
```

请求头(用实际的服务实例逻辑名替换[**serviceInstanceLogicalName**:

*   授权:无记名#变量.访问 _ 令牌#
*   X-UIPATH-TenantName: **【服务实例逻辑名称】**

![](img/4ed3199ffe7a0eef527645ee0509ccca.png)

在**存储到工作流变量**下，将状态变量映射到响应 JSON 中的**状态**属性，即

*   状态:值[0]。状态

![](img/d3f3ad23b87a12813d8c8ed7817c9943.png)

就是这样。Joget 流程已经配置为调用 **UiPath Orchestrator API** 来启动一个作业。

## **第五步。运行流程**

现在，让我们测试这个过程。点击顶部的**运行流程**按钮，然后在确认对话框中再次**运行流程**。

![](img/b21022bc3e64a9032f24edc51493454a.png)

一旦流程开始，这两个工具将按配置执行。要查看流程的结果，导航至**监视器**->-已完成流程。

选择流程实例，您将看到这两个工具被执行。

![](img/cfa47d03c2a8b65a32aa14cbaca6b245.png)

点击每个活动，查看获得 **Orchestrator API** 调用结果的工作流变量的值。

![](img/70691025857f286763ca5eda20ede2df.png)

如果您在本地运行 Joget，并且能够访问系统日志，那么在启用调试选项的情况下，您将能够看到工具请求和响应。如果你使用的是 Joget DX ，你可以直接在网络浏览器中浏览日志。

![](img/73a0ef7771d93cc72acd3de8c0730459.png)

回到 **UiPath Orchestrator** 和 **UiPath Robot** ，您还可以监控作业的执行。

# 结论

在本文中，我们介绍了 UiPath 和 Joget 入门教程，以及它们之间的集成。正如教程中所演示的，集成所需的大量工作是为 UiPath Orchestrator API 准备身份验证机制。一旦所需的集成键可用，Joget 中调用 UiPath API 本身的配置就非常简单了。

要了解每个平台的更多信息，请访问 [UiPath](https://www.uipath.com) 和 [Joget](https://www.joget.com) 。