# 4 构建上下文引擎（Context Engine）

人工智能的演进已从僵化的、基于规则的系统转向灵活的生成式模型。将早期人工智能（通常被称为符号 AI）想象成精心手工编写的规则书。它们在特定领域内令人印象深刻，但在面对模糊性和任何脚本之外的内容时却非常脆弱。现代大语言模型（`LLMs`）反转了这一模式。它们展现出广泛的语言理解能力和通用推理能力，但缺乏特殊性、实时知识，且无法可靠地依赖外部系统。仅靠 `LLMs` 很难提供持续的业务价值。这种差距导致了你在前几章已经看到的两个转变：`RAG`（检索增强生成）通过外部来源的事实上下文为 `LLMs` 提供依据，减少幻觉并使响应具备场景感知；其次，`AI` 代理为 `LLMs` 提供了工具和程度的自主性，使其能够执行多步任务。这些想法共同推动模型更接近实用的工作。

然而，这种快速演进带来了一个新的架构挑战。早期的代理系统硬编码了代理之间的交接，这在演示中看起来还可以，但在生产环境中会迅速变成一团乱。如果你有多个代理协同工作，你一定体会过这种痛苦。随着团队添加专门代理，线性的组合无法推理其自身能力，也无法适应新问题。因此，问题变成了：我们如何从代理集合转向自适应、自主的系统？这就是上下文引擎大显用场。

上下文引擎是一个元系统，一个智能控制器，它围绕高级目标组织专门代理。它不仅仅是一个单一函数；它是一个结构化工作流，将模糊的意图转化为落地的、感知上下文的输出。虽然"上下文引擎"标签特定于此框架，但这种模式在高级生成式 AI 平台上被广泛使用。`ChatGPT` 或 `Gemini` 等系统并非孤立的模型，而是带有控制器的应用程序，用于管理会话、调度工具并维护上下文。从概念上讲，这些控制器的角色与上下文引擎相同。

架构的独特之处在于规划与执行的显式分离，这灵感来自代理推理的前沿研究。上下文引擎并非简单的循环，而是采用了一个两阶段的过程：

- 首先，**规划器（`Planner`）** 作为战略核心，对用户目标进行推理，并根据**代理注册表（`Agent Registry`）** 作为可用能力的"工具箱"，选择最佳的代理或函数。因此，我们将在本章中构建这些新的规划器和代理注册表工具。
- 然后，规划器使用 `LLM` 生成一个针对特定任务定制的动态、分步骤的执行计划。一旦计划就绪，它将移交给**执行器（`Executor`）**，执行器是运营经理，按正确的顺序调用专门代理。

> 注意：如第 3 章介绍的，我们将利用带有静态知识数据和动态指令的双重 RAG 来增强上下文引擎的灵活性。

在本节中，我们将设计上下文引擎。我们将从架构的可视化流程遍历开始，然后解析它所依赖的组件。概览请参见图 4.1。

## 架构概述

上下文引擎是一个动态的多阶段工作流，如图所示。每种颜色代表一个不同的功能层，它们协作将高级目标转化为完全生成的输出。

我们从最外层开始，流程从用户开始并结束于用户。橙色组件代表这两个触点：**用户高级目标**和**最终输出**。目标是一个宽泛的指令，例如"写一个悬疑故事"。这是一个不带任何预定义结构的简单请求。另一方面，最终输出是满足该目标的完整、精炼的结果。

在一切的中心是上下文引擎本身，用蓝色表示。它是协调器，是不直接执行任务的大脑，负责通过战略智能管理整个工作流。它内部包含三个关键模块：

- **规划器（`Planner`）：** 战略核心，接收用户目标并设计分步骤计划。
- **执行器（`Executor`）：** 运营经理，执行计划，调用专门代理并管理它们之间的数据流。
- **追踪器（`Tracer`）：** 透明的记录器，记录每个操作以供调试和提供洞察。

该工作流的第一阶段是战略规划，此时规划器与黄色和红色组件协作。它首先咨询黄色高亮的**代理注册表**，这是一份列出所有可用代理及其能力的工具箱。通过这些信息，它连接到外部的红色高亮服务——即 `LLM`（例如 `GPT-5`）。规划器将用户目标和工具箱同时发送给 `LLM`，然后 `LLM` 返回一个结构化的 JSON 计划。该计划在图中以虚线箭头表示，流回引擎并移交给执行器。

第二阶段是执行，执行器协调绿色的**专门代理**和外部的红色服务。它按照计划逐步执行，调用了以下内容：

- **管理员（`The Librarian`）：** 处理过程性 RAG 以获取风格蓝图。
- **研究员（`The Researcher`）：** 执行事实 RAG 以收集并合成相关信息。
- **作者（`The Writer`）：** 结合风格和事实产生最终内容。

这些代理都会与外部服务交互。管理员和研究员查询向量数据库（`Vector DB`），而研究员和作者可能会调用 `LLM` 来精炼事实或生成文本。

