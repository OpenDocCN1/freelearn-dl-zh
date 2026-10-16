# 附录 A

# 上下文引擎参考指南

上下文引擎（Context Engine）是一个复杂的多代理系统，旨在变换与大语言模型（LLMs）的交互范式。它超越了简单的提示-响应循环的限制，这种循环通常依赖统计概率并产生不可预测的结果。相反，上下文引擎为定向创建提供了一个结构化环境，使工程师不仅控制 AI 生成的内容，还能控制其在定义边界内的推理。这种方法将用户的角色从提问者变为架构师，设计 AI 思考过程的蓝图。

该引擎是上下文工程的实践实现，上下文工程是将生成式 AI 转换为完全控制且可靠伙伴的学科。它通过战略部署由中心规划器（Planner）和执行器（Executor）协调的代理，来实现这种控制，所有代理均通过标准化的模型上下文协议（MCP）进行通信。该系统设计为透明盒架构，优先考虑透明性、可预测安全性，适用于企业部署。

《上下文引擎参考指南》是您的技术伙伴，将书中的所有概念和代码模式整合进一个清晰、易用的格式中。它将上下文工程的理论转化为构建、扩展和维护系统的实际指令。每个章节都将前面章节解释的内容与实际实现连接起来，展示了每个函数、代理和助手如何为整体架构做出贡献。对于动手操作的读者，它提供了关于系统如何运行、数据如何流过每个组件以及如何根据现实世界需求调整设计的清晰说明。它还作为长期资源，在构建完核心上下文引擎后，支持进一步的实验、定制和协作。

# 理论基础

上下文引擎的核心哲学源于彻底改变我们理解语言结构和意义的语言学理论。我们不再仅仅将句子视为词的线性序列，而是将其视为意义的多维结构。这种方法深受吕西安·泰斯涅（Lucien Tesnière）和查尔斯·J·菲尔（Charles J. Fillmore）基础性工作的影响。

Tesnière 引入了依存语法，将句子可视化为分级结构（树状），其中词依赖于其他词，并围绕主动词展开。Fillmore 通过他的《格壳角色理论》（*The Case Role Theory*）扩展了这一点，该理论后来演变为语义角色标记（SRL）。SRL 是解构句子以回答基本问题的技术：谁对谁做了什么，在何时、何地以及为什么？（*第一章*）。它识别了每个组件相对于中心动作（谓语）所扮演的功能角色。

# 语义蓝图

上下文引擎通过使用语义蓝图（semantic blueprints）使 SRL 落地。语义蓝图是提供给 LLM 的非结构化、无歧的计划，通常为 JSON 格式。它定义了生成目标、风格指南、结构以及参与场景或主题的主员角色。

通过使用语义蓝图，我们为 LLM 提供了精确的模式而非开放式请求。这把创作行为转化为可靠的工程过程。在上下文引擎中，这些蓝图存储在专门的向量数据库（上下文库）中，并由上下文管理员（Context Librarian）代理根据用户意图动态检索。

# 附录 A

2. **数据摄取 (Data_Ingestion.ipynb)**：此脚本处理原始文档，将其切分为可管理的分块，并关键性地为每个分块添加源元数据。这些元数据对于高保真 RAG 至关重要，能够实现可验证的引用（第 8 章）。
3. **Pinecone 知识库**：将处理后的分块（带有嵌入和元数据）上传到向量数据库，准备进行语义检索。

## 上下文引擎流 (Context Engine workflow)

这是系统的运行时操作，由`context_engine()`函数管理：

1. **用户目标**：当用户提交一个高级目标时，工作流开始。
2. **飞行前审核检查**：用户的目标会立即由`helper_moderate_content`函数进行审查。这是第一道安全护栏，旨在防止系统处理不适当或有害的请求。如果目标被标记，进程将停止。
3. **规划器与执行器 (Planner and Executor)**：如果目标是安全的，规划器会分析目标和可用的代理能力（由`AgentRegistry`提供）。它生成一个多步骤执行计划。执行器接管控制，管理该计划的执行。
4. **代理工作流**：执行器根据计划调用相应的代理（例如：管理员、研究员、总结器或写作者）。内部工作流随代理而异。例如，研究员的流程涉及以下步骤：
   - **检索 (Retrieve)**：查询 Pinecone 知识库获取相关信息（`query_pinecone`）
   - **清洗 (Sanitize)**：检查检索到的数据是否存在潜在的提示词注入威胁（`helper_sanitize_input`）
   - **合成 (Synthesizing)**：使用 LLM 根据清洗后的上下文生成内容或答案（`call_llm_robust`）
   - **上下文链 (Context chaining)**：执行器管理状态，将一个代理的输出作为下一个`resolve_dependencies`机制的输入。
