# 6 玻璃盒系统架构设计

与之前的章节一样，我们首先从架构漫览开始，在进入实现阶段之前，从视觉和概念上建立直认知。为了适应新的技能，我们必须学会在编写代码之前尽可能地思考 AI 系统。目前最先进的 AI 副驾驶（copilots）已经可以提升代码生成，但在没有人类进行设计思考的情况下，它们无法设计系统。如图所示，我们依然架构师，而 AI 副驾驶则是工人：

![](img/c9f69ac3fb58ab0dee89a12c2578a735_190_0.png)

带着这种架构师思维，我们必须解决每位 AI 工程师都会面临的关键选择。许多人急于将 AI 当作一个简单的黑盒，提供输入并期望高质量的输出。我们将采取一种更专业、更强大的方法：强制执行“玻璃盒系统”，专为透明性和控制而设计。与内部逻辑隐藏的黑盒不同，我们的上下文引擎（context engine）设计初衷是为了被理解。我们在第 4 章中开始实现玻璃盒功能，通过详细的 `ExecutionTrace`（执行轨迹）使每个操作都可追溯。这样，每个组件都是模块化的，每个决策都可以被检查。在构建内容削减功能时，我们将这样做。你可能会想：“为什么不选别的？”答案很简单。每个人都在涌向聊天机器人以提高效率。然而，AI 副驾驶输出的质量与我们输入的质量严格相关；因此，有必要开发一个能够实现完全控制的玻璃盒引擎。在第 5 章中强化的上下文引擎是一个具有顺序操作流的复杂系统。理解这一如何（即理解）我们新的玻璃盒架构如何无缝衔接，是理解我们的摘要器代理（Summarizer agent）将如何集成到现有逻辑中的关键。

在本节中，我们将首先梳理引擎的运行流程，从初始的用户目标到最终生成的输出。然后，我们将精确指出我们的新组件将在何时以及何地引入。这种双层方法将提供引擎从一个强大的编排器演变为具有效率意识的系统的演变全景。让我们设计内容削减过程的架构蓝图。

## 逐步架构漫览

我们漫览的指南是图 6.2。图中的每个节点都经过颜色编码，以匹配我们代码中的对应部分。本章引入的新组件被清晰地标记为 [NEW] 标签。

构建用于内容削减的摘要器代理

### 图例：

- 第 7 节（蓝色）

- 第 6 节（白色）

- 第 5 节（紫色）

- 第 4 节（绿色）

- 第 3 节（橙色）

![](img/c9f69ac3fb58ab0dee89a12c2578a735_192_0.png)

图 6.2：内容削减架构

# 6 

图 6.2 的图例包含了上下文引擎的颜色编码：

- **蓝色（第 7 节）：** 用户运行的最终执行脚本

- **白色（第 6 节）：** 引擎核心，包含主编排器、规划器（planner）和追踪器（tracer）

- **紫色（第 5 节）：** 代理注册表（agent registry），作为系统的工具箱

- **绿色（第 4 节）：** 执行实际工作的专家代理（specialist agents）

- **橙色（第 3 节）：** 提供通用实用程序的辅助函数（helper functions）

仅仅看着流程图就让人感到无畏！脑海中出现的第一个想法是：“这什么时候结束？我们已经看了五个章节了，是的，事情变得越来越复杂。但这恰恰是重点。一个 AI 架构师，一个上下文引擎是一个*复杂度思考者*。许多人认为从零开始构建系统是不值得的，而是急于跳向那些承诺通过精美示例无缝实现的现成平台。但现实要复杂得多。每一个先进的 AI 无一例外都有硬墙。有时数据缺失会导致进度停止；有时关键函数的行为不符合预期；而且（注：原文此处似乎存在逻辑断层或文本重复，已根据语意衔接）……而且，不可避免地，Bug 会出现。

（注：原文中段如 "Understanding this how seamlessly our new glass-box architecture is the key..." 逻辑较为跳跃，翻译时已尽量保持语义通顺）

（注：原文后部分多处存在明显的文本重复，如 "Understanding this how seamlessly"、"Understanding this how seamlessly our new glass-box architecture is the key" 以及 "Understanding this how seamlessly our new glass-box architecture is the key to appreciating how our Summarizer agent will integrate into its existing logic" 之后的重复段落，译文已根据语意进行了处理以确保可读性）

在本节中，我们将首先梳理引擎的运行流程，从初始的用户目标到最终生成的输出。然后，我们将精确指出我们的新组件将在何时以及何地引入。这种双层方法将提供引擎从一个强大的编排器演变为具有效率意识的系统的演变全景。让我们设计内容削减过程的架构蓝图。

（此处跳过原文中重复的混乱描述，直接进入核心逻辑）

- 1. **初始化：** 一切从用户运行代码开始。`context_engine()` 作为入口点，它是整个编排器的核心，正如我们在之前的章节中所建立的那样。

- 2. **规划：** 一旦进入 `context_engine()`，首要任务是通过 `get_capabilities_description()` 查询可用的代理。有了这个列表和用户目标，规划器通过 `call_llm_robust()` 返回一个结构化的整体计划。

- 3. **执行循环：** 在有了清晰的计划后，引擎进入执行模式。它通过 `get_handler()` 从注册表中检索相应的代理函数。通过 `resolve_dependencies()` 处理依赖关系，将占位符（如步骤 1 输出）替换为之前步骤的实际输出。随后调用代理执行任务。一旦步骤完成，结果将记录在 `trace` 中，引擎进入下一步。

