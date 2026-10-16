# 5 加固上下文引擎

在第 4 章中，我们构建了上下文引擎的原型。然而，为了从实验转向生产级系统，我们必须加固引擎。本章侧重于使系统变得健壮、安全且可扩展所需的升级。

转换跨越三个阶段展开：
1. **初始化（Initiation）**：每个操作都从用户操作开始。
2. **规划（Planning）**：引擎创建策略。
3. **执行（Execution）**：专家代理执行任务。

我们从代码的逐步讲解开始。

### 第一阶段：初始化

每个操作都从用户操作开始，即目标被提交且引擎启动的时刻。

- `logging.info`：第一个动作是记录过程的开始。这是任何生产导向系统中跟踪任务的简单但至关重要的一步。

### 第二阶段：规划阶段

一旦进入 `context_engine()`，首要任务就是创建策略。引擎并非盲目行动；它收集关于拥有哪些工具、用户想要以及如何桥接两者的信息。然后它起草了一份下游组件可以遵循的详细执行计划。

- `ExecutionTrace.__init__()`：引擎立即初始化一个 `ExecutionTrace` 对象。该对象充当引擎的“飞行记录器”，准备记录后续每个操作以供透明度和调试。
- 5.3 `AgentRegistry.get_capabilities_description()` (紫色)：为了创建一个有效的计划，规划器（Planner）需要知道它拥有哪些可用工具。它查询 `AGENT_TOOLKIT` 对象以获取所有专家代理及其所需输入的纯文本格式描述。
- 6.2 `planner()` (白色)：在获取用户目标和代理能力列表后，引擎调用 `planner()` 函数。
- 3.1 `call_llm_robust()` (橙色)：`planner()` 将推理任务委托给外部 LLM。LLM 在一个结构严谨的提示词（prompt）中理解目标和能力，并要求 LLM 返回一个具有策略性的、多步骤的 JSON 计划。
- 6.1b `ExecutionTrace.log_plan()` (白色)：一旦从 LLM 接收到 JSON 计划，它就会记录在我们的 `ExecutionTrace` 对象中。这完成了规划规划阶段。

## 第 3 阶段：执行循环

这是上下文引擎（Context Engine）展现活力的地方。有了计划后，系统开始按顺序执行每一步（这里的步骤指的是上下文引擎动态流中实际的、编号的步骤）。执行器（Executor）获取任务所需的正确流程，准备其输入并跟踪每一个输出，形成一个包含规划、执行和验证的持续反馈循环。

循环从执行器从计划中读取后续步骤开始：

- 5.2 `AgentRegistry.get_handler()` (紫色)：计划通过名称指定了一个代理（例如 "Librarian"/管理员）。执行器使用 `get_handler()` 从注册表中检索实际的、可调用的 Python 函数（例如 `agent_context_librarian`）。
- 6.3a `resolve_dependencies()` (白色)：这是上下文链式（context chaining）的核心。执行器检查代理所需的输入。如果它发现像 `$$STEP_1_OUTPUT$$` 这样的占位符，该函数将将其替换为步骤 1 产生的实际数据（上下文引擎动态流的第一步），这些数据存储在引擎的状态字典中。
- 4.x `agent-*` (绿色)：执行器现在调用检索到的代理函数，并向其传递完全解析的上下文。

在每个代理内部，专门的逻辑驱动着系统的解决问题行为。代理通常依赖辅助函数来访问外部资源并执行繁重工作：

- 管理员（Librarian）和研究员（Researcher）代理调用 `3.4 query_pinecone()` (橙色) 在向量数据库上执行语义搜索。该函数反过来使用 `3.2 get_embedding()` (橙色) 将查询文本转换为向量。
- 研究员（Researcher）和作者（Writer）代理调用 `3.1 call_llm_robust()` (橙色) 来合成事实或生成最终内容。
- 6.1c `ExecutionTrace.log_step()` (白色)：一旦代理完成工作并返回输出，就会调用 `log_step()` 方法来记录有关完成步骤的所有信息：使用的代理、输入以及其最终输出。输出也会保存到引擎的状态中，以供后续步骤使用。

循环进入计划中的下一步，重复检索代理、准备其输入、执行步骤和记录结果的整个序列。

## 第 4 阶段：最终完成

最后一个阶段闭合了循环。在所有步骤都成功执行后，上下文引擎通过记录其最终状态并返回结果和追踪记录（trace）来结束其任务。这一阶段发生在平稳完成之后，交付一个清晰的结果以及关于实现该结果的透明记录。

- 6.1d `ExecutionTrace.finalize()` (白色)：引擎在追踪上执行 `finalize()`，记录最终状态（“Success”/成功）和总执行时间。
- 7 `context_engine()` 然后将最终输出（最后一步的结果）和完整的追踪对象返回给用户的脚本。
- 7.0b `logging.info` & 7.1/7.2 `display(Markdown(...))` (蓝色)：脚本记录任务已完成，并向用户显示最终的格式化输出，成功实现了原始目标。

前述流程图中的每个颜色块都代表一个具有单一职责的定义良好的组件。它们共同构成了一个整洁的、分层的架构，这是可扩展多代理系统的重要要素特征：

