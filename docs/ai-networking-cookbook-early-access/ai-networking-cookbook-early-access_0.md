
# AI 网络秘籍（早期访问）

## 4.1 下载 Ollama 模型

## 准备工作

* 工作原理…
* 更多…

## 准备工作（原文重复标题，已合并）

* 工作原理…

## 准备工作

* 工作原理…
* 实验 OpenAI LLM 模型和参数
* 使用 OpenAI 脚本
* 设置本地 AI 模型
* 使用 OpenAI 脚本

# 技术要求

本章不需要人工智能或大语言模型（LLM）的先验知识。如果特定的配方需要额外的技术，将在配方中提及。

# 设置 OpenAI 环境

AI 在涉及大语言模型（LLM）方面是主导者。ChatGPT 应用无疑在 2023 年引发了人工智能（AI）运动，两个月内达到了 1 亿用户。在访问 OpenAI 资源（如 Playground 和 API 请求）之前，我们需要设置开发者环境。

在本配方中，我们将设置 OpenAI 环境以使用 OpenAI 服务，包括生成 API 密钥、熟悉设置页。

## 准备工作

我们需要创建一个 OpenAI 平台账户；这与 OpenAI 账户不同。OpenAI 账户主要供个人用户通过 ChatGPT 等网页界面与 AI 模型交互。另一方面，OpenAI 平台账户旨在让开发者利用 OpenAI 模型构建应用程序和服务。

访问 `https://platform.openai.com/` 并登录您现有的 OpenAI 账户。如果没有账户，可以使用电子邮件注册免费账户。您也可以使用有效的 Google、Microsoft 或 Apple 账户登录 OpenAI。

按照屏幕指令完成身份验证和支付设置。

![Image 5](img/B32646_1_5.png)

图 1.4 – OpenAI 聊天对话

## 工作原理……

Playground 和 API 是我们开发过程中将使用的两个关键组件。Playground 是我们学习和实验不同模型设置、参数和结果的地方。如图 1.5 所示，Playground 由系统消息、聊天输入和对话组成。一旦我们对结果满意，就可以使用 API 以编程的方式与模型进行交互。

图 1.6 –

1. 输入问题“如何在 Cisco IOS-XE 设备上配置 VLAN?”并观察回答。

![Image 8](img/B32646_1_8.png)

图 1.7 –

2. 选择一个不同的模型，例如 `gpt-4.1-nano`，问同样的问题“如何在 Cisco IOS-XE 设备上配置 VLAN?”并观察回答。

3. 我们还可以看到两个模型之间的响应时间。在我的运行中，`gpt-4.1-nano` 生成响应耗时 1.542ms，而 `gpt-4o` 相同的问题花费了双倍的时间达到 5.575ms，nano 模型速度快了一倍以上。

![Image 9](img/B32646_1_9.png)

You are an AI assistant specialized in network engineering. Your role is to assist with tasks, provide explanations, and offer guidance related to network engineering concepts, protocols, tools, and troubleshooting.

图 1.12 –

3. 让我们将系统消息修改为：

* 使用非技术性的语言。

4. 我们可以询问同一个问题，`What are the steps to set up WiFi 7?` 并观察响应：

![Image 14](img/B32646_1_14.png)

图 1.13 –

* “以 YAML 格式提供答案。”

系统消息可以影响 OpenAI 以产生预期的输出。系统消息的几个关键概念是：

* **设置正确的上下文**：提供背景信息或任务描述。例如，“你是一个专注于网络工程的 AI 助手。”
* **指定输出格式**：指定你希望输出如何结构化。例如，`provide the answer in YAML format.`

在其他配方中我们将介绍其他的提示词工程方法，本配方提供了一个良好的开始。

## 更多内容…

## 如何操作…

1. 确保系统消息中没有任何指令。

You are an AI assistant 5th grade teacher specialized in network engineering. Your role is to assist with tasks, provide explanations, and offer guidance related to network engineering concepts, protocols, tools, and troubleshooting

* 使用非技术性的语言。
* 以段落格式提供答案。

![Image 16](img/B32646_1_16.png)

图 1.15 –

2. 观察输出，它们应该是相似的。然而，当我们进入一个带有自动清除的新聊天时，之前的指令会被清除。

## 工作原理…

我们也可以用 Python 与 ChatGPT 交互。

## 准备工作

从上一个配方中获取 OpenAI Key，因为我们将在脚本中使用它。创建一个 Python 虚拟环境来执行以下操作：

```bash
$ source .venv/activate
```

将你的 OpenAI Key 导出为环境变量。

```bash
```

## 如何操作…

1. 安装 `openai` 包。

```bash
```

2. 构造 Python 脚本：

```python
from openai import OpenAI

api_key=os.environ.get("OPENAI_API_KEY"),
model="gpt-4o",
{
    "role": "developer",
},
{
    "role": "user",
},
)
```

```bash
$ python ch01_hello_world.py
WiFi 7 也称为 IEEE 802.11be，是下一代 WiFi 技术，旨在提供更快的数据速率、更高的效率和增强容量。截至我 2023 年 10 月的最后更新，WiFi 7 仍在开发中，可能尚未在消费产品中广泛可用。然而，如果你想在兼容设备可用后安装 WiFi 7，以下是你需要遵循的通用步骤：
```

1. **检查兼容性：** …

## 更多内容…