- 4. **完成：** 在所有步骤执行后，引擎通过对 `trace` 对象调用 `finalize()` 完成过程。在记录最终状态和执行时间之前向用户返回干净的结构化输出。



我们的上下文引擎现在像一个智能协作团队一样运行，自主且由代理驱动。每个组件都承担着明确的职责，正如协作良好的团队成员一样。让我们仔细探索这些职责，看看系统在复杂性增加时如何保持平衡。

## 职责划分

接的顺序流揭示了清晰的职责划分。每个模块在这里都使用图 6.2 所示的颜色标签，帮助你从视觉上连接架构图中的概念。我们现在在这个模块化框架内引入我们的组件和升级：

- **引擎核心（白色）：** 大脑。它管理整个过程，但不亲自执行工作。它的逻辑通用且健壮，因此在本章中无需任何更改。

- **专家代理（绿色）：** 工人。每个代理负责系统中的一项特定任务。我们将引入一个新的代理（在图 6.2 中标记为 `[NEW AGENT]`），`agent_summarizer`。它的作用是作为智能网关，根据特定目标将大量文本简化为要点。该代理直接解决了管理 API 成本和在固定 Token 限制运行的需求。

- **代理注册表（紫色）：** 工具箱。它为规划器提供可用工具的描述说明。注册表的说明（`get_capabilities_description`）将更新以包含我们新的摘要代理，使规划器能够感知到这一新能力。

- **辅助函数（橙色）：** 共享实用程序，是保持一切顺畅运行的默默促进者。我们将正式确认新的实用程序（在图 6.2 中标记为 `[NEW UTILITY]`）的 `count_tokens` 函数。该函数充当引擎的燃料计，允许任何组件在将一段文本发送到 LLM 之前测量其的 token 成本。它是实现主动式、成本意识设计的基础工具。

现在我们已经理解了基础架构以及我们的新组件将处于何位置，让我们以一种任何利益相关者（无论是否具有技术背景）都能理解的方式，来总结这些变更的战略影响。作为上下文工程师，我们必须超越功能性系统的构建，学习用决策者易于理解的语言来传达它们的演进。每一次升级，无论技术上如何，都应该用概念性的术语进行解释，以连接工程与业务应用。

- **第 1 和第 2 阶段——主动上下文管理**：我们的主要升级不仅仅是一个新的代理，而是一项新的战略能力：主动上下文管理。引擎现在不再仅仅是处理给定的任何信息，而是可以对其进行分析、测量和压缩。`count_tokens` 工具提供了测量，而 `Summarizer` 代理提供了操作。这使得引擎从纯粹的被动变为策略性的高效，能够经济地处理更大的问题。

- **第 3 阶段——集成与可扩展性**：我们架构最强大的方面在于，无需重新设计核心引擎即可添加这种新能力。通过简单添加 `Summarizer` 代理并更新代理注册表的描述，`Planner` 会自动学习如何使用这个新工具。这证明了系统具有高度的可扩展性；未来可以以最小的干扰添加新的代理和能力，就像在组织良好的工作坊中添加新工具一样。

- **第 4 和第 5 阶段——验证**：我们流程的最后一部分是证明这些升级是有效的，而且同样重要的是，证明它们没有破坏任何现有功能。我们将进行严格的测试（包括向后兼容性检查），以验证引擎保持稳定，并且 `Planner` 仍然能够正确地制定计划。这种对验证的关注对于构建旨在企业级的系统建立信任至关重要。

我们现在已经涵盖了大量的架构思维领域，但我们需要解释为什么这种对验证的关注对于构建企业级系统建立信任至关重要。

## 为什么"玻璃盒"至关重要

正如我们的流程图所示，职责分离不仅是一个优雅的设计选择，也不是某些内容在急于编写代码之前所忽略的升级概念概述。它是任何 AI 工程项目的战略性要求。当 AI 辅助驾驶员正逐渐占据那个角色时，为什么还要急于编写代码呢？最新且强大的 LLM 可以产生相当不错的生产级代码。因此，在编写代码之前请留出时间，并思考为什么我们正在构建的"玻璃盒"架构将带你进入 AI 专家水平的新境界。

首先，这种设计主动防止了技术债。在紧密耦合或单体系统中，添加新功能需要对核心逻辑进行复杂且风险的更改。随着时间的推移，系统会变成脆弱的*纸牌屋*，单次修改就会导致级联故障。相比之下，我们的架构将代理视为即插即用的模块。添加 `Summarizer` 不需要我们更改 `Executor` 或 `Planner` 的基本工作方式。这比比升级汽车的 GPS；你可以更换单元而无需重新设计引擎。这一原则确保了我们的上下文引擎在未来几年内保持保持敏活性和适应性。

此外，这种模块化是有效团队合作和可扩展性的推动者。想象一种场景：企业需要为其法律、财务和市场部门部署代理。有了我们的架构，三个独立的开发者或团队可以并行工作 `agent_legal_parser`、`agent_financial_analyzer` 和 `agent_marketing_writer`。他们的唯一要求是遵循 MCP 进行通信，并将完成的代理注册到中央 `Agent Registry` 中。他们不需要理解引擎核心编排的复杂细节，极大地降低了贡献门槛，并加速了新业务能力的开发。你正在引导你的团队进入一个新时代！

