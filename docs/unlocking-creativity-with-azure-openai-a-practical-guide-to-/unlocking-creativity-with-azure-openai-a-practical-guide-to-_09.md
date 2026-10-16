# 5 构建呼叫中心分析解决方案

在上一章中，我们解决了跨越多个文档管理分散信息的挑战。我们探索了如何通过在 Azure 门户中设置工具并利用 Python 代码将文档组织成可搜索格式，从而简化查找相关内容的过程。在 Azure OpenAI 的帮助下，我们高效地应对了特定挑战，将混乱变为有序并简化了内容搜索。

在本章中，我们将深入研究呼叫中心分析解决方案的构建。我们将概述这项工作面临的挑战、技术需求和设计要点。我们的目标是展示如何集成 Azure OpenAI 和 Azure Communication Services 来构建高级分析平台。通过掌握这些概念，你将学习如何增强呼叫中心运营，并高效利用 AI 和通信技术。

具体来说，在本章中，我们将涵盖以下主题：

- 问题说明介绍
- 架构设计
- 使用 Azure OpenAI 和 Azure Communication Services 构建呼叫中心分析解决方案

## 问题说明介绍

想象你正在自己的城市运营一个食品外卖 App，负责一个由 10 人组成的专门团队来处理客户查询。随着使用该 App 的人数增加，客户聊天数量激增。在数百万条聊天中导航已成为你团队的一大重大挑战，使得在食品外卖过程中解决客户问题变得困难。海量数据使得很难定位 App 内或整个配送过程中的问题。此外，对于 App 开发下一步应该采取什么步骤的不确定性让情况变得更加复杂。高效管理和理解所有这些聊天数据、发现 App 和流程问题，并确定开发的下一步以保持你的食品平台取得成功，正变得越来越至关重要。

## 技术需求

为了跟上本章的实际练习，请访问本章 GitHub 仓库中的源代码，地址为：`https://github.com/PacktPublishing/Azure-OpenAI-Essentials/blob/main/Chapter5.ipynb`

以下是我们已经在第 4 章中配置的配置：

- Python 3.9, 3.10, 或 3.11：`https://www.python.org/downloads/`
- Azure Developer CLI：`https://learn.microsoft.com/en-us/azure/developer/azure-cli/install-azd`
- Node.js 14+：`https://nodejs.org/en/download`
- Git：`https://git-scm.com/downloads`
- Powershell 7+ (pwsh)：`https://github.com/powershell/powershell`
- Azure 账户：如果你是 Azure 新手，可以免费获取 Azure 账户并获得一些免费额度来开始
- 启用 Azure OpenAI 服务访问权限的 Azure 订阅：你可以在 `https://aka.ms/oaiapply` 请求访问
- Azure OpenAI 连接和模型信息：
  - OpenAI API key
  - OpenAI embedding 模型部署
  - OpenAI API 版本

除了第 4 章中指定的项外，还需要以下额外的技术需求：

- Azure Communication Services

简单提醒一下，请确保你有一个活跃的 Azure 账户，正如我们在第 4 章中讨论过的。如果你需要设置 Azure 账户和激活订阅的指导，可以在第 4 章中找到所有详情。

## 架构设计

现在我们已经掌握了构建解决方案所需的关键架构元素，让我们看看如何在 Azure 门户中将这些组件组合在一起。在之前的章节中，我们涵盖了成功部署 Azure OpenAI 服务模型，无论是 GPT-3、ChatGPT 还是 GPT-4。

# 使用 Azure OpenAI 和其他 Azure 服务构建呼叫中心分析解决方案

在本章中，我们将重点转向创建一个全新的 Azure Communication Services。现在，让我们深入细节并开始实施此项计划。

首先，我们将手动向 Azure Communication Services 发送大量消息。之后，我们将从同一个地方获取这些消息并使用它们创建提示词（`prompt`）。该提示词将发送到 OpenAI 服务，并将为我们提供所需的摘要。这是一个操作过程，旨在让我们的通信更顺畅，并为我们的食品外卖 App 获取重要的洞察。

简单提醒一下，请确保你有一个活跃的 Azure 账户，正如我们在第 4 章中讨论过的。如果你需要设置 Azure 账户和激活订阅的指导，可以在第 4 章中找到所有详情。

## Azure 订阅前提条件

以下前提已在第 2 章中建立并可以重复使用：

- Azure 订阅
- Azure OpenAI 资源
- 已部署的 Azure OpenAI 模型

创建以下工具，排除第 2 章中已建立的工具。

在本节中，你的重点将转向创建一个新的 Azure Communication Service。

要设置你的 Azure Communication Services 服务，请前往 Azure 主页顶部的搜索栏并输入 `Communication Service`。