一个关键创新是上下文链链（在第 1 章引入），由循环的绿色箭头表示。这种机制确保了有状态的工作流，其中一个代理的输出（例如管理员的蓝图）成为下一个代理（作者）的输入。

## 功能深度解析

有了这个概述，我们现在准备好探索让上下文引擎运行起来的功能。让我们看看核心函数：

- **`Context Engine()`：** 主协调器，运行规划器和执行器。
- **`Planner()`：** 驱动引擎，使用 `AgentRegistry.capabilities_description()` 和 `call_llm_robust()` 生成计划。
- **`Executor`：** 迭代计划，使用 `resolve_dependencies()` 组装当前代理的上下文。
- **`Agent Registry`：** 使用 `get_handler()` 获取正确的代理。
- **`Tracer`：** 实现每一步的日志。

专门代理（绿色）：

- **`Librarian()`：** 获取风格蓝图。
- **`Researcher()`：** 合成事实。
- **`Writer()`：** 生成最终内容。

系统组件（蓝色和黄色）：

- **`Context Engine`：** 协调函数。
- **`Agent Registry`：** 代理工具箱列表。

工作流遵循这些箭头：

1. 用户请求 → `context_engine()`。
2. 在引擎内部 → 首先调用 `planner()`。
3. 执行器迭代计划。
4. 执行器使用 `get_handler()` 获取正确的代理。
5. 代理使用：
   - 管理员和研究员 → `query_db()`（使用 `embedding()`）
   - 研究员和作者 → `call_llm_robust()`

每个代理通过 `create_message()` 返回结果，执行器将其存储。循环持续直到计划完成并返回最终输出。

## 组装系统

我们花时间了上下文引擎的结构以及每个组件对整体工作流的贡献。在本节中，我们从理解转向构建。我们的重点现在从架构蓝图转向系统本身。`Context_Engine.ipynb` 将是我们的工作空间，我们将在那里逐步组装引擎，连接齿轮并布线，让我们的图表活起来。

我们将按照逻辑顺序构建引擎，从基础向上。一个强大的协调器的效能取决于它领导的团队，我们的第一个任务是创建专门代理——工具箱，即系统的功能核心。我们的代理已准备就绪，我们将开发它们使其易于发现和管理。

> 注意：关于延迟：书中构建的上下文引擎以及相关仓库执行的是复杂的多步推理，而不是简单的单次调用。你在 `Colab` 中观察到的延迟是思考时间，引擎正在动态规划并执行一系列 API 调用（例如：规划、然后是 RAG，然后是生成）。这也是 `Gemini` 或 `ChatGPT` 等高级平台处理复杂请求时需要时间的原因。

## 专家代理

在我们让上下文引擎（`Context Engine`）完全运行之前，我们需要构建它的前线专家——即实际执行工作的代理。正如我们已经确定的，多代理系统由三个专门的代理组成：管理员（`Librarian`）、研究员（`Researcher`）和作家（`Writer`）。每个代理都有独特的用途，且所有代理都使用 MCP 进行通信，MCP 充当系统中传输信息的通用语言或标准的"集装箱"。如果没有共享标准，每个代理都需要自定义的集成逻辑，这将系统变得脆弱且难以扩展。MCP 通过确保每条数据（无论是来自管理员的风格蓝图还是来自研究员的事实摘要）都以完全格式封装，解决了这个问题。

这种标准化实现了上下文链链。每个代理接收 MCP 消息作为输入并返回另一个输出，允许执行器（`Executor`）只需传递数据，而无需担心翻译或格式问题。这种信息的无缝流动使得卓越的多步推理成为可能，也是整个引擎运行的基础。



## 上下文管理员代理

我们的第一个代理是上下文管理员，实现为`agent_context_librarian`。它的角色是识别用户意图并从向量库中获取相应的语义蓝图。正如我们之前解释过的，该蓝图为作家提供了关于如何构建和风格化内容的指令。

管理员首先从 MCP 消息中提取`intent_query`，例如“悬疑叙事蓝图”。这是解释的关键时刻，将创意构想转化为机器可读的内容。它从高层请求中解码出所需的感觉或格式，将“悬疑”等宽泛的概念翻译为针对其知识库的精确、可操作的搜索查询：

```python
def agent_context_librarian(mcp_message):
    """
    从上下文库中检索合适的语义蓝图。
    """

    print("\n[Librarian] 已激活。正在分析意图...")
    requested_intent = mcp_message['content'].get('intent_query')

    if not requested_intent:
        raise ValueError("管理员要求输入内容中包含 'intent_query'。")
```

管理员使用`query_pinecone()`在上下文向量库中执行语义搜索。与其将这个向量库看作事实数据库，不如看作是一个食谱库，每个蓝图都描述了特定类型的写作应该如何结构化或风格化。搜索不仅仅是匹配单词，而是匹配意义，允许系统在措辞不完全匹配时找到最接近的概念契合：