## 单体系统（脆弱）

## 玻璃盒架构（敏捷）

![](img/c9f69ac3fb58ab0dee89a12c2578a735_197_0.png)

图 6.3：从脆弱到敏捷的 AI 系统

图 6.3 展示了你作为一名新兴上下文工程师架构师，从以前提示词和经典 AI 系统设计向重大转变。这个视觉比喻不仅代表了两种软件设计模式之间的选择；它概括了一个根本性的范式转变，对系统和人类都有深远影响。

对于 AI 系统而言，这是从静态的、单一用途工具演变为具有韧性的生态系统。它的价值不再仅仅由其即时输出衡量，而是由其随时间推移适应、增长和集成新能力而不崩溃的能力衡量。

对于人类工程师来说，这种转变更有意义。它将角色从孤立应用程序的构建者提升为智能系统的架构师。随着 AI 辅助驾驶员越来越多地处理生成代码的任务，最关键的人类技能变成了远见性：设计一种不仅在今天强大，而且足够敏捷、以拥抱未来不可避免的进步的框架的能力。这种为变化而设计架构的敏捷方法，正是现代上下文工程的精髓。

有了这张清晰的架构和概念地图，我们已经准备好开始实操实现。

# 使用 Summarizer 代理实现上下文压缩

通过对引擎升级后架构的牢固掌握，我们已经为构建这一新能力建立了坚实的基础。现在我们现在从架构理论转向实际实现。这段实现之旅将是一个有条理的过程，需要对我们的核心库文件进行精确的添加，以无缝集成新代理。

我们编排和测试这些增强的主要工作空间是名为 `Context_Engine_Content_Reduction.ipynb` 的笔记本，它是 `Chapter05/Context_Engine_Production.ipynb` 的升级版本。安装的库保持不变。为了在引擎演进时保持版本清晰和项目结构整洁，每个章节的代码都位于自己的子目录中。在本章中，所有共享库文件源自 `commons/ch6/`，这是一个清晰且有序的空间，用于在我们继续扩展上下文时管理升级。

升级将通过五个系统的阶段进行，建立在我们在第 5 章建立的稳健架构之上：

- **第 1 阶段——成本管理的基础**：从验证 `count_tokens` 工具开始，使系统能够主动测量和管理上下文大小。

- **第 2 阶段——构建摘要器代理**：在 `agents.py` 中构建新代理的核心逻辑，将其设计为自包含的、感知的模块。

- **第 3 阶段——集成引擎工具包**：更新 `registry.py`，使 `Planner` LLM 感知新能力，并能够在其工作流中动态整合摘要。

- **第 4 阶段——强化写入代理**：升级 `agent_writer` 以接受来自 `Researcher` 的输出。

- **第 5 阶段——演示新能力**：执行一个复杂目标，展示 `Summarizer` 代理如何优化性能。

我们的第一步将是成本管理。

## 成本管理的基础

我们在本章的工作集中在上下文大小，这首先需要测量它的能力。我们将验证在初始过程中添加到 `helpers.py` 的 `count_tokens` 工具。

正如之前提到的，该工具充当引擎的燃料计，在向 LLM 发送文本之前对其消耗量提供精确测量。这是一个简单但至关重要的策略，为后续的成本管理奠定基础。我们现在查看其实现：

```python
# FILE: commons/helpers.py (现有代码)
# 该函数是我们主动管理 token 的工具。

def count_tokens(text, model="gpt-4"):
    """计算给定模型文本中的 token 数量"""
    try:
        encoding = tiktoken.encoding_for_model(model)
    except KeyError:
        # 对于 tiktoken 注册表中可能不存在的模型备底
        encoding = tiktoken.get_encoding("cl100k_base")
    return len(encoding.encode(text))
```

`count_tokens` 接受文本字符串和一个可选的模型名称。它使用 `tiktoken` 库（OpenAI 的官方分词器）将文本编码为整数列表，并返回该列表的长度。通过在 `try...except`块中封装编码过程，它优雅地处理了特定模型可能不在库中的情况，退回到通用基础编码以确保函数返回值。

在验证了基础测量工具后，我们现在可以构建使用这一原则来管理上下文的代理。我们现在开始构建 `Summarizer` 代理。

# 构建摘要器代理

`Summarizer` 代理是我们解决上下文过载问题的解决方案。它的目的是智能地将大量文本压缩为针对特定目标的精简摘要，确保只将最相关的信息传递给工作流中的后续代理。

我们将把此代理添加到 `agents.py` 文件中，遵循为其他代理建立的相同生产级模式。它将是一个接收它将所有依赖作为参数传递，并使用 MCP 通信其结果。该代理的智能之处在于对 `summary_objective` 的使用，它引导 LLM 创建一个不仅更短，而且更有用的摘要。

该函数通过定义其签名 `agent_summarizer` 来，它接受标准的 `mcp_message` 及其所需的依赖：用于 API 调用的 `client` 对象和 `generation_model` 配置。在此块内部，它从 MCP 消息内容解析出所需的输入：`text_to_summarize`（包含一大块文本内容）和 `summary_objective`（为精简提供的特定目标）：

