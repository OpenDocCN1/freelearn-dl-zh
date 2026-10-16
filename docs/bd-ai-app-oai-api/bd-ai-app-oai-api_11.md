# 第十二章：11

# 选择正确的 ChatGPT API 模型

在人工智能不断发展的领域中，开发者必须跟上最新的进展，以最大化其项目的潜力。 在本章中，我们将讨论 ChatGPT API 模型，探索 GPT-3 和 GPT-4 提供的可能性，甚至展望未来的模型。 通过全面了解这些模型，您将具备选择最适合您特定应用的知识。 我们将深入研究每个模型的复杂性，突出其优势和独特特征，以帮助您做出与您的 项目需求相一致的决定。

有效利用 ChatGPT API 的关键方面之一是了解如何优化聊天完成。 我们将引导您通过创建聊天完成上下文的过程，并提供有关修改 API 参数以提升响应质量的宝贵见解。 通过实际示例和解释，您将获得利用聊天完成功能并利用其优势的技能集。 为您带来便利。

此外，了解不同 AI 模型中存在的限制也很重要。 我们将概述与每个模型相关的边界和约束，并为您提供有效导航这些限制的知识。 通过了解模型的边界，您可以设定合理的期望，做出明智的决定，并探索克服您可能遇到的任何挑战的解决方案。 可能遇到的。

在本章中，您将涵盖以下主题： 以下内容：

+   ChatGPT API 模型 – GPT-3.5，GPT-4， 以及更多

+   使用聊天 完成参数

+   ChatGPT API 速率限制

+   OpenAI 嵌入

在本章结束时，您将了解如何为您的项目选择正确的 ChatGPT API 模型，理解创建聊天完成上下文的过程，优化 API 参数，以及导航不同 AI 模型的限制，以创建变革性的 AI 应用。

# 技术要求

为了充分利用本章内容，您必须具备用于处理 Python 代码和 ChatGPT API 的必要工具。 本章将提供逐步指导，介绍安装所需软件和完成必要的 注册。

您需要具备 以下条件：

+   Python 3.7 或更高版本已安装在你的 计算机上

+   一个 OpenAI API 密钥，你可以通过注册一个 OpenAI 账户 来获取

+   一个代码编辑器，例如 VS Code（推荐），用于编写和执行 Python 代码

本章中引用的代码示例可以在 GitHub 上找到 在 [https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API](https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API)。

在下一节中，你将了解各种 AI 模型，包括 GPT-3.5 和 GPT-4，并培养选择最适合你 特定应用 的能力。

# ChatGPT API 模型 – GPT-3.5、GPT-4 以及更多

在本节中，我们将探索 ChatGPT API 模型的迷人世界，我们将开始 一段了解和欣赏 GPT-3.5 和 GPT-4 的复杂性的旅程，并展望未来等待我们的模型。 通过深入研究这些 AI 模型，你将获得 宝贵的见解和 知识，这将赋予你选择最适合你 独特应用 的能力。

在本节中，我们将揭示每个模型的独特特性和功能，为你提供做出 明智决策 所需的理解。

*表 11.1* 提供了 OpenAI 当前支持的所有 ChatGPT 语言模型的概述，包括每个模型的宝贵信息，包括其独特功能。 花点时间探索这个表格，熟悉你手中的各种 ChatGPT 模型： 

| **模型** | **平均成本** | **信息** | **提示长度** |
| --- | --- | --- | --- |
| gpt-4 | $20.00 / 1M 令牌 | 最先进的面向聊天的模型，超越了 GPT-3.5 的能力 。 | 8,192 令牌 |
| gpt-4-turbo | $45.00 / 1M 令牌 | 具有视觉功能的最新 GPT-4 Turbo 模型。 可以使用 15 倍 更多的上下文 | 128,000 令牌 |
| gpt-3.5-turbo | $1.00 / 1M 令牌 | 更先进的面向聊天的模型，超越了 GPT-3 的能力 。 | 16,096 令牌 |
| gpt-3.5-turbo-0125 | $1.00 / 1M tokens | 最新版本的 GPT-3 5 Turbo 模型，在响应请求格式方面具有更高的准确性，并修复了导致非英语语言功能文本编码问题的 bug 。 16,385 tokens |