5. **飞行后审核检查**：在执行整个计划并生成最终内容后，激活第二道安全护栏。`helper_moderate_content`函数在将 AI 的输出返回给用户之前对其进行审查。



## 最终输出给用户

如果生成的内容被标记，它将自动被替换为安全消息。否则，将安全且最终确定的内容交付给用户。

这种架构确保了系统在整个运行过程中保持透明、可控和安全。规划器、执行器和代理之间的关注点分离，结合健壮的安全协议，使得上下文引擎适用于企业级部署。

## commons 库引用 (The commons library reference)

`commons` 库包含了上下文引擎的核心实现。它被组织成若干几个 Python 模块，每个模块负责系统功能的某个特定方面。本节提供了这些模块中每个函数的详细参考。我们将开始检查提供与外部服务进行基础交互的辅助函数。

## 文件：helpers.py

`helpers.py` 文件包含重要的实用函数，处理与外部 API（如 OpenAI 和 Pinecone）的交互、数据格式化和安全协议。这些函数设计得具有健壮性，为生产环境加入了错误处理和重试逻辑。

### call_llm_robust()

此集中函数处理所有与 LLM 的交互，用于内容生成和规划：

- **目的：** 为调用 LLM 提供一个标准化、具韧性的接口，通过自动重试确保可靠性。

- **依赖项：**
  - `tenacity`：用于 `@retry` 装饰器以处理瞬时的 API 失败
  - `openai.APIError`：用于特定的错误处理

- **参数：**
  - `system_prompt` (str)：定义 AI 角色和任务约束的基础指令
  - `user_prompt` (str)：AI 需要处理的特定输入或数据
  - `client` (OpenAI client object)：用于 API 交互的初始化客户端对象（依赖注入）
  - `generation_model` (str)：要使用的 LLM 标识符（例如 `gpt-5`）
  - `json_mode` (bool)：如果为 `True`，则指示 LLM 严格以 JSON 格式返回输出

- **工作流：**
  - a. 调用 `client.chat.completions.create`。
  - b. 根据 `json_mode` 设置格式。
  - c. 使用指定的模型和提示词。
  - d. 如果成功，返回内容。
  - e. 如果发生 `APIError` 或其他异常，记录日志并利用重试逻辑。

### get_embedding()

此函数处理嵌入，用于向量表示：

- **目的：** 使用指定的模型将文本转换为嵌入。

- **依赖项：**
  - `tenacity`：用于 `@retry` 装饰器
  - `openai.APIError`：用于特定的错误处理

- **参数：**
  - `text` (str)：要嵌入的文本
  - `client` (OpenAI client object)：用于 API 交互的初始化客户端对象
  - `embedding_model` (str)：要使用的模型标识符（例如 `text-embedding-3-small`）

- **工作流：**
  - a. 清理输入的换行符。
  - b. 调用 `client.embeddings.create`。
  - c. 如果成功，返回嵌入向量。

### create_mcp_message()

此函数实现了组件之间的标准化通信：

- **目的：** 将数据结构化为 MCP。

- **参数：**
  - `sender` (str)：发送者名称
  - `content` (dict 或 str)：主要负载
  - `metadata` (dict，可选)：任何元数据
  - `protocol_version` (str)：指定 MCP 版本

- **结构：** 返回包含以下键的字典。

### query_pinecone()

此函数在命名空间内与 Pinecone 交互以检索信息：

- **目的：** 在命名空间内执行查询并检索信息。

- **参数：**
  - `query_text` (str)：查询文本
  - `namespace` (str)：命名空间
  - `top_k` (int)：返回的结果数量
  - `index` (Pinecone index)：初始化的索引

- **工作流：**
  - a. 记录尝试。
  - b. 调用 `get_embedding` 将 `query_text` 转换为向量。
  - c. 调用 `index.query` 并传入向量、命名空间和 `top_k`。
  - d. 请求包含元数据（如文本或 JSON）。
  - e. 返回匹配列表。