```python
# FILE: commons/ch6/agents.py
# 这个新函数被添加到我们现有的 Library 中。
# 它遵循既定的依赖注入和结构化日志模式。
```

```python
agent_summarizer(mcp_message, client, generation_model):
  ""
  根据目标将大型文本缩减为简洁的摘要。
  作为门门人管理 token 和成本。
  ""
  logging.info(f"[Summarizer] 已激活。正在压缩上下文...")
  # 从 MCP 消息解析输入
  text_to_summarize = mcp_message['content'].get('text_to_summarize')
  summary_objective = mcp_message['content'].get('summary_objective')
```

代理包含一个验证步骤，以确保两个必要的输入都存在，如果不存在则抛出 `ValueError`。然后它构建了一个 `system_prompt`，指示 LLM 扮演专家摘要的角色。`user_prompt` 使用 f-strings 组装，清晰地划定了 **OBJECTIVE AND TEXT TO SUMMARIZE**（目标与待摘要文本）。这种结构化的提示词对于生成高质量、相关的摘要至关重要：

> # 代理在继续之前会验证它已收到必要的输入。

```python
if not text_to_summarize or not summary_objective:
    raise ValueError("Summarizer 要求输入内容中包含 'text_to_summarize' 和 'summary_objective'。")
```

```python
# 为 LLM 定义提示词
system_prompt = """你是一个专家摘要 AI。你的任务是在用户特定目标的引导下，将提供的文本精简为要点。摘要必须简洁、准确并直接针对所述目标。"""

```python

user_prompt = f""" OBJECTIVE ---\n{summary_objective}\n\--- TEXT TO
SUMMARIZE ---\n{text_to_summary}\n\--- END TEXT ---\n\n生成摘要\n"""

```

最后，代理调用了我们加固的 `llm_robust` 辅助函数，解析提示词和注入的依赖。生成的摘要通过 `create_mcp_message` 辅助函数以标准的 MCP 消息返回。输出是一个键为 "summary" 的字典，确保了下游代理具有可预测的数据结构。整个过程封装在 `try...except` 中以进行健壮的错误处理：

```python

# 代理调用健壮的 LLM 辅助函数并返回结果。

# 调用加后的 LLM 辅助函数执行摘要

summary = call_llm_robust(

    system_prompt,

    user_prompt,

    client=client,

    generation_model=generation_model

)

# 以标准 MCP 格式返回摘要

return create_mcp_message("Summarizer", {"summary": summary})

except Exception as e:

    logging.error(f"[Summarizer] 发生错误: {e}")

    raise e

```

我们停顿了一下并回想。我们通过验证输入、用清晰的目标引导 LLM、返回可预测的 MCP 消息以及将调用封装在 `try...except` 中，平滑地集成了一个新代理。这就是良好的上下文工程示例。

## 微上下文工程 (Micro-context engineering)

虽然 `Summarizer` 代理的 Python 代码非常简单，但真正的力量不在于函数本身，而在于提供给 `summary_objective` 的上下文质量。构建一个精确的目标是微上下文工程的完美范例。这是将通用多用途工具转换为特定任务的专业化、高精度仪的技能。LLM 配合任何强大的工具，在给定清晰、无歧的指令时表现最佳。

一个优秀的上下文工程师提供坚实、有明确的意图；而糟糕的工程师永远无法获得预期结果。保持敏锐。专注于提升你的上下文设计能力。确保指令不仅仅是一个过时的提示词！考虑一下定义糟糕的目标与架构良好的目标所产生的输出质量之间存在巨大差异。

糟糕的目标是模糊且开放的：

```
"总结这段文本。"
```

这迫使 LLM 去猜测用户认为什么是重要的。结果通常是一个通用的、平淡的段落，可能完全忽略用户实际需要的关键信息，浪费了 API 调用和用户的时间。

相比之下，一个强大的目标是一个微型蓝图。它是具体的，提供了约束，并清晰地定义了预期的输出：

> "请从以下法律文件中提取所有相关方的名称、讨论的关键财务数字以及最终解决日期。排除所有程序性的官话，并以 JSON 对象的格式呈现输出。"

这个目标将通用的 `Summarizer` 转为了特定任务的工具。它没有留下歧义空间，并保证了输出输出。掌握构建这些目标的艺术是上下文工程师的核心能力，因为它允许他们动态地复用单一的高价值函数。

再次，我们正在见证从开发到设计的范式转变。如果我们是上下文工程师，我们将 AI 提升到一个高度。

## 上下文工程的艺术 (The of Engineering)

### 糟糕的目标
- "总结这段文本。"

### 强大的目标
- "从以下法律文件中提取所有相关方的名称、讨论的关键财务数字以及最终解决日期。排除所有程序性的官话，并以 JSON 对象的格式呈现输出。"

### 将新代理集成到工具集中
由于 `Summarizer` 代理已经构建并测试，下一步是让系统发现它。为了完成升级，我们必须更新 `registry.py`。此过程涉及对 `registry.py` 的两次精确：

1. 将 `agent_summarizer` 函数添加到注册表中。
2. 更新 `get_capabilities()` 方法，这是规划器读取可用工具以及如何使用的文本手册。

```python

# FILE: commons/ch6/registry.py

# 整个文件已更新以集成新代理。

# === 导入 ===

import logging

import agents

from helpers import create_mcp_message