- 引擎核心（白色）是大脑。它并不孤立工作，而是管理整个过程。它规划策略（`planner`）、按步骤执行计划，并记录一切（`ExecutionTrace`）。它的主函数 `context_engine()` 是主编排器。
- 代理注册表（Agent Registry，紫色）是工具箱。它为两个主体服务：它为规划器提供一份描述性手册（`get_capabilities_description`）以便其思考，为执行器提供实际工具（`get_handler`）以便其行动。
- 专家代理（绿色）是工作者。每个都有一个特定的任务：管理员寻找风格指令，研究员寻找事实，作者负责创建内容。它们是操作的执行手。
- 辅助函数（橙色）是实用程序。这些是共享的低级函数，任何其他组件都可以使用。它们处理重复但至关重要的任务，如与 LLM 通信（`call_llm_robust`）和与向量数据库通信（`query_pinecone`），避免了重复并集中了关键交互。

通过分析这些，我们可以看到系统如何通过逻辑化、可预测且可追溯的简单操作序列来实现行为。现在我们已经详细探索了蓝图，准备好在代码中构建每个组件了。

## 为了在现实世界中的可扩展性进行重构

我们将原型转换为生产级应用程序的旅程从重构开始。在第 4 章中，我们的笔记本包含几个专门安装、初始化客户端的单元格。虽然这种方法可行，但它有两个缺点：它弄乱了我们的主工作区，并且迫使我们在每个笔记本中重复相同的代码。为了解决这个问题，我们现在将这些逻辑移动到 `utils.py` 中。

通过重构，我们将确保每个组件都从一致且可靠的环境开始。这不仅清理了我们的笔记本，还让我们能够专注于高层编排。我们正在从笔记本为中心的原型转向模块化系统。

## 在 `utils.py` 中创建函数

第一步是在 `utils.py` 中创建两个函数：一个处理依赖，另一个用于初始化客户端。

- `install_dependencies()` 函数充当项目的“一键安装器”。通过将所有的 pip 命令封装在这个函数中，我们保证每个笔记本都使用相同版本的库。这对于确保可重复性至关重要。
- `initialize_clients()` 函数是我们的密钥管理员。它集中了从 Colab Secrets 获取 `API_KEY` 和 `PINECONE_API_KEY` 的整个过程，并使用它们创建并配置 OpenAI 和 Pinecone 客户端。

这些函数共同为我们提供了可重用且安全的设置，使得项目的每个组件都准备就绪。

在本节中，我们将升级这些核心函数，使其更加可靠、可测试且透明。架构上的关键转变是实现了**依赖注入**。我们不再让函数去寻找全局变量，而是现在显式地将所有所需的组件（如客户端和配置）作为参数传递。这使得它们从简单的连接器转变为健壮的、自包含模块，能够优雅地处理故障、提供清晰的日志并高效地管理资源。让我们开始吧！

## 通过依赖注入增强模块化

我们助手函数最显著的升级是弃用全局变量。在我们的原型中，像 `call_llm_robust` 等函数隐式地知道应该使用哪个 client 和 `GENERATION_MODEL`，因为它们被定义在全局作用范围内。这对于原型来说很方便，但对于实际应用程序可能是危险的，因为它创建了隐藏依赖，并使得代码难以测试或重新配置。

我们的升级引入了依赖注入，这是一种函数的依赖项作为参数“注入”的做法。这使得每个函数成为一个自包含的单元。客户端或模型名称来自何处不再重要，只要在调用函数时提供它们即可。这使得我们的引擎变得灵活得多。例如，我们可以通过传递不同的 client 对象，同时使用两个不同的 LLM 运行两个不同的引擎任务。

## 通过生产级日志提高透明度

在原型中，我们使用 `print()` 语句来查看发生了什么。对于生产系统来说，这是不够的。专业系统需要正式的日志：一种结构化的、带有时间戳且可靠的事件记录。

我们最后的增强是将所有的 `print()` 语句替换为 Python 内置的 `logging` 模块。这是一个根本性的转变，提升了我们系统的透明度。适当的日志允许我们区分 INFO 信息、潜在的警告和严重错误。这种结构化的输出不仅便于人类阅读，而且是机器可读的，允许日志馈到自动化监控和警报系统中。

```python
# === 配置生产级日志 ===
logging.basicConfig(level=logging.INFO,
                    format='%(asctime)s - %(levelname)s - %(message)s')
```

## 通过主动上下文管理提高效率

构建 LLM 应用的关键方面是管理 token 的“隐形成成本”。我们发送给模型的每个上下文都会消耗 token，这会影响成本和速度，并在超出上下文窗口时导致硬性故障。专业系统不能肆意地使用这些资源。

为了解决这个问题，我们引入了 `count_tokens` 工具函数。该函数像一个“燃料表”，允许引擎在发送提示之前衡量成本。它使用 OpenAI 官方的 `tiktoken` 库来确保准确性。现在构建它，为以后高级功能奠定了基础，例如自动总结上下文以保持在 token 预算内。

```python
# === 上下文管理工具（新增） ===
def count_tokens(text, model="gpt-4"):
    """为给定模型计算字符串中的 token 数量"""
    try:
        encoding = tiktoken.encoding_for_model(model)
    except KeyError:
        # 为可能不在 tiktoken 注册表中的模型回退
        encoding = tiktoken.get_encoding("cl100k_base")
    return len(encoding.encode(text))
```

这个简单的工具函数是让引擎不仅强大，而且具有成本意识且高效的关键一步。

## 升级后的助手函数演示

这是我们核心助手函数的最终加固代码。注意每个签名如何包含了参数如 `client`、`generation_model` 和 `embedding_model`。每条语句都已替换为日志调用，且错误处理更加具体。

```python
# === LLM 交互（使用依赖注入加固） ===
```



`@retry(wait=random_exponential(min=1, max=60), stop=stop_after_attempt(6))`

```python
def call_llm_robust(
    system_prompt, user_prompt, client, generation_model,
    json_model=False
):
```