```python
results = query_pinecone(requested_intent, NAMESPACE_CONTEXT, top_k=1)
```

如果找到了蓝图，它会被封装进 MCP 消息中；否则，管理员仍然返回有效消息，只是带有一个通用指令：

```python
if results:
    match = results[0]
    print(f"[Librarian] 找到蓝图 {match['id']}: 分数:\n    {match['score']:.2f}")
    blueprint_json = match['metadata']['blueprint_json']
    content = blueprint_json
else:
    print(f"[Librarian] 未找到特定蓝图。返回默认值。")
    content = json.dumps({"instruction": "中立地生成内容。"})

return create_mcp_message("Librarian", content)
```

最后一步展示了系统的韧性。在理想情况下，会找到完美匹配且专家制作的蓝图并将其发送给作家，确保高度定制的输出。然而，如果不存在蓝图，系统也不会崩溃。它会优雅地退回到中立指令，确保工作流始终能够完成，尽管结果更加通用。

## 研究员代理

接下来是研究员（Researcher），系统的调查记者。当管理员处理结构和语调时，研究员负责查找到所有事实。它的工作是查询知识向量库，检索与给定主题最相关的信息，并将其合成成简洁的事实摘要。

让我们看看这个代理如何将一个宽泛的主题转化为聚焦且可验证的简报。该过程从代理从传入的 MCP 消息中接收任务`topic_query`开始开始。它的第一个动作是对`NAMESPACE_KNOWLEDGE`向量库进行语义搜索，检索出前-k=3 个最相关的文本块：

```python
def agent_researcher(mcp_message):
    """
    从知识库中检索并合成事实信息。
    """
    print(f"\n[Researcher] 已激活。正在调查主题...")
    topic = mcp_message['content'].get('topic_query')

    if not topic:
        raise ValueError("研究员要求输入中包含 'topic_query'")

    results = query_pinecone(topic, NAMESPACE_KNOWLEDGE, top_k=3)

    if not results:
        print("[Researcher] 未找到相关信息。")
        return create_mcp_message("Researcher", "未找到该主题的数据。")

    print(f"[Researcher] 有 {len(results)} 个相关块。正在合成...")
    source_texts = [match['metadata']['text'] for match in results]
```

这是一个关键的设计选择：通过检索多个源，代理收集了比单一结果提供的更全面、更细致的信息集。然而，代理最重要的工作发生在合成阶段。它并不传递检索到的文本，而是使用一个精心设计的提示词（prompt）指示 LLM 充当专家研究 AI。

系统提示词（system prompt）设定了护栏，命令 LLM严格关注事实：

```python
system_prompt = """你是一个专家研究 AI。
将提供的源文本合成简洁的、要点形式的摘要，并与主题相关。严格关注事实。
不要添加外部信息"""
```

这是减轻幻觉并确保最终摘要基于真实数据的关键步骤。`user_prompt` 通过将检索到的块组装成清晰的格式完成了这一步：

```python
user_prompt = f"Topic: {topic}\nSources:\n" + "\n\n---\".join(source_texts)

findings = call_llm_robust(system_prompt, user_prompt)

return create_mcp_message("Researcher", findings)
```

最终合成的结果代表了经过提炼、简洁且与初始主题直接相关的知识。输出以 MCP 消息的形式返回，供下游代理使用。这种检索-合成步骤确保了上下文引擎的最终输出既结构良好（得益于管理员），又在事实可靠。

## 作家代理

作家是我们流水线上的专家；它是将管理员的蓝图与研究员的结果相结合以生成最终文本的匠。它负责最终的合成，将抽象的风格和原始事实织成流畅、连贯的内容。它还可以处理重写任务，使用之前生成的内容作为输入，这使得引擎具有迭代改进工作的能力。

让我们探索该代理如何组装输入以产生最终输出。代理从 MCP 消息中收集原始材料：来自管理员的蓝图、来自研究员的合成事实，或之前写作步骤的任何`previous_content`：

```python
def agent_writer(mcp_message):
    """
    将事实研究与语义蓝图结合以生成最终输出。
    经过关键增强，可以处理原始事实或用于重写任务之前的内容。
    """
    print("\n[Writer] 已激活。正在将蓝图应用于素材...")

    blueprint_json_string = mcp_message['content'].get('blueprint')
    facts = mcp_message['content'].get('facts')
    previous_content = mcp_message['content'].get('previous_content')
```

# 4 

接下来是一个关键检查：如果没有定义文本编写方式的蓝图，Writer（写入器）将无法执行：

```python
if not blueprint_json_string:
    raise ValueError("Writer requires 'blueprint' in the input content")
```

一旦验证了蓝图，Writer 将通过`if/elif/else`块确定它正在执行的任务类型：是基于研究创建新内容，还是在重写现有内容？

```python
if facts:
    source_material = facts
    source_label = "RESEARCH FINDINGS"
elif previous_content:
    source_material = previous_content
    source_label = "PREVIOUS CONTENT (For Rewriting)"
else:
    raise ValueError("Writer requires either 'facts' or 'previous_content")
```

