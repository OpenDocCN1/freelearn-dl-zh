# 第 2 章智能体工程师的工具箱

## 在开始之前：加入我们 Discord 上的 GenAI & LLM 社区

向作者本人提供反馈，并在我们的 Discord 服务器上与其他早期读者聊天。

![一个带有橙色正方形的二维码，AI 生成的内容可能错误。](img/file15.png)

[`packt.link/4Bbd9`](https://packt.link/4Bbd9)

> *在智能体中，智能表现为目标导向的、自主的行为*



# 智能体工程：核心框架与工具

— Andrej Karpathy，前特斯拉 AI 总监 (2024)

在智能体领域，`tooling` 定义了能力。随着智能体从响应式脚本转向目标导向的自主系统，开发者必须掌握不断扩展的框架、模型和基础设施生态系统。本章对智能体工程领域中可用的工具进行了结构化探索——为读者提供实用的见解和对比分析，以便在整个技术栈上做出明智决策。想象一个智能体能够自主研究市场趋势、合成数据并起草战略报告——一切实时完成。这就是正确工具包所释放的力量。选择合适的工具和框架是智能体开发过程中的一个关键决策点——它不仅会影响开发时间和运营成本（例如，考虑`LLM`推理成本，根据模型和提供者的不同，每百万 token 的成本可能从几分到几美元不等），还会影响你最终系统的基本能力。作为一名智能体工程师，你的工具箱定义了你的智能体可以感知的边界、如何推理，以及它们在世界上可以采取什么行动。虽然`LLMs`为现代智能体提供了认知引擎，但仅凭原始模型不足以构建实用的系统。当你将这些模型与设计良好的工具箱相结合时，智能体开发的真正威力就会显现，它可以实现高效知识检索（例如由向量数据库和`FAISS`等库支持的`RAG`）、工具集成（例如通过`OpenAI`强大的函数调用机制）、监控和部署。在本章中，我们将检查当前使用的主要智能体框架，回顾选择和优化`LLMs`的策略，并深入支持内存、推理、评估和部署的基础工具。本章并不是提供当前可用选项的快照（这些选项可能很快过时），而是侧重于即使在特定实现变化时也会保持相关的底层原理和模式。在整本书中，我们将主要使用`LangChain`和`LangGraph`作为核心开发框架，是因为它们拥有稳健的生态系统、生产就绪的文档。虽然我们会探索其他框架以提供背景并帮助你理解更广泛的格局，但我们的实用示例、代码实现和深度教程将专注于`LangChain`生态系统（编写时为 v0.3.x）。在本章中我们将涵盖以下主题：

- 智能体开发框架：架构蓝图
- 大语言模型：认知核心
- 支持基础设施：智能体生态系统
- 云原生智能体开发平台：托管军库

## 智能体开发框架：架构蓝图

框架是构建智能体的基础。它们提供了结构，规范了模式并封装了最佳实践，使开发者能够从随机实验转向可重复的可扩展工程。选择框架不仅仅是一个工具选择——它是一项影响扩展性、可维护性和性能的战略决策。以下是主要智能体开发框架的总结表。该表对它们的优势、局限性和理想用例场景进行了简要对比。

| **框架** | **优势** | **局限性** | **理想用场景** |
| :--- | :--- | :--- | :--- |
| **LangChain** | 模块化设计、广泛集成 | 原生不支持多智能体 | LLM 流、工具工作流 |
| **LlamaIndex** | 高级检索、语义压缩 | 需要编排支持 | 文档问答、内存层 |
| **AutoGPT** | 自主目标规划 | 可靠性低、控制脆弱 | 研究原型 |
| **CrewAI** | 基于角色的协调 | 处于早期阶段 | 多智能体团队 |

### 表 2.1 – 代理开发框架对比

正如`第 1 章`中关于`cognitive loop model`章节所讨论的，有效的代理必须不断在感知、推理、规划、行动和学习之间循环。我们在本章中探索的框架在代码中实现了这种认知架构，每个框架在结构化这些关键功能方面都有自己的方法。

理解这些不同的方法至关重要，因为你选择的框架不仅决定了你构建和迭代代理的速度，还决定了它们最终能实现什么能力。例如，`LangGraph` 的有向无环图（`DAG`）模型支持复杂多步推理的并行分支，优化了复杂工作流的吞吐量；而 `CrewAI` 的设计则可以为线性、专门化的任务提供更快速、更集中的对话。每个框架都体现了对代理行为的不同假设，为常见模式提供了不同的抽象，并对底层认知过程提供了不同程度的控制。通过研究它们的优势、局限性和最佳使用场景，你将能够做出符合你对性能、可扩展性和可维护性特定要求的明智决策。

## 关键框架的综合分析

起初，是一片混乱：拼凑在一起的临时脚本、模型与工具之间脆弱的连接、以及在复杂性压力下崩溃的脆弱推理链。随后，框架出现了：这种代理架构的结构化方法为这片数字荒野带来了秩序。然而，至关的一点是，虽然框架提供了显著的加速和结构，但它们引入了层层抽象。这种抽象虽然对快速开发有利，但有时会导致对底层机制的低级控制水平和理解的削弱。对于基础学习或高度自定义的场景，从更简单的 Python 代码开始构建代理能力，可以在利用完整框架之前提供更深层次的洞察。每个框架都代表了关于代码中如何结构化智能的不同哲学。

### LangChain：组合式的先驱

在数字星空中拥有超过 70,000 个 GitHub Star，`LangChain` 堪称代理框架中的元老级。它的计算图模型镜像了认知科学中关于顺序推理的理论（通常被称为 `chain-of-thought` 思维链处理），允许开发者从更简单的构建模块组合复杂的行为。这种设计使代理能够遵循分步推理流水线，图中的每个节点都对应一个认知或功能操作，就像人类如何将问题分解为顺序子任务一样。

`LangChain` 的架构围绕几个核心抽象构建，我们将在书中广泛地使用它们：

- **Chains（链）**：连接多个组件的顺序处理流水线。`LangChain` 还提供了稳健的回调和追踪机制，允许开发者观察、记录和调试这些链的执行流，这对于理解复杂的代理行为至关重要。
- **Agents（代理）**：自主决策者，它们接收输入，决定要采取什么行动（例如调用工具或查询 API），然后执行这些行动以实现目标。这一过程可能涉及工具选择、记忆检索或推理步骤，使代理能够以某种程度的自主运行。
- **Tools（工具）**：外部系统和 API 的接口
- **Memory（内存）**：维护对话上下文和长期存储的系统
- **Retrievers（检索器）**：访问和过滤相关信息的组件
- **Embeddings（嵌入）**：用于语义操作的文本向量转换

框架的模块化设计在实际实现显而易见。考虑这个基础代理设置的示例：

```python
from langchain.agents import initialize_agent, AgentType
from langchain.llms import OpenAI
from langchain.tools import Tool
from sympy import sympify
from langchain.tools.ddg_search.tool import DuckDuckGoSearchRun  # hypothetical import

#### 定义代理可以使用的工具
def calculator(expression: str) -> str:
    """安全地评估数学表达式."""
    try:
        result = sympify(expression)
        return str(result)
    except Exception:
        return "Invalid mathematical expression"

#### 使用 LangChain 提供的 DuckDuckGoRun tool
search = DuckDuckGoSearchRun()
tools = [
    Tool(name="Calculator", func=calculator,
         description="Useful for mathematical calculations"),
    Tool(name="WebSearch", func=search.run,
         description="Search the web for current information")
]

#### 初始化代理
llm = OpenAI(temperature=0)
agent = initialize_agent(
    tools=tools,
    llm=llm,
    agent=AgentType.ZERO_SHOT_REACT_DESCRIPTION,
    verbose=True
)

#### 让代理对工具的使用进行推理
result = agent.run("What’s the square root of 144, and can you find recent news about that number?")
```

此示例展示了`LangChain`的强大：代理自动确定它既需要数学计算也需要网络搜索，按适当顺序执行它们，并汇总结果。`ZERO_SHOT_REACT_DESCRIPTION`代理类型实现了 ReAct（推理与行动）模式，模型对要采取的行动进行推理，然后根据这些决策采取行动。`LangChain`的内存系统特别复杂，支持多种内存类型：

```python
from langchain.memory import ConversationBufferMemory, ConversationSummaryMemory
from langchain.schema import BaseMessage

#### Buffer memory 保留原始对话历史
buffer_memory = ConversationBufferMemory(
    memory_key="chat_history",
    return_messages=True
)

#### Summary memory 压缩旧对话
summary_memory = ConversationSummaryMemory(
    llm=OpenAI(),
    memory_key="chat_history",
    return_messages=True
)
```

该框架广泛的生态系统包含了超过 100 个与流行服务的预构建集成，如`OpenAI`、`Anthropic`、`Google`、`AWS`、向量数据库（`Pinecone`、`Chroma`、`Weaviate`）以及无数 API。这种生态系统的丰富性意味着大多数集成挑战已经由社区解决并测试。`LangChain`的核心是模块化的研究：用于顺序处理的链、用于环境交互的工具，以及在对话数字突触中维护上下文的内存系统。其丰富的组件库减少了实现常见代理模式所需的代码，让开发者专注于他们作品的独特之处。然而，`LangChain`的灵活性也带来了复杂性。框架大量的抽象层可能会引入性能开销，而且在没有`LangSmith`等适当的可观测工具的情况下，调试复杂的链可能是挑战性的。正如传统软件工程从简单的打印语句演变为结构化日志，再到全面的可观测平台一样，代理工程也需要类似的进步。此外，虽然`LangChain`在单代理场景中表现出色，但在复杂的多代理系统中还需要额外的编排——这就是`LangGraph`变得至关的地方。**参考 URL**：[`www.langchain.com/`](https://www.langchain.com/)

### LangGraph：工作流可视化器

`LangGraph` 代表了`LangChain`向有状态、循环工作流的演进，这种模式更密地反映了人类的认知过程。`LangChain`的链通常是线性的，而`LangGraph`允许具有循环、条件逻辑和复杂的状态管理的复杂分支决策树。这种可视化且有状态的方法显著缓解了调试复杂代理行为的痛点；通过像`LangSmith`等专用的可观测性工具，或通过集成标准的`OpenTelemetry`钩子来实现全面的跟踪和监控，可以获得更高的透明度。`LangGraph`的核心是将代理工作流建模为有向图：

- `Nodes`（节点）代表离散的处理步骤或代理函数
- `Edges`（边）定义步骤之间的流
- `State`（状态）在`nodes`之间维护并传递
- `Conditional routing`（条件路由）实现了动态决策

这种架构对于实现`第 1 章`中讨论的认知循环特别强大，其中代理必须在感知、推理、规划和行动阶段之间循环。以下是一个实用的示例：

```python
from langgraph.graph import Graph, Node
from langgraph.prebuilt import ToolExecutor
from langchain.tools import DuckDuckGoSearchRun, Calculator

#### 定义代理的工作流节点
def research_node(state):
    """收集关于该主题的信息."""
    query = state.get("user_query")
    search_tool = DuckDuckGoSearchRun()  # 指定它更安全
    research_results = search_tool.run(query)
    return {"research_data": research_results, "next": "analyze"}

def analyze_node(state):
    """分析收集到的信息."""
    research_data = state.get("research_data")
    # 在真实系统中，在此插入 LLM/分析逻辑
    analysis = f"Analysis of: {research_data[:200]}..."
    return {"analysis": analysis, "next": "decide"}

def decide_node(state):
    """决定是否需要更多研究."""
    analysis = state.get("analysis")
    # 带有可选 MAX_ITER 的循环条件
    if "insufficient data" in analysis.lower():
        if state.get("loop_count", 0) >= 3:  # 防止无限循环
            return {"next": "respond"}
        return {
            "next": "research",
            "loop_count": state.get("loop_count", 0) + 1
        }
    else:
        return {"next": "respond"}

def respond_node(state):
    """生成最终响应."""
    analysis = state.get("analysis")
    response = f"Based on my research and analysis: {analysis}"
    return {"final_response": response, "next": "END"}

#### 构建图
workflow = Graph()
workflow.add_node("research", research_node)
workflow.add_node("analyze", analyze_node)
workflow.add_node("decide", decide_node)
workflow.add_node("respond", respond_node)

#### 定义转换
workflow.add_edge("research", "analyze")
workflow.add_edge("analyze", "decide")
workflow.add_conditional_edges("decide", lambda state: {
    "research": "research" if state.get("next") == "research" else None,
    "respond": "respond"
})
workflow.set_entry_point("research")
workflow.set_finish_point("respond")

#### 编译并运行
app = workflow.compile()
result = app.invoke({"user_query": "Latest developments in quantum computing"})
print(result)
```

此示例展示了`LangGraph`的关键优势：如果初始分析不足，代理可以智能地循环回以收集更多信息，并在整个过程中保持状态。这种循环能力对于实现简单链无法实现的复杂推理模式至关重要。`LangGraph`在传统`LangChain`无法胜任的几个关键领域表现：

- 带有条件分支的多步推理
- 带有审批关卡的人机交互工作流：`LangGraph`允许开发者设计暂停执行并等待外部输入或人工审查的节点，例如在财务交易之前进行审批。这使得代理能够与人类就关键决策或复杂任务进行协作
- 复杂的多代理协调和交接
- 跨长时间运行过程的持久化状态管理
- 复杂的错误处理和重试逻辑

该框架还提供了强大的调试和可视化功能：

```python
#### 可视化工作流
from langgraph.graph import draw_mermaid

#### 生成工作流的 Mermaid 图
graph_diagram = draw_mermaid(workflow.get_graph())
print(graph_diagram)  # 显示你的代理逻辑的可视流
```

`LangGraph`的状态管理特别为复杂，支持简单的字典状态和复杂的类型化状态模式：



# Python 类型化状态与代码示例

```python
from typing import TypedDict, List
from langgraph.graph import Graph
class AgentState(TypedDict):
    user_input: str
    research_results: List[str]
    analysis_confidence: float
    iteration_count: int
    final_response: str
#### 图在执行过程中维护此类型化状态
```

`LangGraph` 将 `LangChain` 扩展为基于图的架构，将代理步骤视为有向无环图中的节点。这种推理路径的可视化为代理决策过程提供了所未有的透明度。在实际实现中，`LangGraph` 对多步代理任务提供了精确控制，在具有分支路径、高级错误处理和状态管理要求的复杂工作流中表现出色。虽然它比简单的框架学习曲线更陡，但这种投资在复杂的企业级应用中将获得回报。在本书的剩余章节中，我们将广泛使用 `LangGraph` 来实现复杂的代理工作流，从简单的顺序过程到复杂的多代理编排。

## 智能体框架探索

在智能体框架的殿堂中，`AutoGPT` 曾梦想过一种彻底的变革——真正的自主目标导向行为。凭借惊人的 150,000 GitHub Stars，它向世界引入了递归自我提示（recursive self-prompting），允许智能体在无需人工干预的情况下将高级目标分解为可执行的子任务。`AutoGPT` 为目标分解、任务规划和自主工具选择实现了更高层次的抽象，这些与目标导向认知系统的理论模型相一致。这种方法能够创建能够在极少人工监督下追求复杂目标的智能体。但这种自主性是有代价的。其控制机制仍然脆弱，生产环境的部署需要仔细配置以保持与用户意图一致。然而，在追求真正自主的道路上，`AutoGPT` 暗示了智能体架构的未来可能性。

**参考 URL**: [`github.com/Significant-Gravitas/AutoGPT`](https://github.com/Significant-Gravitas/AutoGPT)

### CrewAI：协作编排者

我们框架探索中的最新竞争者带来了一种不同的哲学——基于角色分化和多智能体协作的理念。`CrewAI` 抽象了“团队”协作工作的概念，通过分配的角色来处理复杂任务。在 `CrewAI` 中，每个智能体都通过角色、目标和背景等属性定义——在协作系统中创建出独特的人格。智能体通过内置的消息和委托机制进行通信，本质上是相互之间“大声思考”以规划和解决问题。`CrewAI` 拥有 30,000 GitHub Stars 且不断增长，代表了向更结构化的协作智能的转变——即专业智能体在中央编排者的指导下结合其能力。

> 从 0.4 版本开始，`CrewAI` 引入了与 `LangChain` 更紧密的集成，依赖 `LangChain` 的智能体和工具抽象来实现工具使用和编排。这使得 `CrewAI` 特别适用于已经在使用基于 `LangChain` 工作流的团队，尽管它也引入了一些依赖方面的考虑。

**参考 URL**: [`github.com/crewAIinc/crewAI`](https://github.com/crewAIinc/crewAI)

### AutoGen：对话架构师

由微软开发的 `AutoGen` 采用了一种独特的智能体编排方法，将 LLM 视为对话参与者。`AutoGen` 不仅仅关注智能体的角色定义或目标分解，而是引入了一种对话编程范式——每个由 LLM 支持的智能体通过消息进行交互，共同解决任务。`AutoGen` 通过将智能体定义为具有角色的函数（例如用户代理、代码执行器、规划器），并通过自定义的消息传递循环进行连接，实现了复杂的、有状态的多智能体工作流。这种结构允许动态协调、自适应规划和工具调用，比响应式智能体具有更高程度的控制力。它的优势在于精粒度的编排——允许开发者显式地管理轮替、输入/输出流和停止条件。`AutoGen` 越来越多地应用于企业级应用，其中透明度、协调性和模块化是关键。

**参考 URL**: [`github.com/microsoft/autogen`](https://github.com/microsoft/autogen)

### 优势、劣势与最佳用例

选择合适的智能体开发框架是一项战略性决策，它从根本上塑造了你的系统能力：

* `LangChain` 通过其模块化编排和庞大的集成生态系统脱而出。它在需要快速原型或复杂工具集成的场景中表现优异。然而，其分层抽象可能会在延迟敏感的应用中引入性能开销。

* `LlamaIndex` 以其以知识为中心的设计与众不同，提供了先进的语义索引和上下文压缩。请注意，上下文压缩是提炼大量信息的技巧。

* `AutoGPT` 专注于自主的目标导向行为，尽管其控制机制仍然脆弱。

* `CrewAI` 代表了向更结构化的协作智能的转变——即专业智能体在中央编排者的指导下结合其能力。

### 构建与集成决策

现代智能体工程遵循“组合优于构建”（compose-over-build）的哲学——通过集成组件组装智能系统，而不是从开始构建单体架构。在实践中，大多数团队遵循混合路径：在开发期间利用 `LangChain` 等框架进行编排，当出现特定的性能或安全限制时，选择性地替换组件。例如，一个团队可能开始使用 `LangChain` 的 `ConversationBufferMemory` 进行快速原型开发，但当其智能体需要持久且语义丰富的内存时，随后将其更换为 `Pinecone` 或 `Chroma` 等生产级向量数据库。

## 认知引擎与模型集成

当我们从框架转向其所编排的模型时，我们进入了人工智能认知领域。大语言模型（LLMs）代表的不仅仅是文本预测系统——它们是构建智能体（agent）智能的基础。这些模型作为`第一章`中`组件之间的通信模式`章节概述的智能体架构中的推理引擎，其中`认知核心`（Cognition Core）在感知、规划、记忆和执行组件之间进行协调。理解它们的能力、局限性和集成模式对于构建有效的智能体架构至关重要。在本节中，我们将研究驱动现代智能系统的认知引擎，以及如何有效利用它们的能力。

### 模型选择与集成

语言模型的选择从根本上决定了你的智能体能够感知、理解和生成什么。模型在能力频谱、专业化程度、上下文窗口、推理性能和运行特性方面存在差异。选择语言模型时需要考虑的因素：

*   模型从轻量且快速（尽管在复杂推理、事实检索或特定领域知识方面存在局限，例如 `Mistral 7B`）到功能强大但计算密集型的系统，例如 `GPT-4`、`Claude 3`、`Gemini` 以及其他以先进的问题解决和逻辑推理能力闻名的尖端模型。

*   某些模型擅长编码，另一些则擅长创意内容生成或多轮推理。

*   上下文窗口范围从 `8K` 到超过 `1M` 个 token。

*   托管选项、定价模型和速率限制差异巨大。

*   许可协议：模型可以是开源权重的（允许完全访问模型权重以进行本地部署和微调），也可以是闭源（通过 API 访问，由第三方提供商管理），影响控制权、定制性和长期成本。

### 混合模型架构

模型集成最引人的迷的方法可能是混合架构——即多个模型协作，每个模型处理与其优势相匹配的任务。这种方法类似于认知分工，不同的模型在统一的系统中承担专门的功能。这种混合方法与`第一章`中`多智能体系统：协作智能`章节中描述的多智能体系统非常契合，其中专门的智能体通过协作实现复杂目标。在混合模型架构中，我们在模型层面而非智能体层面实现了这种协作专业化原则，创造了一场由专门化认知引擎协同工作的交响乐。让我们来看演示这种方法的部分代码：

```python
def route_to_model(self, query, query_type, conversation_history=None):
    """Route query to appropriate model based on classification."""

    if query_type == QueryType.FACTUAL:
        return self._generate_mistral_response(query, conversation_history)
    elif query_type == QueryType.CREATIVE:
        return self._generate_claude_response(query, conversation_history)
    elif query_type == QueryType.ANALYTICAL:
        return self._generate_gpt4o_response(query, conversation_history)
```

这段代码展示了混合模型的实际实现。系统首先将传入的查询分类为类别（事实、创意或分析），然后将每个查询路由到最合适的专门模型。例如，简单的事实性问题由 `Mistral` 高效的 `7B` 模型处理以保证速度和成本效益；而复杂的创意任务则路由到 `Claude` 以利用其卓越的创意能力，分析性工作则利用 `GPT-4` 的推理优势。这个编排层使系统通过确保每个查询都由最适合它的模型处理，从而优化性能和成本。同时，考虑不同模型之间的 token 归一化也至关重要，因为独特的分词方法意味着相同的输入文本可能会导致不同的 token 数量，直接影响成本和对上下文窗口限制的遵循情况。编排层充当 AI 请求的交通调度员，位于用户查询和各种语言模型之间。该编排层将不同类型的查询路由到专门模型——以任何单一模型无法实现的方式平衡性能、成本和能力。在充分理解了如何选择和编排语言模型（从单模型部署到复杂的混合架构）之后，我们现在转向更广泛的工具和服务生态系统，将这些模型转换为生产级的智能体系统。虽然模型提供了认知能力，但支持基础设施使智能体能够记住过去的交互、访问外部数据源、与 API 和工具集成，并在大规模上可靠运行。这一基础设施层是理论潜力变为实际现实的地方。

## 基础设施：智能体生态系统

在框架和模型之外，存在丰富的支持技术生态系统，它们扩展了智能体的能力并确保可靠运行。就像城市需要建筑之外的基础设施——道路、公用事业、通信网络一样，智能体系统需要处理数据存储、外部交互、评估和监控的专门组件。这一基础设施层直接支持`第一章`中讨论的互操作协议，其中 `MCP`（模型上下文协议）和 `A2A`（智能体到智能体）协议为工具发现、调用和协作消息建立了标准接口。我们在本节中探索的支持技术提供了具体的实现，使这些协议能够在生产环境中运行。这些基础设施组件将理论潜力变为现实，使智能体能够感知环境、采取有意义的行动并随着时间的不断改进。掌握这些支持技术至关重要，因为它们决定了你的智能体是孤立的原型，还是能够进行真实世界部署的可扩展系统。你的记忆系统的质量影响智能体从经验中学习的程度；你的工具集成方法决定了智能体可以采取哪些行动；你的评估框架揭示了智能体是否按预期运行；你的监控基础设施确保了大规模的可靠运行。理解这些组件使你能够构建不仅智能而且健壮、可观测且不断改进的智能体。

### 记忆革命：向量数据库如何为 AI 智能体加速

想象你问朋友去年的一段对话。他们回忆细节的能力不仅取决于内存容量，还取决于大脑索引和检索信息的方式。同样，为了让 AI 智能体在我们的世界中智能地运行，它们不仅仅需要原始处理能力——还需要一个理解含义而非仅仅匹配单词的记忆系统。

#### 为什么传统搜索无法通过智能测试

想象一下：你正在排查代码，并搜索如何修复运行时错误。尽管网上有数以万计的相关资源，但你的搜索返回了无帮助的结果，因为最相关的解决方案使用了不同的术语来描述同一个概念，例如“调试代码异常”——它们在语义上相同但在词汇上不同。这不仅令人沮丧，更是*纯基于关键词搜索*的一个根本局限性。传统的搜索就像在拥挤的火车站通过大喊某人的名字来寻找他们，希望他们回应。只有当他们正在听这个名字时，这种方法才有效。虽然许多现代检索系统将关键词方法与更先进的技术相结合，但在搜索概念相关性时，仅依赖精确单词匹配的局限性就显而易见了。向量搜索因此出现了：这是一个范式转移，它理解的是概念，而不仅仅是关键词。这就像是一个只检查书名的管理员与一个理解你想要实现什么目标的管理员之间的区别。这种概念理解如何运作的细节细节——通过高维数学表示和语义相似性计算——将在下一节中探讨。通过使用 OpenAI 的 `text-embedding-ada-002` 等模型将文本转换为高维语义向量，向量搜索捕捉了意义的本质。两个用不同词表达相同思想的文本现在在这个意义的数学空间中“彼此”在一起。

> 深入探索：OpenAI 的嵌入乐场 (`https://platform.openai.com/docs/guides/embeddings`) 让你直可视化相似的概念是如何聚集在一起的，即使措辞不同。

## 魔法背后的数学

向量数据库听起来可能复杂，但其核心原理非常直观：将意义表示为空间中的方向。当你搜索向量数据库时，在后台发生的是一场数学之舞：

1. 你的问题变成了一个向量——本质上是指向高维空间中特定方向的箭头（取决于嵌入模型，例如 768、1,024 或 1,536 维）。

2. 数据库找到指向相似方向的已存储向量。

3. 最接近的向量（通过 `cosine similarity` 或 `dot product` 测量）对应于最相关的信息。

这种方法与搜索精确模式匹配的传统数据库截然不同。它是“找到这些精确的单词”与“找到这个概念”之间的区别。真正的魔法发生在近似近邻 (`ANN`) 算法中，如 `Hierarchical Navigable Small World` (`HNSW`) 或 `Inverted File Index` (`IVF`)，它们使得在毫秒内搜索数十亿高维向量成为可能。

> 冷知识：没有这些算法突破，在十亿向量数据库中找到最近的向量需要几分钟而不是毫秒。高效高维搜索的数学是让现代 AI 助手成为可能。

## 向量数据库格局：谁是谁

向量生态系统正在迅速进化，不同的平台针对不同的需求进行了优化：

* `Pinecone` – 专家：从底层开始为向量搜索构建。当毫秒至关重要且你需要云原生扩展时，`Pinecone` 可以胜任。它的 `upsert` API 让实时知识更新变得轻而许多 (`pinecone.io` (`https://www.pinecone.io/))

* `Weaviate` – 混合力量：在 `GraphQL` 接口将向量搜索与传统过滤相结合。当你的用户既需要语义理解又需要精确元数据过滤时，它是完美的 (`weaviate.io` (`https://weaviate.io/))

* `Chroma` – 开发者的朋友：轻量级、开源，并旨在让本地开发变得愉快。当你想在几分钟而不是几小时内原型 `RAG` 系统时，`Chroma` 表现优异 (`trychroma.com` (`https://trychroma.com/))

* `Milvus` – 企业基础：为大规模和复杂部署而构建。当你的向量搜索需要处理跨分布式系统的数十亿条记录时，`Milvus` 提供了工业级的能力 (`milvus.io` (`https://milvus.io/))

* `Qdrant` – 可靠的开源竞争者：`Qdrant` 为高性能、生产就绪的向量搜索设计。具有健壮的过滤功能、对负载索引的支持以及无缝集成到 `RAG` 流中，它平衡了开发者灵活性与企业级能力 (`qdrant.tech` (`https://qdrant.tech/))

你应该选择哪一个？诚实的答案是这取决于你的特定需求。对于早期实验，`Chroma` 的简单是不可比拟的。对于处理敏感数据的生产系统，自托管的 `Weaviate` 或 `Milvus` 可能更好。对于没有运营开销的纯云性能，`Pinecone` 很有吸引力。

## 构建你 AI Agent 的大脑

当向量数据库集成到代理架构中时，它们真正大放异彩。将它们想象是你 AI 系统的海马体——负责形成、索引和检索记忆的结构。向量数据库的最佳选择通常取决于关键因素，例如所需的规模、检索延迟的服务水平协议 (`SLAs`) 以及为了合规性数据驻留的要求。为了看到这些想法的应用，请考虑向量数据库如何支撑检索增强生成 (`RAG`) 流水线——使 AI 系统能够实时检索相关知识并将其纳入推理中。一个简单但强大的 `RAG` 流如下如下：

1. **分块你的知识**：将文档拆分为易于处理的部分（通常为 500–1000 tokens）

2. **嵌入所有内容**：将分块转换为向量（称为 `embedding`），以机器可以理解和比较的方式捕捉分块的意义。这些向量允许语义搜索和相似性匹配。

3. **带元数据存储**：将向量与源信息和时间戳一起保存

4. **按需检索**：当代理需要上下文时，找到相关的向量

5. **注入提示词**：在 `LLM` 响应之前将这些上下文知识馈给它

这种方法之所以具有革命性，是因为你的代理的知识变成了`动态`的而不是静态的。新信息可以持续添加到向量存储器中，立即增强代理的能力。

> 如 `LangChain` (`docs.langchain.com` (`https://python.langchain.com/docs/introduction/)) 和 `CrewAI` (`github.com/joaomoura/crewai` (`https://github.com/crewAIInc/crewAI)) 等框架为构建这些流水线提供了优雅的抽象。

第一阶段向量检索能帮你找到相关内容的区域，但重排序器（rerankers）能帮助你找到精确的房屋。像 `Cohere` 的 `reranker` 或 `Sentence Transformers` 的 `cross-encoders` 等模型会对查询-文档对进行详细检查，从而大幅提高精度。

### 元数据：你的秘密武器

纯语义搜索虽然强大，但将其与元数据过滤相结合可以产生魔法。想象不仅通过含义，还可以通过：

* 时效性（优先考虑新信息）

* 来源权威性（偏经过验证的来源）

* 部门相关性（关注特定的业务部门）

* 用户交互历史（个性化检索）

### 可观测性：透视黑箱

当检索失败时，理解原因至关重要。像 `LangSmith` ([smith.langchain.com](https://smith.langchain.com/)) 等工具让你可视化：

* 检索了哪些块

* 它们获得了什么样的相似性评分

* 它们是如何影响最终响应的



这种观测性让 RAG 从神秘变得可管理。

## 展望：内存增强的未来

我们正见证内存增强 AI 智能体的开始。未来的系统可能具有以下特征：

* **多模态向量存储**：将图像、音频和文本嵌入到统一空间中
* **推理感知检索**：系统不仅理解存在哪些信息，还理解哪些信息有助于解决特定的推理任务（这一能力目前是活跃的研发领域）
* **自演进的内存**：智能体根据用户反馈完善自己的分块和检索策略

向量数据库不仅仅是技术上的改进——它们代表了 AI 系统与知识之间关系的根本性转变。它们将 LLM 从静态、冻结的知识系统转变为动态的推理器，能够纳入新信息并适应变化的环境。然而，这种向可变内存的转变引入了新的挑战，特别是关于实时更新的写入延迟以及确保分布式内存存储之间的数据一致性问题。对于构建下一代 AI 智能体的开发者来说，掌握向量检索不是可选的——它是创建一个智能聊天机器人与构建真正智能助手之间的区别。

> *智能的衡量在于改变的能力。*

— 阿尔特·爱因斯坦

这句话不仅适用于人类，也适用于我们的 AI 系统。向量数据库赋予了智能体改变其所知的能力——这是迈向真正机器智能的第一步。在探索了向量数据库如何彻底改变智能体内存和知识检索之后，我们现在转向让智能体与外部世界交互并操作的机制。内存系统允许智能体学习和记忆信息，而工具集成框架在智能体的内部推理与其采取具体行动的能力之间提供了关键桥梁——从 API 调用、数据库查询到文件操作和系统命令。

## 工具集成框架

工具是智能体智能的解放，使其能够超越数字限制并操纵世界。在“感知-推理-行动”循环中，工具代表了思想转化为结果的时刻。可用工具的生态系统庞大且不断扩展——从简单的 API 封装器和数据库连接器到复杂的自动化平台和专业的领域工具。我们并没有尝试列出详尽的目录，而是专注于支持几乎所有智能体-工具交互的最两个基础的集成模式：用于 Python 开发的`LangChain`的`Tool`抽象，以及用于直接模型集成的`OpenAI`的`function calling`（函数调用）。注意的是，这两种方法并不是互斥的；例如，`LangChain`可以使用`StructuredTool`类封装`OpenAI`的 JSON Schema，允许开发者在`LangChain`智能体中使用`OpenAI`强大的函数调用能力。理解这些核心模式为集成任何工具提供了基础，无论其具体的实现或领域如何。

### LangChain Tools

`LangChain`提供了`Tool`抽象，这是一种强大的模式，可以将普通的 Python 函数转换为智能体兼容的工具。这种抽象处理了语言模型与外部系统之间复杂的编排，自动管理输入验证、错误处理和响应格式。`Tool`封装器本质上创建了一个智能体可以可靠发现和调用的标准接口，Python 函数在此转换为生产环境中的可靠工具：

```python
from langchain.agents import Tool
def get_stock_price(ticker: str) -> str:
    """返回模拟的股票价格或处理无效输入."""
    try:
        # 虚拟逻辑：在真实场景中，这可以调用 API
        if not ticker.isalpha():
            raise ValueError("Invalid ticker symbol")
        return f"The price of {ticker} is $123.45"
    except Exception as e:
        return f"Error fetching stock price: {str(e)}"
tool = Tool(
    name="StockPriceTool",
    func=get_stock_price,
    description="Fetches the current price of a stock"
)
```

这个示例展示了`Tool`模式的简单性：我们从一个普通的 Python 函数开始，它接受股票代码并返回价格信息。然后`Tool`封装器将该函数与智能体可以理解的元数据打包——一个用于识别的描述性名称、实际执行的函数，以及对工具用途的清晰描述。当智能体需要股票价格信息时，它可以通过名称和描述发现此工具，然后通过带有适当参数的底层函数来调用。`Tool`抽象处理了智能体推理过程与函数执行之间桥接的所有复杂性。

### OpenAI function calling

`OpenAI`的函数调用通过基于 JSON 的 Schema 提供了一套复杂的命令系统：

```json
{
  "name": "get_weather",
  "description": "Get the weather for a city",
  "parameters": {
    "type": "object",
    "properties": {
      "city": { "type": "string" }
    },
    "required": ["city"]
  }
}
```

此 JSON Schema 定义了一个智能体可以调用的天气函数，指定了函数名称、其用途以及带有数据类型的必需参数。与`LangChain`以 Python 为中心的方法不同，`OpenAI`的函数调用使用跨越不同语言和平台的标准 JSON Schema。语言模型可以解释此 Schema 以理解函数的功能以及如何正确调用它，从而在对话期间生成格式正确的函数调用。在有了健壮的工具集成让智能体能够在世界中采取行动后，我们的下一个重点转向如何通过全面的评估和基准测试系统确保这些行动产生预期的结果。

## 云原生智能体开发平台：托管军库

本节提供了 LLM Agent（智能体）开发云原生平台的对比概述，参考了行业报告和云提供商的文档。开源框架为自定义实现和特殊需求提供了无与伦比的灵活性和控制力，而主要的云提供商——Amazon Web Services (AWS)、Microsoft Azure 和 Google Cloud——提供了强大的托管平台，旨在简化生产环境中 LLM Agent 的开发、部署和扩展。这些云原生解决方案抽象了大部分底层基础设施的复杂性，提供了核心能力，如多智能体协作、RAG（检索增强生成）、记忆保留以及用于安全和可靠性的集成护栏。代理工程领域日益增长的趋势是混合方法，即结合这些托管云服务在基础基础设施和核心 LLM 访问方面的优势，以及开源框架在定义复杂代理逻辑和自定义工具集成方面的优势。本节将探索各家主要云供应商的核心产品，重点介绍它们的原生工具、集成能力和部署考虑。

### AWS：灵活的生态系统

AWS 提供了一全套用于构建和部署 LLM Agent 的服务，其特点是模型选择广泛且与现有 AWS 服务有深度集成。

#### 原生工具：Amazon Bedrock Agents

`Amazon Bedrock`是 AWS 旗舰级的生成式 AI 托管服务，通过单一 API 端点访问多种基础模型（FMs），包括 Amazon 的`Titan`模型以及第三方模型，如`Anthropic Claude`、`AI21 Labs Jurassic`、`Cohere Command`、`Meta's Llama 2`和`Stability AI`。`Bedrock`支持按需使用以及微调和 RAG 等自定义技术。AWS 内部的一个独特功能是`Amazon Bedrock Agents`，这是一项完全托管的服务，用于构建和扩展能够自主执行复杂任务的生成式 AI 机器人。关键特性包括：

* **多智能体协作**：`Bedrock Agents`支持编排多智能体工作流，由单个主管（协调器）智能体管理链式任务代理的执行。虽然这实现了分工协作和模块化任务处理，但它目前遵循的是中心化协调模型，而不是去中心化的智能体间协作。

* **RAG**：`Bedrock Knowledge Bases`提供了全托管的 RAG 工作流，负责处理文档摄入、向量数据库中的嵌入存储以及从你的数据中检索上下文。它可以连接到数据库和`S3`等各种数据源。值得注意的是，它支持使用自然语言进行结构化数据检索。

* **编排与多步任务**：`Bedrock Agents`使用基础模型的推理能力来分析用户请求，将其分解为逻辑序列，并自动调用必要的 API（定义为“动作组”）。`AWS Step Functions`是一个强大的无服务器编排器，可以跨多个步骤排序并管理状态，并直接与`Bedrock`API 调用集成。

* **记忆保留**：智能体可以在交互中维护对话历史，以提供个性化且无缝的用户体验，并提高多步任务的准确性。

* **代码解释**：该服务支持在安全环境中动态生成和执行代码，实现复杂的分析查询和数据分析。

* **提示词工程**：`Bedrock Agents`可根据用户指令、动作组和知识库自动创建提示词模板，开发者可以对其进行精炼。

* **护栏**：内置的安全和可靠性功能（如`Amazon Bedrock Guardrails`）可以过滤用户输入和模型响应中的有害内容。然而，这些护栏目前仅支持预定义的审核配置——尚不支持自定义策略脚本或深度定制的安全规则。

对于托管自定义模型或开源 LLM，`Amazon SageMaker`提供了将任何模型部署到具有自动扩展能力的托管端点的功能。此外，`SageMaker JumpStart`为`Llama 3`、`Mixtral`等热门开源模型提供了预构建的推理容器和部署模板——显著减少了生产级部署的运营工作。

#### 在 AWS 上集成开源框架

AWS 积极支持流行的开源 LLM Agent 框架的集成：

* **LangChain 和 LangGraph**：有一个的`langchain-aws`工具包和官方示例，展示了如何将`LangChain`（以及`LangGraph`）与`Bedrock`集成。`LangChain`代理可以直接调用`AWS Lambda`函数作为工具。

* **Strands Agents**：AWS 推出了`Strands Agents`，这是一个通过模型驱动方法进行 AI 开发的开源 SDK。它允许通过自然语言提示词、工具和模型定义代理（通过`LiteLLM`支持`Bedrock`等其他模型）。

* **模型上下文协议 (MCP)**：AWS 支持`MCP`，这是一种定义 AI 模型如何连接到各种数据源或工具的开放标准，旨在标准化智能体与工具的交互并促进企业内部的用（注意：`MCP`规范目前处于草稿阶段，可能会发生变化）。`SageMaker AI`在托管通过`MCP`服务器实现的工具执行动作的 LLM 起着至关重要的作用。

#### AWS 部署架构

AWS 提供灵活的部署架构：

* **无服务器 (AWS Lambda, API Gateway)**：`Lambda`函数是事件驱动应用和微服务的理想选择，作为由`Amazon API Gateway`触发用于实时交互的 LLM Agent 核心逻辑。这种设置提供了自动扩展和按需付费模式。

* **容器化 (Amazon ECS, Amazon EKS)**：对于复杂的、分布式或有状态的 LLM Agent 应用程序，AWS 提供了`Amazon Elastic Container Service (ECS)`和`Amazon Elastic Kubernetes Service (EKS)`。`ECS`是 AWS 专有的容器编排平台，具有成本效益并与其他 AWS 服务深度集成。基于 Kubernetes 构建的`EKS`提供了管理大规模容器化应用的丰富功能和更强的开源支持。实现代理工具的`MCP`服务器可以托管在`EC2`、`ECS`或`EKS`上。

### Azure：企业级动力源

Microsoft Azure 为开发和部署 LLM Agent 提供了一个强大且集成的生态系统，特别是对于深度嵌入微软生态系统的组织。

#### 原生工具：Azure AI Foundry Agent Service

`Azure OpenAI Service`是 Azure 的旗舰产品，提供了对 OpenAI 模型（`GPT-3.5 Turbo`、`GPT-4`、`Codex`、`DALL-E 2`）的 API 访问，这些模型托管在微软云数据中心，具有企业级的安全性和合规性。注意 GPT-4o 目前仅支持多租户模式。Azure AI Foundry Agent Service 是一个统一平台，旨在为企业环境构建、部署和运行由 LLM 驱动的智能代理。它被概念化为代理工厂，在多个维度具有全面的能力：

* **模型**：从不断增长的目录中选择，包括`Azure OpenAI`模型、`Llama`、`Mistral`和`Cohere`。

* **自定义**：通过微调、蒸馏或领域提示词对模型进行定制，以编码代理行为。

* **AI 工具**：智能体配备了访问企业知识（例如`Bing`、`SharePoint`、`Azure AI Search`）并并通过`Azure Logic Apps`、`Azure Functions`或`OpenAPI`采取行动的能力。

* **编排**：“连接的代理体”负责管理工具调用、更新线程状态并处理重试。Azure AI Foundry 支持具有内置代理体间消息传递的多代理体协作。Azure Logic Apps 是一种类似于 AWS Step Functions 的无服务器工作流引擎，适用于定义 LLM 代理流并与各种服务集成。Azure Durable Functions（Azure Functions 的扩展）允许通过代码编写编排函数以处理复杂的序列。Azure AI Studio Prompt Flow 提供了一个可视化画布，用于链接提示词和 Python 代码节点。

* **信任**：企业级功能确保了可靠性，包括通过 Microsoft Entra 进行身份管理、基于角色的访问控制 (RBAC)、内容过滤、加密和网络隔离。

* **观测性**：AI Foundry 捕获日志、跟踪和评估，具有全线程级可见性并与 Azure Application Insights 集成。

Azure 还提供了“OpenAI on Your Data”，这是在 Azure AI Studio 中用于 RAG 的简化设置方案，它可以自动使用 Azure Cognitive Search 为聊天模型索引并检索数据。

#### 在 Azure 上集成开源框架

Azure AI Foundry Agent Service 显式支持结合各种开源 SDK：

* **Semantic Kernel**：Semantic Kernel 中的`AzureAIAgent`提供了高级的对话能力和无缝的工具集成，专注于企业级就绪性、安全合规性。

* **AutoGen**：由微软研究院开发，AutoGen 将一切构化为专业代理体之间的异步对话，适用于多轮对话和实时工具调用。Azure AI Agent Service 可以将 AutoGen 中定义的单个代理体编排成复杂的多代理体工作流。

* **LangChain**：虽然集成深度不如 Semantic Kernel 或 AutoGen，但 LangChain 代理体可以部署在 Azure 的计算服务（如 Azure Container Apps Dynamic Sessions）上，以为代码解释器提供安全的沙箱环境。

#### 在 Azure 上的部署架构

Azure 提供了灵活的部署选项：

* **无服务器 (Azure Functions, Azure Container Apps)**：Azure Functions 适用于部署 Semantic Kernel SDK 和 AutoGen 多代理体应用，提供自动扩展和成本效益。Azure Container Apps 可以托管带有 AI 代理的 Web 端聊天应用程序，并为代码执行提供安全隔离的沙箱环境。

* **容器化 (Azure Kubernetes Service - AKS)**：AKS 是一种托管的 Kubernetes 服务，非常适用于需要大规模容器编排和对 Kubernetes 环境进行深度控制的复杂分布式应用。

### Google Cloud：AI 创新中心

Google Cloud 利用其在 AI 研究领域的领先地位和基础设施实力，提供高度可扩展且成本效益的 AI 服务，并战略性地关注互操作性和企业搜索。

#### 原生工具：Agentspace, Vertex AI Agent Builder/Engine/ADK

Google Cloud 的 LLM 中心是`Vertex AI`，特别是其生成式 AI 产品，如 PaLM 2 和即将推出的 Gemini 模型系列。Vertex AI 还通过其 Model Garden 包含了 Meta 的 Llama 2 等特定的外部模型。Google Cloud 提供了一套用于构建和部署 LLM 代理体的集成服务：

* **Agentspace**：用于企业工作的搜索和 AI 代理体中心，将应用程序连接到 Google 品的多模态搜索和 AI 代理体。它包含“代理设计器”（Agent Designer）和“代理库”（Agent Gallery）。注意 Agentspace 目前处于私测预览阶段。

* **Vertex AI Agent Builder**：用于发现、构建和部署 AI 代理体的全面功能 set。

* **代理体开发包 (ADK)**：一个开源、框架无关的框架，通过对代理体行为的精确控制，简化了复杂的多代理体系统的创建。它驱动了 Agentspace 并简化了多代理体传输和规划。

* **Vertex AI Agent Engine**：一种全托管的 Google Cloud 服务，用于在生产环境中部署、管理和扩展 AI 代理体。它抽象了低级任务，并处理基础设施、扩展、安全、评估和监控。



**Agent2Agent (A2A) 协议**：Google 正在积极开发开放的 A2A 协议，以实现 AI 代理体之间的互操作性，无论其底层框架或供应商为何。

**模型上下文协议 (MCP)**：Google Cloud 还支持 MCP，这是一种代理体以标准化方式连接并利用外部工具及数据源的开放协议。

Google Cloud 的代理工具适用于多种用例，包括企业搜索、内容生成和自动化。作为 Agent Builder 部分的 Vertex AI 扩展允许代理体连接到 Google Workspace 和其他外部 API。

## 在 Google Cloud 上集成开源框架

Vertex AI Agent Engine 设计为与框架无关，为流行的开源 LLM 框架提供灵活支持：

- **LangChain, LangGraph**：Vertex AI Agent Engine 提供与 LangChain 和 LangGraph 的完全集成。LangChain 也可以通过`LangServe`部署在 Google Cloud Run 和 Google Kubernetes Engine (GKE) 上。
- **AutoGen, LlamaIndex**：通过 Vertex AI SDK 与托管模板集成的支持。
- **CrewAI**：通过 Vertex AI Agent Engine 上的自定义模板支持。

Google 对`ADK`和 A2A 协议等开源工具的关注旨在为 AI 代理体培养更广泛、更互操作的生态系统，减少供应商锁定。

## 在 Google Cloud 上的部署架构

Google Cloud 提供了灵活且可扩展的部署选项：

- **无服务器 (Cloud Run)**：一个全托管无服务器平台，为 AI 应用负载和代理体提供可扩展的环境。它根据需要自动扩展实例，提供按需付费模型，并与 Gemini API 或 Vertex AI 端点集成。Cloud Run 可以配置为沙箱化代码执行。
- **容器化 (Google Kubernetes Engine - GKE)**：GKE 是一种托管的 Kubernetes 服务，适用于复杂的微服务架构、有状态应用以及需要自定义基础设施或网络配置的负载。`LangServe`可以简化 GKE 上的 LangChain 部署。Cloud Run 和 GKE 提供了高移植性，允许在两者之间运行相同的容器镜像。

对于核心代理体推理之外的任务（如模型训练、微调和持续 RAG 更新）的健壮工作流编排，`Vertex AI Pipelines`提供了一种定义和执行这些复杂 ML 工作流的托管服务。

## 云平台选择建议

“最佳”的云平台取决于特定的项目需求和现有的组织基础设施：

- **对于 AWS**：适用于已深耕 AWS 生态系统的组织，重视灵活性、多样化模型以及与现有 AWS 服务和数据湖的深度集成。它对多代理体协作和边缘分布式推理的关注使其适用于复杂、大规模的部署。
- **对于 Azure**：当必须使用尖端 OpenAI 模型（GPT-4），或需要将 AI 集成到以微软为中心的组织（Office 365, Dynamics, Teams, SharePoint）时是首选平台。Azure AI Foundry 提供了统一的平台体验，对代理体具有强大的治理和身份管理，吸引优先考虑简化运营和合规性的用户。
- **对于 Google Cloud**：对于那些重视成本效益的扩展、Google 的 AI 研究优势以及更强调开放标准和易工具连接的“整体”代理平台的人来说是一个强有竞争者。它在企业搜索和多模态 AI 方面的优势，结合 Cloud Run 等快速部署选项，使其对偏好编码与托管服务混合的开发者具有吸引力。

你所构建的工具包从根本上塑造了你的系统的感知能力、推理方式以及能够采取的行动。本章探讨了核心组件——框架、模型、数据库、集成机制、评估系统和监控解决方案。

这些关于工具包的决策决定了你的实现在智能 AI 演进框架（Agentic AI Progression Framework）中所处的位置。你的选择决定了智能体未来可能成为边界。

随着格局的演变，保持适应性应始终是一个核心设计原则。在下一章中，我们将探索智能体工作流架构——研究决策循环和多步执行路径。