处理所有带重试的 LLM 交互的集中函数。升级：现在要求传入 `client` 和 `generation_model` 对象。

```python
logging.info("尝试调用 LLM...")
try:
    response_format = {"type": "json_object"} if json_model else {"type": "text"}
    # 升级：使用传入的 client 和模型名称进行 API 调用。
    response = client.chat.completions.create(
        model=generation_model,
        response_format=response_format,
        messages=[
            {"role": "system", "content": system_prompt},
            {"role": "user", "content": user_prompt}
        ]
    )
    logging.info("LLM 调用成功.")
    return response.choices[0].message.content.strip()
except APIError as e:
    logging.error(f"call_llm_robust 中的 OpenAI API 错误: {e}")
    raise e
except Exception as e:
    logging.error(f"call_llm_robust 中发生意外错误: {e}")
    raise e
```

## 嵌入向量（使用依赖注入加固）

`@retry(wait=wait_random_exponential(min=1, max=60), stop=stop_after_attempt(6))`

```python
def get_embedding(text, client, embedding_model):
```

为单个文本查询生成带重试的嵌入。升级：现在要求传入 `client` 和 `embedding_model` 对象。

```python
text = text.replace("\n", " ")
try:
    response = client.embeddings.create(input=[text], model=embedding_model)
    return response.data[0].embedding
except APIError as e:
    logging.error(f"get_embedding 中的 OpenAI API 错误: {e}")
    raise e
except Exception as e:
    logging.error(f"get_embedding 中发生意外错误: {e}")
    raise e
```

将此代码移动到最终的 `agents.py` 暴露了一个微妙但重要的缺陷，以及我们如何修复它以创建最终的、生产就绪的版本。

正如我们将看到的，在最终版本执行第一版本时，agent 的输出包含一个关键缺陷。重要的是要意识到这类问题是开发 AI agent 过程中的自然部分，而系统地解决这些问题正是使系统具有韧性的关键。中间实现中的关键行显示了 agent 升级后的签名及其原始的、错误的返回语句：

```python
# === Context Librarian Agent (Upgraded) ===
# *** Added 'namespace_context' argument ***
def agent_context_librarian(
    mcp_message, client, index, embedding_model, namespace_context
):
    # ... (Logic for querying Pinecone) ...
    if results:
        #
        content = blueprint_json FLAW: content is a raw string
    #
    return create_mcp_message("Librarian", content)
```

我们实现了以下改进：

- 我们通过改进依赖注入，确保函数签名显式地要求其所有外部需求：`client`、`index`、`embedding_model` 和 `namespace_context`。这使得 agent 成为一个自包含的、可测试单元。
- 我们还规范了日志记录。所有的 `print` 语句都已替换为专业的日志调用。
- 我们开发了健壮的错误处理流程。整个逻辑被封装在 `try...except` 块中，以优雅地处理失败。

在此过程中，早期版本中的缺陷变得清晰可见了：它以原始 JSON 字符串的形式返回蓝图。

为了确保系统健壮，每个 agent 必须以一种预测的、结构化的方式返回数据。例如，Writer agent 必须确切知道在 Librarian 的输出中查找哪个键。修复方法是将输出封装在一个具有一致键 `"blueprint_json"` 的字典中。这在 agent 之间建立了一个稳定性的数据契约。

代码的最终修正版本现在位于 `agents.py` 中，反映了这一关键修复以及其他的专业升级。

## 最终加固代码

我们现在有了最终代码，并进行了拆分以显示每个升级的实现位置。首先，我们定义了加固后的函数签名。所有依赖现在都是显式参数，而不是全局变量，使 agent 成为一个自包含的单元：

```python
def agent_context_librarian(
    mcp_message, client, index, embedding_model, namespace_context
):
    """从 Context Library 中检索相应的 Semantic Blueprint."""
```

接下来，我们用专业的日志替换了旧的 `print()` 语句，并将整个函数封装在 `try...except` 块中以进行健壮的错误处理：

```python
logging.info("[Librarian] Activated. Analyzing intent...")
try:
    requested_intent = mcp_message['content'].get('intent_query')
    if not requested_intent:
        raise ValueError("Librarian requires 'intent_query' in the content")
except
```

对 `query_pinecone` 辅助函数的调用现在升级为使用显式的关键字参数以及注入 agent 的所有必要客户端和对象模型：

```python
results = query_pinecone(
    query_text=requested_intent,
    namespace=namespace_context,
    top_k=1,
    index=index,
    client=client,
    embedding_model=embedding_model
)
```

最后，我们实现了在验证阶段发现的最关键的修复。agent 的输出现在是一个具有一致键的字典，为其他 agent 创建了可靠的数据契约：

```python
match = results[0]
logging.info(
    f"[Librarian] Found blueprint '{match['id']}' (Score: {match['score']}")")
```

```python
2f))"
    blueprint_json = match['metadata']['blueprint_json']
    content = {"blueprint_json": blueprint_json}
else:
    logging.warning("[Librarian] No specific blueprint found. Returning default.")
    content = {"blueprint_json": json.dumps(
        {"instruction": "Generate the content neutrally."}
    )}
    )
return create_mcp_message("Librarian", content)
```

这个微小的变化是重构的精髓：它不仅仅是移动代码，而是为了随着系统的增长使其变得更具预测性且更容易维护。

## Researcher agent

与我们完善 Librarian 的方式相同，Researcher 遵循相同的增量改进模式，从笔记本加固到库集成。该过程始于 `Context_Engine_MAS_MCP.ipynb`，引入了依赖注入和结构化日志，并随后模块化到 `agents.py` 文件。