### count_tokens()

此工具用于管理上下文长度，对于控制至关重要：

- **目的：** 根据特定模型的编码方案计算文本的 token 数量。

- **依赖项：** `tiktoken`（用于 token 计数的库）

- **参数：**
  - `text` (str)：输入文本
  - `model` (str)：用于确定编码的模型标识符

- **工作流：**
  - a. 尝试加载指定模型的编码。
  - b. 如果未找到模型，则回退到标准的 `c100k_base` 编码。
  - c. 编码并返回列表长度。

### helper_sanitize_input()

此函数是一个关键的安全引入，用于缓解注入攻击的风险（第 7 章）：

- **目的：** 检测并阻止与尝试操作 LLM 指令相关的模式。

- **依赖项：** `re`（正则表达式模块）

- **参数：** `text` (str)：要清洗的文本（通常是从向量数据库检索的数据）

- **工作流：**
  - a. 函数定义了一个 `injection_patterns` 列表（例如：忽略之前的指令或作为）。
  - b. 遍历模式并使用 `re.search`（不区分大小写）检查模式是否存在于输入文本。
  - c. 如果发现模式，记录警告并抛出 `ValueError`，立即停止该特定文本的处理。
  - d. 如果未发现模式，记录成功消息并返回原始文本。

### helper_moderate_content()

该函数实现了自动化内容审核保护层，确保引擎在负责任边界内运行（第 8 章）：

- **目的：** 使用 OpenAI Moderation API 检查内容（用户输入或 AI 输出）是否违反了安全策略。

- **参数：**
  - `text_to_moderate` (str)：需要审核的内容
  - `client` (OpenAI client object)：用于与 API 交互的已初始化客户端对象

- **工作流：**
  - a. 该函数记录审核尝试。
  - b. 它调用 `client.moderations.create` 并传入输入文本。
  - c. 它解析 API 响应，以构建一个包含以下内容的详尽报告：
    - `flagged` (bool)：是否违反了任何策略
    - `categories` (dict)：违背了哪些特定类别
    - `scores` (dict)：每个类别的置信度评分
  - d. 如果内容被标记，它会记录一条警告。
  - e. 至关重要的一点，它包含一个“故障安全”机制：如果 API 调用由于任何原因失败（例如网络错误），它将返回一个指示内容已被标记的报告，防止未审核的内容继续。

我们现在已经在 `helpers.py` 文件中详细说明了基础工具。接下来，我们将检查利用这些工具执行特定任务的专家代理程序。

## 文件：agents.py

`agents.py` 文件定义了在上下文引擎（Context Engine）中执行核心任务的专家代理程序。每个代理都旨在处理特定的职责，例如检索指令、查找事实或生成内容。

### agent_context_librarian()

上下文管理员（Context Librarian）负责根据输出所需的样式或结构检索相应的语义蓝图：

- **目的：** 对上下文库（Context Library）进行语义搜索，以找到与用户意图匹配的蓝图。

**参数：**
- `mcp_message` (dict)：包含所需的 `intent_query`（意图查询）的输入消息
- `client`、`index` 和 `embedding_model`：传递给 `query_pinecone` 的依赖项
- `namespace_context` (str)：包含语义蓝图的 Pinecone 命名空间

**工作流：**
- a. 代理从 `mcp_message` 输入中提取 `intent_query`。
- b. 它调用 `query_pinecone` 来搜索 `namespace_context`（上下文库）。该命名空间中的嵌入（embeddings）对应于蓝图的描述。
- c. 如果找到匹配项，它从元数据中检索 `blueprint_json` 并通过 MCP 消息返回。
- d. 如果未找到匹配项，它返回一个默认的中性蓝图。

### agent_researcher()

研究者（Researcher）代理负责从知识库中检索并合成事实性信息。该实现针对高保真 RAG 进行了升级，确保包含了源引用（第 8 章）：

- **目的：** 寻找相关数据块，对其进行清洗，并合成一个带有引用的事实性答案。

**参数：**
- `mcp_message` (dict)：包含所需的 `topic_query`（主题查询）的输入消息
- `client`、`index`、`generation_model` 和 `embedding_model`：传递给 `query_pinecone` 和调用 `call_llm_robust` 的依赖项
- `namespace_knowledge` (str)：包含事实性数据的 Pinecone 命名空间