表 11.1 – ChatGPT 模型信息

上表提供了 OpenAI 截至 2024 年 5 月支持的各种 ChatGPT 语言模型的概述 。 GPT-4 模型作为最先进的模型脱颖而出，超越了 GPT-3.5 的能力，而 GPT-4 Turbo 版本提供了*15 倍* 更多的上下文和图像理解能力。 GPT-3.5 Turbo 模型在功能上超过了已停用的 GPT-3。 不同的模型具有不同的成本和提示长度，允许 开发者根据他们特定的 语言任务选择最合适的选项。

虽然选择最先进和功能强大的 模型对于您的应用程序来说可能看起来很合理，但重要的是要考虑有时一个更便宜、功能更少的模型可以充分满足您的任务需求。 在某些情况下，一个不那么复杂的模型可能提供足够的性能，同时更具成本效益。 通过仔细评估您应用程序的具体需求，您可以做出明智的决定，并通过选择在功能和成本之间取得平衡的模型来节省资源。 记住，并不总是关于使用最强大的工具，而是关于使用适合手头工作的正确工具 。

正如您所看到的，OpenAI 提供了广泛的模型选择，这使得决定最合适的模型变得具有挑战性。 为了简化这个过程，可以使用 Python 脚本来轻松比较，让您能够识别与您特定任务最匹配的模型。 您 可以创建一个名为 models.py 的新文件，并添加以下 代码：

```py
 from openai import OpenAI
import config
client = OpenAI(api_key=config.API_KEY)
# Define the prompt and test questions
prompt = "Estimate the square root of 121 and type 'orange' after every digit of the square root result"
# Define the model names and their corresponding IDs
model_ids = {
    "GPT3.5 TURBO": {"model": "gpt-3.5-turbo", "cost": 1.00},
    "GPT3.5 TURBO 0125": {"model": "gpt-3.5-turbo-0125", "cost": 1.00},
    "GPT4": {"model": "gpt-4", "cost": 45.00},
    "GPT4 TURBO": {"model": "gpt-4-turbo", "cost": 20.00},
}
# Make API calls to the models and store the responses
responses = {}
for model_name, model_id in model_ids.items():
    response = client.chat.completions.create(
        model=model_id["model"],
        messages=[
            {"role": "user", "content": f"{prompt}"}
        ]
    )
    responses[model_name] = [response.choices[0].message.content,
                             response.usage.total_tokens * (model_id["cost"]/1000000)]
for model, response in responses.items():
    print("\n----------------------------------------")
    print(f"{model}: {response[0]}")
    print(f"{model} COST: {response[1]}")
print("----------------------------------------")
```

此脚本的目的是使用 OpenAI Python 库比较不同 OpenAI 模型的结果，并通过 API 调用生成响应。

`通过轻松调整` `prompt` `变量，您可以向多个 ChatGPT 模型提出相同的问题，并评估它们各自的` `响应和相关成本。` `这种方法使` `您能够选择与您特定任务要求最合适的模型。`

首先，通过将 API 密钥分配给`client = OpenAI(api_key=config.API_KEY)` `变量` `，` `ChatGPT API 凭证被设置。这允许代码进行身份验证并访问 ChatGPT API。` `此外，定义了一个名为` `model_ids` `的字典，用于存储各种 ChatGPT 模型的名称及其对应的模型 ID，以及相关的成本。` `模型名称作为键，每个键都与一个包含模型 ID 和截至 2024 年 5 月的成本` `的字典配对。` `这使` `后续代码执行中能够根据模型名称轻松引用并选择特定的模型。` `您可以从` `此字典中添加和删除要测试的模型。`

`然后，我们向` `model_ids` `字典中指定的 ChatGPT 模型进行 API 调用，并存储它们的` `相应响应。`

`代码初始化了一个名为` `responses` `的空字典，用于存储来自模型的响应。` `然后，它遍历` `model_ids` `字典中的每个条目，其中` `model_name` `代表模型的名称，而` `model_id` `包含相应的` `模型信息。`