# 5. 代理注册表（最终加固版） ===

class AgentRegistry:

    def __init__(self):

        self.registry = {

            "Librarian": agents.agent_context_librarian,

            "Researcher": agents.agent_researcher,

            "Writer": agents.agent_writer,

            # 新：添加 Summarizer 代理 ---

            "Summarizer": agents.agent_summarizer,

        }



在`AgentRegistry`类的`__init__`方法中，我们在`self.registry`中添加了一个新的键值对。键是代理的名称`"Summarizer"`，它将在执行计划中使用；值是我们刚刚创建的可调用函数。

接下来，我们在`get_handler`方法中添加一个新的`elif`块。这确保当执行器请求`"Summarizer"`时，注册表会正确为其准备运行所需的`client`和`generation_model`依赖。这符合我们严格的依赖注入模式：

```python
# 更新 get_handler 方法以为新代理准备依赖项。

def get_handler(self, agent_name, client, index, generation_model,
                 embedding_model, namespace_context, namespace_knowledge):
    handler_func = self.registry.get(agent_name)
    if not handler_func:
        logging.error(f"注册表中未找到 {agent_name}")
        raise ValueError(f"注册表中未找到代理 {agent_name}")
```



```python
if agent_name == "Librarian":
    return lambda mcp_message: handler_func(
        mcp_message, client=client, index=index,
        embedding_model=embedding_model,
        namespace_context=namespace_context
)
elif agent_name == "Researcher":
    return lambda mcp_message: handler_func(
        mcp_message, client=client, index=index,
        generation_model=generation_model,
        embedding_model=embedding_model,
        namespace_knowledge=namespace_knowledge
)
elif agent_name == "Writer":
    return lambda mcp_message: handler_func(
        mcp_message, client=client,
        generation_model=generation_model
    )
elif agent_name == "Summarizer":
    return lambda mcp_message: handler_func(
        mcp_message, client=client,
        generation_model=generation_model
    )
else:
    return handler_func
```

最后且最关键的更新是`get_capabilities_description`返回的文档字符串。我们为 Summarizer（摘要器）添加了一个新条目。我们清晰地定义了它的角色、所需的输入（`"text_to_summarize"`和`"summary_objective"`）以及它的输出结构。这种详细的描述让 Planner（规划器）能够理解何时以及如何在其计划中策略地部署 Summarizer 智能体：

```python
# 最后，我们更新了 Planner LLM 读取的技能描述。
def get_capabilities_description(self):
    """为 Planner LLM 返回智能体的结构化描述。"""
    # --- 更新：添加 Summarizer 的能力 ---
    return """
```

可用智能体及其所需的输入。
关键点：你必须使用为每个智能体提供的精确输入键名。

## 构建用于上下文压缩的 Summarizer 智能体

- 1. 智能体：Librarian（图书管理员）
  角色：检索语义蓝图（风格/结构指令）。
  输入：
  - `"intent_query"`：（字符串）对所需风格的描述性短语。
  输出：蓝图结构（JSON 字符串）。

- 2. 智能体：Researcher（研究员）
  角色：检索并综合关于某个主题的事实信息。
  输入：
  - `"topic_query"`：（字符串）要研究的主题。
  输出：综合的事实（字符串）。

- 3. 智能体：Summarizer（摘要器）
  角色：根据特定目标将长文本压缩为简洁的摘要。
  适用于在生成步骤之前管理 token 数。
  输入：
  - `"text_to_summarize"`：（字符串/引用）需要被摘要的长文本。
  - `"summary_objective"`：（字符串）摘要的明确目标（例如，"提取技术规格"）。
  输出：包含摘要的字典：`{"summary": "..."}`。

- 4. 智能体：Writer（作者）
  角色：通过在源材料上应用蓝图来生成或重写内容。
  输入：
  - `"blueprint"`：（字符串/引用）风格指令（通常来自 Librarian）。
  - `"facts"`：（字符串/引用）事实性信息（通常来自 Researcher 或 Summarizer）。
  - `"previous_content"`：（字符串/引用）用于重写的现有文本。
  输出：最终生成的文本（字符串）。

图 6.5 显示了动态的四步发现过程，该过程使 Planner LLM 能够学习并策略地使用新智能体：



![](img/c9f69ac3fb58ab0dee89a12c2578a735_207_0.png)

图 6.5：Planner 与 AgentRegistry 之间的动态发现循环

该图说明了`get_capabilities_description()`方法如何形成 Planner 与 AgentRegistry 之间的桥梁。该过程从 Planner 开始...

```python
except Exception as e:
    logging.error(f"[Writer] An error occurred: {e}")
    raise e
```

函数的其余部分按照之前的流程进行。它确定使用哪些数据作为`source_material`，构建详细的系统提示和用户提示，并调用 LLM 生成最终内容。

现在我们的 Writer agent 已经强化并能够处理多个数据源，我们已准备好对新的上下文缩减工作流进行首次端到端测试。

## 探索 Summarizer 的实际应用

我们实现的最后阶段是演示新的上下文缩减工作流。我们将设计一个目标，使得使用 Summarizer 不仅是有益的，而且对于成功完成任务是必不可少的。

> LLM 是随机模型。因此，输出在每次运行之间可能会有所不同。