OpenAI Python API 库 [`github.com/openai/openai-python`](https://github.com/openai/openai-python) 提供更多 Python 库的示例。

# 本地 AI 模型 Ollama

`Ollama` 是一种在本地运行语言模型的简单方法。

## 准备工作

确保你有一个 Ollama 账户，如果需要，请在 [`ollama.com/signup`](https://ollama.com/signup) 注册。同时确保在你想要运行的机器上安装了 Docker Engine（[`www.docker.com/`](https://www.docker.com/)）和 `docker-compose`（[`docs.docker.com/compose/`](https://docs.docker.com/compose/)）。

2. 使用文本编辑器编写 docker-compose 文件，命名为 `docker-compose.yml`：

```yaml
ollama:
services:
    image: ollama/ollama
      - ollama
    # deploy:
    #     reservations:
    #         - driver: nvidia
    #         capabilities: [ gpu ]
      - ./data/ollama:/root/.ollama
    ports:
    image: ghcr.io/ollama-webui/ollama-webui:main
      - ./data/ollama-webui:/app/backend/data
      - ollama
      - 3002:8080
    environment:
    extra_hosts:
    networks:
```

```bash
$ docker-compose up
✔ ollama-webui Pulled                                                    30.0s
   ✔ 24935aba99a7 Pull complete                                            2.5s
✔ ollama Pulled                                                         115.7s
✔ Network ollama-docker_ollama            Created                         0.0s
 ✔ Container ollama-docker-ollama-1        Created                         0.3s
ollama-webui-1  | INFO:     Application startup complete.
```

4. 通过 [`localhost:3002`](http://localhost:3002) 访问 Ollama WebUI。

![Image 17](img/B32646_1_17.png)

6. 使用 [`localhost:11434/`](http://localhost:11434/) 验证 Ollama 正在运行。

![Image 19](img/B32646_1_19.png)

图 1.18 –

图 1.19 –

8. 选择 `Models` 并输入 `gemma3:1b`，点击绿色下载按钮从 Ollama 拉取模型。

![Image 21](img/B32646_1_21.png)

9. 一旦模型完成下载，我们就可以从下拉列表中选择模型。

10. 在聊天框中提问，例如“OSI 模型的第 1 层是什么？”

![Image 22](img/B32646_1_22.png)

此配方涉及两个镜像：`ollama` 和 `ollama-webui`。`ollama` 镜像是模型所在的地方，而 `ollama-webui` 提供了一个管理界面，用于方便地管理模型下载和聊天前端。

默认情况下，docker 镜像中没有安装模型，我们需要搜索并安装模型。我们选择了 `gemma3:1b` 模型，因为它下载相对较小，同时性能仍然合理。有很多模型可供选择，我们可以根据模型的排名以及模型描述来开始选择我们要使用的模型。从网页上，我们可以根据下载次数、模型大小和参数数量来评估模型。

我们还可以向下滚动模型页面查看不同版本的模型，例如文本、视觉等。

![Image 24](img/B32646_1_24.png)

访问 [`ollama.com/`](https://ollama.com/) 获取有关模型和其他启动本地 LLM 模型方法的更多信息。

# Ollama Python 脚本

```bash
$ source .venv/bin/activate
```

## 操作步骤

1. 构建一个简单的 Python 脚本：

```python
from ollama import Client
client = Client(host='http://localhost:11434')
{
    'role': 'user',
},
print(response['message']['content'])
```

2. 执行并观察输出：

```bash
```

OSI 模型的第二层是`数据链路层`（Data Link Layer）。它是离用户最近的一层，负责本地网络（如你的家庭或办公室网络）中直接连接的节点之间（例如你的计算机和路由器）的数据可靠传输。

2. **关键功能：**

* **物理地址：** 第二层使用 `MAC`（介质访问控制）地址来识别本地网络上的设备。这允许设备在不需要理解 IP 地址的情况下直接通信。
* **成帧：** 它将数据封装成帧（frames），这是网络上传输的基本单位。这包括头部和尾部信息等。
* **逻辑地址：** 虽然它没有 IP 地址，但它使用 `MAC` 地址进行标识，并管理网段内的数据流。

## 工作原理……

该脚本演示了如何与本地 AI Ollama 服务器交互。它使用 `ollama` `Python 库` 来查询我们安装的 `gemma3:1b` 模型。我们初始化了 `client` 对象以与端口为 `11434` 的服务器通信。

使用 `client.chat` 方法向 AI 模型发送对话并接收响应。

# 网络工程师的 OpenAI 配方

在[第 1 章]中，我们讨论了设置环境的各种方法，包括 OpenAI 的开发环境——这被许多人认为开启了 AI 大模型革命的公司。OpenAI 的大语言模型（LLM）为网络工程师提供了一个强大的工具包，用于自动化常规任务、生成拓扑结构、设备配置，并在许多其他领域提供专家级的协助。

在本章中，我们将深入探讨使用 OpenAI 的模型来帮助我们处理网络工程背景下的任务。

* 在 Postman 中 OpenAI API
* 微调 OpenAI 模型
* 使用 OpenAI 生成操作程序说明书 (MOP)

正如我们在[第 1 章]中看到的，OpenAI 的产品是基于云的，他们从未官方提供过其现代模型（如 `GPT-3` 或 `GPT-4`）的本地托管版本。他们曾一度提供过一个开源模型版本，即 `GPT-2`，可以在本地运行，但与他们目前的产品相比，这是一个较旧且功能受限的模型。因此，在本章中，我们将关注云端产品。

# 技术要求

`curl`（[`github.com/curl/curl`](https://github.com/curl/curl)）是测试 API 通信时最通用的命令行工具之一。它允许直接在命令行界面中学习与 OpenAI API 通信的基础知识，例如建立连接、处理身份验证和处理响应。

## 准备工作

对于许多 Linux 发行版，`curl` 是默认安装的。我们可以通过 `curl` 命令检查情况是否如此：

```bash
$ curl --version
```

## 如何操作……

1. 将 OpenAI API 密钥导出为环境变量。

```bash
export OPENAI_API_KEY="your-openai-api-key"
```

```bash
```

我们应该能看到可用模型列表，在执行时你的输出可能会有所不同：

```bash
$ curl -v https://api.openai.com/v1/models -H "Authorization: Bearer $OPENAI_API_KEY" -H "Content-Type: application/json"
* IPv6: (none)
* IPv4: 162.159.40.25
* Connected to api.openai.com (162.159.40.25) port 43
…
{
  "data": [
    {
      "object": "model",
      "owned_by": "openai"
    },
      "id": "gpt-4",
      "object": "model",
      "owned_by": "openai"
    },
      "id": "gpt-3.5-turbo",
      "created": 167761062,
      "owned_by": "openai"
    …
    {
      "object": "model",
      "created": 1713497002,
    }
}
* Connection #0 to host api.openai.com intact
```

```bash
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
    "model": "gpt-3.5-turbo",
      {
        "role": "system",
      },
        "role": "user",
        "content": "解释 TCP 和 UDP 的区别，包括在网络应用中何时使用它们。"
    ],
    "temperature": 0.3
```



```json
{
  "object": "chat.completion",
  "created": 1752981163,
  "choices": [
    {
      "index": 0,
      "message": {
        "content": "TCP (Transmission Control Protocol) 和 UDP (User Datagram Protocol) 是计算机网络中最常用的两种传输层协议。以下是 TCP 和 UDP 之间的主要区别：\n\n1. **面向连接 vs 无连接**: TCP 是面向连接的，这意味着在数据传输之前，它会在发送者和接收者之间建立连接。另一方面，UDP 是无连接的，数据可以在无需事先通信的情况下发送。\n\n2. **可靠性**: TCP 提供可靠的、经过错误检查的数据交付。它确保数据按正确顺序接收，并重传任何丢失的数据包。而 UDP 不保证数据包的交付或顺序。它被认为是不可靠的，但由于开销较小速度更快。\n\n3. **流量控制和拥塞控制**: TCP 包含流控和拥塞控制机制以管理数据传输速率并防止网络拥塞。UDP 没有内置的流控或拥塞控制机制。\n\n4. **头部大小**:",
        "annotations": []
      },
      "finish_reason": "length"
    }
  ]
}
```

3. 对于交互式应用或长响应，我们希望使用流式和无缓冲区（`no-buffer`）选项。

```bash
"model": "gpt-3.5-turbo",
"messages": [
  {
    "role": "system"
  },
  {
    "content": "Create a detailed network security audit checklist for a mid-size company with 500 employees, covering perimeter security, internal segmentation, and monitoring."
  }
],
"max_tokens": 800
```

## 工作原理

在步骤 1 中，作为一种安全实践，我们导出 `OPENAI_API_KEY` 并将其创建为环境变量。如果我们启动另一个会话，我们需要重新创建它。对于 Windows 提示符，请使用 `set OPENAI_API_KEY="your-api-key"`。

在步骤 2 中，我们向 `https://api.openai.com/v1/models` 发送 HTTP `GET` 请求，`-v` 切换开启详细模式，在学习过程中为我们提供更多信息。`/v1/models` 端点列出了所有可用模型。`-H` 指定要包含在请求中的头部。在这种情况下，我们使用 `Authorization: Bearer` 来指示身份验证的 HTTP 头部，并在发送请求时使用 `$OPENAI_API_KEY` 指定实际值。`Content-Type: application/json` 告诉服务器我们以 JSON 格式发送数据。最后，`max-time 30` 设置了操作允许的最大时间。这主要是为了防止出现任何网络问题时会话挂起。

在步骤 4 中，我们预期的响应将会很长。在传统的 API 调用中，服务器将收集所有答案并一次性响应。对于较长的响应，可能需要 30 秒。通过使用 `stream: true` 选项，我们能够立即看到响应，从而提升用户体验。

## 更多内容

- 使用 shell 脚本进行错误处理、日志记录和批量处理。
- 使用 `jq` (https://jqlang.org/) 进行 JSON 输出处理。

# 使用 Postman 配合 OpenAI API

本教程涵盖了使用 Postman 配合 OpenAI 的基本功能，如环境变量管理、自动化测试和团队协作。

## 准备工作

Web 界面和应用程序界面在设计上相当。

![Image 1](img/B32646_2_1.png)

这里是 Postman Mac 应用程序（Windows 类似）：

![Image 2](img/B32646_2_2.png)

图 2.2：Postman App

我发现 Postman 应用程序更容易操作，因此在本教程中我将使用 App 进行演示。如果你更喜欢，可以使用 Web 界面。

1. 我们可以先为本教程创建一个专门的工作空间。

   图 2.3：创建新工作空间

2. 通过点击左侧边栏中的“Environments”并选择“**Create Environment**”创建一个环境。创建后，我们可以使用三个点重命名此环境。在示例中，我们将此环境命名为“`OpenAI 网络工程 – 测试`。”

   ![Image 4](img/B32646_2_4.png)

   ![Image 5](img/B32646_2_5.png)

   图 2.5：创建集合

图 2.6: 添加授权头

图 2.7: 集合变量

图 2.8: 添加请求

![Image 9](img/B32646_2_9.png)

![Image 10](img/B32646_2_10.png)

![Image 11](img/B32646_2_11.png)

![Image 12](img/B32646_2_12.png)

## 11. 如果所有输入都正确，点击 `Send` 后，我们应该能够看到之前见过的响应。

![Image 13](img/B32646_2_13.png)

## 工作原理……

这个配方非常直观，因为我们没有从 `curl` 部分向请求中引入任何新元素。头、主体和响应都是完全相同的。我们引入的是新的 Postman 界面，它对用户更友好，并将我们的 API 调用简化为层次结构。我们使用环境（`Environments`）来区分高层环境，如测试、QA 和生产环境。我们还使用集合（`Collections`）将不同的 API 调用根据其逻辑用例进行隔离。

当 OpenAI 处理我们的请求时，它会返回一个 HTTP 响应，状态码 `200` 表示成功，`401` 表示身份验证问题，`500` 表示服务器错误。有了语法高亮，故障排除也变得更容易了。

## 更多……

OpenAI LLM 基础模型（有时称为基础模型 `Foundation model`）在处理我们交给它的绝大多数甚至所有任务方面功能异常强大。正如我们目前所看到的，它能够帮助我们完成许多通用网络工程任务，例如配置 VLAN、分配 IP 地址和更改接口描述。

但它们目前无法做的一点（因为没有任何现成工具可以）是针对我们特定的组织需求提供个性化反馈。例如，如果我们的团队在配置 BGP 时始终使用 `Loopback 0` 作为 `router-id`，基础模型将不知道这一点。

## 准备工作

请查看第一章关于设置 OpenAI 开发环境的教程，包括使用 Python 虚拟环境、OpenAI API key 以及安装 `openai` Python 包。

## 如何操作……

```python
import os
from openai import OpenAI

api_key = os.getenv('OPENAI_API_KEY')
client = OpenAI(api_key=api_key)
query = "Configure BGP for AS 6501 with neighbor 192.168.1.2 in AS 6502"
response = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[
        {
            "role": "system"
        },
        {
            "role": "user",
            "content": query
        }
    ],
    max_tokens=200,
)
print("BASE MODEL OUTPUT:")
print(response.choices[0].message.content)
```

2. 结果是一个通用的 BGP 配置，没有问题，但它已经非常基础了。

```bash
BASE MODEL OUTPUT:

neighbor 192.168.1.2 remote-as 65002
```

```python
import json

training_examples = [
    {
        "messages": [
            {"role": "system", "content": "You are a network engineer. Always use Loopback0 for BGP router-id and enable log-neighbor-changes."},
            {"role": "user", "content": "Configure BGP for AS 65001 with neighbor 192.168.1.2 in AS 65002"}
        ]
    },
    {
        "messages": [
            {"role": "user", "content": "Set up BGP for AS 65010 with neighbor 10.0.1.1 in AS 65020"}
        ]
    },
    {
        "messages": [
            {"role": "user", "content": "Setup BGP routing for AS 66500 connecting to 203.0.113.254 AS 66600"}
        ]
    }
]

with open('bgp_training_data.jsonl', 'w') as f:
    for example in training_examples:
        f.write(json.dumps(example) + '\n')

print(f"Total examples: {len(training_examples)}")
```

4. 我们将使用以下脚本，`recipe_3_03_create_fine_tuned.py` 来创建新的微调模型。

```python
import os
from openai import OpenAI

client = OpenAI(api_key=api_key)

with open('bgp_training_data.jsonl', 'rb') as f:
    file_response = client.files.create(file=f, purpose="fine-tune")

print(f"File uploaded: {file_response.id}")
print("Starting fine-tuning job...")
job = client.fine_tuning.jobs.create(
    model="gpt-3.5-turbo",
    training_file=file_response.id,
    suffix="bgp-standards"
)

print("Waiting for completion...")

while True:
    job_status = client.fine_tuning.jobs.retrieve(job.id)
    if job_status.status == "succeeded":
        print(f"Fine-tuning completed: {job_status.fine_tuned_model}")
        with open('fine_tuned_model_id.txt', 'w') as f:
            f.write(job_status.fine_tuned_model)
        break
    elif job_status.status == "failed":
        print("Fine-tuning failed")
        break
    else:
        print(f"Fine-tuning {job_status.status}")
        break
    time.sleep(30)
```

5. 当我们执行上述脚本时，将需要 10 到 30 分钟不等。最后，我们将获得微调后的模型 ID，并将该 ID 保存到名为 `fine_tuned_model_id.txt` 的文件中，以供后续步骤使用。

```bash
$ python recipe_3_03_create_fine_tuned_model.py
File uploaded: file-QFBjPJCse6L9Zfbtan5A
Starting fine-tuning job...
Waiting for completion...
Fine-tuning completed: ft:gpt-3.5-turbo-0125:network-automation-nerds:bgp-standards:BvTBFsu
```

6. 我们将能够在 OpenAI 的控制台上查看微调模型的进度以及成功/失败情况。

![Image 14](img/B32646_2_14.png)

```python
from openai import OpenAI

client = OpenAI(api_key=api_key)

with open('fine_tuned_model_id.txt', 'r') as f:
    fine_tuned_model = f.read().strip()

query = "Configure BGP for AS 65001 with neighbor 192.168.1.2 in AS 65002"

response = client.chat.completions.create(
    model=fine_tuned_model,
    messages=[
        {"role": "user", "content": query}
    ],
    temperature=0.1
)

print("FINE TUNED OUTPUT:")
print(response.choices[0].message.content)
```

```text
description BGP_ROUTER_ID
!
router bgp 65001
  bgp log-neighbor-changes
  neighbor 192.168.1.2 remote-as 65002
```

## 准备工作

我们将使用几个 Python 软件包来实现此功能，请在你的虚拟环境中安装以下包。

```bash
$ sudo apt-get install graphviz
```

## 如何操作…

1. 第一步我们需要向 OpenAI 提供拓扑描述并生成一个 Graphviz dot 文件，我们将此脚本命名为 `recipe_4_01.generate_dot.py`。

```python
#!/usr/bin/env python3
"""
生成 Graphviz DOT 文件的脚本
"""
import openai
import os
import re

# 设置 OpenAI
client = openai.OpenAI(api_key=os.getenv('OPENAI_API_KEY'))

# 生成 dot 文件内容
prompt = """Generate a Graphviz DOT file for a fat-tree network topology:

- 4 个脊层路由器: spine1, spine2, spine3, spine4 
- 8 个叶节点路由器: leaf1, leaf2, leaf3, leaf4, leaf5, leaf6, leaf7, leaf8

每个核心路由器连接所有脊层路由器，每个脊层路由器连接所有叶节点路由器。

为每种类型的路由器使用不同的颜色。仅返回 DOT 文件内容。"""

response = client.chat.completions.create(
    model="gpt-4",
    messages=[
        {"role": "user", "content": prompt}
    ],
    temperature=0.3
)

# 提取并保存 dot 文件内容
content = response.choices[0].message.content

# 查找实际 DOT 内容（graph/digraph 与结束大括号之间）
dot_match = re.search(r'((?:di)?graph\s+\w*\s*\{.*\})', content, re.DOTALL)

if dot_match:
    dot_content = dot_match.group(1)
else:
    # 备用：尝试清理内容
    lines = content.split('\n')
    in_graph = False
    dot_lines = []
    for line in lines:
        if 'graph' in line or 'digraph' in line:
            in_graph = True
        if in_graph:
            dot_lines.append(line)
        if in_graph and '}' in line:
            break
    dot_content = '\n'.join(dot_lines)

with open('fat_tree_topology.dot', 'w') as f:
    f.write(dot_content)

print("Generated fat_tree_topology.dot")
```

2. 执行脚本并查看生成的 DOT 文件。

```bash
$ python recipe_4_01_generate_dot.py
Generated fat_tree_topology.dot
$ cat fat_tree_topology.dot
```
输出示例：
```dot
graph topology {
    node [shape=circle];
    // Core routers
    "core1" [color=red];
    // Spine routers
    "spine1" [color=blue];
    "spine2" [color=blue];
    "spine3" [color=blue];
    // Leaf routers
    "leaf1" [color=green];
    "leaf2" [color=green];
    "leaf3" [color=green];
    "leaf4" [color=green];
    "leaf5" [color=green];
    "leaf6" [color=green];
    "leaf7" [color=green];
    "leaf8" [color=green];
    ...
}
```

3. 我们将使用以下脚本生成实际的拓扑结构。

```python
#!/usr/bin/env python3
"""
将 dot 文件转换为 PNG 拓扑可视化的简单脚本
"""
import subprocess
import sys

dot_file = sys.argv[1]
output_file = "topology.png"

# 从 dot 文件生成 PNG
subprocess.run(['dot', '-Tpng', dot_file, '-o', output_file])
print(f"Generated {output_file}")
```

```bash
$ python recipe_4_02_generate_topology.py fat_tree_topology.dot
Generated topology.png
```



![Image 15](img/B32646_15.png)

图 2.15: 拓扑文件

```python
#!/usr/bin/env python3

# 使用 OpenAI API 的 VLAN MOP 生成器
# 生成用于 Cisco IOS VLAN 配置的操作程序（MOP）文档

import openai
import sys
from datetime import datetime

class VLANMOPGenerator:

    def __init__(self, api_key):
        self.client = openai.OpenAI(api_key=api_key)

        """为 VLAN 配置生成 MOP"""
        # 为在 Cisco IOS 设备上配置 VLAN 生成一份操作程序（MOP）文档：
        # VLAN: {vlan_config.get('vlans', [])}
        # Trunk 端口: {vlan_config.get('trunk_ports', [])}
        # 包含：前提条件、配置命令、验证步骤和回滚程序。

        response = self.client.chat.completions.create(
            messages=[
                {"role": "system", "content": "你是一个网络工程师。请为 Cisco IOS VLAN 配置生成专业的 MOP 文档。"}
            ],
            max_tokens=2000,
        )

def main():
    vlan_config = {
        "device_name": "SW-CORE-01",
        "vlans": [
            {"id": 10, "name": "USERS"},
            {"id": 20, "name": "SERVERS"},
        ],
        "trunk_ports": ["GigabitEthernet1/0/1", "GigabitEthernet1/0/2"],
        "access_ports": {
            "GigabitEthernet1/0/10": {"vlan": 10},
            "GigabitEthernet1/0/11": {"vlan": 20},
        }
    }

    api_key = os.getenv('OPENAI_API_KEY')
    if not api_key:
        sys.exit(1)

    generator = VLANMOPGenerator(api_key)
    mop_document = generator.generate_mop(vlan_config)
    timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    filename = f"VLAN_MOP_{timestamp}.txt"

    with open(filename, 'w') as f:
        f.write(mop_document)

    print(f"MOP 已保存至: {filename}")

if __name__ == "__main__":
    main()
```

2. 我们可以执行该脚本来生成 MOP。

```bash
MOP 已保存至: VLAN_MOP_20250720_214956.txt
```

3. 当前目录下应该出现一个新的 `VLAN_MOP_[date].txt` 文件。

```bash
$ cat VLAN_MOP_20250720_214956.txt
操作程序 (MOP) 文档
标题: Cisco IOS 设备上的 VLAN 配置 (SW-CORE-01)
目标: 在 Cisco IOS 设备 (SW-CORE-01) 上配置 VLAN 并将它们分配到相应的端口。

前提条件:
1. Cisco IOS 设备的网络管理员凭据。
2. 确保 trunk 和 access 端口运行正常。

步骤:
1. 使用网络管理员凭据登录 Cisco IOS 设备 (SW-CORE-01)。
```

```bash
SW-CORE-01>enable
```

3. 创建 VLAN 并分配名称：

```bash
SW-CORE-01(config)#vlan 10
SW-CORE-01(config-vlan)#name USERS
SW-CORE-01(config)#vlan 20
SW-CORE-01(config-vlan)#name SERVERS
SW-CORE-01(config-vlan)#exit
SW-CORE-01(config-vlan)#name GUEST
SW-CORE-01(config-vlan)#exit
... [输出已略]
```

此脚本非常直观。在步骤 1 中，脚本从环境变量中获取`OPENAI_API_KEY`，然后结合工程师定义的 VLAN 设置。这部分可以轻松调整或从另一个文件中读取。接下来的部分是 AI 魔法发生的地方。脚本获取`vlan_config`并使用 OpenAI 的`gpt-4`编写一份操作程序文档。这种魔法依赖于 OpenAI 的训练知识。

## 更多内容

# 总结

在下一章中，我们将进入 LLM 最重要的主题之一：提示工程（Prompt Engineering）。下章节见！

# 3 用于获得可靠输出的提示工程

在过去的两个章节中，我们看到了如何利用 LLM 模型回答许多问题。现在我们知道，与 AI 交互很大程度上取决于我们如何“询问”我们需要的内容。例如，在第 1 章中，我们看到提供不同的系统消息会产生不同的响应。这对于我们来说并不意外。毕竟，作为网络工程师，我们花了几十年时间掌握通过 CLI 命令与设备通信的艺术。当我们打开喜欢的终端窗口并 ssh 连接到路由器时，我们实际上打开了一个通信的“窗口”来告诉设备我们要做什么。随着时间的推移，我们逐渐为设备集成更多的通信方法，例如 SNMP 和 API，但最终目标仍然是提供一组指令让设备按照我们的指示执行。

现在随着 LLM 成为我们工具箱中的强大工具，我们需要掌握与模型高效通信的方式，即提示工程。我们可以将提示工程视为模型中等同于“CLI 语法”，但与厂商制定的严格 CLI 语法不同，我们使用自然语言来帮助 AI 理解我们需要做什么。正如格式错误的 BGP 配置可能会导致我们的网络崩溃一样，编写不当的提示词可能会给我们带来危险的响应。

在本章中，我们将介绍一些构建有效提示词的方案：

*   为模型提供指令
*   指定返回格式
*   提供反馈
*   逐步构建提示

准备好了吗？让我们开始吧。

```bash
$ python3 -m venv .venv
```

2. 安装`openai`和`python-dotenv`软件包。

```bash
$ pip install openai python-dotenv
```

3. 使用文本编辑器创建一个`.env`文件并输入你的`OPENAI_API_KEY`。

```bash
$ touch .env #创建文件
$ vim .env #编辑文件
```

某些配方需要特定的额外 Python 软件包，这些包将包含在配方的“准备工作”（Getting Ready）章节中。

# 为 AI 模型提供指令

## 准备工作 (Getting Ready)

## 如何操作……

```python
import os
from dotenv import load_dotenv

client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

# 坏示例 - 模糊且缺乏方向

response_vague = client.chat.completions.create(
    messages=[{"role": "user", "content": vague_prompt}]
)

print(response_vague.choices[0].message.content)
print("\n" + "="*50 + "\n")
```

2. 执行脚本查看反馈，你的结果可能会略不同。

```bash
Vague Response:
边界网关协议 (BGP) 是一种用于自治系统 (AS)之间路由的复杂路由协议。以下是在你的路由器上配置 BGP 的基本示例：

1. 你的网络的自治系统编号 (ASN)
2. 你将添加到 BGP 网络的路由器 IP 地址

Router#configure terminal

Router(config)#router bgp <ASN> // 进入你的 ASN
Router(config-router)#network <network> // 指定你想要宣告的网络

Router(config-router)#neighbor <neighbor IP address> remote-as <AS of neighbor> // 定义相邻的 BGP 路由器并指定其 AS 号
Router(config-router)#exit

Router#copy running-config startup-config // 将配置保存到内存以便在重启后依然保留
```

将 `<ASN>`, `<network>`, `<neighbor IP address>` 和 `<AS of neighbor>` 替换为实际值。

请记住，每个路由器的配置将根据你的网络拓扑和大小而不同。在尝试实施 BGP 之前，对你的网络现有配置良好的理解。

请注意，错误配置 BGP 可能对你的网络产生严重后果，因此在进行更改时务必小心，如果你不确定过程中的任何步骤，请考虑咨询网络专家。

在尝试在网络上实施 BGP 之前，请彻底学习 BGP 及相关主题。有很多书籍、网站和正式培训课程可以在这提供帮助。

在将配置更改应用于生产设备之前，请考虑先在测试环境中进行练习。在推广到整个网络之前，测试新设置的性能和可靠性是一个主意。

最后，确保你的配置符合所有安全最佳实践。BGP 与任何其他互联网协议一样，如果不妥善安全，很容易被滥用。确保你的 BGP 路由器是安全的，并经过配置以限制攻击和误配置的风险。

==================================================
```

```python
import os
from dotenv import load_dotenv

client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

directed_prompt = """
我需要你为 Cisco ASR9000 路由器生成一个 BGP 配置，要求如下：

1. 与两个上游 ISP (AS 65001 和 AS 65002) 建立 EBGP 对等
2. 在两个上游之间设置负载均衡
3. 为 BGP 会话包含基础的安全加固
4. 网络细节：
    - ISP1 (AS 65001): 192.168.1.1
    - ISP2 (AS 65002): 192.168.2.1
5. 以 IOS-XR 格式提供配置并带有解释性注释。
"""

response_directed = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": directed_prompt}]
)

print("Directed Response:")
print(response_directed.choices[0].content)
```

4. 执行脚本查看反馈。

```bash
$ python 3_1_directed_prompt.py

Directed Response:

好的，这里是根据要求在 ASR9000 上运行 IOS-XR 的基本配置：

! ASR 配置

router bgp 65100
  bgp router-id 192.168.0.1
  address-family ipv4 unicast
    ! 定义 IPv4 单播族上下文
    ! 在两个等代价路径之间启用负载均衡
  neighbor 192.168.1.1
    remote-as 65001
    ! 设置 ISP1 的远程 AS 为 65001
    address-family ipv4 unicast
      send-community
      ! 向邻居发送社区属性
      maximum-prefix 1
  neighbor 192.168.2.1
    ! 为 ISP2 配置 BGP 邻居
    ! 设置 ISP2 的远程 AS 为 65002
    address-family ipv4 unicast
      ! 与 ISP2 启用 IPv4 单播
      ! 向邻居发送社区属性
      maximum-prefix 1
      ! 将从 ISP2 接收的前缀限制为 1

route-policy RECEIVE-DEFAULT-ROUTE
  if destination in (0.0.0.0/0) then
    pass
  else
  endif
end-policy

router bgp 65100
  neighbor 192.168.1.1
    address-family ipv4 unicast
      ! 应用路由策略仅从 ISP1 接受默认路由
  neighbor 192.168.2.1
    address-family ipv4 unicast
      ! 应用路由策略仅从 ISP2 接受默认路由
!

现在让我们加固 BGP 安全：

```cisco
router bgp 65100
```

## 工作原理……

引导性提示词效果更好，因为它们：

*   **设定角色**：我们告诉模型以“高级网络工程师”的身份进行回复。
*   **定义范围**：我们定义了清晰的范围，例如“Cisco ASR9000 路由器”、带有上游路由器的 ASN 以及`EBGP`对等。
*   **提供上下文**：我们还提供了网络细节。

提示词中给出的方向是稍微改进提示词就能让我们收到的结果产生巨大差异的地方。

## 更多内容……

我们可以通过负向指令进一步改进提示词，例如：

“不要使用路由反射（route-reflection），不要包含已弃的`IOS-XR`命令。”

我们还可以包含多个步骤：

“1. 首先，分析当前的建议配置。2. 然后，识别任何安全漏洞”

## 另见……

# 指定返回格式

在本配方中，让我们尝试不同的返回格式。

我们需要安装额外的 Python 包`pyyaml`。我们还将在环境变量中导出`OPENAI_API_KEY`。

```bash
$ pip install pyyml

$ export OPENAI_API_KEY="your-openai-api-key"
```

## 如何操作……

1. 创建脚本，提供一个基础场景并请求带有指令的`JSON`返回格式。



## 基础场景

我有一个包含以下设备的网络：
- 2 台核心交换（`Cisco Catalyst 9400`）
- 4 台分布交换机（`Cisco Catalyst 9300`）

为此网络创建一个清单文档。

### 请求 JSON 格式

```python
json_prompt = base_scenario + """

- 每个设备应包含：hostname, model, role, mgmt_ip
- 按角色（core, distribution, access）对设备分组
- 包含一个带有总数的摘要部分
仅提供 JSON 输出，不要有额外的文本。
"""
response_json = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": json_prompt}]
)

print("JSON Format Response:")
print(response_json.choices[0].message.content)
```

### 执行脚本并观察输出

```bash
$ python 3_2_Return_JSON_Format.py
JSON Format Response:
{
  "network_inventory": {
    "core": [
        "hostname": "core_switch_1",
        "model": "Cisco Catalyst 9400",
        "role": "core",
      },
      {
        "model": "Cisco Catalyst 9400",
        "role": "core",
        "mgmt_ip": "192.168.1.2"
    ],
    "distribution": [
      {
        "model": "Cisco Catalyst 9300",
        "role": "distribution",
      },
      {
        "hostname": "distribution_switch_2",
        "role": "distribution",
        "mgmt_ip": "192.168.1.4"
      <output skipped>
    ],
    "access": [
        "hostname": "access_switch_1",
        "model": "Cisco Catalyst 9200",
        "mgmt_ip": "192.168.1.7"
      },
      {
        "model": "Cisco Catalyst 9200",
        "role": "access",
      }
      <output skipped>
    ],
    "summary": {
      "total_core": 2,
      "total_distribution": 4,
    }
  }
```

### 请求 YAML 格式

```python
yaml_prompt = base_scenario + """

结构如下：
- 按角色对设备分组
- 确保可以直接用于 Ansible playbook

仅提供输出，不要有额外的文本。
"""
response_yaml = client.chat.completions.create(
    messages=[{"role": "user", "content": yaml_prompt}]
)

print(response_yaml.choices[0].message.content)
```

```bash
YAML Format Response:
```yaml

all:

    core:

      hosts:

          ansible_host: 192.168.1.1

        core_switch_2:

          ansible_host: 192.168.1.2

        os_type: 'ios'

        default_credentials:

          password: 'password'

    distribution:

      hosts:

          ansible_host: 192.168.1.3

        distribution_switch_2:

      <output skipped>

    access:

      hosts:

          ansible_host: 192.168.1.7

        access_switch_2:

    <output skipped>

```
```

## 给出示例

以团队中一位新入职的工程师同事作为示例。如果我们想让这位能力很干的同事配置路由器，我们可能会向他参考另一台类似的路由器，对吧？“嘿 Bob，看下`lax-cor-r1`，然后尝试配置`lax-cor-r2`，它们除了环回接口、IP 地址、BGP 路由器 ID 等之外，都非常相似。”这就是我们向 AI 给出示例的意思，我们希望给它提供可以遵循的具体模板。

与之前的配方设置相同，确保已导出`OPENAI_API_KEY`环境变量（如果没有）。

### 如何操作…

1. 从提供标准的接口描述开始，然后请求输出。

```python
#!/usr/bin/env python3
import os

from openai import OpenAI
client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

task_description = """

描述应该信息丰富且简洁，遵循特定的格式。
以下是正确格式和风格的示例：

Example 1:
Input: GigabitEthernet0/1 connected to SW-DIST-01 port Gi0/5

Output:
description UPLINK_TO_SW-DIST-01_Gi0/5_10GB_CORE

Example 2:
Input: GigabitEthernet0/2 connected to user workstation in room 205

Output:
description ACCESS_ROOM205_WORKSTATION_1GB_USER

Example 3:
Input: TenGigabitEthernet0/1 connected to firewall outside interface
Output: interface TenGigabitEthernet0/1

现在，将相同的格式和风格应用于此输入：

"""
    model="gpt-4",
    messages=[{"role": "user", "content": task_description}]
)
print("Generated Interface Description:")
```

```bash
Generated Interface Description:

description IP_PHONE_CONFROOMB_1GB_VOICE
```

2. 利用提供的示例，向更复杂的 VLAN 配置示例进阶。

```python
#!/usr/bin/env python3

from openai import OpenAI

# Task 描述
task_description = """

按照我们的公司标准生成 VLAN 配置。
每个 VLAN 应该有适当的命名、描述和关联的服务。

Example 1:

Input: VLAN for management network, ID 100, subnet 10.100.0.0/24

Output:
vlan 100
name USERS_WORKSTATION
state active

!
interface Vlan100
ip address 10.100.0.1 255.255.255.0

no ip redirects
no ip proxy-arp

Example 2:
Input: VLAN for guest WiFi, ID 200, subnet 192.168.200.0/24

Output:
vlan 200
name GUEST_WIFI
state active
!
interface Vlan200
description Guest WiFi Network
ip address 192.168.200.1 255.255.255.0
ip helper-address 10.10.10.100
ip access-group GUEST_FILTER in

no ip redirects
no ip proxy-arp

Example 3:
Input: VLAN for IP cameras, ID 150, subnet 10.150.0.0/24

Output:
"""
response = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": task_description}]
)

print(response.choices[0].message.content)
```

3. 执行并接收输出。

```bash

Generated VLAN Configuration:

vlan 150
name IP_CAMERAS
state active
!
interface Vlan150
description Cameras
ip address 10.150.0.1 255.255.255.0
ip helper-address 10.10.10.100

no ip proxy-arp
```

## 工作原理…

脚本提供了以下内容：

- AI 模型尝试理解模式。
- AI 模型识别一致性。
- AI 模型遵循示例。
- AI 模型遵循规则。
- AI 模型遵循要求。

### 两轮迭代过程

1. 创建以下用于两轮迭代过程的响应的脚本。

```python
#!/usr/bin/env python3

import os

from openai import OpenAI
client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

print("=== ITERATION 1: Initial Request ===")

initial_prompt = """

我们需要控制内部网络和互联网之间的流量。

"""

response = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": initial_prompt}]
)

response1 = response.choices[0].message.content

print(response1)

print("\n" + "="*60 + "\n")

print("=== ITERATION 2: After Feedback ===")

feedback = """之前的 ACL 太简单了。请根据以下要求进行改进：

1. 我们的内部网络是 192.168.1.0/24
2. 允许出站网络流量（HTTP/HTTPS）
3. 允许向 8.8.8.8 查询 DNS
4. 阻止访问 Facebook 和 Twitter
5. 显式拒绝其他所有内容

请根据这些要求创建一个更具体的 ACL。
"""

# 使用反馈反馈继续对话
conversation = [
    {"role": "user", "content": initial_prompt},
    {"role": "assistant", "content": response1},
    {"role": "user", "content": feedback},
]

response = client.chat.completions.create(
    model="gpt-4",
    messages=conversation,
)

response2 = response.choices[0].message.content

print(feedback)

print("\nImproved response:")
print(response2)
```

2. 执行并查看结果。

```bash
$ python 3_4_feedback.py

Response:
访问控制列表 (ACL) 是一组用于控制网络流量和减少网络攻击定义的规则。以下是一个基础访问控制列表示例。

假设：
- 内部网络：192.168.1.0/24
- 互联网：任何其他 IP

1. 允许从任何源到任何目的端口 80 (HTTP)：
`Access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 80`

2. 允许从任何源到任何目的端口 443 (HTTPS)：
`Access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 443`

3. 允许 ICMP 流量：
`Access-list 101 permit icmp 192.168.1.0 0.0.0.255 any`

4. 允许 DNS 流量：
`Access-list 101 permit udp 192.168.1.0 0.0.0.255 any eq 53`

5. 用于 SMTP 邮件：
`Access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 25`

6. 用于 FTP 数据和控制流量：
`Access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 20`
`Access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 21`

7. 拒绝所有其他流量：
`Access-list 101 deny ip any any`

创建 ACL 后，必须将其应用到连接互联网的接口上。假设接口为 FastEthernet 0/0，在路由器上应用的命令将为：
`int Fa0/0`

这是一个用于办公室网络流量控制的基础 ACL。具体需求可能会有所不同，因此可以根据要求进行更改。应采取安全措施，因为错误的 ACL 可能会拒绝重要的流量。

=== ITERATION 2: After Feedback ===

提供的反馈：
1. 我们的内部网络是 192.168.1.0/24
2. 允许出站网络流量（HTTP/HTTPS）
3. 允许 DNS 查询到 8.8.8.8
4. 阻止访问 Facebook 和 Twitter
5. 显式拒绝所有内容。请创建一个更具体的 ACL。

改进后的响应：
1. 允许从任何源到任何目的 Web 流量的 HTTP (80) 和 HTTPS (443)：
`Access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 80`
`Access-list 101 permit tcp 192.168.1.0 0.0.0.255 any eq 443`

2. 允许 DNS 查询到 8.8.8.8：
`Access-list 101 permit udp 192.168.1.0 0.0.0.255 host 8.8.8.8 eq 53`

3. 阻止到社交媒体网站的流量有点困难，因为它们有多个 IP 地址并且在不断变化。如果可能，请可以使用按 URL 阻止的防火墙，或使用过滤网页内容的解决方案。尽管如此，假设 Facebook (31.13.71.36) 和 Twitter (104.244.42.129) 的示例 IP 地址如下：
`Access-list 101 deny tcp 192.168.1.0 0.0.0.255 host 31.13.71.36 eq 443`
`Access-list 101 deny tcp 192.168.1.0 0.0.0.255 host 104.244.42.129 eq 80`
`Access-list 101 deny tcp 192.168.1.0 0.0.0.255 host 104.244.42.129 eq 443`

4. 拒绝所有其他流量：
`Access-list 101 deny ip any any`
```

```python
# 完整提示包含所有层级要求
complete_prompt = layer3 + layer4 + layer5
response = client.chat.completions.create(
    model="gpt-4",
    messages=[{"role": "user", "content": complete_prompt}]
)
print(response.choices[0].message.content)
```

```python
layer4 = """
SECURITY REQUIREMENTS:
- 启用 SSH 用于远程管理，禁用 Telnet
- 配置强密码并启用 secret
- 阻止访问社交媒体网站 (Facebook, Twitter)
- 安全 SNMP 团体字符串
在设备设置中包含所有安全配置。
"""
```

## 更多内容…

### 添加更多监控功能

```python
# Layer 5: Monitoring requirements (NEW)

layer5 = """
MONITORING REQUIREMENTS:
- 启用 SNMP 用于网络监控
- 配置 syslog 将日志发送到 192.168.10.100
- 启用接口监控和告警
- 配置 NTP 以获取准确的时间戳 (pool.ntp.org)
- 添加 CDP/LLDP 用于网络发现
包含所有监控配置并确保日志设置恰当。
"""
```



## 工作原理……

* 建立上下文。
* 逐步增加复杂度。
* 在每一层纠正错误。

关键在于意识到 AI 模型更擅长通过层级结构一次处理一件事（就像它们的人类一样）。

## 还有更多……

一旦我们熟悉了这些独立的脚本，我们也可以将它们合并成单个脚本，而不会丢失层级结构。

## 网络工程师的优势

许多专家预测 AI 革命将比云计算规模更大，有些人甚至认为它将比互联网更大。在这种情况下，它将威胁**每一个人**的工作，而不仅仅是网络工程师。然而，如果我们使用之前的网络自动化和云计算示例，网络工程师的角色会随着新技术而演进，某些功能会被取代，而其他功能将不断进化，并引入一些我们以前从未见过的新工作。虽然没有人能预见未来，但我认为网络专业人员确实具有显著优势，正如本章中通过提示词工程的概念所示：

* 网络工程师通常需要指定精确的需求和约束条件。
* 网络工程师需要提供清晰的示例和文档。
* 网络工程通常需要多次迭代和完善的解决方案。

这些技能听起来熟悉吗？它们应该是，因为它们与我们在本章中讨论的用于构建良好提示的技能相同。成为优秀网络工程师的技能与在提示词工程方面与 AI LLM 模型工作所需的技能是相同的。本章介绍的技能并不是为了取代你的网络知识，而是为了放大它们。正如我们在配方中看到的，提供 BGP、VLAN 和安全信息可以在 AI 的帮助下显著缩短解决方案的时间。

让我们昂首挺胸，学习如何利用 AI 为我们所用！

# 总结

我们应该注意，提示词工程并不是一种新技术，它是在新的界面上应用现有的网络工程技能，这个界面作为通往 AI LLM 伙伴的网关。如果我们把网络工程技能中同样的系统性思维、精确需求和迭代过程应用于与 AI 模型的工作中，我们就可以准备好了。

# 4. 网络工程中的本地 AI LLM 场

* 数据主权：在某些行业（如医疗、金融和法律），尽管供应商承诺不使用你的数据进行训练，你仍倾向于将数据（包括网络信息）存储在自己的环境中。
* 合规性要求：GDPR、HIPAA 和 SOX 合规要求物理隔离或本地部署。
* 延迟：云端解决方案为每次交易增加了往返路由延迟，这对于实时应用程序可能是无法接受的。

正如我们在第一章中看到的，我们可以为网络自动化任务运行本地大语言模型。在本章中，我们将扩展已有的知识，并查看更多使用本地 AI LLM 模型的配方。本章将涵盖以下配方：

* 微调 LLM 资源
* 使用 CodeLlama 生成网络代码示例
* 使用 Llama 2 Uncensored 模型进行高级代码生成
* 使用本地 LLM 生成网络文档

让我们从技术要求开始。

# 技术要求

在本章中，我们将重点使用带有 docker 容器的 Linux 系统运行必要的配方。在以下示例中，我们使用 `Ubuntu 24.04` `LTS` 并直接从 Docker 安装 Docker，而不是来自 Ubuntu 默认仓库：

```
$ lsb_release -a

No LSB modules are available.

Description:    Ubuntu 24.04.2 LTS

Release:        24.04

Codename:       noble

$ sudo apt update

$ sudo apt install apt-transport-https ca-certificates curl software-properties-common

$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

$ apt-cache policy docker-ce

$ sudo apt install docker-ce

$ sudo apt install docker-compose
```

让我们验证 docker 进程正在运行：

```
$ sudo systemctl status docker

● docker.service - Docker Application Container Engine

   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: enabled)
```

我们还可以可选地将用户添加到 docker 组，这样以后在执行 docker 命令时就不需要输入 `sudo`（我们需要注销并重新登录以便新设置生效）：

```
$ sudo usermod -aG docker ${USER}
```

```bash
$ docker-compose --version
docker-compose version 1.29.2, build unknown
```

## 4.1 下载代码 Ollama 模型

请注意：与大多数“自建还是购买”的决策一样，在运行本地 LLM 模型时，我们选择了“自建”。因此，我们需要维护硬件和软件组合。首先要考虑的是使用哪种模型。可本地下载的模型在用途上通常更加宽松，范围从 100% 开源到闭源但免费使用。请务必仔细检查你想要使用的模型的使用指南。

本章的配方需要一台本地机器（物理或虚拟）来运行 docker 容器。我建议使用至少具有 16G RAM 和至少 50GB 存储空间的机器。对于这些配方，我使用了一个 AWS `t2.xlarge` 实例（[https://aws.amazon.com/ec2/instance-types/t2/]），它拥有 4 个 vCPU、16G RAM 和 100GB EBS 存储。

## 如何操作……

1. 创建名为 `ch04` 的文件夹并进入该文件夹：

```
$ mkdir ch04 && cd ch04
```

2. 创建 `docker-compose.yml` 文件：

```yaml
networks:
  ollama:
    ollama:
      image: ollama/ollama
      networks:
      volumes:
        - ./data/ollama:/root/.ollama
      ports:
      restart: unless-stopped
      # Linux 特有的资源限制
      deploy:
          limits:
            memory: 8G
      ollama-webui:
        image: ghcr.io/ollama-webui/ollama-webui:main
        volumes:
          - ./data/ollama-webui:/app/backend/data
        depends_on:
          - ollama
        ports:
          - 3002:8080
        environment:
          - 'OLLAMA_API_BASE_URL=http://ollama:11434/api'
        extra_hosts:
          - host.docker.internal:host-gateway
        networks:
          - ollama
        restart: unless-stopped
```

3. 使用 `docker-compose` 启动容器：

```bash
$ sudo docker-compose up
```

4. 检查容器名称，默认情况下，它使用 `<文件夹名>_<容器名>;` 的格式。在我们的示例中，它是 `ch04_ollama_1` 和 `ch04_ollama-webui_1`。这是我们需要在容器上执行命令时需要使用的名称：

```bash
$ sudo docker ps
CONTAINER ID   IMAGE                                    COMMAND               CREATED          STATUS         PORTS                                             NAMES
fe33a3ecadac   ghcr.io/ollama-webui/ollama-webui:main   "bash.sh"       18 minutes ago   Up 5 minutes   0.0.0.0:3002->8080/tcp, [::]:3002->8080/tcp       ch04_ollama-webui_1
232ae96fe447   ollama/ollama                            "/bin/ollama serve"   18 minutes ago   Up 5 minutes   0.0.0.0:11434->11434/tcp, [::]:11434->11434/tcp       ch04_ollama_1
```

5. 列出 `ollama` 容器上的当前模型，它应该是一个空列表：

```bash
$ sudo docker exec -it ch04_ollama_1 ollama list

NAME    ID    SIZE    MODIFIED
```

6. 我们也可以通过 `localhost` 端口 `3002` 或通过 IDE 的端口转发访问 `ollama-webui`。由于这是第一次访问该实例，我们需要创建一个账户。

![Image 1](img/B32646_4_1.png) 图 4,1: ollama-webui 页面

7. 一旦登录后，如果我们使用“select a model”的下拉菜单，我们也会看到一个空列表。

![Image 2](img/B32646_4_2.png) 图 4.2: 模型列表

8. 一旦登录后，如果我们使用“select a model”的下拉菜单，我们也会看到一个空列表。

![Image 2](img/B32646_4_2.png) 图 4.2: 模型列表




7. 一旦登录后，如果我们使用“select a model”的下拉菜单，我们也会看到一个空列表。
8. 一旦登录后，如果我们使用“select a model”的下拉菜单，我们也会看到一个空列表。

![Image 2](img/B32646_4_2.png)

图 4.2: 模型列表

> 注：由于原文文本在第 8 点后存在大量重复和乱码，我仅为您翻译了可识别的部分。

## 4.2 Fine Tuning LLM Resources

We will quickly find out housing local LLM models puts resource constraints for memory, storage, and CPU cycles. We are in this constant battle to balance available resources and resources given to LLM models.

### 准备工作

### 如何操作……

```bash
$ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
  "model": "codellama:7b",
  "stream": false
}'
```

<使用 `Ctrl+C` 终止>

2. 使用 `Ctrl+C` 停止 `docker compose` 服务。

```yaml
memory: 12G
reservations:
  image: ghcr.io/ollama-webui/ollama-webui:main
  depends_on:
    - ollama
  environment:
    - 'OLLAMA_API_BASE_URL=http://ollama:11434/api'
networks:
```

```bash
$ sudo docker-compose up
Starting ch04_ollama-webui_1 ... done
Attaching to ch04_ollama_1, ch04_ollama-webui_1
<output skipped>
```

5. 现在执行相同的 `curl` 命令应该会降低延迟。

```json
{
  "model": "codellama:7b",
  "prompt": "Write a Python script using netmiko to connect to Cisco routers and retrieve device information."
}
```

```json
{"model":"codellama:7b","created_at":"2025-07-26T18:42:35.180942975Z","response":"```\nimport json\nfrom netmiko import ConnectHandler\n\ncisco_routers = ['192.168.100.1', '192.168.100.2']\n\n# Create a dictionary to store the device information\ndevice_info = {}\n\nfor router in cisco_routers:\n    # Set up the connection with the router using netmiko\n\n"}
```

### 工作原理……

### 更多……

* Ollama Docker 官方文档：[`github.com/ollama/ollama/blob/main/docs/docker.md`](https://github.com/ollama/ollama/blob/main/docs/docker.md)
* Ollama Docker Compose 示例仓库：[`github.com/rolfhelder/ollama-docker-compose`](https://github.com/rolfhelder/ollama-docker-compose)

## 准备工作

## 如何实现……

```bash
$ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
  "stream": true
}'
```

```bash
$ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
  "stream": true
}'
```

```json
{"model":"codellama:7b","created_at":"2025-07-26T18:56:16.184958516Z","response":"Here","done":false}
{"model":"codellama:7b","created_at":"2025-07-26T18:56:16.184958516Z","response":""}
```

3. 提供更具体的数据收集需求。

```bash
$ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
  "stream": true
}'
```

```bash
$ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
  "model": "codellama:7b"
}'
```

3. 提供更具体的数据收集需求。

```json
{
  "model": "codellama:7b"
}
```

```bash
{
  "prompt": "Write a Python script using netmiko that connects to multiple Cisco routers in parallel. Use threading or asyncio for concurrent connections.",
  "stream": true
}
```

5. 添加日志。

```json
{
  "model": "codellama:7b",
  "prompt": "Write a Python script using netmiko that connects to multiple Cisco routers in parallel. Use threading or asyncio for concurrent connections."
}
```

```bash
$ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
  "stream": true
}'
```

7. 最终提示词。

```json
{
  "model": "codellama:7b"
}
```

* 来自第 3 章的第五个教程：`逐步构建 Prompt`。

部署本地 AI 大语言模型（LLM）的好处之一是可以自由“尝试”新模型。当我们使用托管模型（如 `OpenAI` 或 `Anthropic`）时，我们使用的是他们提供的有时带有主见的版本。当我们部署自己的模型时，就可以尝试“野外”环境的模型。在本教程中，我们将使用 Llama 2 Uncensored（未审查）模型进行测试。

* 对于复杂的网络配置，脚本生成不受限制。
* 我们可能不希望有任何内容过滤限制，例如针对授权渗透测试的安全评估。
* 模型在生成涉及敏感系统细节的代码时，不会施加任何限制。

**重要信息：** 俗话所说，“能力越大，责任越大。”使用未审查模型可能是我们希望确保对安全影响采取额外预防措施的场景之一。毕竟，这是一个“未经过审查”的模型。

我们将从下载 Llama 2 Uncensored 模型开始。

## 准备工作

我们可以使用 CLI（命令行界面）或 WebUI 来拉取 Llama 2 Uncensored 模型。

```bash
$ sudo docker exec -it ch04_ollama_1 ollama pull llama2-uncensored
pulling manifest
pulling 6aa74acf170f: 100% ██████████████████████ 3.8 GB                       
<output skipped>
verifying sha256 digest
writing manifest
success
```

## 如何操作…

1. 使用以下 Prompt 进行高级网络安全自动化。

```
为 Linux 系统创建一个用 Python 编写的综合网络渗透测试框架，包括：
1. 集成 nmap 的自动网络发现
2. 使用自定义漏洞利用模块进行漏洞扫描
3. 带有智能限流的凭据暴力破解
4. 使用 Scapy 进行自定义数据包构造以协议模糊测试（fuzzing）
5. 使用 JSON 和 XML 报告的结果聚合
6. 与 Linux 日志系统和 syslog 集成
7. 针对 Linux 进程管理优的多线程执行

包含适当的命令行界面、配置文件支持以及用于扩展的模块化架构。
```

2. 生成高级网络操控工具

```
开发一个基于 Linux 的网络流量操控工具包，能够：
1. 使用原始套接字（raw sockets）实时拦截并修改网络数据包
2. 实现隐蔽通信的自定义协议隧道
3. 创建用于基础设施验证的网络压力测试工具
4. 构建用于测试的自定义网络协议实现
5. 包含流量分析和模式识别能力
6. 支持多个网络接口和 VLAN 操作
7. 与 iptables 和 Linux 网络栈集成

针对 Linux 内核网络功能进行优化，并对特权操作包含妥当的错误处理。
```

3. 生成高级自动化和规避技术

```
为 Linux 构建一个高级网络自动化框架，包括：
1. 避免被检测的隐身网络扫描技术
2. 针对常见网络漏洞的自动利用链
3. 动态负载生成和交付系统
4. 网络拓扑映射和服务端指纹识别
5. 带有执行摘要的自动报告生成
6. 与主流安全框架（Metasploit, Burp Suite）集成
7. 支持跨多个 Linux 主宿机的分布式扫描

包含适当的权限提升处理、网络命名空间利用和基于容器的隔离。
```

4. 生成合规与审计自动化

```
为 Linux 环境开发一套网络安全合规自动化套件，其功能能够：
1. 自动进行跨网络设备的安全策略验证
2. 为行业标准（SOX, PCI, HIPAA）生成合规报告
3. 对常见的安全配置错误实施自动修复
4. 提供持续监控和告警能力
5. 与 SIEM 系统和日志聚合平台集成
6. 支持多厂商网络设备管理
7. 包含加密凭据管理和安全通信

针对企业 Linux 环境进行优化，具备适当的日志记录、监控和集成能力。
```

## 工作原理…

Llama 2 Uncensored 模型为安全测试、渗透测试和其他复杂技术提供了不受限的代码生成。对于高级用户来说，这与标准模型相比是一个明显的优势。

## 更多内容…

* Llama 2 7B Uncensored: [`huggingface.co/georgesung/llama2_7b_chat_uncensored`](https://huggingface.co/georgesung/llama2_7b_chat_uncensored)
* WizardLM Uncensored: [`huggingface.co/QuixiAI/Wizard-Vicuna-7B-Unensored`](https://huggingface.co/QuixiAI/Wizard-Vicuna-7B-Unensored)

## 4.5 使用自定义网络数据微调模型

本教程将使用我们特定的网络环境数据对现有的本地 LLM 进行自定义，以提高其与我们的相关性。我们将使用简单的模拟 Cisco 配置数据来演示步骤，但请记住在生产环境中你应该使用自己的网络数据。为了保持简单，我们还将使用简单的数据集，在生产环境中，数据越多越好。

## 准备工作

请确保你已经完成了第一个教程。

## 如何操作…

1. 创建一个 `SimpleModelfile`。

```
FROM codellama:7b
# 参数
PARAMETER temperature 0.1
PARAMETER top_p 0.9
# 系统提示词
SYSTEM """
你是专注于 Cisco 设备的专家
始终提供
- 准确的 Cisco 语法
- 妥当的 Python 错误处理
- 清晰简洁的解决方案
"""
# 响应模板
TEMPLATE """{{if .System}}{{.System}}\n{{if .Prompt}}{{.Prompt}}\n{{end}}Assistant: {{.Response}}
```

2. 将模型文件拷贝到 ollama 容器。

```bash
$ docker cp Simpleelfile ch04_ollama_1:/tmp/
Successfully copied 3.07kB to ch04_ollama_1/
```

3. 创建自定义模型。

```bash
$ docker exec -it ch04_ollama_1 ollama create simple-network -f /tmp/Simpleelfile
gathering model components
using existing layer sha256:3a43f93b78ec50f7c4e4dc8bd1cb3fff5a900e7d574c51a6f7495e484860dac
using existing layer sha256:8c17c2ebb0ea011be9981cc3922dbca8fa61e828c5d3f44cb6ae342bf80460b
using existing layer sha256:590d74a5569b8a20eb2a8b0aa869d1d1d3faf6a7fdda1955ae827073c7f50fc
using layer sha256:c5b2c3cf05e314b46d7ea7459f1c1991e2a9bf633e9f719b047c751562043dd5
using existing layer sha256:4953dc72d86d1f737be24538809149b59c604ca94598c200edb01cc8419c7f4
using existing layer sha256:138527391cc130fa2ef2f1000760d340edbeb0c41a828a5857b5d84fc292ec08
writing manifest
success
```

4. 验证模型已创建。

```bash
$ docker exec -it ch04_ollama_1 ollama list
NAME                        ID              SIZE      MODIFIED     
simple-network:latest       2bd567b1fa19    3.8 GB    42 seconds ago  
llama2-uncensored:latest    44040b922233    3.8 GB    3 hours ago     
codellama:7b                8fdf8f752f6e    3.8 GB    25 hours ago 
```

5. 让我们查询模型以生成 VLAN 配置。

```bash
$ curl -X POST http://localhost:11434/api/generate -H "Content-Type: application/json" -d '{
  "model": "simple-network",
  "prompt": "configure vlan 20",
  "stream": true
}' | jq -r '.response' | tr -d '\n'
```

<output 已跳过>

以下是 VLAN 20 的示例配置。

```
switch(config)#vlan 20
switch(config-vlan)#name MyVLAN
switch(config-vlan)#exit
switch(config)#interface vlan 20
switch(config-if)#ip address 192.168.10.1 255.255.255.0
```

以下是 VLAN 30 的示例配置。

```
switch(config)#vlan 30
switch(config-vlan)#name MyVLAN
switch(config-vlan)#exit
switch(config)#interface vlan 30
switch(config-if)#ip address 192.168.11.1 255.255.255.0
```

以下是 VLAN 40 的示例配置。

```
switch(config)#vlan 40
switch(config-vlan)#name MyVLAN
switch(config-vlan)#exit
```




## 6. 我们也可以从 WebUI 选择新创建的模型并生成配置。

![Image 5](img/B32646_4_5.png)

图 4.5: Ollama-WebUI 中的新模型交互

## 工作原理……

让我们拆解这些步骤，看看我们如何将基础 CodeLlama 模型转换为使用我们的网络数据进行自定义的模型。当我们要求原始的通用 CodeLlama 模型为 'vlan 50' 生成配置时，它可能会生成通用的配置语法并混合不同厂商的命令。

在我们的模型文件中，我们使用自定义系统注入来更改模型的行为：

```text
# 系统上下文
SYSTEM """

你是一个专注于 Cisco 设备的网络自动化专家。

你的专业包括：
- Cisco IOS 配置语法
- VLAN 配置与管理
- 带 IP 地址的接口配置
- 基础路由配置
- 使用 netmiko 库进行 Python 自动化
- 网络设备监控与验证

始终提供：
- 准确的 Cisco IOS 语法
- Python 脚本中适当错误处理
- 清晰简洁的配置
- 可运行的自动化代码

专注于为网络工程师提供实用且即插用的解决方案。

"""
```

系统上下文建立了背景，并引导模型像网络工程师一样思考，同时设定了接收 Cisco 特有语法和规范训练数据的预期。

参数调优对输出也起了作用：

```text
PARAMETER temperature 0.1
PARAMETER top_p 0.9
PARAMETER repeat_penalty 1.1
```

温度（`temperature`）越低，响应的确定性越高，这是我们所偏好的，因为我们希望输出是精确的而不是创造性的。`Top_p` 指定了最可能的词并减少了随机性。`Repeat_penalty`（重复惩罚）防止模型陷入循环或重复命令。

响应模板指定了返回的输出格式：

```text
TEMPLATE """{{ if .System }}<|start_header_id|>system<|end_header_id|>

{{ .System }}<|eot_id|>{{ end }}{{ if .Prompt }}<|start_header_id|>user<|end_header_id|>

Network Task: {{ .Prompt }}

<|eot_id|>{{ end }}<|start_header_id|>assistant<|end_header_id|>

{{ .Response }}<|eot_id|>"""
```

该模板确保了相同结构的格式一致性，而 `Network Task` 前缀提醒了模型其角色。

## 还有更多……

我们可以调整参数来实验不同的模型参数：

```text
PARAMETER temperature 0.05  # 更加确定
PARAMETER top_p 0.95        # 更具创造性
PARAMETER num_ctx 8192      # 更长的上下文窗口
```

正如我们在之前的食谱中看到的，我们可以通过 `temperature` 参数更改响应的确定性水平。此外，`top_n` 和 `num_ctx` 分别允许我们更改创造力和上下文窗口。

# 4.6 使用本地 LLM 生成网络文档

本食谱演示了如何从配置自动生成网络文档。

## 准备工作

请确保你已经完成了此部分的第一个食谱。

## 如何操作……

1. 可选使用 `bgp_config.py` 生成示例配置，或使用书籍仓库中的现有配置。

```bash
$ cat configs/bgp-config.txt
hostname Router-BGP-01
router bgp 65001
  bgp router-id 10.1.1
```

2. 查看文档文件。

![Image 6](img/B32646_4_6.png)

图 4.6: 简单的 BGP 文档

## 工作原理……

让我们拆解 `bgp_doc_generator.py` 文件。第一步是读取 BGP 配置 `config/bgp_config.txt`。我们为 LLM 准备了如下提示词（`prompt`）：

```python
prompt = f"""Analyze this BGP configuration:

{config}

Provide:

1\. AS number and Router ID

2\. BGP neighbors and their AS numbers

3\. Networks being advertised

Keep it simple and clear."""
```

我们将使用 `subprocess` 模块查询本地 LLM，并将结果存储在变量中：

```python
# Query the LLM
result = subprocess.run([
    'docker', 'exec', 'ch04_ollama_1',
    'ollama', 'run', 'codellama:7b', prompt
], capture_output=True, text=True)
```

脚本将获取结果并保存到文档文件中：

```python
# Create documentation
doc_content = f"""# BGP Configuration Documentation

## Analysis

{result.stdout}

## Configuration

"""

# Save documentation
with open("bgp_documentation.md", 'w') as f:
    f.write(doc_content)
print("BGP documentation saved: bgp_documentation.md")
```

这是一个简单的脚本，展示了我们如何捕获 LLM 输出以创建简单文档的整体结构。

## 更多内容……

尝试添加额外的配置，并针对 BGP 路由策略和其他 BGP 相关配置修改提示词。

# 总结

本章向网络工程师介绍了如何使用本地 LLM 进行网络自动化任务的实际应用。通过使用 `Ollama` 在本地运行 LLM，我们可以在保持对数据完全控制权的同时，利用 AI 辅助，并消除对其他云端 LLM 服务的依赖。

在下一章中，我们将看到如何使用开源项目 `LangChain` 进行 AI LLM 开发。

# 7. 构建 AI LLM 应用后端

在 Web 早期，甚至在几年前，我们为 Web 服务通常只有一个单体应用。我们使用 `HTML/CSS/JavaScript` 处理用户可见的部分，并添加一些逻辑和代码来处理业务逻辑以及用户界面与逻辑之间的连接器。快到今天，大多数 Web 应用已经将应用分为两个主要部分：前端（Frontend）和后端（Backend）。分离的原因大致归纳为三点：

* **专业化**：开发者可以专注于用户体验（前端）或业务逻辑和数据（后端）。
* **可扩展性**：在进行更新时，只需更新部分代码库，因此出错的概率较低，且更具扩展性。
* **安全性**：大多数敏感数据存储在后端以受到保护。

在前端和后端之间，我遇到过的最好的类比之一是餐厅。前端类似于用餐区，即客户看到并与之交互的地方。后端就像厨房，许多工作在客户看不到的地方进行，烹饪菜肴并在送达客户之前送到用餐区。这些每个领域都需要不同的技能组合和技术，但它们共同工作以交付最终产品（我不知道你们怎么想，但我现在有点饿了）。

在上一章中，我们看到了使用 `Streamlit` 创建前端的简单方法。然而，许多业务逻辑仍然存在于我们创建的同一个 `Streamlit` 应用中。如果我们想开始使用数据库来持久化数据怎么办？如果我们将应用拆分为更小的部分进行扩展呢？我们已经看到了如何直接向 `OpenAI` 调用 API，那么为我们自己的服务创建一个类似的服务又如何？

在本章中，我们将开始学习如何创建一个基础 API 服务器（即 Web 后端），为我们的应用程序提供服务。我们将构建一个 `FastAPI` 应用，使用 `OpenAI` 的 `GPT` 模型智能地回答网络问题。

在本章中，我们将涵盖以下配方：

* 简单的网络 AI API
* 添加设备上下文
* 添加数据库存储
* 简单的 Web 界面
* Docker 部署

与之前的章节不同（之前的配方之间是相互独立的），在本章中，我们将从每个配方逐步构建出我们的应用程序。

# 技术要求

在开始之前，请记住为本章创建一个新文件夹以及一个 Python 虚拟环境来存放代码。由于本章是逐步构建应用程序，我们不需要为每个配方创建子文件夹。

我们还需要安装以下新的 Python 包并导出我们的 OpenAI API 密钥：

```bash
$ pip install fastapi uvicorn openai sqlalchemy python-multipart openai
$ export OPENAI_API_KEY="your-openai-api-key"
```

# 7.1 简单的网络 AI APIs

让我们创建一个基础的 `FastAPI` 应用，使用 `OpenAI` 的 `GPT` 模型回答问题。我们在其他配方中完成过类似的任务，但在本配方中，将在 `FastAPI` 的上下文中完成，以学习该框架的基础。

## 准备工作

此配方不需要新软件，请确保你已经安装了“技术要求”部分中的包。

## 如何实现……

1. 创建以下 Python 文件 `main_1.py`。

```python
from fastapi import FastAPI
from pydantic import BaseModel
import openai
import os

app = FastAPI(title="Network AI")
client = openai.OpenAI(api_key=os.getenv("OPENAI_API_KEY"))

class Request(BaseModel):
    question: str

@app.post("/ask")
def ask_question(request: Request):
    if not client.api_key:
        return {
            "answer": "Please set your OPENAI_API_KEY"
        }
    try:
        response = client.chat.completions.create(
            model="gpt-3.5-turbo",
            messages=[
                {
                    "role": "system",
                    "content": "You are a network engineer assistant Give concise, practical answers about network troubleshooting, configuration, and performance issues."
                },
                {"role": "user", "content": request.question}
            ],
            max_tokens=150
        )
        return {"answer": response.choices[0].message.content}
    except Exception as e:
        return {"answer": f"AI service unavailable: {str(e)}"}

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, port=8000)
```

2. 我们可以如下运行服务器：

```bash
$ python main_1.py
INFO:     Started server process [142204]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
INFO:     127.0.0.1:44394 - "POST /ask HTTP/1.1" 200 OK
```

3. 我们可以使用 `curl` 和 `/ask` 端点来尝试该函数。

```bash
$ curl -X POST "http://localhost:8000/ask" -H "Content-Type: application/json" -d '{"question": "Why is my BGP session down?"}'
{"answer":"您的 BGP 会话可能由于多种原因中断，例如配置不当、对端配置错误、网络连接问题、MTU 不匹配、路由问题或 BGP 邻居密码不匹配。检查配置、网络连通性和日志以识别 BGP 会话中断的确切原因。"}
```

4. 后端和前端最大的区别之一是，后端主要关注提供内容，*而不是*用户界面。如果我们导航到 `127.0.0.1:8000`，我们将看到一个并不漂亮的页面：

![Image 1](img/B32646_7_1.png)

图 7.1: 默认 FastAPI 页面

5. 然而，在 FastAPI 中，文档始终是“一等公民”。我们可以在 `/docs` 端点看到自动生成的文档：

![Image 2](img/B32646_7_2.png)

图 7.2: FastAPI 文档

## 工作原理……

FastAPI 将我们的应用程序封装在一个函数中，该函数会自动处理 HTTP 请求、JSON 序列化和 API 文档生成。`title` 参数会在每次显示 `title` 时（例如文档页面）设置该名称。

```python
app = FastAPI(title="Network AI")
```

FastAPI 最优特性之一是与 Pydantic 的集成。在以下代码中，如果有人为 `question` 发送了无效数据（例如类型错误或缺少值），Pydantic 会自动验证传入的 JSON 请求，FastAPI 将生成错误消息。

```python
class QuestionRequest(BaseModel):
    question: str
```

接下来的部分是 AI 魔法所在，我们编写提示词（`prompt`）、指定模型并解析响应。

```python
response = client.chat.completions.create(
    model="gpt-3.5-turbo",
    messages=[
        {
            "role": "system",
            "content": "你是一个网络工程师助手。
                        请就网络故障排除、配置和
                        性能问题提供简洁、实用的回答。"
        },
        {"role": "user", "content": request.question}
    ],
    max_tokens=150
)
return {"answer": response.choices[0].message.content}
```

结尾处的导入看起来可能很奇怪，但这是 FastAPI 的最佳实践。它只在需要时（直接运行该文件时）才导入 `uvicorn`。

```python
if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, port=8000)
```

## 更多内容……

为了获得更详细的日志，我们可以使用以下模式来记录更多信息。

```python
import logging
logging.basicConfig(level=logging.INFO)
@app.post("/ask")
def ask_question(request: QuestionRequest):
    logging.info(f"Question received: {request.question}")
```

## 另请

请访问 [`fastapi.tiangolo.com/`](https://fastapi.tiangolo.com/) 查看更多关于 FastAPI 的信息。

# 7.2 添加设备上下文

让我们增强我们的 AI 应用，包含一个提供特定设备建议的功能。这将带给我们更详细的答案，因为额外信息将提供特定的命令。此教程旨在演示我们如何重构代码以添加更多功能。

## 准备工作

请完成教程 7.1

## 如何实现……

1. 我们将增强在 7.1 中创建的脚本，复制相同的脚本并命名为 `main_v2.py`。

2. 我们将在 `QuestionRequest` 类中添加一个额外的 `device_type` 参数。

```python
class QuestionRequest(BaseModel):
    question: str
    device_type: str = "generic"
```

3. 我们可以添加一个 `if-else` 循环来修改我们的提示词。



进入`/path/to/xxx`目录，找到`xxx.json`。

在表达式`cvar = avar + bvar`中，加法运算符（`+`）将`avar`与`bvar`相加，得到它们的和`cvar`

在`List.of(arg0, arg1, arg2)`中，`List`接口的工厂方法`of()`接受一系列的元素，返回包含它们的只读列表。

之后我们这样调用`cmd`命令：`cmd arg0 arg1 arg2`。

```
if (condVar > someVal) {console.log("xxx")}
```



以下是排版后的 Markdown 格式文本：

---

首先，引入所需模块：

```python
from sqlalchemy import create_engine, Column, Integer, String, DateTime
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker
from datetime import datetime
```

# 之前的设置代码（`OpenAI`，数据库等）

```python
client = openai.OpenAI(api_key=os.getenv("OPENAI_API_KEY"))
engine = create_engine("sqlite:///questions.db")
Base = declarative_base()
Session = sessionmaker(bind=engine)
```

定义 `Question` 数据模型：

```python
class Question(Base):
    __tablename__ = "questions"
    id = Column(Integer, primary_key=True)
    question = Column(String)
    answer = Column(String)
    device_type = Column(String)
    timestamp = Column(DateTime, default=datetime.now)

Base.metadata.create_all(engine)
```

创建 `FastAPI` 应用实例：

```python
app = FastAPI(title="网络 AI")
```

定义请求体模型：

```python
class QuestionRequest(BaseModel):
    question: str
    device_type: str = "generic"
```

# 核心 AI 回答函数

```python
def get_ai_answer(question: str, device_type: str = "generic"):
    if not client.api_key:
        return "请设置您的环境变量 OPENAI_API_KEY"

    system_msg = "你是一个网络工程师助手。 "
    if device_type.lower() == "cisco":
        system_msg += "关注 Cisco IOS/IOS-XE 命令。 "
    elif device_type.lower() == "juniper":
        system_msg += "关注 Junos 命令。 "
    elif device_type.lower() == "arista":
        system_msg += "关注 Arista EOS 命令。 "
    elif device_type.lower() == "palo alto":
        system_msg += "关注 Palo Alto 防火墙命令。 "
    system_msg += "给出简洁、实用的答案，并带有具体的命令。"

    try:
        response = client.chat.completions.create(
            model="gpt-3.5-turbo",
            messages=[
                {"role": "system", "content": system_msg},
                {"role": "user", "content": question}
            ],
            max_tokens=200
        )
        return response.choices[0].message.content
    except Exception as e:
        return f"AI 服务不可用：{str(e)}"
```

# Web 界面路由

```python
@app.get("/", response_class=HTMLResponse)
def web_interface():
    return """
    <!DOCTYPE html>
    <html>
    <head>
    <title>网络 AI 助手</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 4px; max-width: 800px; }
        .form-group { margin: 2px 0; }
        input[type="text"], select { width: 100%; padding: 10px; font-size: 16px; }
        button { background: #07cba; color: white; padding: 2px 24px; border: none; font-size: 16px; cursor: pointer; }
        .answer { background: #f5f5f5; padding: 20px; margin: 20px 0; border-radius: 5px; }
    </style>
    </head>
    <body>
        <h1>🤖 网络 AI 助手</h1>
        <form method="post" action="/web-ask">
            <div class="form-group">
                <label>提出您的网络问题：</label>
                <textarea name="question" rows="3" placeholder="例如：我如何排查 BGP 邻居 Down 问题？"></textarea>
            </div>
            <div class="form-group">
                <label>设备类型：</label>
                <select name="device_type">
                    <option value="generic">通用</option>
                    <option value="cisco">Cisco</option>
                    <option value="juniper">Juniper</option>
                    <option value="arista">Arista</option>
                    <option value="palo alto">Palo Alto</option>
                </select>
            </div>
            <button type="submit">获取 AI 回答</button>
        </form>
        <div>
            <h3>最近的问题</h3>
            <p><a href="/history">查看对话历史</a></p>
        </div>
    </body>
    </html>
    """
```

处理表单提交：

```python
@app.post("/web-ask", response_class=HTMLResponse)
def web_ask_question(question: str = Form(...), device_type: str = Form("generic")):
    answer = get_ai_answer(question, device_type)

    # 保存到数据库
    session = Session()
    session.add(Question(question=question, answer=answer, device_type=device_type))
    session.commit()
    session.close()

    return f"""
    <!DOCTYPE html>
    <html>
    <head>
    <title>网络 AI 助手</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 40px; max-width: 800px; }
        .question { background: #e3f2fd; padding: 15px; margin: 10px 0; border-radius: 5px; }
        .answer { background: #f5f5f5; padding: 20px; margin: 10px 0; border-radius: 5px; white-space: pre-wrap; }
        .back { margin: 20px 0; }
        a { color: #007cba; text-decoration: none; }
        strong { font-weight: bold; }
    </style>
    </head>
    <body>
        <h1>🤖 网络 AI 助手</h1>
        <div class="question">
            <strong>您的问题 ({device_type})：</strong><br>
            {question}
        </div>
        <div class="answer">
            <strong>AI 回答：</strong><br>
            {answer}
        </div>
        <div class="back">
            <a href="/">再提一个问题</a>
        </div>
    </body>
    </html>
    """
```

历史记录与设备支持接口：

```python
@app.get("/history")
def get_history():
    session = Session()
    questions = session.query(Question).order_by(Question.timestamp.desc()).limit(10).all()
    session.close()
    return [{"question": q.question, "answer": q.answer, "device_type": q.device_type, "time": q.timestamp} for q in questions]

@app.get("/devices")
def supported_devices():
    return {
        "supported_devices": ["cisco", "juniper", "arista", "palo alto", "generic"],
        "usage": "在您的 JSON 请求中包含 device_type 以获取特定设备的帮助"
    }
```

应用入口：

```python
if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, port=8000)
```

---

*   HTML 表单元素，参考 [developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/form](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/form)。

# 7.5 Docker 部署

在本教程中，我们将把网络 AI 助手应用封装进 `Docker` 容器，这样它就可以在任何地方运行。

## 准备工作

请确保已完成第 7.4 节并安装了 `Docker`。

## 如何操作……

1. 创建一个名为 `dockerfile` 的 Docker 文件。注意我们使用了 `main_v4.py`，但你也可以使用之前的版本。

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY main_v4.py .

EXPOSE 8000

CMD ["python", "main_v4.py"]
```

2. 对于 `FastAPI` 文件，我们在 `uvicorn` 配置中仅监听了 `localhost`，我们需要将其更改为监听所有接口。

```python
if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

3. 创建 `requirements.txt` 文件。

```text
fastapi
uvicorn
openai
sqlalchemy
python-multipart
```

4. 在终端中使用 `docker build -t <name> .` 构建容器镜像。

```bash
$ docker build -t network-ai .
[+] Building 6.2s (7/9)                                    docker:default
 => => resolve docker.io/library/python:3.11-slim@sha256:0ce77749ac  0.0s
 => => sha256:f3bfd8e9386c2a97d52a0f28a4cc8db81b034 5.37kB / 5.37kB  0.0s
 => => sha256:59e22667830bf04fb35e15ed9c70023e9d1 28.23MB / 28.23MB  0.4s
 => => sha256:abd846fa1cdb2ae1ef7731213cd4f0c40b05f 3.51MB / 3.51MB  0.1s
 => => sha256:b7b61708209ad8f9b9a11c61dc9df90f74c 16.21MB / 16.21MB  0.3s
```

5. 使用 `OPENAI_API_KEY` 运行容器。

```bash
$ docker run -p 8000:8000 -e OPENAI_API_KEY="your-openai-key" network-ai
/app/main_v4.py:14: MovedIn20Warning: declarative_base() function is now available as sqlalchemy.orm.declarative_base(). (deprecated since: 2.0) (Background on SQLAlchemy 2.0 at https://sqlalche.me/e/b8d9)
  Base = declarative_base()
INFO:     Started process [1]
INFO:     Waiting for application.
INFO:     Application startup.
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```

6. 我们应该在端口 8000 看到相同的 Web 界面。

![Image 5](img/B3264_7_5.png)
图 7.5：来自容器的界面

7. 测试容器化的 AI 网络助手。

```bash
$ curl -X POST "http://127.0.0.1:8000/ask" -H "Content-Type: application/json" -d '{"question": "How do I configure VLANs on a switch?", "device_type": "cisco"}'
{"answer":"To configure VLANs on a Cisco, follow these steps:\n1. 进入全局配置:\n```\nenable\nconfigure terminal\n```\n\n2. 创建 VLAN 并为它们命名：\n```\nvlan <vlan_id>\nname <vlan_name>\n```\n\n3. 将接口分配到 VLAN：\n```\ninterface <interface_type> <interface_number>\nswitchport mode access\nswitchport access vlan <vlan_id>\n```\n\n4. 退出配置模式并保存配置：\n```\nend\nwr\n```\n\n5. 验证 VLAN 配置：\n```\nshow vlan\nshow interfaces\n```","device_type":"cisco"}
```

---

**注意：** 对于 HTML 表单元素，如果您的文本是英文原文，可以保留原有链接格式；若为中文，则保持原样。以上内容已包含在`[content]...[/content]`标记中。



# 工作原理……

这是一个构建 Docker 容器的标准工作流：

1. 创建`Dockerfile`。
2. 创建`requirements.txt`。
3. 构建并运行容器，根据需要添加环境变量。
4. 测试容器化应用程序。

一旦我们对应用程序进行容器化，就可以将其移动到许多提供商，如 DigitalOcean、AWS 或 Azure。

## 更多……

请查看第 4 章关于 Docker 安装的技术要求。

# 总结

在本章中，我们构建了一个完整的 AI 驱动的后端，并带有一个简单的 Web 界面，我们的前端团队可以使用它。正如任何开发者所说，工作永无完止。

在本文写作时，通常有两种“氛围编程”（vibe code）的方法。第一种是将工具集成在集成开发环境（IDE）内部，另一种较新的方法是将工具集成在终端中。对于已经习惯 IDE 编写代码的人来说，使用 IDE 更加熟悉，而终端更具通用性（例如在手机上）。我们将演示这两种方法：第一种使用配合 VSCode 的 GitHub Copilot，第二种使用 Anthropic 的 Claude Code。

信息：请注意，在写作时，GitHub Copilot 提供免费层级，且`VSCode`也是免费的，但这两者都可能发生变化。`Claude Code`目前只有付费订阅层级，但我认为其提供的功能物超有所值。

该领域正在迅速发展，如果你发现定价或安装方式与下方不同，请检查相关资源以获取最新信息：

* GitHub Copilot: [`github.com/features/copilot`](https://github.com/features/copilot)
* Anthropic Claude Code: [`www.anthropic.com/claude-code`](https://www.anthropic.com/claude-code)

让我们从设置 GitHub Copilot 开始。

# 10.1 设置 GitHub Copilot

在本教程中，我们将在本地计算机上设置 GitHub Copilot。

## 准备工作

你需要一台能够运行`VSCode`的计算机。下方的截图截自 Apple Mac 电脑，但在不同操作系统上的安装指令说明也是可以轻松获取的。

## 如何操作……

访问 [`code.visualstudio.com/`](https://code.visualstudio.com/)，浏览器将检查你的操作系统并提示你下载相应的软件，你也可以点击右上角的“Download”按钮进行下载。

![Image 1](img/B32646_10_1.png)

图 10.1：Visual Studio Code 首页

1. Visual Studio Code 从诞生起初一直是一个免费产品。此时，GitHub Copilot 也提供了免费和多个付费层级。

![Image 2](img/B32646_10_2.png)

图 10.2：GitHub Copilot 定价

2. 打开`VSCode`并点击`Extension`（扩展）选项卡，搜索`GitHub` `Copilot`并点击`Install`（安装）。

![Image 3](img/B32646_10_3.png)

图 10.3：GitHub Copilot 扩展

3. 安装完成后，你应该能在顶部看到 Copilot 图标。如果右侧面板被隐藏了，请使用右上角的图标显示它。打开右侧面板后，寻找聊天窗口。

![Image 4](img/B32646_10_4.png)

图 10.4：Copilot 面板

4. 在对话框中，寻找下拉菜单以在不同模式切换：`Agent`、`Ask`和`Edit`。我们将在 Agent 和 Edit 之间切换。

![Image 5](img/B32646_10_5.png)

图 10.5：配置模式

5. 在右侧的下拉框中，你还可以找到选择不同模型的选项，可以自由选择任何标准或高级模型（高级模型会产生少量费用）。

![Image 6](img/B32646_10_6.png)

图 10.6：模型选择

6. 选择`Ask`模式并将`GPT-4.1`作为模型。提一个问题，例如`explain the structure of a JSON file`。

![Image 7](img/B32646_10_7.png)

图 10.7：使用 GPT-4.1 的 Ask 模式

7. 切换到`Agent`模式并使用模型`GPT-4.1`。输入指令在你为此章节创建的文件夹创建一个虚拟环境。例如：`please change to /home/ubuntu/ai_network_cookbook/ch10 and create a Python virtual environment named .venv.`。

警告。请谨慎使用 Agent 模式，它**将**代表你执行更改。虽然通常有撤销更改的选项，但并非总是如此。在 Agent 模式下请格外小心。

8. 如果你点击指令并选择`Continue`（继续），代理将代表你执行并做出更改。我们可以点击`Cancel`（取消），因为我们已经手动创建了虚拟环境。

![Image 8](img/B32646_10_8.png)

图 10.8：使用 GPT-4.1 的 Agent 模式

## 工作原理……

Ask 模式类似于直接在 IDE 中拥有一个 AI LLM 聊天机器人。先从 Ask 模式开始，手动复制粘贴代码或命令。谨慎使用 Agent 模式，它会代表你执行并更改。通常有“confirm”（确认）或“accept”（接受）选项，但如果我们不小心，在 agent 模式下更容易犯不可逆的错误。

## 更多功能……

我们可以包含其他文件和链接来帮助回答。点击`Add Context`（添加上下文）按钮，我们可以从本地文件或其他链接中选择包含的内容。

![Image 9](img/B32646_10_9.png)

图 10.9：添加上下文

## 参见……

GitHub Copilot 教程：[`github.com/features/copilot`](https://github.com/features/copilot)

# 10.2 设置 Claude Code

在 IDE 内进行氛围编程的能力很棒。然而，并非所有平台都适合 IDE（例如平板和手机），而且每个人都习惯在 IDE 中工作。

Anthropic ([`www.anthropic.com/`](https://www.anthropic.com/)) 作为 Claude LLM 模型的创建者，创建了一个名为`Claude Code`的氛围编程工具，通过`CLI`直接在终端运行。在写作时，Claude Code 只有付费订阅模式。希望当你读到这里时，Claude Code 已提供免费层级。

![Image 10](img/B32646_10_10.png)

图 10.10：Claude Code 定价

## 准备工作

本教程请使用你喜欢的终端窗口。

## 如何操作……

1. 访问 [`docs.anthropic.com/en/docs/claude-code/overview#install-and-authenticate`](https://docs.anthropic.com/en/docs/claude-code/overview#install-and-authenticate) 获取最新的安装指令。
2. 通过`npm`安装`@anthropic-ai/claude-code`。如果你的平台没有 npm，请遵循此处的说明：[`docs.npmjs.com/downloading-and-installing-node-js-and-npm`](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)，适用于你的操作系统。

```
# npm install -g @anthropic-ai/claude-code

added 3 packages in 3s

2 packages are looking for funding

run `npm fund` for details

npm notice

npm notice New major version of npm available! 10.9.2 -> 11.5.2

npm notice Changelog: https://github.com/npm/cli/releases/tag/v11.5.2

npm notice To update run: npm install -g npm@11.5.2

npm notice
```

3. 安装完成后，我们可以进入项目目录并使用`claude`启动 Claude Code（Linux 平台需要 root 权限）。
4. 请记住所有操作都在终端中进行，因此我们需要使用方向键进行导航，使用 Enter 键确认。
5. 首次启动时，Claude Code 会询问显示样式。

![Image 11](img/B32646_10_11.png)

图 10.11: Claude Code 文本样式

6. 按照屏幕指令登录 Claude 账户。

![Image 12](img/B32646_10_12.png)

图 10.12: Claude 账户登录

7. 登录后，将出现 Claude Code 控制台。

![Image 13](img/B32646_10_13.png)

图 10.13: Claude Code 控制台

8. 输入`/`查看命令选项。

![Image 14](img/B32646_10_14.png)

图 10.14: Claude Code 选项

9. 这是*交互模式模式*，你可以直接与 Claude Code 对话以提问、请求任务并获取即时响应。
10. 输入`quit`退出 Claude Code

## 工作原理……

如果你习惯了在 IDE 中通过图形界面编写代码，可能需要一点时间来适应。不过，我发现终端窗口的操作速度比 GUI 界面更快。

## 更多内容……

尝试按照 [`docs.anthropic.com/en/docs/claude-code/quickstart#step-5%3A-use-git-with-claude-code`](https://docs.anthropic.com/en/docs/claude-code/quickstart#step-5%3A-use-git-with-claude-code) 上的说明进行*对话式**Git**操作*。

![Image 15](img/B32646_10_15.png)

图 10.15: 使用 Claude Code 进行 Git 操作

## 延伸……

Claude Code 快速上手：[`docs.anthropic.com/en/docs/claude-code/quickstart`](https://docs.anthropic.com/en/docs/claude-code/quickstart)

# 10.3 对话式日志分析与威胁检测

本示例演示了如何通过对话式 AI 开发构建威胁检测能力。我们将使用 GitHub Copilot 作为示例。

## 准备工作

请完成*配方 10.1*以准备 GitHub Copilot 开发环境。

## 如何实现……

1. 请创建一个新目录`mock_data`来存储模拟数据：`firewall_logs.txt`、`network_info.yaml`和`threat_intelligence.json`。这些文件可以从书籍下载页面和书籍的 GitHub 仓库下载。
2. 防火墙日志混合了不同厂商的带有威胁模式的防火墙日志。

```
%ASA-6-302013: Built inbound TCP connection 12345 for outside:203.0.113.100/443 (203.0.113.100/443) to inside:10.0.1.50/80 (10.0.1.50/80)

%ASA-4-106023: Deny tcp src outside:203.0.113.50/1337 dst inside:10.0.1.100/22 by access-group outside_in

%ASA-6-302014: Teardown TCP connection 12345 for outside:203.0.113.100/443 to inside:10.0.1.50/80 duration 0:02:30 bytes 2048

%ASA-4-106023: Deny tcp src outside:198.51.100.25/4444 dst inside:10.0.1.200/3389 by access-group outside_in

…

# pfSense 日志

filterlog: 5,,,1000000103,em0,match,block,in,4,0x0,,64,1024,0,DF,6,tcp,40,203.0.113.100,10.0.1.50,443,80,0,S,1024,,

filterlog: 5,,,1000000104,em0,match,pass,in,4,0x0,,64,1024,0,DF,6,tcp,40,10.0.1.50,203.0.113.100,80,443,0,A,1024,,

filterlog: 5,,,1000000105,em0,match,block,in,4,0x0,,64,1024,0,DF,6,tcp,40,198.51.100.25,10.0.1.200,3389,0,A,1024,,

filterlog: 5,,,1000000106,em0,match,block,in,4,0x0,,64,1024,0,DF,6,tcp,40,10.0.1.50,203.0.113.100,80,443,0,A,1024,,

…

# Suricata 日志

1/15/2024-12:00:0.12345 [1:20049:7] GPL CHAT IRC [Classification: policy-violation] [Priority: 3] {TCP} 203.0.113.100:667 -> 10.0.1.50:2768

1/15/2024-12:05:15.7890 [1:20012:5] ET MALWARE Conficker Request [Classification: trojan] [Priority: 1] {TCP} 0.0.1.100:4915 -> 198.51.100.25:80

1/15/2024-12:10:30.45689 [1:20085:5] ET SSH Scan [Classification: attempted-recon] [Priority: 2] {TCP} 203.0.113.50:22 -> 10.0.1.0/24
```



## 使用 GPT-4.1 Ask 模式

1. 首先切换到正确的目录，“请切换到 `/home/ubuntu/ai_network_cookbook/ch10/Recipe_10_1` 文件夹。”
2. 让 Copilot 描述数据，“描述 `mock_data` 目录下的文件。” 这是 AI 的回答：

   ![Image 16](img/B32646_10_16.png)

   图 10.16：Copilot 对 Mock Data 文件的回答

3. 发出开始构建脚本的指令，“在 `mock_data/firewall_logs.txt` 文件中，可能包含端口扫描和暴力破解攻击。帮我构建一个可以识别这些威胁模式的解析器。”
4. 查看逻辑和文件输出，如有需要通过指令进一步修改。

   ![Image 17](img/B32646_10_17.png)

   图 10.17：脚本输出

5. 切换到 Agent 模式并让 Copilot 构建它。这是一个示例指令，“请构建脚本并将其命名为 `log_analysis.py`。”
6. 脚本已编写完成，你可以选择保留或撤销。![Image 18](img/B32646_10_18.png)图 10.18：`log_analysis.py` 的创建

# 10.4 用于事件响应的快速安全脚本开发

在本食谱中，我们将使用 Claude Code 快速开发事件响应自动化。就像食谱 10.3 一样，我们将使用对话风格来完成任务。

## 准备工作

请完成食谱 10.2 设置 Claude Code，并从书中代码拷贝模拟数据文件 `incident_scenarios.json` 和 `network_topology.yaml`。

## 如何操作……

1. 在终端提示符中使用 `claude` 启动 Claude Code。
2. 因为这只是一个终端，我们可以使用提示符中可用的所有 Linux 工具，如 `pwd`、`cd` 和 `ls`。

   ![Image 19](img/B32646_10_19.png)

   图 10.19：使用 Linux 工具的 Claude

3. 从对话开始，“需要构建一个事件响应系统，可以自动隔离受感染系统、阻止恶意 IP 并收集取证证据。该系统应该处理勒索软件、数据外泄和凭据破解等不同事件类型。请帮我从零开始设计这个系统。”
4. Claude Code 将列出几个步骤并开始处理该过程。![Image 20](img/B32646_10_20.png)图 10.20：Claude Code 设计解决方案
5. Claude Code 将提出一个 `incident_resposne_system.py`，你可以选择创建文件或对脚本进行迭代。![Image 21](img/B32646_10_21.png)图 10.21：创建 `incident_response_system.py`
6. 开始参与其他对话，例如，“这种结构的最佳方式是什么，以便它可以处理多种事件类型并与不同的安全工具集成？”

## 其工作原理……

Claude Code 对话式事件响应开发方法比简单的代码补全具有多个优势：

* **自然语言问题解决对话**：Claude Code 在复杂的多轮讨论中保持上下文。
* **架构指导**：Claude 在架构指导方面可能具有更好的理解。它是否比 GitHub Copilot 等工具更好取决于主观判断，但它绝对优于传统的代码补全。
* **终端集成**：Claude Code 直接集成在终端中，为丰富的 CLI 用户提供了更流畅的体验。
* **迭代精炼**：就像 GitHub Copilot 一样，Claude Code 允许根据用户反馈实时完善响应逻辑、升级程序和集成方法。

## 更多……

Claude Code 还可以使用 `claude-sdk` 与 Python 配合，[`docs.anthropic.com/en/claude-sdk/python`](https://docs.anthropic.com/en/claude-sdk/python)。以下终端命令安装 Python 的 `claude-sdk`。

```
$ pip install claude-sdk
```

![Image 22](img/B32646_10_22.png)

图 10.22：Claude Code Python 基础用法

## 其工作原理……

对话式方法使我们能够快速开发解决方案并进行迭代。其中几个关键机制包括：

* **自然语言问题定义**：我们没有编写正式的规格文档，而是直接描述了问题。
* **迭代模式识别**：注意 AI 自动识别出我们需要正则表达式匹配并创建了初始集合。
* **持续重构**：注意我们没有写任何一行代码来接收文件，我们可以继续我们的“开发 -> 测试 -> 改进”循环，直到我们获得一个准备好投入生产环境的解决方案。

## 更多……

请尝试以下操作：

* **不同的模式检测**：尝试扩展对话以处理更复杂的攻击。
* **角色区分**：尝试针对高级资深工程师或初学者定制代码以提高可维护性。
* **优化代码执行**：尝试优化代码以提高执行速度。

## 另见……

* 使用 GitHub Copilot 的最佳实践：[`docs.github.com/en/copilot/get-started/best-practices`](https://docs.github.com/en/copilot/get-started/best-practices)
* Copilot Chat 食谱：[`docs.github.com/en/copilot/tutorials/copilot-cookbook`](https://docs.github.com/en/copilot/tutorials/copilot-cookbook)
* Claude Code 常见工作流：[`docs.anthropic.com/en/claude-code/common-workflows`](https://docs.anthropic.com/en/claude-code/common-workflows)

# 总结

在本章中，我们引入了“对话编程”作为一种开发网络安全的对话方法。与传统的开发方法不同，对话编程优先通过与 AI 助手的对话进行快速原型设计。核心概念包括：

* **对话优于正式规格需求**：安全人员不再提供正式文档，只需描述问题和预期解决方案。
* **速度优于完美**：我们很少在第一次或第二次就写对脚本，但由于生成脚本的时间非常快，我们可以非常快速地迭代。
* **领域知识优先**：对话编程允许领域专家利用其专业知识，并让对话编程工具编写代码。

使用 GitHub Copilot，我们能够在 Visual Studio Code 中直接使用对话编程。助手可以在 Ask 或 Agent 模式下运行，并有不同的模型组合供选择。通过 Claude Code，对话编程助手集成到了终端中。

这是本书的最后一章。感谢你与我一起走过 AI LLM 与网络工程交叉领域不断演变的景观。你在这本书中所投入的时间精力将为你网络专业人士解决复杂问题的根本性转变提供良好帮助。本书的目的是作为你的起点，而非终点。当你采取这些模式并将其适用于自己的用法时，真正的魔法就会发生。

再次感谢你抽出时间阅读这本书。我希望书中的解决方案能为你带来价值，并祝你的 AI LLM 网络工程之旅一切顺利。网络工程的未来将由像你这样的从业者编写，利用 AI 来增强而非取代人类工程师。直直向前吧！