这种条件逻辑让 Writer 变得非常灵活。它可以在模式（新内容生成或重写）之间切换，而无需改变其核心设计。在大型工作流中，这种灵活性意味着代理（agent）可以被复用于多个精炼阶段。

一旦选择了内容输入，Writer 会构建两个提示词（prompt）：一个定义如何写的系统提示词（system prompt），以及一个定义写什么的用户提示词（user prompt）：

> # Writer 的系统提示词包含了动态检索的蓝图

```python
system_prompt = f"""你是一个专家级内容生成 AI。
你的任务是根据提供的 SOURCE MATERIAL 生成内容。
至关重要的是，你**必须**根据下方提供的 SEMANTIC BLUEPRINT 中定义的规则对输出进行结构化、风格化和约束。

--- SEMANTIC BLUEPRINT (JSON) ---
{blueprint_json_string}
--- END SEMANTIC BLUEPRINT --"""

严格遵守蓝图的指令、风格指南和目标。蓝图定义了你**如何**写；而源素材定义了你写**什么**内容。
"""

user_prompt = f"""

--- SOURCE MATERIAL ({source_label}) ---

{source_material}

--- END SOURCE MATERIAL ---

现在请严格按照蓝图生成内容。

"""
```

这种提示词的这种正是 Writer 真正的智能所在。Writer 的系统提示词提供了创造性框架，以及从蓝图中得出的风格和结构约束。用户提示词则提供了实质内容，即 LLM 必须在其基础上构建的事实或现有内容。

通过将“如何做”与“做什么”分离，Writer 确保了模型的清晰度。LLM 确切知道被要求做什么，以及在什么约束下执行，从而产生风格上一致且有事实依据的输出。

最后，Writer 将提示词发送给 LLM，捕获结果并将其封装成标准的 MCP 消息供下游使用：

```python
final_output = call_llm_robust(system_prompt, user_prompt)

return create_message("Writer", final_output)
```

此阶段的输出是整个上下文引擎（Context Engine）工作流的结晶。这是一份由证据支持并以精确风格交付的内容内容。

## 代理注册表 (Agent Registry)

现在我们已经构建了专家代理团队，下一个问题是，谁来跟踪它们呢？Planner（规划器）不能仅仅猜测哪些代理存在或它们具备什么能力。它需要一个可靠的目录，列出所有可用专家、他们的角色以及如何调用他们。为了解决这个问题，我们引入了代理注册表（Agent Registry）：一个将代理集合转换为协调团队的中央目录。

`AgentRegistry` 类负责管理所有代理及其能力，使它们对 Planner 可用。让我们来看看它是如何结构的。

`__init__` 方法定义了一个字典`self.registry`，它是所有可用代理的函数性名单。每个人类可读的名称（例如“Librarian”/管理员）都直接映射到其对应的 Python 函数，例如`agent_context_librarian`。这种设计使得系统非常灵活。如果以后添加了新代理（例如“Critic”/评论员或“Editor”/编辑），只需在这里注册它们，而无需更改代码库的任何部分：

```python
class AgentRegistry:

    def __init__(self):

        self.registry = {

            "Librarian": agent_context_librarian,

            "Researcher": agent_researcher,

            "Writer": agent_writer,

        }
```

接下来是让其他组件（如 Executor/执行器）能够根据需要检索这些代理的方法。`get_handler()` 方法是 Executor 与注册表交互的方式。当 Executor 需要运行计划中的某个特定步骤时，它调用类似`get_handler("Librarian")`的方法，该方法会返回被调用的函数。这是一个整洁的设计：没有硬编码，没有执行过程中的导入，也没有脆弱的条件判断。只有一个 Executor 精确运行以执行的简单查询：

```python
def get_handler(self, agent_name):

    """获取与代理名称关联的函数。”

    handler = self.registry.get(agent_name)

    if not handler:

        raise ValueError(f"{agent_name}' not found in registry.")

    return handler
```

最后一个也是最重要的方法不是为代码设计的，而是为 LLM 本身设计的。在这里，`get_capabilities_description()` 返回了关于每个代理目的、输入和输出的结构化、人类可读的描述。这是 Planner 的 LLM 的上下文。当 Planner 调用 LLM 生成计划时，它会包含这段文本，以便模型知道哪些工具可用、每个工具期望什么，以及它们输出什么：

```python
def get_capabilities_description(self):

    """

    为 Planner LLM 返回代理的结构化描述。

    这对于 Planner 理解如何使用这些代理至关重要。

    """

    return """

可用代理及其要求：

1. 代理: Librarian (管理员)

角色: 获取蓝图。

输入：

- "intent": (意图) 目标。

输出: 蓝图 (string)。

2. 代理: Researcher (研究员)

角色: 整合信息。

输入：

- "topic": (string) 主题。

输出: 整合后的的事实 (string)。

3. 代理: Writer (写入器)

角色: 生成或调整内容。

输入：

- "blueprint": (string) 蓝图。

- "facts": (string) 事实。

- "previous_content": (string) 现有内容。

输出: 最终文本 (string)。

"""
```