与 Librarian 类似，这个版本的版本暴露了一个微妙但关键的缺陷。在验证期间，我们发现 agent 返回的是原始字符串而不是结构化数据，这是一个很容易犯的错误，但它会影响整个工作流：

```python
# == 4.2. Researcher Agent (Upgraded) ===
def agent_researcher(
    mcp_message, client, index, generation_model, embedding_model,
    namespace_knowledge
):
    # (Logic for querying Pinecone and synthesizing ...)
    if not results:
        return create_mcp_message("Researcher", "No data found on the topic.")  # FLAW: returns a raw string
    # ...
    findings = call_llm_robust(...)
    return create_mcp_message("Researcher", findings)  # FLAW: returns a raw string
```

## 第 5 章 141

此阶段的升级在结构上与应用于 Librarian 的升级相似：

- 我们通过显式要求所有外部需求（`client`、`index`、`generation_model` 和 `namespace_knowledge`）加强了依赖注入
- 我们将 `print` 语句替换为专业的日志
- 我们将逻辑封装在 `try...except` 块中以进行健壮的错误处理

在此过程中，早期版本中的缺陷变得清晰可见：它以原始字符串的形式返回蓝图。

## 写入代理 (Writer agent)

写入代理完成了我们专家组合的最后成员，它的重构展示了系统的最终集成。与管理员（Librarian）和研究员（Researcher）一样，它最初在 `Context_Engine_MAS_MCP.ipynb` 中通过结构化日志、错误处理和依赖注入进行了加固。

这次升级揭示了关于集成系统最重要的教训：当你更改一个组件时，必须更新依赖于它的组件。记住，从设计到完美的道路没有捷径。因此，知道如何解决问题与开发功能同样重要！写入代理的初始加固版本与旧的管理员和研究员配合，但它包含一个关键缺陷，使其无法与其新的升级版本协同工作。

该中间实现中的关键行显示了代理在输入处理上的缺陷：

```python
# === 4.3. Writer Agent (Upgraded) ===
def agent_writer(mcp_message, client, generation_model):
    """将研究结果与蓝图结合以生成最终输出."""
    logging.info("[Writer] Activated. 正在将蓝图应用于源素材...")
    try:
        # Fix：从之前的步骤中解包结构化输入
        blueprint_data = mcp_message['content'].get('blueprint')
        facts_data = mcp_message['content'].get('facts')
        previous_content_data = mcp_message['content'].get(
            'previous_content'
        )
```

让我们回顾初始的升级：

- 函数签名经过加固，显式要求 `client` 和 `generation_model`，使其成为一个自包含的单元。
- 所有的打印语句都被替换为专业的日志调用。
- 整个逻辑封装在 `try...except` 块中，以优雅地处理失败。

让我们看看如何修复集成流。

写入代理处理输入的方式存在缺陷。它期望原始字符串，而不是包含 `"blueprint_json"` 和 `"facts"` 的结构化字典。为了修复这个问题，我们升级了写入代理以智能地解包这些结构。这不仅仅是一个错误修复；它是创建真正互连且稳健的多代理系统的最后一步，使数据能够可预测地从一个组件流到下一个。出现在 `agents.py` 中的最终修正版代码反映了这一关键的集成修复。

## 最终加固代码

这是最终代码，按每个升级实现的位置进行了拆分。首先，我们加固了下一个函数签名和初始日志设置：

```python
def agent_writer(mcp_message, client, generation_model):
    """将研究结果与蓝图结合以生成最终输出."""
    logging.info("[Writer] Activated. 正在将蓝图应用于源素材...")
    try:
```

接下来，我们实现了最关键的修复。这段代码智能地解包结构化输入，检查它们是字典（来自我们新的代理）还是原始字符串（为了向后兼容或其他用途），使代理变得健壮：

```python
blueprint_data = mcp_message['content'].get('blueprint')
facts_data = mcp_message['content'].get('facts')
previous_content_data = mcp_message['content'].get(
    'previous_content'
)
# 提取实际字符串，同时处理字典和原始字符串输入
blueprint_json_string = blueprint_data.get('blueprint_json') if isinstance(blueprint_data, dict) else blueprint_data
facts = facts_data.get('facts') if isinstance(facts_data, dict) else facts_data
previous_content = previous_content_data  # 假设这已经是字符串
```

```python
if not blueprint_json_string:
    raise ValueError("Writer 要求输入内容中包含 'blueprint'。")
```

代理的其余逻辑保持不变，但它现在在正确解包的 `facts` 和 `blueprint_json_string` 变量上运行：

```python
if facts:
    source_material = facts
    source_label = "RESEARCH FINDINGS"
elif previous_content:
    source_material = previous_content
    source_label = "PREVIOUS CONTENT (For Rewriting)"
else:
    raise ValueError('Writer 需要 "facts" 或 "previous_content"。')
# ... (系统提示和用户提示词的构建保持不变) ...
```

最后，代理对 LLM 进行了依赖注入调用并返回输出，现在确信已正确解释了来自前序代理的数据：

```python
final_output = call_llm_robust(
    system_prompt,
    user_prompt,
    client=client,
    generation_model=generation_model
)
return create_mcp_message("Writer", final_output)
except Exception as e:
    logging.error(f"[Writer] 发生错误: {e}")
    raise e
```




随着整个代理团队现在已升级到生产标准，我们可以继续重构管理它们的组件。

## 重构代理注册表 (Agent Registry)

我们将注意力转向`AgentRegistry`，它是作为我们引擎总工的组件，持有主剪事板。在第 4 章中，它作为一个简单的路由器，将代理名称映射到它们的函数。现在，我们将其升级为智能中心工具箱。