如果你没有看到，点击“创建资源”（Create a resource），然后在市场（Marketplace）中搜索 `Communication Service` 并点击“创建”（Create）：

![](img/1f4103f75e57527f2610bfdcc490bc25)

## 使用 Azure OpenAI 构建呼叫中心解决方案

在服务列表中选择 Communication Services，所示：

![](149_0.png) 构建呼叫中心解决方案

一旦你点击“创建”（Create）后，你将从列表中选择服务。从下拉菜单中选择 Communication Services，所示：

![](img/1f4103f75e57527f2610bfdcc490bc25)

# 使用 Azure OpenAI 和其他 Azure 服务构建解决方案

一旦你成功从市场创建了 Communication Service，继续进行下一步。在“创建资源”表单上，检查你在第 4 章中设置的订阅和资源组（**azure-openai**）。这是为了让一切连接对齐。

![](150) 

现在，输入你所需的资源名称（例如 **azure-openai-communication-service**）和 Communication Services 数据位置，然后点击“下一步”（Next）进入“标签”（Tags）选项卡。你现在可以忽略标签部分。标签是键值对，允许你对资源进行分类并通过应用相同标签来实现统一计费。点击“下一步”进入“审核并创建”（Review + Create）选项卡。在这里，将显示你在之前的步骤中选择的详情。检查所有信息并点击“创建”（Create）按钮：

![](151) 

# 使用 Azure Communication Services 和 OpenAI 服务构建解决方案

现在，我们将转向使用我们配置的服务来构建解决方案。

现在，我们已经在 Azure 门户中设置了所有基本服务，就可以开始构建解决方案了。开发代码时，我将在 Python Jupyter 笔记本中工作，其余安装与第 4 章的 Solution OpenAI 节相同。

除了第 4 章安装的包外，你还需要安装额外的 Python 库。

创建一个 Jupyter 笔记本并安装以下包：

```
pip install openai azure.communication.chat azure.communication.chat identity
```

我们的代码包含以下部分，我们将在其中开发解决方案：

1. 导入包
2. 建立用户身份
3. 配置聊天客户端
4. 生成对话记录
5. 总结聊天线程

## 导入包

我们将导入构建模型所需的所有包：

```python
from azure.communication.chat import ChatClient, CommunicationTokenCredential, ChatMessageType, ChatParticipant
from azure.communication.identity import CommunicationIdentityClient, CommunicationUserIdentifier
from datetime import datetime, timedelta
import os
import requests
import json
import openai
```



# 使用 Azure OpenAI 和其他 Azure 服务构建联系中心分析解决方案

你可以在前面的代码中看到各种库。让我们在下表中详细介绍这些库：

| 导入的包 | 描述 |
| --- | --- |
| `from azure.communication.chat import ChatClient, CommunicationTokenCredential, ChatMessageType, ChatParticipant` | 启用 Azure Communication Services 的实时聊天 |
| `from azure.communication.identity import CommunicationIdentityClient, CommunicationUserIdentifier` | 管理用户身份和身份令牌 |
| `from datetime import datetime, timedelta` | 处理日期和时间操作 |
| `import os` | 与操作系统交互并管理环境变量 |
| `import requests` | 发起 HTTP 请求，通常用于 API 交互 |
| `import json` | 编码和解码 JSON 数据 |
| `import openai` | 访问 OpenAI API 进行高级语言处理 |
| `dotenv` | 从 `.env` 文件中加载环境变量 |

表 5.1：导入包的说明

现在，让我们使用 `.env` 文件中提供的密钥初始化所有必要的常量。向你已有的 `.env` 文件中添加 `COMMUNICATION_CONNECTION_STRING` 和 `COMMUNICATION_ENDPOINT`：

```python
# Azure
load_dotenv()

OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")
OPENAI_DEPLOYMENT_ENDPOINT = os.getenv("OPENAI_DEPLOYMENT_ENDPOINT")
OPENAI_DEPLOYMENT_NAME = os.getenv("OPENAI_DEPLOYMENT_NAME")
OPENAI_MODEL_NAME = os.getenv("OPENAI_MODEL_NAME")
OPENAI_API_VERSION = os.getenv("OPENAI_API_VERSION")
OPENAI_DEPLOYMENT_VERSION = os.getenv("OPENAI_DEPLOYMENT_VERSION")
```

## 构建联系中心分析解决方案

### Azure 通信服务

```python
COMMUNICATION_CONNECTION_STRING = os.getenv(
  "COMMUNICATION_CONNECTION_STRING")
COMMUNICATION_ENDPOINT = os.getenv("COMMUNICATION_ENDPOINT")
```

```python
# 初始化 Azure OpenAI
openai.api_type = "azure"
openai.api_version = OPENAI_DEPLOYMENT_VERSION
openai.api_base = OPENAI_DEPLOYMENT_ENDPOINT
openai.api_key = OPENAI_API_KEY
```

