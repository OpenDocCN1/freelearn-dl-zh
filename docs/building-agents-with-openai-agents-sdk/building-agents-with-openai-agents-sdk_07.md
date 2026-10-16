## 模型与上下文管理

到目前为止，在使用 OpenAI Agents SDK 构建智能体时，我们主要依赖默认的 GPT-4o 模型。然而，Open Agents SDK 最强大的功能之一是其模型无关的设计。也就是说，你并不受限于单一的模型提供商或配置。这种灵活性在复杂工作流或多智能体系统中变得尤为宝贵，因为过程的不同阶段可能受益于不同的能力或成本/延迟。

考虑一个多智能体系统，它从一个分诊代理（triage agent）开始，其简单的工作向用户提出简单的问题。对于这项任务，使用 `GPT-4` 可能并不合适。相反，我们可能想要简单的模型或非 OpenAI 的开源模型，例如 `LLaMA`，它的成本几乎为零。同时，一些要求更高的任务（例如执行研究或数学运算）可能需要先进的 Anthropic 模型。同时，头脑风暴代理可能需要标准的 `GPT-4o` LLM，但需要更高的温度（temperature）。

`OpenAI Agents SDK` 支持这种架构模式。每个智能体可以使用不同的模型及其自有的配置设置。

你将在本章中学到以下内容：

- **模型管理**：我们将首先学习如何调整模型、模型设置，以及如何将第三方模型集成到智能体中。

- **上下文管理**：你将学习如何利用本地上下文将信息传递给工具，而不让 LLM 获知这些信息。

在本章结束时，你将学会如何在智能体解决方案中有效地管理模型和上下文。

## 技术要求

请遵循第 3 章中的详细步骤设置你的环境。