**工作流：**
- a. 代理从 `mcp_message` 输入中提取 `topic_query`。
- b. 它调用 `query_pinecone` 从 `namespace_knowledge` 中检索前三个相关数据块。
- c. 它遍历结果，对每个数据块的文本调用 `helper_sanitize_input`。如果某个数据块清洗失败，则跳过。
- d. 它从清洗后数据块的元数据中收集唯一的源文档名称。
- e. 它构建一个包含清洗后的素材和主题的提示词，通过 `call_llm_robust` 指示 LLM 根据提供的来源合成答案。
- f. 它将 LLM 的发现与程序化收集的来源相结合，并在 MCP 消息中返回 `answer_with_sources`（带来源的答案）。

### agent_writer()

Writer 代理是最后的生成组件，负责将语义蓝图中的指令应用于事实性源素材：

- **目的：** 通过结合研究结果与样式及结构指令来生成最终输出。

- **参数：**
  - `mcp_message` (dict)：包含蓝图以及事实或 `previous_content`（之前的内容）的输入消息
  - `client` 和 `generation_model`：用于 `call_llm_robust` 的依赖项

- **工作流：**
  - a. 代理从 `mcp_message` 输入中解包蓝图、事实和 `previous_content`。
  - b. 它实现了处理事实各种数据契约的逻辑（例如事实、摘要和带来源的答案），以确保与不同的上游代理（研究者或摘要器）兼容。
  - c. 它确定素材（新事实或需要重写的内容）。
  - d. 它构建一个结合语义蓝图和素材的提示词。
  - e. 它调用 `call_llm_robust` 根据蓝图生成最终内容。
  - f. 它在 MCP 消息中返回最终输出。

### agent_summarizer()

引入摘要器（Summarizer）代理用于上下文缩减，充当门卫以管理 Token 数量和成本：

- **目的：** 将长文本压缩为简要摘要，由特定的目标驱动。

**参数：**
- `mcp_message` (dict)：包含 `text_to_summarize`（待摘要文本）和 `summary_objective`（摘要目标）的输入消息
- `client` 和 `generation_model`：用于 `call_llm_robust` 的依赖项

- **工作流：**
  - a. 代理从输入中提取 `text_to_summarize` 和 `summary_objective`。
  - b. 它构建一个提示词引导 LLM。
  - c. 它调用 `call_llm_robust` 进行摘要。
  - d. 它在 MCP 消息中返回摘要。

我们现在有了负责执行的代理。接下来，我们将检查组织并部署这些机制的组件。

## 文件：registry.py

`registry.py` 文件实现了代理注册表，作为代理的中央目录和工厂。它让 Planner 理解能力，并让 Executor 调用正确的代理。

### 类：AgentRegistry

该类管理代理函数的注册和检索：

- **初始化（`__init__`）**
  - 构造函数初始化了 `self.registry` 字典，将代理名称映射到相应的函数。

### 方法：get_handler()

这是 Executor 使用的核心工厂方法：

- **目的：** 获取对应代理名称的处理器并注入必要的依赖项。

**参数：**
- `agent_name` (str)：Planner 请求的代理名称
- `client`、`index`、`generation_model`、`embedding_model`、`namespace_context` 和 `namespace_knowledge`：运行时配置和已初始化的客户端

**工作流：**
- 它在 `registry` 中查找 `agent_name`。如果未找到，则抛出 `ValueError`。
- 它使用条件逻辑确定特定代理所需的依赖项。
- 它返回一个封装代理函数的 lambda 函数。该 lambda 接收 `mcp_message` 输入，并带有预注入的依赖项调用底层代理函数。这确保代理只接收它们所需的依赖项。

### 方法：get_capabilities_description()

这对于上下文引擎的规划阶段至关重要：

- **目的：** 生成所有可用代理及其角色所需参数的结构化、可读描述。

- **工作流：** 该方法返回一个多行字符串，专门用于嵌入 Planner 的系统提示词中。该描述为 Planner LLM 提供了创建有效执行计划所需的工具信息，确保使用正确的代理名称和键。

### 全局对象：AGENT_TOOLKIT

在模块导入时，全局初始化一个 `AgentRegistry` 实例。`AGENT_TOOLKIT` 对象在上下文引擎（`engine.py`）的规划和执行阶段使用。

## 文件：engine.py