该描述的清晰度决定了 Planner 的质量。模糊或不完整的描述会导致计划失败；清晰的描述则赋予推理能力。最后，我们实例化注册表。这一行代码激活了系统：

```python
AGENT_TOOLKIT = AgentRegistry()
```

有了这个结构，Planner 现在可以对其团队进行智能推理，确切知道它们如何协作。

## 上下文引擎 (Context Engine)

我们已经了专家并在注册表中记录了他们。下一步是让整个团队共同思考并行动。这就是上下文引擎的工作。

引擎的核心是运行一个简单但强大的循环，模仿人类解决复杂问题的方法：规划、执行、反思。Planner 进行战略思考，将宏大目标拆解为清晰的计划。Executor 投入执行执行该计划，在正确的时间调用正确的代理，并在执行过程中传递上下文。Tracer 扮演着安静但至重要的观察者角色，记录每一步，以便我们之后不仅看到系统做了什么，还能知道为什么。

在接下来的章节中，我们将从 Planner 开始，进入 Executor，然后添加 Tracer。最后，我们在`context_engine()`中将它们连接起来，并运行一个示例。

## Planner

`planner` 函数是引擎的策略核心。它充当项目经理，将模糊的高层目标转化为精确的、分步骤的、机器可读的 JSON 计划。它的来自于利用 LLM 进行推理的能力。

```python
def planner(goal, capabilities):

    """

    分析目标并使用 LLM 生成结构化的执行计划。

    """

    print("[Engine: Planner] Analyzing goal and generating execution plan...")
```



在这里，`goal`是高层请求；`capabilities`是从`AgentRegistry.get_capabilities_description()`返回的关于代理及其输入输出的人类可读目录。

接下来，`Planner`为LLM准备简报。这个提示词同时承担两个工作：它列出了可用工具，并设定了制定计划的规则。你可以将其视为`Planner`在说：这里你可以使用的专家，这是如何引用之前的结果，以及这是要求的输出格式：

# 能力边界结束 ---

指令：

- 1. 计划**必须**是一个JSON对象列表，其中每个对象代表一个“步骤（step）”。
- 2. 你**必须**使用上下文链（Context Chaining）。如果某个步骤需要上一个步骤的输入，请使用语法`$$STEP_OUTPUT$$$`来引用它。
- 3. 要具有策略性。将复杂目标（如顺序重写）拆分为不同的步骤。为写入代理（Writer agent）使用正确的键（'facts' 对比 'previous_content'）。

示例目标：“写一个关于阿波罗11号的悬疑故事。”
示例计划（JSON列表）：

```json
[{"step": 1, "agent": "Librarian", "input": {"intent_query": "悬疑叙事蓝图"}}, {"step": 2, "agent": "Researcher", "input": {"topic_query": "阿波罗11号登陆细节"}}, {"step": 3, "agent": "Writer", "input": {"blueprint": "$$STEP_1_OUTPUT$$", "facts": "$$STEP_2_OUTPUT$$$"}}]
```

示例目标：“写一份关于朱诺号（Juno）的技术报告，然后用口语化的重写。”
示例计划（JSON列表）：

```json
[{"step": 1, "agent": "Librarian", "input": {"intent_query": "技术报告结构"}}, {"step": 2, "agent": "Researcher", "input": {"topic_query": "朱诺号任务技术"}}, {"step": 3, "agent": "Writer", "input": {"blueprint": "$$STEP_1_OUTPUT$$", "facts": "$$STEP_2_OUTPUT$$$"}}, {"step": 4, "agent": "Librarian", "input": {"intent_query": "口语风格"}}, {"step": 5, "agent": "Writer", "input": {"blueprint": "$$STEP_4_OUTPUT$$", "previous_content": "$$STEP_3_OUTPUT$$"}}]
```

简报准备就绪后，规划器（Planner）调用LLM作为推理伙伴。整个过程封装在一个健壮的`try...except`块中，以处理潜在的错误，例如来自LLM的格式不正确的JSON。在调用LLM并解析响应后，规划器会执行关键验证。它检查输出是否为有效列表。为了确保韧性，它还会


# 4 

处理LLM可能将列表封装在字典中（例如`"plan": [...]`）的情况，并据此提取列表。如果结构不正确或发生任何其他错误，`except`块将记录详细的错误信息（包括用于便于调试的原始LLM输出），并抛出异常以停止引擎。成功解析并验证的计划将返回给执行器（Executor）使用：