在本书中，每章的实用示例和完整代码将通过附带的 GitHub 仓库提供 [`github.com/PacktPublishing/Building-Agents-with-OpenAI-Agents-SDK`](https://github.com/PacktPublishing/Building-Agents-with-OpenAI-Agents-SDK)。

鼓励你克隆仓库，复用并适配提供的代码示例，并在学习章节时根据需要参考它。

## 模型管理

在 `OpenAI Agents SDK` 中，每个智能体必须由一个 `LLM` 驱动。`LLM`充当智能体的大脑，能够读取信息、调用工具并生成响应。到目前为止，我们在实例化智能体时使用的是默认配置。这会将智能体的模型默认为 `GPT-4o` 并使用默认模型设置。在本节中，我们重点介绍如何以及何时修改此参数。

### 调整底层模型

`OpenAI Agents SDK` 允许你显式地选择要使用的底层模型。每个模型都有其优缺点，为你正在构建的特定类型的智能体选择正确的模型是有益的。例如，`GPT-4o` 可以产生快速且准确的响应，而 `o3-pro` 可以推理并解决复杂问题，但速度慢得多。

要调整模型，你可以在实例化智能体时直接使用 `model` 参数。此演示将展示如何调整智能体使用的模型：

```python
agent = Agent(
    name="SampleAgent",
    instructions="You are an AI agent",
    model="gpt-4o"
)
```

该参数可以接受 OpenAI 的任何有效模型名称。要查看所有模型名称列表及其优处和成本，请访问 [`platform.openai.com/docs/models`](https://platform.openai.com/docs/models)。

让我们通过一个例子来说明为什么使用此参数选择正确的模型是至关重要的。在这个例子中，我们将创建两个智能体，每个智能体使用不同的模型参数（`GPT-4o` 与 `o3-pro`）。我们将看到它们如何回答同一个问题，以及每个回答需要多长时间。

创建一个名为 `different_models.py` 的新 Python 文件并输入以下代码。此演示将向我们展示如何拥有不同底层模型的智能体：

```python
from agents import Agent, Runner
import time
# 创建两个智能体
gpt4o_agent = Agent(
    name="GPT4o Agent",
    instructions="You are an AI Agent",
    model="gpt-4o"
)
o3pro_agent = Agent(
    name="o3-pro Agent",
    instructions="You are an AI Agent",
    model="o3-pro"
)
prompt = "How many integers from 1 to 10000 (inclusive) are divisible by 3 or by 5 but not by both? Do reasoning but only return only the answer."
print("gpt4o agent")
start_fast = time.time()
response = Runner.run_sync(gpt4o_agent, prompt)
print(response.final_output)
end_fast = time.time()
print(f"Time taken: {end_fast - start_fast:.2f} seconds")
print("---")
start_fast = time.time()
print("o3pro agent")
response = Runner.run_sync(o3pro_agent, prompt)
print(response.final_output)
end_fast = time.time()
print(f"Time taken: {end_fast - start_fast:.2f} seconds")
print("---")
```

运行此程序将得到以下输出：

```text
gpt4o agent
3334
Time taken: 1.18 seconds
---
o3pro agent
4001
Time taken: 8.41 seconds
---
```

在实践中，`GPT-4o` 智能体通常在几秒内响应，但遗憾的是它对这个复杂的数学题给出了错误答案。然而，`o3-pro` 智能体回答正确，但花费的时间是八倍（实际上成本可能比标准的 `GPT-4o` 模型高出 10 倍）。

因此，准确性与延迟/成本之间存在固有的权衡，但 `OpenAI Agents SDK` 允许你为智能体选择正确的模型。

### 调整模型设置

让我们不仅选择使用哪个模型。通常，你希望保持相同的模型，但你可能希望微调模型生成响应的方式。OpenAI SDK 暴露了 `ModelSettings` 对象传递给智能体。

- `temperature`：控制输出的随机性。较低的值（如 `0.2`）使模型更具确定性，而较高的值（如 `0.8`）生成更具创意或多样化的响应。

- `max_tokens`：限制模型可以生成的标记（或单词）数量。这有助于控制智能体响应长度。

你可以在 OpenAI 参考指南中找到这些设置的完整列表：[`openai.github.io/openai-agents-python/ref/model_settings/#agents.model_settings`](https://openai.github.io/openai-agents-python/ref/model_settings/#agents.model_settings)

我们将通过一个例子来说更改模型设置对其产生的影响。与之前的例子类似，我们将创建两个智能体：一个是高温度和长 `max_tokens` 设置，另一个是低温度和短 `max_tokens` 设置。

创建一个名为 `different_model_settings.py` 的新 Python 文件并输入以下代码：

```python
from agents import Agent, Runner
from agents.model_settings import ModelSettings
### 创建智能体
creative_agent = Agent(
    name="CreativeAgent",
    instructions="You are an AI agent that answers questions.",
    model="gpt-4o",
    model_settings=ModelSettings(
        temperature=1.0,
        max_tokens=300
    )
)
precise_agent = Agent(
    name="PreciseAgent",
    instructions="You are an AI agent that answers questions.",
    model="gpt-4o",
    model_settings=ModelSettings(
        temperature=0.2,
        max_tokens=50
    )
)
prompt = "Describe the future of AI in customer service."
print("Creative agent:")
response = Runner.run_sync(creative_agent, prompt)
print(response.final_output)
print("---")
print("Precise agent:")
response = Runner.run_sync(precise_agent, prompt)
print(response.final_output)
print("---")
```



运行此脚本将产生两种非常不同的输出风格。`creative_agent` 对象产生了更长、更有想象力的响应，有时会包含推测或未来的语言。`precise_agent` 对象则给出了更短、更谨慎的总结。事实上，由于 `max_tokens` 参数非常小，`precise_agent` 对象实际上被截断了。以下显示了两个输出：

```
创意型代理：
AI 在客户服务领域的未来将是变革性的，继续提升效率、个性化和客户满意度。以下是一些关键趋势和发展：
1. **高级聊天机器人和虚拟助手**：AI 驱动的聊天机器人将变得更加复杂，处理复杂的查询，更好地理解上下文，并提供更像人类的交互。它们将无缝集成到多个通信渠道中。
…
6. **AI 代理协作**：人类代理将越来越多与 AI 协同工作，利用 AI 驱动的洞察增强自身的响应能力。AI 将处理例行查询，释放人类专注于复杂问题。
7. **自服务优化**：AI 将增强自服务平台，让客户更容易、更快地找到解决方案，无需人工干预。
---
精准型代理：
AI 在客户服务领域的未来有望带来变革，提供增强的效率、个性化和可扩展性。以下是一些关键趋势和发展：
1. **24/7 全天候**：AI 驱动的聊天机器人和虚拟助手将提供全天候
```

这个简单的示例展示了调整模型设置（例如控制随机性的 `temperature` 和控制响应长度的 `max_tokens`）如何显著改变代理的语气和冗长程度，而无需更改底层模型。这使你能够将同一个模型适配到不同类型的代理，例如客户服务销售机器人与研究想法生成器，只需修改模型设置即可。

## 第三方模型

正如我们在本章开头提到的，OpenAI Agents SDK 是模型无关的（model-agnostic）。它可以运行于任何提供商的任何模型，只要它遵循某些特性。虽然 GPT-4o 和其他 OpenAI 模型是极好的默认选择，但在许多情况下，使用不同的 LLM 可能会更好。例如，Anthropic 的 Claude 模型可能能够更好地遵循某些指令，或者你可能出于成本考虑想使用 Meta 的 LLaMA 模型。

OpenAI Agents SDK 通过 `LiteLLM` 实现了兼容其他模型。`LiteLLM` 是一个轻量级的 Python 库，为许多流行的 LLM 提供商（Anthropic、Google 等）提供了统一的 API。使用 `LiteLLM` 集成第三方模型变得非常简单。`LiteLLM` 在后台处理 API 密钥管理、请求路由和响应格式化，因此你可以在不更改代理代码的情况下切换模型提供商。

第一步是安装 `LiteLLM` 库。为此，打开你的终端并输入以下命令：

```
$ pip install "openai-agents[litellm]"
```

之后，我们需要一个非 OpenAI 模型提供者的 API 密钥。在本示例中，我们将选择 Anthropic。获取 Anthropic API 密钥，请访问 `https://www.anthropic.com/` 创建账户并进入账户仪表板。在 `API keys` 下点击 `Create new key`，然后复制你的 API 密钥。之后，回到我们在*第 3 章*中设置的 `.env` 文件，添加包含以下内容的新行：

```
ANTHROPIC_API_KEY=sk-ant-api03-[your-remaining-api-key]-995
```

安装好 `LiteLLM` 并配置所需的 API 密钥后，我们现在可以使用 Anthropic 的 LLMs 构建 AI 代理。为此，你只需要在实例化代理时将 `model` 参数设置为指定模型参数和模型的 LiteLLM 字符串即可。以下是一些 LiteLLM 字符串示例：

* `litellm/gemini/gemini-pro`

* `litellm/anthropic/claude-opus-4-20250514`

* `litellm/meta_llama/Llama-3.3-70B-Instruct`

让我们来看一个简单的示例。创建一个名为 `third_party_models.py` 的文件并输入以下代码：

```python
from agents import Agent, Runner
import time
### 创建一个代理
agent = Agent(
    name="Claude Agent",
    instructions="You are an AI Agent",
    model="litellm/anthropic/claude-opus-4-20250514"
)
question = "我该如何重启电脑？请用几个字回答。"
response = Runner.run_sync(agent, question)
print(response.final_output)
```

运行此程序将像以前一样创建并执行一个代理，但它使用的是 Anthropic Claude 模型来执行。以下是一个响应示例。

```
**Windows:** 开始菜单 → 电源 → 重启
**Mac:** 苹果菜单 → 重启
**或者:** 长按电源按钮，然后重新开启
```

通过这种方式，你可以无缝切换代理底层的 LLM，而无需重构你的逻辑或流水线。无论是由 GPT-4o、Claude 还是 LLaMA 驱动，代理的行为都是相同的（只要模型接口在 `LiteLLM` 内部处理）。

这种抽象在你为特定用例对不同提供商进行基准测试，或者在具有不同隐私、延迟或成本限制的环境中部署代理时非常有用。例如，你可能想在开发期间使用 GPT-4o 进行原型设计，并在环境中切换到 Claude 以获得更好的摘要。

## 上下文管理

`context` 指代理可以访问的所有内容。一个很好的类比是：代理的 LLM 是它的大脑，而代理的上下文（context）是传递给大脑以生成响应的信息。我们已经讨论了向代理暴露重要信息的几种方法，包括通过系统指令、之前的对话历史、提示词注入（prompt injections），甚至通过工具调用进行知识检索。

在本节中，我们将上下文范围缩小到**本地上下文（local context）**。这指的是实例化代理所需的信息，并作为工具（tools）和钩子（hooks）的依赖。

## 本地上下文

本地上下文允许代理访问某些信息（在代理实例化时产生），而这些数据无需显式地成为 LLM 提示词的一部分。这对于存储用户特定信息（例如用户 ID、名称、偏好）非常有用，以便工具根据这些信息获取或计算答案。这开启了一种强大的设计模式。它允许代理使用特权或应用程序数据（如用户偏好、身份验证令牌或内部状态）运行，而无需将这些数据嵌入发送给模型的提示词中。这意味着代理可以根据敏感或专利信息做出决策或生成个性化输出，同时将这些信息完全留在 LLM 的提示词之外。

在 OpenAI Agents SDK 中，你通过创建一个对象并在调用 `Runner` 时通过 `context` 参数传递它来实现本地上下文。在使用该对象实例化代理后，任何工具都可以使用它，只要该工具也包含 `context` 参数。

让我们通过一个具体的例子来说明。假设我们的客户服务代理可以根据用户配置和活跃订单检查订单物流状态。我们将通过提供一个包含用户名和订单详情的 `OrderContext` 对象，以及一个使用这些信息提供物流更新的工具函数来模拟这一点。

首先，创建一个新的 Python 文件名为 `local_context.py` 并输入以下代码：

```python
from dataclasses import dataclass
from agents import Agent, Runner, RunContextWrapper, function_tool
@dataclass
class OrderContext:
    customer_name: str
    order_id: str
    shipping_status: str
order_context = OrderContext(
    customer_name="Henry Habib",
    order_id="123",
    shipping_status="Delayed"
)
@function_tool
def get_shipping_status(wrapper: RunContextWrapper[OrderContext]) -> str:
    """Provide the shipping status for the current order."""
    ctx = wrapper.context
    return (
        f"Hi {ctx.customer_name}, your order {ctx.order_id} is currently: "
f"{ctx.shipping_status}."
    )
agent = AgentOrderContext
question = "Where is my order?"
result = Runner.run_sync(agent, input=question, context=order_context)
print(result.final_output)
```

我们首先定义了一个名为 `OrderContext` 的数据类，它包含了三个字段：客户姓名、订单 ID 以及当前的物流状态。此对象代表了我们希望智能体（agent）访问的本地上下文（即工具函数可以直接调用的信息）。

然后，我们创建了一个包含示例客户和订单信息的 `OrderContext` 对象。在实际应用中，此上下文可能会根据该智能体在何处被“调用”而为每个用户会话动态生成。

接下来，当我们定义名为 `get_shipping_status` 的工具函数时，我们让它接收一个上下文对象（`RunContextWrapper[OrderContext]`）作为参数。这告诉智能体将上下文对象作为参数传递给工具函数。当我们定义一个名为 `"Shipping Support Agent"`（物流支持智能体）的智能体时，它配备了 `get_shipping_status` 工具，但最重要的是，智能体使用了相同的 `OrderContext` 类型进行参数化，因此它知道自己将接收什么类型的上下文。这非常重要。这是在智能体实例化时（`agent = Agent[OrderContext]`）完成的。

由于智能体具有这个额外的参数，在调用 `Runner.run_sync()` 时，必须定义该类型的上下文对象。SDK 会自动将查询路由到工具，工具使用上下文数据生成响应。

这是一个示例响应，显示了智能体将上下文信息传递给了工具函数调用：

```
Hi Henry Habib, your order (123) is currently delayed. If you have any further questions or need assistance, please let me know!
```

这说明了本地上下文的核心优势之一：智能体可以使用从未出现在模型提示词（prompt）中的敏感信息来生成准确且个性化的响应。总而言之，本地上下文对象为你的智能体提供了一种“内部内存”，这种内存并不会直接暴露给用户或模型提示词。它是向智能体工具链中注入领域知识或用户特定数据的一种形式。

# 总结

在本章中，我们涵盖了如何选择和配置每个智能体背后的模型。我们学习了如何为每个智能体调整模型及其设置。我们学习了如何通过 `LiteLLM` 集成第三方模型，从而在 Claude、Gemini 和 LLaMA 等提供商之间轻松切换。最后，我们介绍了本地上下文，这是一种让智能体访问敏感或会话特定数据的方法，而无需将其包含在模型提示词中。

这些技术的结合让你能够完全控制智能体的思考和响应方式。在下一章中，我们将重点转向智能体的管理、管理和安全保障。你将学习如何监控智能体活动、执行护栏（guardrails）以及在生产环境中应用治理最佳实践。

## 立即解锁本书的专属福利

扫描此二维码或访问 [`packtpub.com/unlock`](https://packtpub.com/unlock)，然后按名称搜索此书。

| ![一个白色背景上的二维码，AI 生成的内容可能不正确。](img/Unlock-01.png) | ![一个白色背景上的二维码，AI 生成的内容可能不正确。](img/Unlock2.png) |

**注意**：在开始之前请准备好您的购买发票。