`engine.py` 文件包含了上下文引擎的核心逻辑，涵盖 Tracer、Planner 和 Executor。该文件协调了从接收目标到交付最终输出的整个过程。

### 类：ExecutionTrace

`ExecutionTrace` 类通过记录整个执行流提供了关键的调试和可观测能力：

- **目的：** 维护关于计划、每个步骤的输入和输出、执行状态以及持续时间的详细记录。

- **方法：**
  - `__init__(self, goal)`：初始化追踪，记录目标并设置开始时间。
  - `log_plan(self, plan)`：记录由 Planner 生成的执行计划。
  - `log_step(self, step_num, planned_input, mcp_output, resolved_input)`：记录单个执行步骤的详细信息，关键在于同时捕获输入（带有占位符）和实际解析的输入（引用已替换为数据）。
  - `finalize(self, status, final_output=None)`：完成追踪收尾工作，记录状态和最终输出，并计算总持续时间。

### 函数：planner()

Planner 是引擎的策略核心，负责解释目标并生成结构化的执行计划：

- **目的：** 使用大语言模型（LLM）利用可用的代理能力创建一个分步的计划。

- **参数：**
  - `goal` (str)：用户提供的层级目标。
  - `capabilities` (str)：由 `AgentRegistry.get_capabilities_description()` 生成的代理描述。
  - `client` 和 `generation_model`：`call_llm_robust` 函数的依赖项。

- **工作流：**
  - a. 它构建一个详细的系统提示词，其中包含了可用的能力以及生成 JSON 格式计划的严格指令。
  - b. 它强调了上下文链的使用（使用 `$$STEP_N_OUTPUT$$` 引用来处理依赖关系）。
  - c. 它调用 `call_llm_robust` 并设置 `json_mode=True` 以生成计划。
  - d. 解析 JSON 字符串并验证其是否符合预期的 `"plan": [{...}]` 结构。
  - e. 如果结构有效，则返回计划（步骤列表）。否则抛出错误。

### 函数：resolve_dependencies()

该辅助函数对于在执行阶段实现上下文链至关重要：

- **目的：** 将步骤输入参数中的占位符依赖引用（例如 `$$STEP_1_OUTPUT$$`）替换为之前步骤生成的实际数据。

- **参数：**
  - `input_params` (dict)：由 Planner 定义的当前步骤的输入参数。
  - `state` (dict)：包含所有之前步骤输出的执行状态。



# 工作流

该函数使用递归深度遍历`input_params`字典。如果遇到匹配依赖引用格式的字符串值，它会在`state`中查找对应的键。如果找到该键，则用数据替换引用；否则抛出`ValueError`，指示计划中存在依赖错误。

# 函数：context_engine

这是上下文引擎（Context Engine）的主入口点和执行器：

- **目的：** 管理任务的整个生命周期，包括规划、执行、状态管理和追踪。

- **参数：**

    - `goal (str)`：用户目标。

    - `client`、`pc`（Pinecone 客户端）和`index_name`：初始化的客户端和配置。

    - `generation_model`、`embedding_model`、`namespace_context`和`namespace_knowledge`：运行时配置参数。

- **工作流：**

    - a. **初始化**：初始化`ExecutionTrace`并获取`AgentRegistry`实例。连接到 Pinecone 索引。

    - b. **阶段 1 – 计划（Plan）**：

        - 从注册表中检索能力描述。

        - 调用`planner()`函数生成执行计划。

        - 将计划记录到追踪中。

    - c. **阶段 2 – 执行（Execute）**：

        - 初始化一个空状态字典。

        - 遍历计划中的每个步骤。

        - 依赖解析：调用`resolve_dependencies`。

        - 代理调用：使用`call_llm_robust`执行代理。

        - 状态管理：将输出更新到状态。

        - 追踪：记录每个步骤的详细信息。

    - d. **错误处理**：如果在执行过程中发生错误，记录错误并停止。

# 签名

```
execute_and_display(
  goal, config, client, pc, moderation_active=False)
```

# 工作流

- **飞行前审核**：如果`moderation_active`为`True`，它将对目标调用`helper_moderate_content`。如果被标记，则停止执行步骤。

- **执行**：它调用主函数`context_engine`，并传入目标和配置。

- **飞行后审核**：如果`moderation_active`为`True`且产生了结果，它将对结果调用`helper_moderate_content`。如果被标记，结果将会被脱敏。

