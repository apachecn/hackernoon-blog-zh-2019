# 如何在 REST 端点上使用 Google Ads API

> 原文：<https://medium.com/hackernoon/using-google-ads-api-with-rest-endpoints-c8957eab1d31>

# 通过 REST 端点使用 Google Ads API

如果你想构建一个直接与 Google Ads 平台交互的 API，并且没有使用 Google 提供的受支持的客户端库，包括 Java、C#、PHP、Python 和 Ruby，那么这篇文章可能会对你有所帮助。在我们的例子中，我们使用了 Javascript，构建了一个 NodeJS 后端，它需要与 Google Ads 平台进行交互，为我们的客户执行各种不同的操作，例如获取和显示他们所有活动的信息，以及更复杂的信息，例如将 IP 地址添加到活动的 IP 阻止列表中。我们将在随后的 Google 自己的 Quickstart 文档中讨论如何完成所有这些工作，并更好地解释一些更复杂的部分。

首先，确保您有正确的帐户设置来继续所有的 API 调用。当我们测试和开发这个 API 时，我们希望使用测试帐户，以免弄乱任何生产数据或直播广告。

您需要一个生产经理帐户来获取您的开发人员令牌。因此，如果您还没有管理员帐户，请创建一个，然后前往设置 API 中心获取您的开发者令牌。注意:您可以使用此开发者令牌与任何其他帐户进行交互，而不仅仅是您自己的帐户。

您还需要一个测试管理员帐户，可以在这里创建[。我建议为你所有的测试账户使用一个单独的电子邮件地址，以免混淆。如果有必要，创建一个新的 gmail，当你在测试帐户和生产经理帐户之间来回切换时，它会有很大帮助，因为上面有开发者令牌。](https://ads.google.com/um/Welcome/Home?a=1&sf=mt&authuser=0#ta)

接下来，您需要获得一个访问令牌和刷新令牌。这部分我们不讨论，因为你可以自由地使用你喜欢的任何客户端库。我们使用了 React，所以我们使用了这个[库](https://github.com/anthonyjgrove/react-google-login)。使用它，我们能够在成功登录到拥有测试管理器帐户的帐户后，获得返回的授权代码，从而获得刷新令牌。更多关于 OAuth [的信息请点击这里](https://developers.google.com/google-ads/api/docs/oauth/overview)。

在这里登录到您的测试管理员帐户是很重要的，因为在您的开发人员令牌被批准之前，您只能与测试帐户进行交互。

第一个 API 调用是测试我是否正确设置了所有的东西，为此我们使用 customers:listAccessibleCustomers。给定您的开发者令牌和访问令牌，此调用将列出您有权使用的所有帐户。在那里，您可以解析出客户 ID，并将其用于您想要拨打的任何其他电话。

下面的 cURL 请求将得到这个结果。用上面你自己的变量填充缺失的变量。

```
curl -X GET \   [https://googleads.googleapis.com/v1/customers:listAccessibleCustomers](https://googleads.googleapis.com/v1/customers:listAccessibleCustomers) \   -H 'Authorization: Bearer {your-access-token-here}' \   -H 'developer-token: {your-developer-token-here}'
```

如果你收到了一份客户名单，那你的思路就对了。再深入一点，让我们来看看我们的一个客户帐户下列出的所有活动。为此，我们将使用这个 cURL 请求:

```
curl -X POST \   [https://googleads.googleapis.com/v1/customers/5509440813/googleAds:search](https://googleads.googleapis.com/v1/customers/5509440813/googleAds:search) \   -H 'Authorization: Bearer {your access token here}' \   -H 'Content-Type: application/json' \   -H 'developer-token: {your developer token}' \   -H 'login-customer-id: {customer manager account id, if under a manager account}' \   -d '{   "query": "SELECT campaign.id, campaign.name, campaign.status, campaign.serving_status FROM campaign"   }
```

使用正确的查询，可以点击请求所点击的 URL 来检索大部分帐户信息。这里有一个关于上面使用的查询语言的概述。如果您习惯于 SQL 查询，那么这个查询对您来说会有些熟悉。唯一的事情是你需要知道你想从正确的资源中访问哪些字段，否则，你会得到一个模糊的错误。这里有一个[参考文献](https://developers.google.com/google-ads/api/reference/rpc/google.ads.googleads.v2.resources)的链接，它提供了所有可用资源的列表。

注意:`login-customer-id`是测试客户的经理帐户 id。如果您尝试访问的客户拥有一个经理帐户，则需要在标题中提供该 id，如上所述。为了测试的目的，你只能使用一个测试经理账户，这意味着你基本上每打一个电话。

如果您想要与从上面检索到的活动进行交互，API 调用略有不同，需要您浏览每个可用的服务及其定义的原型文件[这里是](https://github.com/googleapis/googleapis/blob/master/google/ads/googleads/v1/services/campaign_service.proto)——链接到活动服务的原型文件，以建立您想要的请求。以下是向特定活动的 IP 阻止列表添加 IP 地址的请求示例。

```
curl -X POST \[https://googleads.googleapis.com/v1/customers/{CUSTOMER_ID}/campaignCriteria:mutate](https://googleads.googleapis.com/v1/customers/5509440813/campaignCriteria:mutate) \ -H 'Authorization: Bearer {your access token}' \ -H 'Content-Type: application/json' \ -H 'developer-token: {your developer token}' \ -H 'login-customer-id: {manager account id, if needed}' \ -d '{ "customer_id": "{CUSTOMER ID - ex: 1234567890}", "operations": [ { "create": { "campaign": "customers/{CUSTOMER_ID}/campaigns/{CAMPAIGN_ID}", "negative": "true", "ip_block": { "ip_address": "127.0.0.1" } } } ] }
```

同样，关于这些的文档有些模糊，这是一个非常具体的例子，但是您可以使用它来构建对任何其他服务的请求，使用提供的 API [引用](https://developers.google.com/google-ads/api/reference/rpc/google.ads.googleads.v2.services)和 [proto](https://github.com/googleapis/googleapis/tree/master/google/ads/googleads/v1/services) 文件，它将遵循相同的格式。

浏览此请求:URL 包含您调用此操作的客户 ID—控制台中显示的所有数字都没有破折号。然后是 JSON —同样是 URL 中的客户 ID，`operations`:您可以在服务的 proto 文件中看到可用操作的列表，在我们的例子中，CampaignService 包括`create`、`update`和`remove`。创建的对象中的`campaign`是指遵循该格式的活动资源名称。`negative`在这种情况下，有没有因为 IP 块特别要求。然后我们采取行动的标准是`ip_block`。

总而言之，使用 Google Ads API 有点困难，尤其是当您试图将 REST 端点与 JSON 一起使用时，因为它们很大程度上是没有文档记录的，并且大量工作是从一个引用到另一个引用来拼凑您需要的东西。对我来说，这是 IP 封锁，我希望这为一些人提供了一些清晰度，并把他们放在正确的轨道上做他们需要的谷歌广告 API。

当有疑问时， [Google Ads API 论坛](https://groups.google.com/forum/?utm_medium=email&utm_source=footer#!forum/adwords-api)实际上非常活跃，团队中知识渊博的成员会在一两天内回答大多数人的问题(根据我的经验)。对于一些特定的用例，那里已经有了大量的信息。

*原载于 2019 年 6 月 28 日*[*https://www . syntx . io*](https://www.syntx.io/api/using-google-ads-api-with-rest-endpoints/)*。*