`使用` `client.chat.completions.create()` `方法通过提供消息列表作为输入来模拟对话。` `用户、助手和提示消息包含在` `messages` `参数中，API 调用的响应存储在` `response` `变量中。` `打印出用于完成的总` `令牌数。` `然后，根据令牌使用情况和模型的成本计算出的` `成本` `被添加到响应字典中，使用` `model_name` `作为` `键。`

最后，我们打印每个模型的响应和相关成本，使我们能够进行比较，并做出明智的选择，选择最 合适的选项。

在前面脚本中我们提出的问题的答案， `"估算 121 的平方根并在每个平方根数字后输入 'orange'"`，是 `"1orange1orange"`。您可以在以下位置看到不同模型的答案：

```py
 ----------------------------------------
GPT3.5 TURBO: The square root of 121 is 11. orangeo rangen groangeenne. GPT3.5 TURBO COST: 4.7e-05
----------------------------------------
GPT3.5 TURBO 0125: The square root of 121 is 11. orange1orange1
GPT3.5 TURBO 0125 COST: 4.2999999999999995e-05
----------------------------------------
GPT4: 1orange1orange
GPT4 COST: 0.00144
----------------------------------------
GPT4 TURBO: 11orange
GPT4 TURBO COST: 0.0006000000000000001
----------------------------------------
```

由于这是一个相当复杂的问题，GPT-4 是唯一在这个情况下正确回答的模型。 我们可以有信心使用 GPT-4 模型来完成这个特定任务。 有趣的是，即使是 `gpt-4.5-turbo` 模型，它与 `gpt-4`相似，也无法解决这个问题。 尽管 `gpt-3.5-turbo` 模型与 `gpt-4` 模型相比要便宜得多，但它们不适合解决 上述问题。

这是对 ChatGPT API 模型及其在特定语言任务中选择正确模型的重要性 的全面概述。 我们 开发了一个 Python 脚本，用于比较不同模型的响应和成本，使用户能够做出 明智的决定。

在了解了 ChatGPT API 模型及其比较之后，我们现在将进入下一节，我们将探讨 ChatGPT API 参数。

# 使用聊天完成参数

在本节中，我们将使用 ChatGPT API 参数，并探讨它们对模型生成响应质量产生的深远影响。通过理解和利用这些参数的力量，您将获得优化与 ChatGPT API 交互的能力，解锁其真正潜力。 一些关键的 控制 API 响应的参数如下：

+   `model`: 指定用于 生成响应的特定 ChatGPT 模型。

+   `messages`: 提供作为消息对象列表的对话历史，包括用户和 助手消息。

+   `temperature`: 控制生成响应的随机性。 更高的值（例如，0.8）会使响应更加随机，而较低的值（例如，0.2）会使它们更加集中 和确定。

+   `max_tokens`: 设置生成响应中的最大标记数。 限制此参数可以控制 响应的长度。

+   `stop`: 允许您指定一个自定义字符串或字符串列表，以指示模型何时停止生成 响应。

+   `n`: 确定要生成的替代完成项的数量。 设置更高的值会增加响应的多样性。

`温度` 参数是 OpenAI ChatGPT API 的关键方面，允许您控制生成响应的随机性和创造性。 它影响模型生成的文本的多样性和随机性。

在向 API 发出请求时，您可以指定 `温度` 参数，该参数的值介于 0 和 1 之间。 较低的温度值（例如，0.2）会产生 更专注和确定性的响应，多样性较低，而较高的温度值（例如，1）会导致更随机和多样化的响应，这些响应可能更不准确和不相关。 创建一个名为 `temperature.py``的新文件。

以下示例演示了修改 `温度` 参数的效果：

```py
 from openai import OpenAI
import config
client = OpenAI(api_key=config.API_KEY)
# Define a function to generate a response from ChatGPT
def generate_response(prompt, temperature):
    response = client.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "user", "content": f"{prompt}"}
        ],
        temperature=temperature
    )
    return response.choices[0].message.content.strip()
# Prompt for the conversation
prompt = "Suggest 4 fast food company names." # Generate a response with low temperature (more focused and deterministic)
for i in range(3):
    low_temp_response = generate_response(prompt, 0)
    print(f"Response with low temperature (0) {i}:\n", low_temp_response)