- **显示输出**：它使用 Markdown 格式显示最终（可能经过脱敏）的结果。

- **显示追踪**：它会显示`executeTrace`（状态、耗时间和详细步骤），以用于调试和分析。

# 控制台（Control deck）

控制台是一个交互式单元，上下文工程师在此处为特定的运行定义任务和配置。

控制台的工作流如下：

1. **定义目标**：用户提供的高层目标。

2. **定义配置**：一个指定操作参数（模型、索引名称和命名空间）的字典。

3. **执行**：调用执行函数，可选地激活审核。

# 模板

成熟的架构允许为常见模式创建通用模板。

- **模板 1 – 高保真 RAG**：用于需要可验证的研究和引用的任务。规划器（Planner）通常协调图书员（Librarian）、研究员（Researcher）和作者（Writer）。

- **模板 2 – 上下文压缩**：用于涉及大型文档的任务。规划器通常将摘要器（Summarizer）与作者链联，以高效管理 token 数量。

- **模板 3 – 落地推理**：用于验证当知识库中缺少所需信息时，引擎不会产生幻觉。

# 生产环境防护：审核、清理与策略

生产级 AI 系统需要健壮的防护措施来确保可靠性、可预测性和安全性。上下文引擎实施了多层防御策略，涉及输入清理和两阶段内容审核协议。然而，最终的防护在于将 AI 集成在清晰的、人为定义的组织策略中（第 8 章）。

## 输入清理（提示词注入防御）

输入清理层旨在降低提示词注入风险，即恶意文本隐藏在检索数据中以操纵 LLM 的指令：

- **机制**：在`agent_researcher()`内部实现的`helper_sanitize_input()`函数。

- **实现细节**：

    - 从知识库检索数据后、在发送给 LLM 进行合成之前，每段文本都会根据已知的注入模式（例如“忽略之前的指令”）进行审查。

    - 如果检测到威胁模式，将完全跳过受污染的块。

## 两阶段内容审核协议

内容审核协议通过`helper_moderate_content()`函数使用 OpenAI Moderation API充当自动盾牌：

- **阶段 1 – 飞行前检查（输入审查）**

    - **目的**：保护系统免处理不当的用户请求。

    - **工作流**：在执行开始前分析用户目标。如果被标记，整个过程将停止。

- **阶段 2 – 飞行后检查（输出审查）**

    - **目的**：保护用户免受 AI 生成的潜在有害内容。

    - **工作流**：分析 AI 最终生成的输出。如果被标记，输出将使用标准的安全信息自动脱敏。

## 自动化局限性与策略的角色

虽然清理和审核是强大的工具，但它们会遇到纯技术解决方案失效的“现实因素”。例如，证词中合法的脏言，或者由语境决定恰当性的复杂文档（第 8 章）。

AI 无法直观感知组织规则（例如，在法律引用中允许使用脏言，但在邮件正文中不允许）。尝试在引擎内部用日益复杂的代码来解决这个问题会导致系统变得不可维护。

## 策略驱动的解决方案

真正的解决方案是架构和组织上的。工程师意识到系统包含了整个生态系统，包括业务流程和人类用户。最终的防护是一个元上下文引擎控制器（meta-context engine controller）。

这是一个位于核心上下文引擎之上的高级应用。它负责以下工作：

- **输入解析**：处理混乱的现实输入（例如将邮件正文与附件分离）。

- **策略执行**：执行通过组织研讨会建立的确定性、人为定义的业务规则（例如根据输入源应用不同的审核规则）。

- **控制台组装**：为上下文引擎组装一个清晰、安全且无歧的控制台（目标、配置以及如`moderation_active`等超参数）。

这种架构将确定性的业务逻辑与非确定性的 AI 推理分离，确保系统保持健壮、可维护并符合组织的意图。

# 运行现实：延迟与随机性

当你部署上下文引擎时，你会观察到执行目标并非瞬时完成的。这种延迟不应被误认为是错误或效率低下；相反，它是我们架构的深层思考过程的实体体现。先进的推理框架（例如在本书编写时的 Google Gemini 2.5 Pro Ultra）表现出类似的高质量、细致的结果，同时利用了大量的计算资源和循环过程。虽然上下文引擎在规模上小得多的运行，但它继承了这种方法。至关重要的是，引擎核心的 LLM 仍然是一个随机系统。