```python
plan_json = ""

try:
    plan_json = call_llm_robust(system_prompt, goal, json_mode=True)
    plan = json.loads(plan_json)
    # 验证输出结构
    if not isinstance(plan, list):
        # 处理 LLM 将 List 封装在字典中的情况（例如 "plan": [...]）
        if isinstance(plan, dict) and "plan" in plan and isinstance(plan["plan"], list):
            plan = plan["plan"]
        else:
            raise ValueError("规划器未返回有效的 JSON 列表结构.")
    print("[Engine: Planner] 计划生成成功.")
    return plan
except Exception as e:
    print(f"[Engine: Planner] 生成有效计划失败。错误: {e}。原始 LLM 输出: {plan_json}")
    raise e
```

这种设计之所以有效，是因为它为LLM提供了一个明确的可遵循的标准。接下来，我们将看到执行器如何将此计划转化为行动。

# 执行器 (Executor)

执行器是系统的现场经理：它运行每个步骤，调用正确的代理，最重要的是推进上下文，以便后续步骤可以建立在早期结果的基础上。

在调用代理之前，执行器会在输入中查找占位符（诸如`$$ . . $$`）。这些标记充当对之前结果的引用，告诉系统：“在此处使用步骤 1 的输出”。通过解析这些占位符，执行器将计划转换为一个连通的工作流。

这就是上下文链的实际应用。`resolve_dependencies()`辅助函数驱动了这一过程。它扫描代理所需输入中的任何`$$$`占位符。

该函数的外部部分定义了`resolve_dependencies`并创建输入参数的深拷贝。这确保了在替换占位符时，原始计划结构（在其他地方需要）不会被意外修改（变异）：

```python
def resolve_dependencies(input_params, state):
    """
    辅助函数，用执行状态中的实际数据替换 $$$占位符。
    """
    # 使用 copy.deepcopy 确保原始计划结构不被修改
    resolved_input = copy.deepcopy(input_params)
```

内部解析器（基础情况）解析嵌套函数。它的“基础情况”是处理单个值链的逻辑。它检查字符串是否为占位符（以`$$`开头并以`$$`结尾）。如果是，它会在`state`字典中查找引用键（例如`STEP_1_OUTPUT`）并返回实际数据：

```python
# 处理潜在嵌套结构的递归函数
def resolve(value):
    if isinstance(value, str) and value.startswith("$$") and value.endswith("$$"):
        ref_key = value[2:-2]
        if ref_key in state:
            # 从上一个步骤的输出中检索实际数据（字符串）
            print(f"[Engine: Executor] 正在解析 {ref_key}")
            return state[ref_key]
        else:
            raise ValueError(f"在 state 中未找到依赖 {ref_key}.")
```

（注：原文此处存在大量重复的段落，翻译时已按逻辑保留核心内容并确保格式一致）

# 执行追踪器 (Execution Tracer)

在规划器和执行器的协同工作，执行追踪器（Tracer）已经具备了功能，但尚未开始工作。让我们仔细观察。

当新任务开始时，追踪器用目标初始化。

```python
class ExecutionTrace:
    """
    记录 LLM 计划的执行情况。
    用于调试和分析，让我们能够准确看到系统是如何推理并执行任务的。
    """
    def __init__(self, goal):
        self.goal = goal
        self.plan = None
        self.steps = []
        self.status = "Initialized"
        self.final_output = None
        self.start_time = time.time()
```

过程完成后，追踪器使用`finalize()`将其封装。它记录了最终状态，存储了最终输出，并计算了任务所需的时间。

一旦规划器创建了策略，追踪器就会使用`log_plan()`进行记录。该方法存储整个计划，为我们提供了执行开始前系统意图执行的操作记录：

```python
def log_plan(self, plan):
    self.plan = plan
```

当执行器遍历计划时，`log_step()`记录每一个动作。对于每个步骤，它捕获了了哪个代理、接收了什么输入（`planned_input`）、在什么上下文中运行（`resolved_context`）以及输出了什么（`output`）。这创建了计划如何执行的清晰时间化记录：

```python
def log_step(self, step_num, agent, planned_input, mcp_output, resolved_context):
    """记录单个执行步骤的细节."""
    self.steps.append({
        "step": step_num,
        "agent": agent,
        "planned_input": planned_input,
        "resolved_context": resolved_context,
        "output": mcp_output['content']
    })
```

```python
def finalize(self, status, final_output=None):
    self.status = status
    self.final_output = final_output
    self.duration = time.time() - self.start_time
```

这些方法共同构成了追踪器的严密审查。它让我们能够在任何时候重构系统的推理过程，无论我们是在调试故障、分析性能，还是仅仅研究工作流是如何展开的。本质上，执行追踪器为上下文引擎提供了每个自主系统所需要的要素：可见性、问责制，以及我们始终能看到其思考方式的安心感。

## 整合一切

现在所有的构建模块都已就绪：负责思考的**规划器 (Planner)**、负责执行的**执行器 (Executor)**以及记录每一次移动的**追踪器 (Tracer)**。最后是将它们连接到单一控制循环中。

`context_engine()`函数是系统的开关。它通过协调整个两个阶段的过程让一切运转起来：它先制定计划，然后执行计划。让我们逐步分析。