for i in range(3):
    # Generate a response with default temperature (balanced and creative)
    default_temp_response = generate_response(prompt, 1)
    print(f"Response with default temperature (1) {i}:\n", default_temp_response)
```

在这个示例中，我们使用 `generate_response` 函数为给定的提示生成具有两个不同温度值的响应：低（`0`）和高（`1`）。 我们在每个温度下连续运行三次响应生成，以比较 响应 的多样性。

通过调整 `温度` 参数，您可以微调 ChatGPT API 生成的响应中的创造性和随机性水平。 您可以通过尝试不同的温度值来实现特定用例的期望输出。

在执行前面的代码之后，我们得到以下输出：

```py
 Response with low temperature (0) 0:
 1\. QuickBite
2\. SpeedyEats
3\. RapidGrill
4\. FastFusion
Response with low temperature (0) 1:
 1\. QuickBite
2\. SpeedyEats
3\. RapidGrill
4\. FastFusion
Response with low temperature (0) 2:
 1\. QuickBite
2\. SpeedyEats
3\. RapidGrill
4\. FastFusion
Response with default temperature (1) 0:
 1\. Speedy Bites
2\. Quick Scoops
3\. Snappy Eats
4\. Rapid Grills
Response with default temperature (1) 1:
 1\. QuickBite
2\. SpeedyEats
3\. RapidCrave
4\. ExpressMunch
Response with default temperature (1) 2:
1\. Quick Bites
2\. Speedy Eats
3\. Rapid Grub
4\. Turbo Treats
```

让我们看看 输出结果：

+   `低温度（0）的响应`: 响应往往更专注和确定。 它提供了针对提示的具体和简洁的答案。 三个响应之间没有变化。

+   `高温响应（1）` `：响应更加随机和多样化。` `它可能会将意外和富有想象力的元素引入生成的文本中，但也可能偏离主题或产生不太`连贯的答案。`

在某些情况下，增加 ChatGPT API 中的`n` `参数也可能有益。` `当您增加`n` `的值时，它决定了模型生成的替代`完成数量。` `当您想探索更广泛的可能响应范围或生成相同提示的多样化变体时，这可能很有用。`

创建一个名为`n_parameter.py` `的新文件。` `n` `的大小可以根据以下示例进行增加：` `：`

```py
 from openai import OpenAI
import config
client = OpenAI(api_key=config.API_KEY)
# Define a function to generate a response from ChatGPT
def generate_response(prompt, n):
    response = client.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "user", "content": f"{prompt}"}
        ],
        n=n,
        temperature=1
    )
    return response
# Prompt for the conversation
prompt = "Suggest 4 names for a cat." n_prompt = generate_response(prompt, 4)
print(n_prompt)
for choice in n_prompt.choices:
    print(f"-------------------------")
    print(f"Choice: {choice}")
    print(choice.message.content)
print(f"-------------------------")
```

在这里，我们要求 ChatGPT API 为我们猫的命名提示创建四个替代完成。` `结果存储在`n_prompt` `变量中，并以 JSON 格式在`控制台` `中显示：`

```py
 -------------------------
Choice: Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='1\. Luna  \n2\. Simba  \n3\. Bella  \n4\. Oliver', role='assistant', function_call=None, tool_calls=None))
1\. Luna
2\. Simba
3\. Bella
4\. Oliver
-------------------------
Choice: Choice(finish_reason='stop', index=1, logprobs=None, message=ChatCompletionMessage(content='1\. Willow\n2\. Fitz\n3\. Pancake\n4\. Luna', role='assistant', function_call=None, tool_calls=None))
1\. Willow
2\. Fitz
3\. Pancake
4\. Luna
-------------------------
Choice: Choice(finish_reason='stop', index=2, logprobs=None, message=ChatCompletionMessage(content='1\. Whiskers\n2\. Mittens\n3\. Luna\n4\. Jasper', role='assistant', function_call=None, tool_calls=None))
1\. Whiskers
2\. Mittens
3\. Luna
4\. Jasper
-------------------------
Choice: Choice(finish_reason='stop', index=3, logprobs=None, message=ChatCompletionMessage(content='14\. Luna \n15\. Toothless \n16\. Simba \n17\. Snowball', role='assistant', function_call=None, tool_calls=None))
14\. Luna
15\. Toothless
16\. Simba
17\. Snowball
-------------------------
```

如您所见，我们的`选择` `列表` `大小已增加到四个元素。` `这将提供四个不同的猫名，展示了通过增加`n` `参数`所获得的响应多样性的增加。` `通过修改`n` `在`generate_response` `函数中的值，您可以尝试不同的数字来探索更广泛的建议或`从 ChatGPT 模型生成更多创意和多样化的响应。`