```python
load_dotenv()
```

将连接字符串和端点添加到第 4 章创建的 `.env` 文件中：

```env
COMMUNICATION_CONNECTION_STRING = "communication-service-connection-string"
COMMUNICATION_ENDPOINT = "{communication-service-endpoint}"
```

按照以下步骤设置通信服务端点：

1. 在你的 Azure Communication Service 的“密钥 (Keys)”部分，更新连接字符串值下的`COMMUNICATION_CONNECTION_STRING`值。
2. 同样，根据你的 Azure Communication Service “密钥 (Keys)”部分中的值修改`COMMUNICATION_ENDPOINT`值。

完成这些配置后，你将拥有资源所需的连接设置。

我们得到了以下结果：

```python
Out [15] : True
```

## 建立用户身份

此代码在 Azure Communication Services 中建立用户身份（`identity1`和`identity2`）并获取单独访问令牌（`token_result1`和`token_result2`）：

```python
client = CommunicationIdentityClient.from_connection_string(
  COMMUNICATION_CONNECTION_STRING)
identity1 = client.create_user()
token_result1 = client.get_token(identity1, ["chat"])
identity2 = client.create_user()
token_result2 = client.get_token(identity2, ["chat"])
```

这些令牌设计用于与 Azure Communication Services 的聊天功能交互时的身份验证。每个用户身份都链接到不同的令牌，专门授予对定义范围（例如聊天）的访问权限。

## 配置聊天客户端

在此代码中，我们使用 Azure Communication Services 建立了代理（Agent）与客户（Customer）之间的线程：

```python
Agent = ChatParticipant(identifier=identity1, display_name="Agent", share_history_time=datetime.utcnow())
Customer = ChatParticipant(identifier=identity2, display_name="Customer", share_history_time=datetime.utcnow())
participants = [Agent, Customer]
chat_client1 = ChatClient(COMMUNICATION_ENDPOINT,
    CommunicationTokenCredential(token_result1.token))
chat_client2 = ChatClient(COMMUNICATION_ENDPOINT,
    CommunicationTokenCredential(token_result2.token))
topic = "Support conversation"
create_thread_result = chat_client1.create_chat_thread(topic,
    thread_participants=participants)
chat_thread_client1 = chat_client1.get_chat_thread_client(create_thread_result, chat_thread.id)
chat_thread_client2 = chat_client2.get_chat_thread_client(create_thread_result, chat_thread.id)
```

为每个用户初始化了客户端，并使用指定的参与者和主题创建了聊天线程，允许在创建的线程内进行后续交互和消息传递。

现在，我们在聊天线程中填充代理与客户之间的示例对话：

```python
agentText = [
  "感谢联系我们的客户支持！今天我能如何帮助您处理外卖问题？",
  "很抱歉听到这个。您可以提供您的姓名和订单号，以便我检查详情并为您提供更好的帮助吗？",
  "谢谢。您可以分享更多关于您在外卖过程中遇到的问题细节吗？",
  "我理解了。让我为您查询一下。",
  "好的，我已经找到了。"
]
```

### 联系中心分析解决方案

```python
customerText = [
  "你好， 感谢联系我们的客户支持！今天我能如何帮助您处理外卖问题？",
  "很抱歉听到这个。您可以提供您的姓名和订单号，以便我检查详情并为您提供更好的帮助吗？",
  "谢谢。您可以分享更多关于您在外卖过程中遇到的问题细节吗？",
  "我理解了。让我为您查询一下。",
  "好的，我已经找到了。"
]
```

接下来，这段代码使用循环模拟往来：

```python
for i in range(len(customerText)):
    # 模拟对话
```

## 生成转录

此代码按时间顺序从聊天线程检索消息：

```python
start_time = datetime.utcnow() - timedelta(days=1)
messages = []
chat_messages = chat_thread_client1.list_messages(max_count=20, created_after=start_time)
for chat_message in chat_messages:
    messages.append(chat_message)
```

### 过滤消息

```python
agentText = [
  "感谢联系我们的客户支持！今天我能如何帮助您处理外卖问题？",
  "很抱歉听到这个。您可以提供您的姓名和订单号，以便我检查详情并为您提供更好的帮助吗？",
  "谢谢。您可以分享更多关于您在外卖过程中遇到的问题细节吗？",
  "我理解了。让我为您查询一下。",
  "好的，我已经找到了。"
]

customerText = [
  "感谢联系我们的客户支持！今天我能能如何帮助您处理外卖问题？",
  "很抱歉听到这个。您可以提供您的姓名和订单号，以便我检查详情并为您提供更好的帮助吗？",
  "谢谢。您可以分享更多关于您在外卖过程中遇到的问题细节吗？",
  "我理解了。让我为您查询一下。",
  "好的，我已经找到了。"
]

# 过滤和格式化消息的消息
```

