# Azure 表存储与 GCP 数据存储

> 原文：<https://medium.com/hackernoon/azure-table-storage-vs-gcp-datastore-dc409ea60297>

*这是我的“* [*天蓝色之旅*](https://ravivyas.com/tag/journey-with-azure/) *”系列*的一部分

所以作为玩 Azure 的一部分，我决定用 Azure 克隆 AirMeet 的代码库作为后端服务。 [Airmeet](https://airmeet.online) 当前运行在 [AppEngine](https://hackernoon.com/tagged/appengine) 上，数据存储作为[数据库](https://hackernoon.com/tagged/database)。Azure 端的对等物是以 TableStore 为数据库的 AppService。基于这种体验，这里有两个 NoSQL 的键值存储之间的一些差异。

# 构建您的存储

表存储需要分区键和行键，而数据存储只需要一个键。现在您可能认为这是额外的工作，但是这也允许您围绕您的数据结构进行规划，并且为将来可能的性能好处进行规划。

# Web 界面和 Azure 存储浏览器

GCP 的界面稍微好一点。例如，我可以在界面中进行多选来删除多个实体，但我不能在表存储中进行同样的操作。

另一方面，Azure 在 Storage Explorer 应用程序中有一个很大的优势，它允许你通过一个应用程序查看你的表、队列和 blobs。

# 更新实体

这是我最喜欢的 TableStorage 特性，虽然在 Datastore 中更新是可能的，但更多的是读取实体，在客户端修改它并写入整个对象。

# 读取部分实体

表存储有一个类似 SQL 的查询模型，因此你实际上只能读取一个实体的一部分，当你有一个大的实体并想节省带宽成本时，这非常有用。

# 成本比较

进行成本比较是不公平的，更重要的是，由于这两种产品的定价方式，这种比较是不准确的。Azure 根据您希望如何复制数据提供了更多的计费选项。GCP 不提供这种灵活性(或者根据你如何看待它来限制复杂性)。在成本方面，Azure 也平等地对待你的读取、写入和删除，而 GCP 对每个操作都有不同的定价，其中读取的成本是写入的 1/3。Azure 上的总体运营成本较低，对于存储在美国东部、具有本地冗余复制的表，每 10，000 个事务的运营成本为 0.00036 美元。GCP 每 100，000 个实体读取 0.036 美元。如前所述，写入成本增加了 3 倍。在大多数情况下，Azure 会更便宜，除非你可以将你的工作负载放入 GCPs 免费月层。

# 挑剔的

当从数据存储转移到表存储时，我最初遇到的一个问题是，我不得不担心“创建表”的问题。同样的逻辑在 TableStore 端不起作用，事实上我的 *insertEntity* 方法失败了，直到我用 *createTableIfNotExists* 方法包装它。如果文档中有什么东西，我肯定错过了。#

*原载于 2019 年 3 月 19 日*[*ravivyas.com*](https://ravivyas.com/2019/03/19/azure-table-storage-vs-gcp-datastore/)*。*