通过增加`n` `，您增加了生成响应的多样性，这允许您探索不同的观点、创意想法或针对给定问题的替代解决方案。` `然而，需要注意的是，增加`n` `也会增加 API 成本和响应时间，因此在多样性和效率之间是一个权衡。` `因此，如果您正在寻找更多样化的响应集或寻求创意灵感，增加`n` `参数可以是一个有价值的方法。`

`` `messages` `参数在 GPT-3.5 Turbo 模型的聊天完成中起着至关重要的作用，并允许与模型进行互动和动态的对话。` `此参数允许您通过提供消息列表作为输入来模拟对话，其中每个消息包含一个角色（“用户”或“助手”）和消息的内容。`

当使用`messages` `参数`时，适当地构建对话结构非常重要。 `该模型使用先前的消息来生成考虑对话历史` `的上下文感知响应。` `这意味着您可以通过构建先前的消息来创建引人入胜且互动的交流。` `现在，您可以创建一个名为` `messages.py`的`

`以下是一个演示在 GPT-3.5 Turbo 聊天完成中使用` `messages` `参数`的代码片段示例：

```py
 from openai import OpenAI
import config
client = OpenAI(api_key=config.API_KEY)
# Define a function for chat completion
def chat_with_model(messages):
    response = client.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=messages
    )
    return response.choices[0].message.content