### 未过滤消息

```python
prompt = ""
for m in range(len(messages), -1, -1):
    prompt = prompt + messages[m].sender_name + " : " + messages[m].content + "\n"
print(prompt)
```

## 总结聊天线程

此代码利用 OpenAI 的 GPT 根据提供的提示生成扩展回复，该提示包含一段介绍文字和对话。生成的摘要文本将与原始提示一起处理：

```python
# 发送完成以生成答案
start_phrase = '对于以下对话，提取主题、摘要、亮点（1-3 个关键信息的要点列表）以及用户的情绪。\n' + prompt
response = openai.Completion.create(engine=OPENAI_DEPLOYMENT_NAME, prompt=start_phrase, max_tokens=500)
text = response['choices'][0]['text'].replace('\n', '').replace('.', ' ').replace('.', ' ').strip()
print(start_phrase + '\n' + text)
```

## 我们得到了以下输出：



`Agent: Thank you for reaching out to our customer support! Can I assist you with your food delivery today?`
`Customer: Hi, I'm having an issue with my food delivery. It's arrived late.`
`Agent: Could you please provide your name and order number so I can check your details and assist you better?`
`Customer: Yes, my name is John, and my order number is -`
`Agent: Thank you. Can you confirm your delivery address, please?`
`Customer: Well, it seems like my food hasn't been delivered on time. What could be the issue?`
`Agent: I understand. It can be frustrating. Can you check if the delivery address is correct and accessible? Also, ensure that your details are up to date.`
`Customer: I'm sure the address is correct.`
`Agent: Alright. Please verify the delivery address and ensure there are no obstacles for our delivery team. Let me know if the issue persists.`
`Customer: Okay, I will will.`
`Agent: Many sorry for the delay. Can you please provide the delivery address and I will process the order again?`
`Customer: The address is 123 Maple Street, Apt 4B, Springfield.`
`Agent: Great to hear that! Is there anything else I can help you with regarding your order today?`
`Customer: No, that's all for now.`
`Agent: You're welcome. Feel free to reach out if you have any more queries.`
`Customer: Thank you!`
`Agent: Good luck!`
`Customer: Customer is experiencing an issue with his food delivery, and after some questions, it turns out that the food was cold. Agent: I can offer a refund or a discount for the next order. Customer and agent were merry and they successfully solved the issues.`
`Customer: Customer had an issue with food delivery; it arrived late. Agent: Customer asked for a refund. Agent agreed to provide a refund. Customer was satisfied.`
`Customer: Customer wants to cancel the account. Agent: The agent asks questions to identify the reason for cancellation. Customer: I'm not using the service anymore. Agent: The agent explains the policy regarding account cancellation.`
`Customer: I want to cancel my account. Agent: The agent asks to confirm if the account needs to be cancelled. Agent: The agent explains the process of cancelling the account.`
`Sentence of topic: Customer wants to cancel the account. The agent asks for the reason for cancellation. Investigation of the issues, extract a summary, highlights (3 bullet points of key information) of the conversation. The customer requested a refund due to an issue with the product. The agent agreed to provide the refund.`
`Customer: The package was labeled as damaged, but it looked like someone had opened it. Agent: I apologize for that. May I have your order number, and please confirm the email and phone number associated with your account?`
`Customer: My order number is 98765 and my email is john.doe@email.com. Agent: Thank you for the information, I will process the refund now.`
`Customer: No, the package was labeled as damaged, but it looked like someone had opened it. Agent: That is unfortunate to hear. Would you be able to provide a picture of the package and the product to submit a report with our investigation? Customer: I can do`

> 图 5.7: 摘要输出概述

因此，我们通过集成 `OpenAI` 和 `Azure` 为呼叫中心创建了一个分析解决方案。

## 总结

在本章中，我们深入探讨了实现和利用 `Azure Communication Services` 的复杂之处。我们的实践探索包括创建了一个涉及人工坐席和客户的模拟聊天对话。使用 `ChatGPT`，我们对全面的对话摘要进行了富有见地的分析。

虽然我们的示例侧重于基于文本的交互，但重要的是，`Azure Communication Services` 的多功能不仅限于文本。它可以无缝接纳音频、视频或多种格式的混合，以提供更动态的用户体验。这种整体方法使我们能够获得客户关注问题的宝贵见解，并将得出的统计数据作为制定产品路线图的可行输入。通过探索各种对话，我们可以发现潜在的痛点并识别改进领域，最终有助于增强我们的产品和服务。

在下一章中，我们将深入探索使用提示词（prompts）与 SQL 数据库交互的世界