当一个新任务开始时，该函数会打印目标并创建一个执行追踪器的实例，追踪器会立即开始记录。它还会加载代理注册表（Agent Registry），这是一个告诉系统它拥有哪些引擎以及如何访问它们的目录。从这一刻起，每一个动作、决策和输出都将被记录在追踪（trace）中。

```python
def context_engine(goal):
    """
    Context 引擎的主入口。管理规划与执行。
    """
    print(f"\n=== [Context Engine] Starting New Task ===\nGoal:{goal}\n")
    trace = ExecutionTrace(goal)
    registry = AGENT_TOOLKIT
```

接下来是规划（阶段 1）：

```python
## Phase 1: Plan
try:
    capabilities = registry.get_capabilities_description()
    plan = planner(goal, capabilities)
    trace.log_plan(plan)
except Exception as e:
    trace.finalize("Failed during Planning")
    # 即使失败也返回追踪信息以便调试
    return None, trace
```

引擎获取用户的目标并开始制定计划。它从注册表中检索代理能力的描述，并将其交给规划器，规划器使用大语言模型（LLM）创建一个JSON格式的行动计划。该计划将目标分解为不同的、有序的步骤，描述了哪个代理应该做什么以及按什么顺序执行。一旦规划完成后，追踪器就会记录计划，在执行开始前保留策略。如果在此阶段发生任何错误（例如格式错误或规划失败），系统会整洁地记录并带有完整的追踪信息退出。

计划就绪后，引擎进入执行阶段（阶段 2）：

```python
print(f"\n[Engine: Executor] Starting Step (step_num): {agent_name})")
try:
    handler = registry.get_handler(agent_name)
    # 上下文组装：解决依赖关系
    resolved_input = resolve_dependencies(planned_input, state)
    # 通过 MCP 执行代理
    # 为代理创建一个包含已解析输入*的 MCP 消息
    mcp_resolved_input = create_mcp_message("Engine", resolved_input)
    mcp_output = handler(mcp_resolved_input)
    # 更新状态日志追踪
    output_data = mcp_output["content"]
    # 存储输出数据（上下文本身）
    state[f"STEP_{step_num}_OUTPUT"] = output_data
    trace.log_step(step_num, agent_name, planned_input, mcp_output, resolved_input)
    print(f"[Engine: Executor] Step {step_num} completed.")
except Exception as e:
    error_message = f"Execution failed at {step_num}({agent_name})"
    print(f"[Engine: Executor] ERROR: {error_message}")
    trace.finalize(f"Failed at Step {step_num}")
    # 返回追踪信息以调试失败原因
    return None, trace
```

这个循环是Context引擎实际工作的地方。执行器遍历计划的每一步，为任务调用相应的代理。在调用代理之前，它会使用`resolve_dependencies()`检查输入中的任何占位符（例如对早期步骤输出的引用），并将它们替换为存储在当前状态中的实际数据。然后，将输入封装成MCP消息并发送给代理，代理的响应将成为该步骤的输出。每个输出都存储在状态字典中，创建了一个后续步骤可以引用的短期记忆。追踪器会记录每一个细节：调用了哪个代理、接收了什么输入、解析了什么上下文以及产生了什么结果。

当所有步骤运行完成后，引擎会完成流程：

```python
final_output = state.get("STEP_{len(plan)}_OUTPUT")
trace.finalize("Success", final_output)
print("\n==[Context Engine] Task Complete ===")
```

此时，最后一步的输出会被检索并返回。通过标记运行成功、记录结果并测量从开始到结束的时长，该函数返回生成的输出和完整的执行追踪。

## 运行引擎

我们已经构建了拼图的每一个碎片：专家、注册表、规划器、执行器和追踪器。在完成这些架构工作后，是时候启动并听引擎运行了。让我们看看当Context引擎被要求处理一个创意目标时会发生什么。我们将从一个简单的提示开始，要求一个关于阿波罗11号登月的悬疑故事：

```python
print("******** Example 1: STANDARD WORKFLOW (Suspenseful Narrative)")
goal_1 = "为一个关于阿波罗11号登月的儿童故事写一个简短的悬疑场景，突出危险性。"

# 运行 Context 引擎
# 确保 Pinecone 索引已填充（参考第 3 笔记本）以便此工作
result_1, trace_1 = context_engine(goal_1)
```



```python
if result_1:
    print("\n******* FINAL OUTPUT 1 ***********\n")
    display(Markdown(result_1))
    print("\n\n" + "*"*50 + "\n\n")
    # 可选：显示追踪以查看引擎的过程
    trace_1.display_trace()
```

## 组装 Context 引擎

当此目标传递给引擎时，规划器开始行动：