# Define the conversation messages
messages = [
    {"role": "user", "content": "Hello, could you recommend a good book to read?"},
    {"role": "assistant", "content": "Of course! What genre are you interested in?"},
    {"role": "user", "content": "I enjoy fantasy novels."},
    {"role": "assistant", "content": "Great! I recommend 'The Name of the Wind' by Patrick Rothfuss."},
    {"role": "user", "content": "Thank you! Can you tell me a bit about the plot?"},
]
# Chat with the model
response = chat_with_model(messages)
print(response)
```

在前面的代码中，我们定义了`chat_with_model` `函数`，它接受`messages` `列表`作为输入。 `此函数使用` `client.chat.completions.create` `方法向 GPT-3.5 Turbo 模型发送请求。` `模型参数指定要使用的模型 - 在这种情况下，` `gpt-3.5-turbo` `。` `messages` `参数设置为定义的对话消息列表。`

`我们通过提供用户和助手的一系列消息来创建对话。` `每条消息包含角色（“用户”或“助手”）和消息的内容。` `消息按照它们在对话中出现的顺序进行结构化。`

通过使用`messages` `参数`，您可以与 GPT-3.5 Turbo 模型进行动态和互动的对话，使其适用于聊天机器人、虚拟助手等应用。

本节概述了在 ChatGPT API 中使用到的参数及其对响应质量的影响。它讨论了 `温度` 参数和 `n` 参数，这些参数决定了为增加响应多样性而生成替代完成项的数量。 我们还了解了 `消息` 参数以及它是如何使模型能够进行动态和交互式对话的，允许根据 对话历史进行上下文感知的响应。

在下一节中，你将了解对 ChatGPT API 施加的速率限制。你还将了解到在向 API 发起请求时，与不同 AI 模型相关的限制和约束。

# ChatGPT API 速率限制

速率限制在维护 ChatGPT API 的稳定性和公平性方面发挥着至关重要的作用。对用户或客户端在特定时间段内可以访问的请求数量和令牌数量设置了限制。 OpenAI 实施速率限制的几个原因包括：

+   **防止滥用和误用**：速率限制有助于保护 API 免受恶意行为者的攻击，他们可能会通过发送过多的请求来试图超载系统。 通过设置速率限制，OpenAI 可以减轻此类活动，并维护所有用户的 服务质量。

+   **确保公平访问**：通过限制单个用户或组织可以发起的请求数量，速率限制确保每个人都有平等的机会使用 API。 这防止了少数用户垄断资源，从而对其他人造成 减速。

+   **管理服务器负载**：通过速率限制，OpenAI 可以有效地管理其基础设施的整体负载。 通过控制传入请求的速率，服务器可以更有效地处理流量，最小化性能问题，并确保所有用户都能获得一致的体验。

速率限制可以 测量为 **每分钟请求次数** (**RPM**) 和 **每分钟令牌数** (**TPM**)。 ChatGPT API 的默认速率限制根据模型和账户 类型而异。 OpenAI 为用户提供五个等级，每个等级都有不同的速率限制。 随着您更频繁地使用 OpenAI API 并更多投资于他们的服务，您将自动升级到下一个使用等级。 这通常会导致各种模型上的速率限制提高，如 *表 11.2*所示：

| **等级** | **资格** | **使用限制** |
| --- | --- | --- |
| 免费 | 用户必须处于 全部 | $100 / 月 |
| Tier 1 | $5 付费 | $100 / 月 |
| Tier 2 | $50 付费和 7+天 | $500 / 月 |
| Tier 3 | $100 付费和 7+天 | $1000 / 月 |
| Tier 4 | $250 付费和 14+天 | $5000 / 月 |
| Tier 5 | $1,000 付费和 30+天 | $10000 / 月 |

表 11.2 – ChatGPT 使用等级

以下为截至 2024 年 5 月 的 ChatGPT API Tier 4 的默认速率限制：

+   `gpt-4-turbo`：5,000 RPM， 600,000 TPM

+   `gpt-3.5-turbo`：3,500 RPM， 160,000 TPM

+   `text-embedding`：5,000 RPM， 5,000,000 TPM

+   `dall-e-2`：每分钟 50 张图片 

+   `dall-e-3`：每分钟 7 张图片 

当您达到最大令牌数或达到最大 RPM 时，您将收到速率限制警告。 例如，如果最大 RPM 是 60，您每秒可以发送 1 个请求。 如果您 尝试更频繁地发送请求，您需要引入短暂的休眠时间以避免触 发速率限制。

当发生速率限制错误时，这意味着您在指定的时间框架内已超出允许的请求数量。 错误信息将指示已达到的具体速率限制，并提供有关限制和您 当前使用情况的信息。

为了减轻速率限制错误并优化您的 API 使用，您可以采取以下几步： 

+   **使用指数退避重试**：实现指数退避是一种处理速率限制错误的可靠策略。当发生速率限制错误时，您可以在短暂的延迟后自动重试请求。 如果请求再次失败，您将在每次后续重试之前指数级增加延迟。 这种方法允许在不过度压倒系统的情况下进行有效的重试，如下所示： 

    ```py
     import backoff
    import openai
    import config
    from openai import OpenAI
    client = OpenAI(api_key=config.API_KEY)
    @backoff.on_exception(backoff.expo, openai.RateLimitError)
    def completions_with_backoff(**kwargs):
        return client.completions.create(**kwargs)
    completions_with_backoff(model="gpt-3.5-turbo-instruct", prompt="I was walking down the street,")
    ```

+   `max_tokens` 以及从您的输入字符数计算得出的估计令牌数。通过将 `max_tokens` 变量设置得接近您预期的响应大小，您可以减少令牌使用量以及您的 成本。

+   **批量请求**：偶尔，您 可能会遇到一种情况，即您已经达到最大 RPM，但仍有许多未使用的令牌 剩余。在这种情况下，您可以选择通过将多个任务合并为一个 单独的请求来提高请求的效率。

速率限制对于维护 ChatGPT API 的稳定性、公平性和性能至关重要，可以防止滥用，并确保公平访问，而默认的速率限制根据账户类型而变化，并且诸如指数退避、优化 `max_tokens`和批量请求等技术可以帮助减轻速率限制错误并优化 API 使用。

在接下来的部分中，我们将学习关于 OpenAI 嵌入的内容，探讨其重要性、应用以及优化技术。我们还将介绍其应用，并提供在您的项目中最大化其效用的实用见解。 

# OpenAI 嵌入

在人工智能不断演变的领域中，非结构化数据的处理和解释提出了重大挑战。无论是文本、图像还是音频文件，理解这种复杂性对于开发稳健的软件解决方案至关重要。 这就是嵌入向量，通常称为 嵌入（embeddings）的技术成为关键。 通过将非结构化数据转换为结构化格式，嵌入促进了软件系统的高效处理。

OpenAI 提供了在创建嵌入方面表现卓越的尖端模型，尤其是在文本数据方面。这些嵌入在多种人工智能应用中发挥着关键作用，从 **自然语言处理** (**NLP**) 到图像识别和推荐系统。通过理解嵌入的基本原理，你将深入了解如何利用这些强大的工具来增强软件开发项目。

首先，让我们揭开嵌入（embeddings）的本质。

嵌入就像是数据的 DNA，提供了一种结构化的表示，计算机可以轻松理解和操作。想象一下，一个图书馆里充满了各种类型的书籍，每本书都代表人类知识和经验的独特方面。 现在，假设你被要求组织这个庞大的收藏。 你决定根据书籍的主题、主题 和内容进行分类。

为了实现这一点，你创建了一个复杂的系统，其中每本书都由一组属性表示——可能是它们的类型、作者、出版年份和主题。这些属性共同形成每本书的多维向量，将其定位在一个反映其特性的概念空间中。具有相似属性的书籍在这个空间中聚集在一起，反映了它们的主题或概念上的相似性。

在这个类比中，嵌入（embeddings）充当组织图书馆的蓝图。通过将书籍的非结构化数据转换为结构化向量，嵌入使计算机能够导航并理解图书馆内容的复杂性。 正如图书管理员可以根据书籍的属性高效地定位书籍一样，算法可以根据其 嵌入表示来分析和操作数据。

OpenAI 通过其先进的嵌入模型将这一概念提升到了新的高度。 这些模型利用复杂的算法和庞大的数据集来捕捉文本数据的细微表示。 通过考虑不仅单个单词，还包括它们的上下文用法，OpenAI 的嵌入产生了更精确和有意义的 向量表示。

此外，OpenAI 的嵌入由最先进的机器学习技术支撑，使它们能够从大量数据中提取见解。 这种能力使嵌入能够识别和利用数据景观中的复杂模式和关系，超越了传统缩放和 降维方法的限制。 因此，OpenAI 的嵌入在识别和利用数据景观中的相似性和差异性方面具有卓越的能力。

接下来，我们将学习如何使用嵌入来比较两个句子的意义以寻找相似之处。 由于我们在比较之前将句子转换为向量，因此我们需要了解术语 **余弦相似度**。简洁的相似度度量提供了两个向量之间相似度程度的紧凑表示，允许对文本元素（如句子或文档）进行高效的比较和分类。 或文档。

余弦相似度通过测量两个向量之间角度的余弦值来量化两个向量之间的相似度。 它的范围从 -1 到 1，其中 1 表示完美的相似度，0 表示没有相似度，-1 表示 完美的不同度。

从我们之前提到的内容来看，如果我们想检查两个句子之间的相似度，我们需要使用 OpenAI API 找到它们的向量，并使用余弦相似度方法进行比较。 我们可以轻松地利用 Python 来完成这项工作，并将句子表示为向量，然后使用余弦相似度计算它们的简洁相似度。 以下是一个演示 此过程的 Python 脚本：

```py
 from openai import OpenAI
