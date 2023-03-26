# 用 Python 将人工智能管道连接到 Word 文档

> 原文：<https://medium.com/hackernoon/hooking-up-an-ai-pipeline-to-a-word-document-in-python-162d1fe7d5ef>

![](img/f448a43815a62c5c2e190737892ecf5f.png)

在这里，我将向您展示如何在 web 浏览器中可视化 Word 文档中的合同信息。

对于上下文，假设您有一个处理 Word 或 PDF 文档的 AI 管道。也许它们是法律文件；或许是合同。

> 如何将结果呈现给人类用户？

这是一个关键的面向客户的 UI 设计选择，当然:-你的客户如何与你的产品互动？用起来是一种享受吗？文档看起来像在 Word 中一样吗？

但是文档可视化在任何初始人工智能训练阶段和在线学习中同样重要。

在初始训练阶段，您需要准备训练数据集。你能让这个变得多简单？然后，当你评估结果时，同样的问题:QA 能容易地看出人工智能得到的是对的还是错的吗？

在 [Native Documents](https://nativedocuments.com/) 中，我们已经构建了一个 API，使您可以轻松地在 web 应用程序中显示 Word 文档的结果。在本文中，我们将演示与 [LexNLP](https://github.com/LexPredict/lexpredict-lexnlp) 的交互，这是我们所知的唯一一个为处理法律文本而设计的开源 NLP 库。

一般模式如下:

*   获取文本或富文本形式的文档
*   让它通过你的 NLP 管道
*   使用我们的 ranges API 用 NLP 结果注释文档。结果将显示为文字注释

![](img/2cd03b11dd943b1acf9e0527397d2ec1.png)

*   或者，将任何自定义用户操作附加到注释中(如接受、拒绝、标记/争议)
*   或者，导出为 Word 注释

> 我们能用它做什么？

在法律领域有很多人工智能的应用。这些包括人工智能辅助的合同起草、审查和谈判。在合同签署之前，您通常需要能够编辑它，这意味着您需要一个 Word 格式的文档，而不是 PDF。

在这里，我们将通过一个“实体提取”的例子。

实体是文档中感兴趣的项目。例如，交易方名称或日期:如果您要将合同纳入合同管理系统，或者清除敏感信息，您需要查找这些内容。

> 输入 LexNLP

在这里，我们将使用 [LexNLP 的定义提取](https://lexpredict-lexnlp.readthedocs.io/en/latest/modules/extract/en/definitions.html#extract-en-definitions)功能:如果您想要实现合同起草助手功能以及知识管理/先例搜索应用程序，定义是有用的。

# 准备

我假设您已经安装了一个功能性的 Python。

要跟进，您需要一个本地文档 dev id 和 secret。在[https://developers.nativedocuments.com/](https://developers.nativedocuments.com/)买票

然后访问 https://canary.nativedocuments.com 的 T2，输入你的开发者 id 和密码。(有几种方法可以处理您的 dev id 和 secret，但就本演练而言，这是最简单的。在生产中，你可以把它编译成你的 Javascript。)

接下来，获取/安装我们的 python 工具:

```
pip install — upgrade [http://downloads.nativedocuments.com/python/ndapi.tar.gz](http://downloads.nativedocuments.com/python/ndapi.tar.gz)
```

在 Windows 上，创建 C:\Users\name\。ndapi\config.default-config 并在 Linux/Mac 上创建~/。ndapi/config.default-config

config.default-config 文件应该具有以下形式:

```
{
 “dev_id”: “YOUR DEV ID GOES HERE”,
 “dev_secret”: “YOUR DEV SECRET GOES HERE”,
 “service_url”: “[https://canary.nativedocuments.com](https://canary.nativedocuments.com)",
 “api_ver”: “DEV” 
}
```

这就是原生文档配置完成。

接下来，安装 LexNLP，类似于:

```
git clone [https://github.com/LexPredict/lexpredict-lexnlp.git](https://github.com/LexPredict/lexpredict-lexnlp.git)
cd lexpredict-lexnlp
python setup.py install
```

现在，您可以检查一切是否正常:

```
wget [http://downloads.nativedocuments.com/python/extractDefinitions.py](http://downloads.nativedocuments.com/python/extractDefinitions.py)
python
exec(open(“extractDefinitions.py”).read())
extractDefs(“default-config”, “[https://www.ycombinator.com/docs/YC_Form_SaaS_Agreement.doc](https://www.ycombinator.com/docs/YC_Form_SaaS_Agreement.doc)")
```

如果你在 Chrome 中加载结果 URL，你应该在那个文档中标识出其中的定义。如果您没有看到文档或定义，请在下面或在 https://helpdesk.nativedocuments.com/[发表评论](https://helpdesk.nativedocuments.com/)

接下来，我们将回顾刚刚发生的事情。

为此，首先您需要一个示例文档(Word。多克还是。docx 格式)。我们将假设您使用了 YC _ 表单 _ SaaS _ 协议. doc

# 概观

概括一下，我们需要做的是:

*   将文档加载到本机文档中并导出为文本
*   获取感兴趣的项目(使用 NLP)
*   创建注释说明

然后，您可以可视化结果，和/或以 docx 的形式获得结果。

# 步骤 1:加载文档

```
$ python
Python 3.6.8 (default, Jun 1 2019, 16:00:10) 
>>> import ndapi
>>> ndapi.init(user=”default-config”)
True
>>> mydocx=open(“./YC_Form_SaaS_Agreement.doc”,”rb”)
>>> sessionInfo = ndapi.upload_document_to_service(mydocx)
>>> print(sessionInfo)
{‘nid’: ‘DEV7BDVM9BGGAIMAUI487AB49QUQS000000000000000000000000002I0THD1D829MQUI4EVGNMFDM860000’, ‘author’: ‘eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJuYW1lIjoiTmF0aXZlIERvY3VtZW50cyIsIm5pY2tuYW1lIjoibmQifQ.tN0R0W3YjpwjnFBUmAj8dxfEMRtcNsXA677QcDtHb2s’, ‘rid’: ‘I2522’}
```

注意 **nid** 。这就是我们与文档交互所需要的。

```
>>> nid=sessionInfo[‘nid’]
```

旁白:此时你可以在我们的编辑器中查看你的文档。

这里有一个小函数来计算打开文档的 URL。

```
def printEditUrl(nid, author):
 print( “[https://canary.nativedocuments.com/edit/](https://canary.nativedocuments.com/edit/)" + nid + “?author=” + author)
```

要获取 URL:

```
printEditUrl(sessionInfo[‘nid’],sessionInfo[‘author’])
```

如果您想查看文档，请打开该 URL。

没起作用？你访问过 https://canary.nativedocuments.com 并输入了你的开发者 id 和秘密吗？

# 步骤 2:导出为纯文本

像大多数 NLP/AI 一样，LexNLP 消耗纯文本。所以让我们开始吧。

```
>>> ndRawText = ndapi.download_document_from_service(nid, mime_type=”application/vnd.nativedocuments.raw.text+text”) 

>>> print(ndRawText.data)
b”urn:nd:stream:5591RH3HVNOMMJM0LOH9DIDRTENGG00000…Jf0seo4p6vv-sf\nSAAS SERVICES ORDER FORM\nCustomer:\t\t\nContact: \nAddress:\nPhone: \n \nE-Mail: \n\nServices: [Name and briefly describe services here]
```

您可以看到结果由流头和文档文本组成，由换行符分隔。

让我们暂时把标题放在一边。

```
splits = ndRawText.data.decode().split(‘\n’,1)
```

# 第三步:与 NLP 交谈

在这里，我们与 LexNLP 对话以获得定义:

```
import lexnlp.extract.en.definitionsprint(list(lexnlp.extract.en.definitions.get_definitions_explicit(splits[1])))[(‘Service(s)’, ‘Services: [Name and briefly describe services here] ___________ (the “Service(s)”).’, (140, 158)),
:
(‘Support Hours’, ‘EXHIBIT C\nSupport Terms\n[OPTIONAL]\nCompany will provide Technical Support to Customer via both telephone and electronic mail on weekdays during the hours of 9:00 am through 5:00 pm Pacific time, with the exclusion of Federal Holidays (“Support Hours”).’, (22449, 22466))]
```

对于 LexNLP 找到的每个定义，您可以看到它返回定义的术语、含义以及找到定义的术语的字符范围。

我们只是重复使用字符范围来告诉本机文档在哪里附加注释。

# 步骤 4:将 NLP 响应转换为注释指令

为了让原生文档显示注释，我们需要向它发送以下形式的 JSON:

```
urn:nd:stream:5591RH3HVNOMMJM0LOH9DIDRTENGG00000…Jf0seo4p6vv-sf 
“[140..157):0.MEANING”: { “meaning”: “Services: [Name and briefly describe services here].. (the Service(s))”}
:
“[22449..22465):17.MEANING”: { “meaning”: “EXHIBIT C Support Terms [OPTIONAL] Company will provide.. Holidays (Support Hours)”}
```

为此，让我们使用几个辅助函数:

```
class ndComment:
 def __init__(self, start, end, slot, mng, content):
   self.start = start
   self.end = end
   self.slot = slot
   self.mng = mng
   self.content = content.replace(‘\n’, ‘ ‘)
   self.content = self.content.replace(‘\t’, ‘ ‘)
   self.content = self.content.replace(‘\”’, ‘’)
 def toString(self):
   return “\”[“ + str(self.start) + “..” + str(self.end) + “):” + str(self.slot) + “.” + self.mng + “\”: { \”meaning\”: \”” + str(self.content) + “\”} \n”def constructPayload():
 definitions=lexnlp.extract.en.definitions.get_definitions_explicit(splits[1])
 results = [ndComment(d[2][0], d[2][1]-1,ind, “MEANING”, d[1]).toString() for ind, d in enumerate(definitions)] 
 payload=splits[0]+”\n”+’’.join([‘’.join(result) for result in results]) 
 return payload
```

现在，我们可以使用它将注释附加到文档:

```
annotations = constructPayload()
ndapi.upload_document_to_service(annotations, “ranges”, nid=nid, mime_type=”application/vnd.nativedocuments.raw.text+text”)
```

# 可视化注释文档

现在让我们看看编辑器中的文档。

首先，请访问[https://canary.nativedocuments.com](https://canary.nativedocuments.com)如果你还没有，然后输入你的开发密钥和秘密。

这样做之后，你就可以打开网址了。要获取要加载的 URL:

```
printEditUrl(sessionInfo[‘nid’],sessionInfo[‘author’])
```

如果你在 Chrome 中查看它，你可能会注意到注释中的一点调试；在后续文章中，我将向您展示如何定制注释批注的外观。

# 把所有的放在一起

你可以在[http://downloads . native documents . com/python/extract definitions . py](http://downloads.nativedocuments.com/python/extractDefinitions.py)找到代码

它包含一个函数 extractDefs，该函数接受两个参数。ndapi/config(如“config-default”)，以及 Word 文档的路径或 URL(doc 或 docx)。例如:

```
python
exec(open(“extractDefinitions.py”).read())
extractDefs(“default-config”, “[https://www.ycombinator.com/docs/YC_Form_SaaS_Agreement.doc](https://www.ycombinator.com/docs/YC_Form_SaaS_Agreement.doc)")
```

它返回一个您可以在 web 浏览器中打开的 URL。使与同事共享工作 URL 变得容易(但不是为了生产！)，URL 包含 devid 作为参数。

您可以修改它来提取 LexNLP 支持的其他信息，或者完全替换其他 NLP 引擎。

欢迎反馈和评论。