它的升级是加固组件的最后也是最关键的一步，因为它负责正确管理并向所有专门代理注入依赖项。与其他组件一样，我们首先在`Context_Engine_MAS_MCP.ipynb`笔记本中对其进行升级。初始升级重点在于增强`get_handler`方法，以管理新要求的依赖项（如`client`、`index`和`namespace`配置）。然而，与之前的重构一样，第一遍暴露了一个根本性的架构缺陷。笔记本版本直接引用代理函数，这种捷径只在单文件环境中有效：

```python
class AgentRegistry:

    def __init__(self):
```

## 加固上下文引擎 (Context Engine)

```python
self.registry = {
  # 为函数名添加 "agents." 前缀
  "Librarian": agents_context_librarian,
  "Researcher": agent_researcher,
  "Writer": agent_writer,
}
```

在此阶段，`get_handler`方法已经显著改进。它接受所有关键依赖（`client`、`index`、`generation_model`等），并使用 lambda 函数为每个代理生成处理器。但一旦将注册表迁移到`.py`文件，出现了一个熟悉的问题。正如我们模块化代理一样，原始代码假设函数存在于全局命名空间中，这导致重构后的模块出现`NameError`。解决方案（类似于管理员和研究员的修复）是让注册表自包含。它需要显式导入代理，例如`agents.context_librarian`。这个微小但重要的更改确保了组件确切依赖的位置，建立了完全模块化的架构。

`registry.py`中的最终修正版代码反映了这一改进。

## 最终加固代码

这是最终代码，按每个升级实现的位置进行了拆分。首先，注册表正确导入了依赖：

```python
import logging
import agents
```

接下来，在`__init__`方法内部，它使用`agents.`前缀引用函数，解决了模块化问题：

```python
class AgentRegistry:

  def __init__(self):
    self.registry = {
      # 为函数名添加 "agents." 前缀
      "Librarian": agents.agent_context_librarian,
      "Researcher": agents.agent_researcher,
      "Writer": agents.agent_writer,
    }
```

`get_handler`方法保持我们在笔记本中构建的强大注入器，但它现在在正确导入的代理函数上运行：

```python
def get_handler(
    self, agent_name, client, index, generation_model,
    embedding_model, namespace_context, namespace_knowledge
):
    handler_func = self.registry.get(agent_name)
    if not handler_func:
        logging.error(f"在注册表中未找到代理 {agent_name}")
        raise ValueError(f"在注册表中未找到代理 {agent_name}")
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
    else:
        return handler_func
```

## 升级中心编排器

随着代理（agents）和注册表（registry）的强化和模块化，我们将转向架构的核心：上下文引擎（Context Engine）本身。作为系统的中央“大脑”，它将所有组件整合成一个连贯的思考机器。

在第 4 章中，我们构建了驱动此编排循环的核心元素。在本节中，我们将升级其中的每一个元素，以满足我们新的生产就绪标准。上下文引擎作为整个应用的中央神经系统，包含了三个关键组件：`ExecutionTrace`类（追踪器）、planner 函数以及`context_engine`编排器。

从`Context_Engine_MAS_MCP.ipynb`迁移到特定的`engine.py`标志着我们向模块化生产系统转型的最后也是最重要的一步。虽然`ExecutionTrace`等单个类在笔记本中已经过加固，但它们仍然保持单体结构是一个关键缺陷。为了让引擎具有可重性和维护性，其逻辑必须与笔记本环境完全解耦。

正如我们在其他组件中看到的那样，从单笔记本原型转向模块化代码会暴露隐式依赖。上下文引擎的笔记本版本假设所有支持模块（代理、助手和注册表）都于相同的作用域内。一旦我们将引擎移动到自己的文件中，该假设就失效了：

```python
class ExecutionTrace:
    # ... (Tracer Logic 已经健壮) ...

def planner(goal, capabilities):
    # ...

def context_engine(goal, ...):
    # 缺陷：此函数直接调用 planner 等其他函数
    # 并使用 AGENT_TOOLKIT，假设它们是全局可用的。
```

遵循应用于代理和注册表的相同流程，引擎现在已移动到它自己的文件`engine.py`中。这种模块化不仅修复了架构缺陷，还将上下文引擎建立为一个自包含的组件，它显式地导入其所依赖的所有内容。

# 5 

`engine.py`文件现在导入了助手函数和注册表（注册表本身导入了代理）。
结果是一个独立的引擎，它可以被导入到任何笔记本中或直接集成到更大的应用程序中，实现了我们构建真正的生产级、可重用系统的目标。

接下来所示的最终修正版代码代表了一个完整的、端到端的编排层，它将所有组件整合在一起。

## 最终加固后的代码

这里是最终代码，经过分解以显示其关键组件。首先，我们有了模块化的导入，它们修复了单体笔记本的核心架构缺陷。

```python
import logging
import time
import json
import copy
from helpers import call_llm_robust, create_mcp_message
from registry import AGENT_TOOLKIT
```

接下来是`ExecutionTrace`类。它已经非常健壮，因此直接移动到模块中，无需任何更改。

```python
class ExecutionTrace:
    """记录整个执行流以供调试和分析"""
    # ... (提供了完整的类代码) ...
```

planner 也被移动到了模块中。它作为战略核心，使用 LLM 生成分步骤的计划。

```python
def planner(goal, capabilities, client, generation_model):
    """分析目标并使用 LLM 生成结构化的执行计划（Execution Plan）。"""
    # ... (提供了完整的 planner 逻辑) ...
```