import numpy as np
import config
from sklearn.metrics.pairwise import cosine_similarity
client = OpenAI(api_key=config.API_KEY)
def get_embedding(sentence, engine="text-embedding-3-large"):
    response = client.embeddings.create(
      input=sentence,
      model=engine
    )
    embedding = response.data[0].embedding
    return np.array(embedding)
def compare_sentences(sentence1, sentence2, engine="text-embedding-3-large"):
    embedding1 = get_embedding(sentence1, engine)
    embedding2 = get_embedding(sentence2, engine)
    # Compute cosine similarity
    similarity = cosine_similarity([embedding1], [embedding2])
    return similarity
# Example Usage
sentence1 = "I love reading books." sentence2 = "Reading is my favorite hobby." similarity_score = compare_sentences(sentence1, sentence2)
print("Similarity:", similarity_score)
```

在这里，代码导入了 必要的库，包括 OpenAI 的 API 客户端、用于数值计算的 NumPy 以及 scikit-learn 的 `cosine_similarity` 函数，用于计算余弦相似度。 它定义了一个函数， `get_embedding`，用于使用 OpenAI 的文本嵌入模型获取输入句子的嵌入。 然后， `compare_sentences` 函数利用 `get_embedding` 获取两个输入句子的嵌入，并计算它们的 余弦相似度。

一个示例用法展示了两个句子（**我爱读书。** 和 **阅读是我最喜欢的爱好。**）的比较，并打印出产生的相似度分数。 鉴于这些句子的语义邻近性，相似系数预计会很高， 接近 1。

执行代码后，代码返回以下 相似度分数：

```py
 Similarity: [[0.69882148]]