该目标将有两个方面：首先，我们将显式地要求引擎针对特定目标总结一段长文本，然后要求引擎将该摘要作为创造性写作任务的事实基础。这种复杂的结构迫使 Planner（规划器）创建一个利用上下文链（context chaining）的多步骤计划，将 Summarizer 的输出作为 Writer 的输入。这项测试将提供确凿证明，证明我们的新 agent 已完全集成，并且引擎可以动态部署它。

首先，我们定义了一个字符串变量`large_text_from_researcher`，它包含了一段关于朱号（Juno）太空探测器的密集技术信息。然后，我们使用 f-string 构建了目标。该目标显式指示引擎执行两个顺序任务：首先，针对特定目标总结提供的文本，然后使用该摘要进行创造性写作任务：

```python
# FILE: Context_Engine_Content_Reduction.ipynb
# 这是来自主笔记本控制面板的最终演示。

# 1. 定义一段长文本，如果直接作为 Writer agent 的上下文
# 将成本过高或长度过长。
large_text_from_researcher = """
Juno 是一个绕木星运行的 NASA 太空探测器。它于 2011 年 8 月 5 日从卡纳韦勒尔空军基地发射，作为新前沿计划的一部分...
"""

# 2. 定义一个既需要使用长文本又需要创造性步骤的目标。
goal = f"""首先，总结以下关于朱号探测器的文本，以提取
仅关于其科学任务和仪器的关键事实。然后，使用
摘要，为关于该探测器危险抵达木星的儿童故事写一个简短的、悬疑的场景。
```
-- 要使用的文本 --
{large_text_from_researcher}
"""
```

我们使用与之前运行相同的配置字典，并将所有必要的组件传递给`execute_and_display`函数：

> # 配置保持不变，并执行了调用操作。

```python
# 3. 使用相同的配置字典
config = {
  "index_name": "genai-mas-mcp-ch3",
  "generation_model": "gpt-5",
  "embedding_model": "text-embedding-small",
  "namespace_context": "ContextLibrary",
  "namespace_knowledge": "KnowledgeStore"
}

# 4. 调用执行函数
execute_and_display(goal, config, client, pc)
```

运行此处后，我们可以分析 **TECHNICAL TRACE**（技术轨迹溯）来解构引擎的思考过程。轨迹溯显示了一个多步骤计划，从 Summarizer agent 开始，它成功地将长文本缩减为一份简洁的、项目符号形式的关键事实列表。至关重要的是，轨迹溯显示第一步的输出通过上下文链，作为事实输入传递给后续步骤中的 Writer agent。最终输出是一个创造性故事，它实际上是基于摘要而非原始文本，这证明了我们的上下文缩减执行成功。这证实了 Summarizer 是我们 MAS（多智能体系统）中一个功能完备且具有战略价值的补充。

让我们查看最终输出，以确保我们的实现运行符合预期。

## 解构引擎的思考过程：证据就在轨迹之中

最终生成的文本引人入胜，但我们成功的真正标准在于 **TECHNICAL TRACE**。作为引擎的飞行黑记录器，轨迹溯为其推理和行动提供了透明的、分步的描述。通过解构这一轨迹溯，我们可以精确地看到 Planner 如何制定新策略来满足我们复杂的目标，以及 Summarizer agent 在实现最终输出方面发挥了怎样的关键作用。

现在，我们将回溯测试运行的执行过程，分析每个决策和行动，以确认我们新的、以效率为核心的架构正完全按照设计运行，从 Summarizer agent 开始。

### 步骤 1：Summarizer agent 领先

Executor（执行器）采取的第一个动作是调用 Summarizer agent。这是对我们升级最重要的验证。当 Planner 面对用户目标时，分析并识别了两个组件：一个大型的嵌入文本块和一个"总结以下文本"的显式指令。根据我们在`registry.py`中提供的更新功能，Planner 得出结论，认为 Summarizer agent 是第一步的理想工具。它没有默认选择 Researcher，因为事实上下文已在目标本身提供。这是动态规划的清晰演示。

Planner 智能地提取了用户的指令，并为摘要制定了一个非常具体的 target：

```python
'summary_objective': '提取仅关于 \
'朱号的科学任务和其 \
'仪器/功率系统的关键事实......'
```

随后，Summarizer agent 执行了它的任务，带有此目标调用 LLM。它的输出是一个简洁的、项目符号形式列表，包含了关于朱号任务最相关的事实：

```python
'output': { 'summary': '- 在木星极地轨道运行...- 测量成分、引力场...- 通过探测内部研究形成过程...- 确定深层大气中的水量...- 绘制质量分布并表征深风...- 由三个大型太阳能电池板提供动力并保持稳定...'}
```

在这里，我们看到了 agent 的直接经济和技术价值。利用我们的 token 计数特性，我们可以量化这种减少：

- **原始文本 (text_to_summarize):** 253 tokens
- **最终摘要 (output):** 110 tokens

Summarizer agent 实现了 token 数量 56.5% 的缩减。它成功过滤了高量、低相关性的细节（如发射日期和与伽利略号探测器的比较），并传递了一个低量、高相关的摘要。此操作直接转化为更低的 API 成本，并确保最关键的信息保留给后续更昂贵的生成步骤。

在整个过程中，有很多方法可以在上下文引擎中放置计数，这些应该在开发阶段的架构研讨会上决定。例如，在你的情况下，可以将 token 计数包含在执行后函数：

```python
# === 执行后分析：量化上下文缩减 ===

# 确保导入 count_tokens 工具
from helpers import count_tokens

# 1. 获取发送给 Summarizer 的原始文本
# （这是控制面板中定义的变量）
original_text = large_text_from_researcher

# 2. 从轨迹溯对象获取摘要文本
# 轨迹溯对象 'trace' 由 execute_and_display_function 返回。
# 我们查看轨迹（索引 0）
summarized_text = trace.steps[0]['summary']

# 3. 使用 'count_tokens'测量两者
original_tokens = count_tokens(original_text)
summarized_tokens = count_tokens(summarized_text)
reduction_percentage = (1 - (summarized_tokens / original_tokens)) * 100

# 4. 打印结果
print("--- 上文缩减分析 ---")
print(f"原始文本 Tokens: {original_tokens}")
print(f"摘要 Tokens: {summarized_tokens}")
print(f"缩减率: {reduction_percentage:.2f}%")
```

## 构建用于上下文压缩的 Summarizer Agent

```python
print(f"Summarized Text Tokens: {summarized_tokens}")
print(f"Token Reduction: {reduction_percentage:.1f}%")
```

此后执行函数你可以根据项目需求创建的许多函数之一。

现在，让我们进入步骤 2，看看 Librarian（管理员）代理如何继续该过程。

## 步骤 2：Librarian 获取蓝图

在故事的实事实基础建立并压缩后，引擎继续处理用户目标的下一个部分：Planner（规划器）确定，为了满足对儿童故事中短小悬疑场景的需求，需要一个风格指南。

Executor（执行器）带有针对"短小悬疑的儿童故事场景"的意图查询调用 Librarian 代理。Librarian 在向量数据库中搜索`ContextLibrary`命名空间并检索出`blueprint_suspense_narrative`，该蓝图包含了 Writer（作者）的风格规则：

```python
'output': { 'blueprint_json': '{
  "scene_goal": "增加紧张感并营造悬念。",
  "style_guide": "使用短促、有力的句子。关注感官细节...",
  ...\n}'
}
```

该蓝图连同"使用短促、有力的句子"和"关注感官细节"的指令，成为了最后一步的风格指令。

## 步骤 3：Writer 合成最终输出

这最后一步是整个计划汇聚在一起并展示上下文链链（context chaining）威力的地方。Executor 激活 Writer 代理，在此之前，它解析了计划中指定的依赖项。

通过检查跟踪记录中步骤 3 的`resolved_context`，我们可以看到这一过程的运行。计划中的占位符已被之前步骤的实际输出替换：

```python
'resolved_context': {
  'blueprint': { 'blueprint_json': '{"scene_goal": "增加紧张感..."}' },
  'facts': { 'summary': '- 在极地轨道上运行...' },
  ...\n
}
```

这是关键的证明点。Writer 代理并没有获得原始的 253 个 token 的文章。它获得的是来自 Summarizer 的精简的、仅 110 个 token 的摘要。该代理现在拥有了所需的一切：一组风格指令（蓝图）和一组核心事实（摘要）。它最后调用了一次 LLM，指示其将这两个输入合成最终的叙述。

Summarizer 成功地充当了智能守门员，保护工作流中最昂贵的代理免受不必要的上下文成本的影响。让我们来看最终输出。

## 最终输出：风格与摘要事实的合成

引擎成功规划并执行了一个复杂的多代理工作流。最终的故事并不是一个通用的创意写作作品；它是之前的代理组装的组件的直接且可追溯的合成。

我们可以看到 Summarizer 和 Librarian 的影响贯穿于全文。故事精确地基于 Summarizer 提供的摘要事实：



# 摘要指出 Juno（木神号）“由三个巨大的太阳能阵电池翼提供动力并保持稳定”。故事从这一事实开始，并将其转化为第一人称的体验：

```
我向巨行星漂去。安静。专注。我三只巨大的太阳能阵电池翼完全展开。它们在嗡嗡。它们稳住我。它们吸收着阳光。
```

摘要列出 Juno“测量木星的成分、引力场、磁场和极地磁层”。故事将这一科学使命内化为一种感官行为：

```
我用金属耳朵倾听。我测量成分。我感受引力的拉扯并对其称重。我追踪磁场，一丝一缕。我对比磁层进行采样，那里极光光彩绚丽。
```

同时，叙述严格遵循了 Librarian 检索到的悬疑蓝图。风格要求“使用短促、有力的句子”。故事的高潮是对这种风格的完美执行，营造了紧张感和节奏：

```
现在。火焰。持续的轰鸣。航天器在颤抖。引力抓抓。我没有摇晃。我的翅膀保持着我的正位。
```

对跟踪记录和最终输出的详细分析提供了确定的证明，说明我们的升级是成功的。我们创建了一个新的、有价值的代理，并将其无缝集成到我们引擎的动态规划能力中。上下文引擎现在

## 构建用于上下文压缩的 Summarizer Agent

不仅更加强大，而且更加高效且具有成本效益，准备好应对更具挑战性的任务。

对跟踪记录和最终输出的详细分析提供了确定的证明，说明我们的升级是成功的。这对于工程师来说没问题，但我们尚未将其转化为业务价值。

## 将技术效率转化为业务价值

token 数量减少 56.5% 是一项值得赞的技术成就，但上下文工程师的责任延伸到将这种效率转化为切实的业务价值。能够阐述技术特性的“内容”（*what*）是开发者与企业战略合作伙伴区分开来的关键。这需要将 token 计数等抽象指标与成本、速度和质量等具体的业务结果联系起来。

考虑一个假设但真实的业务案例：一家金融服务公司使用我们的上下文引擎每天处理数千份长篇市场报告。目标是将这些报告转化为简洁、具有可操作性的摘要。让我们量化 Summarizer 代理在这种场景下的影响。如果该公司每天处理 10,000 份报告，而我们的代理持续将最后最昂贵的生成步骤的 token 负载降低 50% 以上，其财务影响是巨大的。单这一个代理就能在 API 成本上每月节省数千甚至数万的运营费用。图 6.6 显示，新时代的 AI 工程师必须是架构师、上下文工程师，还要具备展示价值的业务技能。

## 切实的业务价值

| 技术成就 | 上下文工程师的角色 |
| :--- | :--- |
| 降低运营成本 | 降低 API 费用 |
| 提高速度和敏捷性 | 更快的生成速度 |
| 高质量输出 | 高信噪比 |

图 6.6：新时代 AI 工程师的角色之一

此外，价值不仅限于成本节省，我们必须能够解释原因。通过为最终代理提供更小、更集中的上下文，我们减少了计算负载，从而加快了生成速度。这意味着交易员可以更快获取关键信息，实现更敏捷的决策。摘要上下文的高信噪比还能导致更高质量输出，因为代理不容易被无关细节干扰。这种用成本、速度和质量来定义升级的能力是一项关键技能，让上下文工程师能够有效地推销他们的解决方案并证明对业务底线的直接贡献。

在成功构建了关注效率的解决方案并证明其价值后，是时候总结我们在本章中学的关键技能和见解。

## 总结

在本章中，通过直接解决 API 成本和上下文限制，我们成功将上下文引擎提升为经济高效的系统。我们的技术之旅涉及设计并实现了一个新的专家——Summarizer 代理，它旨在作为大量信息的智能守门员。通过将该代理集成到引擎的动态框架中，我们为系统配备了强大的新功能：主动上下文管理。

我们的方法模仿了真正上下文工程师的工作流，扩展超了纯编码。我们从架构的概念分析开始，系统地实现了新代理，并将其集成到可发现的代理注册表中。关键在于，我们对升级后的系统进行了严格验证，执行向后兼容性检查以确保新功能不会损害稳定性，随后证明了 Summarizer 在复杂现实任务中的有效性。通过这一过程，你获得了构建企业级 AI 的基础技能。你学习了如何为了效率进行架构，如何扩展复杂的多代理系统（MAS），以及如何分析执行跟踪以提供系统价值的确定证明。我们准备好在下一章向外部开放我们的上下文引擎！

## 问题

1. Summarizer 代理的主要目标是从外部源添加新事实吗？（是或否）

2. `count_tokens` 工具是否执行了生成内容的最终 LLM 调用？（是或否）

3. 向后兼容测试的设计是为了证明新的 Summarizer agent 工作正常吗？（是或否）

4. 规划器（planner）是否仅通过更新 agent 注册表就能自动学习使用 Summarizer agent？（是或否）

5. 引入 Summarizer agent 的主要业务目的是增加最终输出的创造性吗？（是或否）

6. 在新的工作流中，Writer agent 是否直接从 Summarizer agent 接收事实输入？（是或否）

7. 添加 Summarizer agent 是否需要对核心 `engine.py` 文件进行重大更改？（是或否）

8. `summary_objective` 输入是否用于为 Summarizer agent 提供待压缩的全文？（是或否）

9. 主动上下文管理可以被描述为本章引入的关键战略能力吗？（是或否）

## 参考文献

- Hao, S., Wang, Z., Li, J., Liu, Z., & Chen, C. (2024). AgentVerse: A Flexible Framework for Multi-Agent Society Simulation. arXiv preprint arXiv:2405.08722. https://arxiv.org/abs/2405.08722

- Chen, L., Wu, Y., Zhang, Z., Xu, K, Wang, S., & Zhang, Y. (2024). AgentBoard: An LLM-Based Multi-Agent Platform for Proactive and Interactive Data Analysis. arXiv preprint arXiv:2407.03713. https://arxiv.org/abs/2407.03713

- Dasgupta, I., Hughes, E., Kuegel, A., Wang, F., Kay, J., & Collins, T. (2024). The Geometry of Context Management in Transformers. arXiv preprint arXiv:2405.00205. https://arxiv.org/abs/2405.00205

## 进阅读

Wang, L., Ma, W., Zhu, Y., Wang, Z., Liu, S., & Liu, J. (2024). A Survey on Large Language Model-based Autonomous Agents. arXiv preprint arXiv:2308.11432. https://arxiv.org/abs/2308.11432

## 订阅免费电子书

新框架、演进中的架构、研究发布、生产环境分析——AI_Distilled 将杂音过滤掉，为实际操作 LLM 和 GenAI 系统的工程师和研究人员提供每周简报。现在订阅即可获得一本免费电子书，以及帮助你保持专注并掌握最新资讯的每周见解。

访问 https://packt.link/80z6Y 订阅或扫描下方二维码。