最后，`context_engine`函数充当主编排器或执行器。它初始化追踪器、规划器，并遍历计划的每一步，使用`AGENT_TOOLKIT`获取正确的代理处理器并执行任务。这是赋予整个计划生命力的引擎核心。

```python
def context_engine(
    goal, client, pc, index_name, generation_model, embedding_model,
):
    """上下文引擎的主入口。"""
    # (提供了完整的执行循环逻辑) ...
```

随着引擎的大脑现在已完全升级，整个框架已经加固并准备就绪。本章的最后一步是通过实际运行来测试我们新重构的系统。

## 运行加固后的引擎

我们成功地对上下文引擎的每个组件进行了重构和加固。我们提升了代理的可靠性，模块化了依赖注入，创建了智能代理注册表（Agent Registry），并将核心逻辑整合到`context_engine`编排器中。现在，真理时刻已经到来。是时候初始化整个系统并观察它了。

在本节中，我们将运行完全组装的引擎。我们不仅是在测试它是否可行；我们是在证明我们的架构升级带来了一个透明、动态且多步推理的系统。

## 可视化追踪

我们的`engine.py`模块现在已经完成。然而，还需要向笔记本环境中添加最后一个实用工具。日志提供了详细的审计记录，但引擎行为的快速视觉化总结对于分析分析是有无价的。

为了实现这一点，我们将保持`engine.py`库整洁，并在笔记本内部定义一个名为`execute_and_display`的专用演示函数。该函数作为我们的“引擎室”，处理整个过程：调用引擎，接收最终结果和详细追踪对象，并对两者进行格式化以实现整洁的人类可读显示。这种方法将核心引擎逻辑与面向用户的输出严格分离。

它将集成到最后的生产前笔记本章节。现在让我们运行标准执行。

## 标准工作流执行

我们的第一个测试使用了一个简单、熟悉的目标，与第 4 章的示例类似。我们将要求引擎写一个悬疑场景，这自然会触发经典的“图书管理员（Librarian）→ 研究员（Researcher）→ 作者（Writer）”工作流。

此测试的目标是确认重构后的引擎能够处理多步工作流。我们将在单元格中定义目标：

```python
goal = "写一个悬疑场景，这自然会触发经典的 图书管理员 → 研究员 → 作者工作流。"
execute_and_display(goal, client, pc)
```

输出提供了海明风格的响应：

```
--- OUTPUT ---
这里是阿波罗11号的快速简报
- 首次载人登月，往返约8天。从肯尼迪航天中心（LC-39A）发射。
- 日期：1969年7月16日发射；7月20日02:17:40 UTC 着陆；7月21日02:56:15 UTC 第一步；7月24日溅水。
- 船员：尼尔·阿姆斯特朗（指挥官），巴兹·奥尔德（月球舱）
```

然后我们得到追踪：

```
--- TRACE (for the reader) ---
Trace Status: Success
Total Duration: 149.33 seconds
...
```

## 复杂工作流执行

我们的第二个测试将引擎推向简单的线性工作流。这次，我们将赋予它更复杂的目标：首先写阿波罗11号登陆的事实总结；然后用简约、富有冲击力的海明风格重写。

这种场景迫使两个代理链在一起，并将第一个的输出作为第二个的输入。

```python
goal_2 = "首先，写阿波罗11号登陆的事实总结。然后，用简约、富有冲击力的海明风格重写。"
execute_and_display(goal_2, client, pc)
```

输出显示：

```
--- OUTPUT ---
月球是灰色而寂静的。人们进入舱。尘土飞扬了。阿姆斯特朗走了出去。这是小小的一步。世界是寒冷而遥远的。
```


## 第5章

```
[Engine: Planner] 正在分析目标并生成执行计划...
[Engine: Planner] 计划生成成功。
```

此初始区块确认引擎已接收目标。规划器（Planner）立即激活，分析请求，调用代理注册表（Agent Registry）获取可用工具，并制定了一个实现目标的三个步骤的JSON计划。这是引擎的思考阶段。

## 执行步骤 1 – 图书管理员 (The Librarian)

```
[Engine: Executor] 正在启动第1步：图书管理员
[Librarian] 已激活。正在分析意图...
[Librarian] 找到蓝图 'blueprint_suspense_narrative' (得分: 0.66)
[Engine: Executor] 第1步完成。
```

执行器（Executor）现在开始根据计划行动。它带有输入意图`intent_query: "suspful children's story"`激活图书管理员代理。管理员随后将其转换为向量，并在 Pinecone 的 Contexti 库命名空间中搜索。它找到了一个匹配的蓝图`blueprint_suspense_narrative`，其中包含了如何以悬疑语调写作的指令（例如：使用短句、关注感官细节、营造紧张气氛）。

## 执行步骤 2 – 研究员 (The Researcher)

```
[Engine: Executor] 正在启动第2步：研究员
[Researcher] 已激活。正在调查主题...
[Researcher] 找到2个相关片段。正在综合中...
[Engine: Executor] 第2步完成。
```

接下来，执行器带有输入主题查询`topic_query: "Apollo 11 moon landing danger"`激活研究员。研究员搜索 KnowledgeStore 命名空间并检索出两个与阿波罗11号任务相关的事实片段。然后，它使用 LLM 将这些事实合成一个简洁的摘要，作为其输出。

## 执行步骤 3 – 作家 (The Writer)

```
[Engine: Executor] 正在启动第3步：作家
[Engine: Executor] 已解析依赖项 STEP_1_OUTPUT。
[Engine: Executor] 已解析依赖项 STEP_2_OUTPUT。

[Writer] 已激活。正在将蓝图应用于源材料...
[Engine: Executor] 第3步完成。
```