- 1. 它推理认为，为了创建一个突出“危险性”的悬疑场景，首先需要一个风格指南。因此，它的第一个规划步骤是调用管理员来获取悬疑风格的蓝图。
- 2. 接下来，它识别出事实部分“阿波罗11号登月”，并知道需要准确的细节。它的第二个步骤是调用研究人员来获取相关事实。
- 3. 最后，在兼顾风格和内容的计划下，它创建了第三步：调用作家生成最终内容，将第 1 步的蓝图与第 2 步的事实相结合。

靴子悬停在空中。我先看到了影子。在我落下之前，它先吻了表面。接着是接触。第一步。在月球上。世界与我同在，遥远又无处不在。宇服里的空气在低语。表面承住了重量。这一刻被拉得很长，像一个音符，没有破碎。

在生产环境中，你可以决定只向管理员显示执行轨迹，或将输出保存到执行日志中。或者，你也可以决定在训练阶段让用户看到执行轨迹，以理解上下文引擎（Context Engine）。这可以在实施期间的研讨会上决定。

随着这次成功执行，我们现在可以退一步，欣赏我们构建的内容。每一个架构组件都在协同工作，将抽象的意图转化为结构化、可追踪且有意义的输出。让我们回顾这一架构飞跃，以及使其在实践中运行如此优雅的设计原则。

# 总结

本章解决的核心挑战是预定义工作流的僵化。目标是创建一个高级控制器，通过对可用工具进行推理来动态执行复杂任务。这段旅程始于构建规划器（Planner），它作为战略核心，创建实现目标的逐步配方。然后我们构建了执行器（Executor），即执行计划的运营经理。最后，包含了一个追踪器（Tracer），以确保整个过程透明且可调试。

引入的关键创新是规划阶段。在这里，规划器会咨询代理注册表（Agent Registry），该工具包描述了每个专家代理（管理员、研究员、作家）的能力。凭借这些知识，规划器使用外部 LLM 生成一个为用户特定高层目标量身定制的自定义、多步骤的 JSON 计划。这将“做什么”与“怎么做”分离开了。执行器遵循 JSON 计划，按正确的顺序调用专家代理。这一阶段实现了上下文链（Context Chaining），这在第一章中讨论过。这使得系统能够处理线性工作流无法处理的复杂依赖任务。

本章以一个功能完备、能够独立思考的上下文引擎告终。通过动态生成自己的工作流，系统不再是简单的流水线，而是一个智能且模块化的工作坊。在下一章中，我们将通过集成生产级功能（如上下文管理、安全防护和错误处理），对该引擎进行加固，以用于实际应用。

# 4 

125

## 问题

- 1. 执行器是否创建了实现用户目标的战略性、逐步的计划？（是或否）
- 2. 代理注册表的主要目的是为了记录每个操作以供调试吗？（是或否）
- 3. 文本中描述的上下文链是指一个代理的输出成为下一个代理的输入的过程吗？（是或否）
- 4. 专家代理（管理员/Librarian、研究员/Researcher 和作家/Writer）是否在架构图中用蓝色表示？（是或否）
- 5. `resolve_dependencies()` 函数是否允许代理查询向量数据库？（是或否）
- 6. 规划器是否使用代理注册表中的 `get_capabilities_description()` 方法来理解哪些工具可用？（是或否）
- 7. `context_engine()` 函数是通往外部 LLM 的主要接口吗？（是或否）
- 8. 作家（Writer）代理是否处理程序化 RAG 以获取风格蓝图？（是或否）
- 9. 整个过程首尾是否由代表用户目标和最终输出的橙色组件界定？（是或否）
- 10. 所有代理是否都使用模型上下文协议（MCP）进行通信？（是或否）

## 参考文献

- * Wei, J., Wang, X., Schuurmans, D., Bosma, M., Chi, E., Le, Q., & Zhou, D. (2022). Chain-of-Thought Prompting Elicits Reasoning in Large Language Models. Chain-of-Thought Eliciting Reasoning in Large Language Models. arXiv preprint arXiv:2201.11903.
- * Yao, S., Zhao, J., Yu, D., N, Ha., & Tsvetkov, Y. (2022). ReAct: Synergizing Reasoning and Acting in Language Models. arXiv preprint arXiv:2210.03629.
- * Schick, T., Dwivedi-Yu, J., Dessi, R., Raileanu, R., Romeli, M., Zettlemoyer, L., Cancedada, N., & Scialom, T. (2023). Toolformer: Language Models Can Teach Themselves to Use Tools. arXiv preprint arXiv:2302.04761.

# 续读

- Wu, Q., Bansal, G., Zhang, J., Wu, Y., Zhang, S., Zhu, E., Li, B., Jiang, L., Zhang, J., & Wang, C. (2023). AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation. AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation. *arXiv preprint arXiv:2308.08155.*

# 订阅免费电子书

新框架、演进中的架构、研究发布、生产环境分析——AI_Distilled 将噪音过滤为为亲手操作 LLM 和生成 AI 系统的工程师和研究人员提供的每周简报。现在订阅即可获得免费电子书，以及有帮助的每周见解。