```

这个分数表示高度相似，因为它超过了 0， 接近 1。

接下来，通过将`sentence2` 的值更改为 **金融市场已经度过了更好的日子** 并重新运行脚本，我们得到了一个 不同的结果：

```py
 Similarity: [[0.0532481]]
```

余弦相似度系数显著下降，现在比之前低 10 多倍，更接近 0。 这反映了这两个句子之间存在的实质性差异。

通过使用嵌入和余弦相似度指标探索语义相似度，出现了引人注目的发现，揭示了这项技术的深远影响和实际应用。 通过比较具有不同程度语义相似度的句子，我们 观察到它们的文本内容和产生的相似度分数之间存在明显的相关性。 这强调了嵌入在捕捉和量化文本数据点之间语义关系方面的有效性。 这些见解超越了简单的句子比较，为各种应用提供了宝贵的实用性。 例如，在社会媒体数据的情感分析中，嵌入能够从 X（前身为 Twitter）等平台提取细微的意见，使企业和研究人员能够衡量公众情绪，追踪趋势，并指导决策。 此外，在推荐系统中，嵌入有助于根据其潜在的语义相似性识别相关项目或内容，增强用户体验，并推动参与度。 因此，嵌入在利用非结构化数据中嵌入的信息财富方面发挥着不可或缺的作用，使各个领域实现变革性进步。 的各个领域。

# 摘要

在本章中，我们探讨了各种 ChatGPT API 模型。在 *ChatGPT API 模型 – GPT-3.5、GPT-4 以及更高级的模型* 部分，我们探讨了不同的 ChatGPT API 模型。 随后，我们为您提供了对这些 AI 模型及其特性的深入了解，使您能够为您的特定应用选择最合适的模型。 本章强调了在选择模型时考虑成本、质量和提示长度等因素的重要性，因为最先进和功能强大的模型并不总是最佳选择。 此外，我们使用 Python 比较了不同模型的响应和成本，有助于决策过程。

我们还关注了 ChatGPT API 的各种参数及其对响应质量的影响。 我们强调了如模型、消息、温度、最大令牌数、停止和 n等关键参数，并解释了如何操作它们以优化与 ChatGPT API 的交互。 您了解了在保持 ChatGPT API 稳定性和公平性方面，速率限制的重要性。 最后，我们探讨了如何实施适当的策略，以提高使用 ChatGPT API 的效率和成本效益。

在第 *12 章*中，*微调 ChatGPT 以创建独特的 API 模型*，我们将深入了解微调 ChatGPT API 模型的过程。 本章旨在为您提供教授 ChatGPT 针对特定项目或应用定制信息的必要技能。 通过一系列案例研究，您将深入了解微调在现实世界中的应用，并受到鼓励进行创造性思考。 此外，我们将强调微调在开发 AI 应用中的成本节约潜力。