这是最后也是最关键的一步。执行器首先执行上下文链（context chaining）。它用来自管理员的悬疑蓝图解析`$$STEP_1_OUTPUT$$`占位符，并用研究员的事实摘要解析`$$STEP_2_OUTPUT$$`占位符。随后激活作家代理，接收风格指令和事实内容作为输入。它将两者结合以生成最终文本。

## 最终化

随着最后一步完成，引擎任务成功结束。

```
=== [Context Engine] 任务完成 ===
```

最终输出是三个代理工作流的直接产物。它是请求风格与经过验证的作品的合成，完美和谐：

> ******** 最终输出 1 ********

> 1969年7月20日。阿波罗11号。NASA阿波罗计划。太空竞赛在背景中嗡嗡，宛如远处的风暴。

> 我们有三个人。尼尔·阿姆斯特朗。巴兹·奥尔德林。迈克尔·柯林斯。

> 月球填满了窗户。黑色的天空。硬光。长影。

> 嗡嗡声升起，又落下。鹰号稳住了。场景收缩到针尖大小。

> 接着是寂静。

> 一只靴子悬空着。我先看着影子。在我之前，它先吻了表面。

> 接着是接触。第一步。在月球上。

> ...

> 这一刻拉得很长，像一个音符，没有断裂。

让我们分析一下输出：

- **图书管理员的影响（风格）：** 输出完美匹配了`blueprint_suspense_narrative`。句子简短、陈述性且具有冲击力（“舱内很小。”“燃料不足。”）。它专注于感官细节（“耳边轻微的嘶声”、“脚下的震抖”）并使用了唤发情感、营造紧张感的语言（“场景收缩到针尖大小”、“瞬间拉得很长……没有断裂”）。这是蓝图风格指导的体现。

- **研究员的影响（事实）：** 故事基于研究员检索的事实信息。它正确地提到了日期（1969年7月20日）、任务名称（阿波罗11号）、机组人员（阿姆斯特朗、奥尔德林、柯林斯）以及登月舱的名称（鹰号）。

此输出清楚地证明了强化后的上下文引擎正按预期工作。成功成功规划了一个动态工作流，协调了多个代理，并通过上下文链产生了一个连贯的结果。我们已经正式从一个机明的原型进化为一个健壮、有韧性且透明的系统。下一步是让它的使用变得真正无缝。

## 引擎模块化 (Modularizing the engine)




随着上下文引擎的每个组件现在已通过强化，我们可以迈出原型到生产前旅程中最后也是最重要的一步：为了模块化进行重构。在此阶段，我们将通过将核心逻辑移动到单独的 Python 库，将单体笔记本转换为整洁、可扩展的应用程序。

这个过程不仅仅是简单的复制粘贴。这是一次实用的调试之旅，揭示了 Python 模块如何通信、管理依赖以及交换数据。我们将从一个干净的笔记本 `Context_Engine_Pre_Production.ipynb` 开始，它作为我们的控制台，并将强化后的代码组织到专门的 `commons/library` 中。

我们的目标是将 `Context_Engine_MAS_CP.ipynb` 笔记本中第 3、4、5、6 部分的代码分离到自己的 `.py` 文件中：`helpers.py`、`agents.py`、`registry.py` 和 `engine.py`，它们将位于新的 `commons/` 目录下。重构标志着。

## 本地导入 (Local imports)

我们面临的第一个挑战是 `commons/` 的导入语句将失败。

```python
# 这对于库是有效的
import helpers
import agents
from engine import Engine
```

解决方案是显式地连接模块。我们编辑 `registry.py` 以匹配。

```python
# 告诉这个文件
import agents
import logging
class AgentRegistry:
    def __init__(self):
        self.registry = {
            # 指定找到每个
            "Librarian": agents.agent_librarian,
            "Researcher": agents.agent_researcher,
            "Writer": agents.agent_writer,
        }
        # ... 类的其余
```

这种识别依赖项的过程持续到。`engine.py` 需要导入我们的模块，而 `agents.py` 需要从 `helpers.py` 导入函数。

## 模块独立 (Module independence)

运行新的单元格立即一个孤岛。模块对……无所知。

## 缺失代理 (Missing agents)

导入 `registry.py` 时发生第一个错误：

```
NameError: name 'agent_context_librarian' is not defined
```

## 数据结构不匹配 (Mismatch of data structures)

在解决所有导入错误后，引擎在最后一步（例如 `{'blueprint_json': '...'}`）之间期望源材料是原始字符串。作家的最终输出不是故事，而是错误。

修复方法是让作家代理更智能。我们修改它以检查输入（蓝图和事实）是否为字典。如果是，它将从正确的键提取字符串值。这确保了无论内部数据结构如何，代理都能获取其所需的文本。

`agents.py` 中修改后的 `agent_writer` 代码如下：

```python
# 修复：从之前的步骤解包结构化输入
blueprint_data = mcp_message['content'].get('blueprint')
facts_data = mcp_message['content'].get('facts')
previous_content = mcp_message['content'].get('previous_content')

# 提取实际字符串，处理字典和原始输入
blueprint_json_string = blueprint_data.get('blueprint_json')
    if isinstance(blueprint_data, dict)
        else blueprint_data
facts = facts_data.get('facts')
    if isinstance(facts_data, dict)
        else facts_data

previous_content = previous_content_data # 假设如果提供的则已经是字符串
```

## 最终生产前笔记本 (The final pre-production notebook)

我们最后的笔记本 `Context_Engine_Pre_Production.ipynb` 现在非常整洁，作为我们强大引擎的高级“控制台”。它包含两个主要部分。

## 中心化执行函数（引擎机房）

第一部分定义了一个单一函数 `execute_and_display()`，它封装了运行引擎和展示结果的所有逻辑。它接收用户目标、一个配置字典以及初始化的客户端，然后为用户打印一份精化的最终输出，并为开发者打印一份详细的技术轨迹追踪（technical trace）。

```python
# === 引擎机房：主执行函数 ===
# 此函数包含了运行引擎的所有逻辑。
# 我们在这里定义它，是为了让最后的单元格代码保持非常简单。

import logging
import pprint
from IPython.display import display, Markdown

def execute_and_display(goal, config, client, pc):
    """
    使用给定的目标和配置运行上下文引擎，
    然后显示最终输出和技术轨迹追踪。
    """
    logging.info(f"******* Starting Engine for Goal: '{goal}' ***********\n")

    # 1. 使用提供的配置运行上下文引擎
    result, trace = context_engine(
        client=client,
        pc=pc,
        **config # 将配置字典解包为关键字参数
    )

    # 2. 为主读者显示最终结果
    print("--- FINAL OUTPUT ---")
    if result:
        display(Markdown(result))
    else:
        print(f"The engine failed to produce a result. Status: {trace.status}")

    # 3. 为开发者/技术读者显示技术轨迹追踪
    print(f"\n\n--- TECHNICAL TRACE (for the tech reader) ---")
    if trace:
        print(f"Trace Status: {trace.status}")
```



```python
print(f"Total Duration: {trace.duration:.2f} seconds")
print(f"Execution Steps:")
# 使用 pprint 处理整洁、可读的字典
pp = pprint.PrettyPrinter(indent=2)
pp.pprint(trace.steps)
```

现在有了中心化的执行流程。让我们探索用户交互。

## 用户交互（控制台）

最后的调用是用户唯一需要交互的接口。在这里，我们定义了高层目标和一个包含运行所需所有技术参数的配置字典（例如模型名称和 Pinecone 命名空间）。这种关注点的清晰分离——将逻辑放在引擎机房，将用户输入放在控制台——是良好设计应用的标志。

配置位于用户目标上方的单元格中：

```python
# 1. 在字典中定义本次运行的所有配置变量
config = {
  "index_name": "genai-mas-mcp-ch3",
  "generation_model": "gpt-5",
  "embedding_model": "text-embedding-3-small",
  "namespace_context": "ContextLibrary",
  "namespace_knowledge": "KnowledgeStore"
}
```

控制台现在已经过优化：

```python
# 示例 1
# 定义高层目标
goal = ["YOUR GOAL"]
# 调用上方单元格中的执行函数
execute_and_display(goal, config, client, pc)
```

系统生成的输出结构与“运行加固引擎”部分相同。我们已经构建了一个稳健的生产前环境上下文引擎。让我们总结一下这段旅程并继续。

## 总结

在本章中，我们将上下文引擎从一个功能原型提升到了生产级系统。通过重构和重建其架构，我们执行了专业的工程原则，使引擎变得健壮、透明、可靠，并成为开发韧性多智能体系统的实用蓝图。

影响最大的变化是引入了依赖注入，它消除了对全局变量的依赖，使得每个组件都是自包含的，并显式地感知其依赖项。这种解耦设计极大提高了引擎的灵活性、可测试性和可维护性。

我们还通过添加结构化日志和精确的错误处理，加固了从辅助函数到智能体的每一个子系统。这些升级创建了完全审计的工作流。最后，我们展示了如何将单体笔记本重构为模块化应用。

结果是一个架构良好的上下文引擎，用户界面与核心逻辑之间实现了清晰分离。在下一章中，我们将开始将上下文引擎应用于实际问题。

## 问题

- 1. 本章的主要目标是从零开始构建上下文引擎吗？

- 2. 在规划阶段，`planner()` 函数是否直接调用智能体来完成工作？

- 3. `resolve_dependencies()` 函数的主要作用是通过替换占位符（如 `STEP_1_OUTPUT`）来处理依赖链吗？

- 4. 智能体注册表（Agent Registry）被描述为管理整个过程的大脑吗？

- 5. 在重构期间创建 `utils.py` 文件是为了处理引擎的高层规划逻辑吗？

- 6. 第 4 章原型中主要的日志方法是使用 Python 内置的 `logging` 模块吗？



- 7. 在重构管理员（Librarian）和研究员（Researcher）智能体时，是否发现它们返回的是字符串而非结构化字典？

- 8. 将代码移动到单独的 `.py` 文件后，注册 `.py` 文件是否因为找不到智能体函数而崩溃？

- 9. 复杂工作流测试（重写后的）是否证实了引擎总是遵循“管理员 -> 作者”的顺序？

## 参考文献

- Qian, C., Wu, Y., & Ahmed, J. (2024). LLM-Multi-Agent Systems for Software Engineering: Literature Review and the Road Ahead. arXiv preprint:2404.08334

- Zhang, W., Wang, Z., Li, Y. (2025). Knowledge-Based Multi-Agent Framework for Architecture Design. arXiv preprint:2503.02536

- Zhang, Z., Chen, Z., Sun, S., ... & Liu, B. (2025). AgentOrchestra: Hierarchical Multi-Agent Framework for General-Purpose Task Solving. arXiv preprint:2506.12508

- Cito, J., Kassab, M., & Parnin, C. (2021). Cataloging Dependency Injection-Patterns in Software Systems. arXiv preprint:2109.04256

## 延伸阅读

Wooldridge, M. (2009). An Introduction to Multiagent Systems (2nd ed.). John Wiley & Sons.

