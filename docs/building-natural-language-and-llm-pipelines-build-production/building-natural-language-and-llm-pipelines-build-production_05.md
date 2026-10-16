# deepset 的 Haystack 简介

在上一章中，我们概述了 LLM 领域最先进的模式，从提示词工程到上下文工程，从 LLM 向小微模型和推理模型演进，并提出了一种混合架构。该架构利用了不断变化的 AI 领域中两个关键源力量的优势：deepset 的 **Haystack** 和 **LangGraph**。我们通过建议的混合架构映射了它们各自的优势：一个有状态的编排层（LangGraph）指导一组可靠、可审计的工具层（Haystack）。这种关注点分离是构建复杂的、生产级 AI 的关键。

本章开始深入研究使用 Haystack 2.0 构建该层的方法。虽然 Haystack 提供了自身的智能体能力，但我们将通过概述来展示这种方法在什么情况下是足够的；我们对该框架的主要关注在于它作为“流水线优先”引擎的角色。它的真正优势在于健壮、可衡量且可部署的数据流流水线，这些是高级智能体的基础。

在构建智能体将调用的高级 RAG 系统（第 4 章）或作为微服务部署的自定义 RAG 和 NLP 流水线（第 7 和第 8 章）之前，我们必须首先掌握框架的基础物理机制。本章介绍了这些核心构建模块：**组件（components）**、**流水线（pipeline）**以及将它们结合的**数据协议（data protocols）**。

在本章中，我们将涵盖以下主题：

- deepset 概述

- Haystack 进化的 LLM 编排框架

- Haystack 2.0 核心构建模块：从组件到智能体

# 3 

- 核心特性与优势

- 用例与应用

- 深度解析 Haystack 架构

- 将 Haystack 整合到你的工作流

## 技术要求

我们将使用与第 2 章相同的技术设置。本章使用的 Jupyter notebook 可以在以下仓库的 `ch3/` 文件夹中找到：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines。

有一个专门为本章准备的 `pyproject.toml` 文件。建议在独立的 VS Code 窗口中打开 `ch3` 并安装依赖项：

```
$ cd ch3/
$ uv sync
$ source .venv/bin/activate
```

要在 Jupyter notebook 中激活虚拟环境作为内核，请点击“Select Kernel”，然后点击指向文件夹虚拟环境（名为 `rag-with-haystack-ch3`）的路径，或虚拟环境的相对路径：

`.venv/bin/python`

我们将逐步引入 notebook，因此你可以通过提供的 URL 相应的相对路径选择讨论过的 notebook。

## deepset 概述

deepset 由 Milos Rust、Malte Pietsch 和 Timo Möller 于 2018 年创立，伴随着 BERT 模型（https://huggingface.co/docs/transformers/model_doc/bert）的兴起而旨在构建可扩展的 NLP 工具。最初，deepset 专注于通过其 FARM 框架（https://github.com/deepset-ai/FARM）进行开源微调，以及流行的 Hugging Face 模型，如 `roberta-base-squad2`（https://huggingface.co/deepset）。2021 年，FARM 的核心功能合并到了 deepset 的旗舰框架 Haystack 中。

随着行业从专门的微调转向通用 LLM，deepset 通过完全重构框架来适应这一变化。结果是 Haystack 2.0：一个专为生成式 AI 时代设计的平台。Haystack 2.0 超越简单的提取，编排基于 API 的 LLM 以构建复杂的 RAG 系统和自主智能体。这种演进将 Haystack 从一个专门的 NLP 工具转变为现代 AI 开发的多功能平台。

## Haystack 进化的 LLM 编排框架

Haystack 2.0 代表了根本性的范式转变，从专门的搜索演变为现代 AI 的通用工具箱。它的核心哲学建立在三个支柱之上：它是 Python 原生的、显式的且模块化的。最显著的进展是采用了有向图（DG）。开发者不再依赖隐式数据；而是显式地实例化带有连接插槽（sockets）的 `Pipeline`。这种透明性允许条件路由（例如，如果查询是德语，则路由到模型 A）和并行处理（例如，同时运行关键词搜索和向量搜索），使系统完全透明。

这种架构的重构是对智能体工作流需求的策略响应。虽然框架原生支持智能体和循环，但对我们而言，它最大的优势在于其可靠性。通过围绕具有严格契约的显式有向图重新构建基础，Haystack 为创建确定性工具提供了一个稳定的平台。

为了充分理解这一演进的意义，下表提供了 Haystack 1.x 和 2.x 核心范式的直接对比。

| 特性 | Haystack 1.x (旧版) | Haystack 2.0 (当前) |

| --- | --- | --- |

| 流水线定义 | 使用 YAML 文件或带有隐式节点命名的 `Pipeline()` 类 | 在纯 Python 中使用带有显式 `.add_component()` 和 `.connect()` 方法的 `Pipeline()` 类 |

## 组件

| 特性 | Haystack 1.x (旧版) | Haystack 2.0 (当前) |

| --- | --- | --- |

| 组件定义 | 基于类，需要继承自 `BaseComponent` 类 | 基于装饰器，在标准 Python 类上使用简单的 `@component` 装饰器 |

| 数据流 | 组件之间隐式传递字典，可能难以追踪 | 使用组件输入输出定义的显式、类型化的输入输出插槽 |

| 调试 | 通常需要特定框架的知识，且可能不透明 | Python 原生调试；可以使用 `.draw()` 方法可视化流水线以查看精确的结构和数据流 |

| 核心抽象 | 节点的线性序列，使得复杂逻辑难以实现 | 有向无环图 (DAG)，原生支持复杂、非线性工作流的分支、合并和循环 |

| 主要关注点 | 抽取式问答和语义搜索 | 通用 LLM 编排，包括高级 RAG、智能体以及任何自定义多步工作流 |

表 3.1 – Haystack 1.x vs. Haystack 2.0：范式转变

# Haystack 2.0 核心构建块：从组件到代理

Haystack 2.0 引入了清晰的抽象层次结构，允许开发者以结构化且可重用的方式构建日益复杂的应用程序。这种层次结构从工作原子单元——组件（Component）开始，一直扩展到代理（Agent）。理解这些构建块是精通该框架的基础。

### 组件 (Components) - 工作原子单元

Haystack 2.0 的基石是组件。组件是一个自包含的 Python 类，旨在在管道（Pipeline）中执行单个特定的任务。这可以是从清理文本文档、将查询发送到网络、从数据库检索文档，到调用 LLM 进行生成的任何操作。

可以在此处找到探索组件的 Jupyter notebook：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch3/jupyter-notebooks/components.ipynb。

该 Jupyter notebook 提供了初始化和使用现有组件的示例。Haystack 2.0 的核心架构选择之一是创建自定义组件的方式。所有组件都遵循这一模式，而这正是将现有组件和自定义组件集成到管道中成为可能的关键。

组件的创建非常简单且符合 Python 风格，由两个关键特性定义：

- **@component 装饰器**：只需在 Python 类定义上方添加 `@component` 装饰器，即可将任何 Python 类转换为 Haystack 组件。该装饰器会将类注册到 Haystack 框架中，并信号它可以在管道中使用。

- **`run()` 方法**：每个组件必须有一个 `run()` 方法。这是组件逻辑的入口点，也是执行主任务的地方。

让 Haystack 2.0 中的组件如此健壮的是类型化输入输出插槽（Sockets）的概念。它们通过 `run` 方法签名上的 `@component.output` 装饰器定义。插槽作为组件的显式连接点，定义了它预期接收什么类型的数据以及将产生什么类型的数据。例如，检索器（retriever）可能有一个名为 `query` 的输入（期望类型为 `str`）和一个名为 `documents` 的输出插槽（产生 `List` 类型）。这种显式合约确保了管道内部的安全性，使数据流透明化，并允许 IDE 提供更好的自动补全和错误检查。这比 Haystack 1.x 中隐式的字典传递有了显著改进，后者经常会导致运行时错误和难以调试。

可以在此处找到探索自定义组件定义的 Jupyter notebook：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/ch3/jupyter-notebooks/your-first-custom-component.ipynb

在笔记本 notebook 中，我们定义了一个前缀处理器文档（`prefixer`），它使用用户提供的前缀字符串为每个文档添加注释。

### 管道 (Pipelines) —— 将组件编排成图

虽然组件执行单个任务，但管道负责在连贯的工作流中编排它们。正如过的，Haystack 2.0 管道是一个有向无环图（DAG），其中组件是节点，插槽之间的连接是边。

在 Python 中构建管道是一个显式的、三步的过程：

1. 实例化管道：

```python
pipe = Pipeline()
```

2. 添加组件实例：使用唯一名称将组件添加到管道中。这是一个示例：

```python
pipe.add_component(
    name="retriever",
    instance=InMemoryEmbeddingRetriever(document_store)
)
pipe.add_component(name="generator", instance=OpenAIGenerator())
```

3. 连接组件：通过连接一个的输出插槽到另一个的输入插槽来定义数据流：

```python
pipe.connect("retriever.documents", "generator.documents")
```

这种 `connect` 语法是 Haystack 2.0 透明性的基石。它消除了关于数据如何在系统中移动的歧义。此外，框架提供了一个重要的调试和可视化工具：`.draw()` 方法。调用 `pipe.draw("my_pipeline.png")` 将生成一个视觉化表示管道图结构的图像文件，显示所有组件及其之间的连接。图 3.1 显示了所讨论示例中管道的 Mermaid 图。这是一个简单的 RAG 管道，通过通过虚拟文档来增强 LLM。

### deepset 介绍的 Haystack

![](img/502c06ed872898cf88f18f9c142647a5_97_0.png)

图 3.1 – 提示词的 Mermaid 图

可以在此处找到探索简单管道及其可视化的 Jupyter notebook：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch3/jupyter-notebooks/your-first-pipeline.ipynb。

# 3 



在图中可视化并显式连接组件的能力是透明度的巨大飞跃。然而，随着这些图的增长，你经常会发现自己在反复构建相同的模式。Haystack 提供了一种强大的抽象来解决这个问题。

## 超组件 (SuperComponents) —— 封装和复用复杂性

随着管道变得复杂，某些连接组件模式会反复出现。例如，标准的文档索引工作流可能始终由文件转换器、文档清理器、文档分割器、嵌入器和文档写入器组成，且都按顺序连接。在每个新管道中重新构建此子图将枯燥且容易出错。

为了解决这个问题，Haystack 2.0 引入了超组件的概念。超组件是一个预包的、可重用的管道，可以被视为大型管道中的单个组件。它封装了一个复杂的子图，仅向外部暴露必要的输入和输出插槽。

例如，之前描述的整个索引工作流可以封装进`IndexingSuperComponent`中。开发者只需一行代码将其添加到更大的管道中：

```
pipe.add_component(
    name="indexer",
    instance=IndexingSuperComponent()
)
```

这极大地简化了主管道的定义，并促进了清晰、模块化的设计。超组件是管理复杂性和创建高级功能库的强大工具。让我们探索一个简单的模式，如提供的 notebook 所示。可以在此处找到探索超组件的 Jupyter notebook：<https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch3/jupyter-notebooks/supercomponents.ipynb>。

我们定义了一个预处理管道，用于清理并将文档分割成块。

![](img/502c06ed872898cf88f18f9c142647a5_99_0.png)

图 3.2 – 预处理管道的 Mermaid 图

在 notebook 中，我们将预处理管道抽象为预处理器超组件，然后将其连接到我们在自定义组件 notebook 中看到的前缀处理器组件，该地址为：<https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch3/jupyter-notebooks/your-first-custom-component.ipynb>。

最终管道定义仅包含两个组件：预处理器超组件和前缀处理器组件。可以在图 3.3 中清晰看到，该图可视化了最终管道结构，其中预处理管道被抽象为单个预处理器组件。

![](img/502c06ed872898cf88f18f9c142647a5_100_0.png)

## 代理 (Agents) 与工具 (Tools) —— LLM 驱动的推理

工具本质上是 Haystack 组件或抽象为超组件的整个管道的语义封装。它利用底层的确定性逻辑

- **Name**：短唯一标识符（例如：`internal_knowledge_search`）
- **Description**：工具能力的自然语言说明（例如：*搜索内部知识库*。**输入为字符串查询**）

当代理运行时，它会进入推理循环。它检查可用工具的描述，制定计划，选择合适的工具，生成输入并执行。输出（观察结果）会被馈回循环中。

这种架构创建了严格的层次结构：`组件 -> 管道 -> 超组件 -> 工具 -> 代理`。

我们现在来分析这种层级结构的优缺点。

- **优点（可靠性）：** 这种层级结构确保了智能体的执行步骤由健壮且类型安全的引擎支持。由于工具是对经过验证的 Haystack 管道的封装，智能体调度的是可靠的、确定性的工作单元，而不是脆弱的脚本。
- **缺点（上下文不透明性）：** 正如我们在第 2 章中所探讨的，当前的上下文工程需要对智能体的状态和内存进行精确控制，以防止上下文腐化。Haystack 智能体内部封装了推理循环。这种黑盒方法隐藏了状态，使得难以实现高级的内存策展策略或像 LangGraph 这样的框架提供的复杂状态管理。

## 核心特性与优势

向 Haystack 2.0 的架构演进带来了大量特性，使其成为现代 AI 堆栈中工具层的理想引擎。这些优势直接源于其核心设计原则：显式图、类型化合约和模块化：

- **透明度和可调试性：** 这是生产工程中最大的优势。通过要求在类型化组件插座（sockets）之间建立显式连接，框架消除了不透明数据传递的歧义。开发者可以确切看到数据来自何处以及流向何处。`.draw()`方法补充了这一点，它可以渲染管道的实际图结构，将调试从一种游戏变成了标准的检查任务。
- **通过 DAG 实现灵活性：** 采用 DAG（有向无环图）架构允许任意复杂的工作流。虽然简单的 RAG 管道是线性的，但框架原生支持非线性路径，例如条件路由（如果是 PDF 使用 OCR；如果是文本则跳过）和并行分支。这允许你构建健壮的数据处理逻辑，优雅地处理边缘情况而不是崩溃。
- **可扩展性与部署：** deepset 引入了 HayHooks <https://docs.haystack.deepset.ai/docs/hayhooks/>，这是一个通过序列化和加载管道将 Haystack 部署为 REST 端点的框架。这使得 Haystack 成为完美的能力提供者，允许你构建作为独立微服务运行的复杂确定性工具。
- **支持智能体的架构：** Haystack 2.0 的架构旨在作为智能体系统的后端。在内部，它的`SuperComponent`和工具抽象允许你将任何组件（或整个 RAG 管道）封装成具有清晰描述的单一能力单元。在外部，当与 Hayhooks 结合时，这些管道变成了工具，可以通过模型上下文协议（MCP）被其他框架的智能体轻松消费，MCP 是连接 AI 应用程序与外部系统的开放标准（<https://modelcontextprotocol.io/docs/getting-started/intro>， <https://docs.haystack.deepset.ai/docs/hayhooks/mcp-support>）。
- **可扩展性：** 如本章介绍的以及我们在第 5 章及后续章节中看到的，创建可以作为 Haystack 管道一部分与现有元素连接的自定义组件遵循简单的路径。`@decorator`装饰器为封装自定义逻辑提供了极佳的接口。这种低门槛确保了微组件设计模式，将特定逻辑封装在可重用的块中，而不是单体脚本。
- **全面的生态系统：** Haystack 与更广泛的 MLOps 领域无缝集成，防止了供应商锁定。它支持所有主要的模型提供商（OpenAI， Hugging Face, Cohere, Azure, Google 和 Amazon），并提供了生产级向量数据库（Pinecone, Weaviate 和 Qdrant）以及评估框架（Ragas 和 DeepEval）的集成。这些特性使其成为一种健壮、适应且可扩展的解决方案，用于构建可以作为智能体系统一部分的 RAG 和数据处理工作流。现在，让我们探索使用这些特性可以构建的实际应用和模式。

## 应用场景与应用

本节提供了构建 2025 年 RAG 和智能体应用中一些常见模式的实用指南，展示了如何将高层概念映射到具体的 Haystack 组件和管道上。重点从构建单一类型的系统转向理解如何组合组件以创建针对特定需求的定制解决方案。

## 构建 RAG 管道

RAG 是现代 LLM 应用的基石，允许模型根据私有知识库回答问题，从而减少幻觉并提供最新的、有据的信息。Haystack 2.0 在构建 RAG 系统方面表现出色，提供了实现从基础设置到顶尖混合搜索管道的所有组件。

构建 RAG 系统的一个关键概念是将关注点分离为两个不同的管道：索引管道和查询管道：

- **索引管道：** 该过程处理知识库的摄取和归一化。它将范围涵盖从非结构化 PDF 和数据到结构化 JSON 的原始输入转换为`DocumentStore`中可检索的向量。传统上被视为离线任务，Haystack 2.0 的自定义组件架构允许这些管道实时运行，例如通过使用自定义消费者组件直接从 Kafka 等事件流中触发摄取。
- **查询管道：** 这是负责回答请求的在线实时管道。它接收输入查询，从`DocumentStore`中检索相关内容，并使用 LLM 合成答案。在我们的架构中，当由用户直接触发时，该管道经常作为智能体调用的工具进行搜索。该管道的复杂性随策略而异；让我们检查三种最常见的场景：语义搜索、词汇搜索和混合检索。

在构建现代 RAG 系统时，最重要的架构决策之一是如何检索信息。今天最有效的方法是混合检索。这种策略结合了不同的搜索方法，创建了一个既强大又灵活的系统。

为了理解为什么混合检索是必要的，我们首先需要查看使用单一搜索方法的局限性。用户查询范围从宽泛的概念性问题到对非常具体的字面信息的请求。一个健壮的 RAG 系统需要有效处理这整个范围。

### 语义搜索：稀疏检索器 (BM25)

传统的搜索系统依赖于基于关键词的，即稀疏检索。这种方法的常用算法是 BM25。这些系统通过匹配用户查询中的精确单词来工作。

- **优点：** 稀疏检索快速、高效，且无需任何模型训练即可工作。它在用词至上的情况下（如法律研究或技术文档）表现出色。
- **缺点：** 它的主要缺点无法理解同义词或概念关系，使其在面对语言多样性时非常脆弱。

### 词汇搜索：密集检索器 (嵌入)

为了解决词汇不匹配问题，开发了密集检索。这种技术使用嵌入模型将查询和文档转换为数值向量（即嵌入）。这些向量捕捉了文本的语义。系统随后寻找向量表示与查询最接近的文档。

- **优点：** 密集检索非常擅长理解细微差别、上下文和意图。它能够找到概念上相关的文档，即使关键词完全不同。
- **缺点：** 在捕捉宽泛含义的过程中，密集检索有时会忽略特定的、字面的术语。对精确错误代码（例如 HTTP 404）的搜索可能会返回关于通用网络服务器的文档，而不是包含该精确、关键字符串的那个文档。

| 特性 | 稀疏 (BM25) | 稠密 (Embeddings) |
| --- | --- | --- |
| 表示形式 | 高维稀疏向量（倒排索引） | 低维稠密向量 |
| 优点 | 关键词命中率高、计算开销小、无需训练 | 能处理同义词和概念、理解语法、对词汇不匹配具有鲁棒性 |
| 缺点 | 词汇不匹配、缺乏语义理解、脆弱 | 可能会漏掉特定关键词、计算开销大、需要训练数据 |
| 计算特征 | 索引快；查询极快 | 索引慢（需要嵌入）；查询快（配合 ANN 时） |
| 理想用例 | 法律搜索、产品代码、以及包含特定术语或名称的查询 | 通用问答、主题探索和概念性搜索 |

表 3.2 – 稀疏 vs. 稠密检索

如你所见，稀疏检索和稠密检索的缺点是互补的。当一种方法失效时，另一种方法往往能成功。这就是为什么最稳健的 RAG 系统会结合两者的优点。一个顶尖的系统必须能够以同样的技能处理概念查询（例如：“机器意识的哲学意义是什么？”）和精确查询（例如：“查找提到‘Project Chimera’和‘2023年第三季度’的文档”）。混合检索不仅是技术升级，它是构建系统的核心设计原则，使其能够适应人们寻求信息的灵活且多样化的方式。我们将在下一节深入探讨这项技术。

## 混合检索

一旦你理解了各个组件，下一步就是设计一个能够融合其结果的系统。实现这一目标有两种主要的架构模式：由你控制的灵活流水线（pipeline）方法，以及集成到数据库中的精简化方法。最常见且灵活的架构是流水线方法。在这种模型中，你的应用程序代码通过一系列步骤来编排整个过程：

- **并行获取：** 用户的查询被同时发送给稀疏检索器（如 BM25）和稠密检索器。每个检索器都会返回其排序后的候选文档列表。
- **融合：** 将两个结果列表合并为一个集合。实现此点的简单方法是合并列表并去重。更先进且有效的技术是倒数排名融合（Reciprocal Rank Fusion, RRF），它根据文档在每个列表中的位置进行重新排序，避免了比较两个不同评分系统的复杂问题。
- **排序：** 合并后的列表被传递给最终的重排序模型（可以是使用`BAAI/bge-reranker-base`的 Transformer 模型：<https://huggingface.co/BAAI/bge-reranker-base-base>）。这通常是一个交叉编码器（cross-encoder），它同时查看查询和每份文档，提供更准确的相关性评分。这一步计算密集，但对于将候选结果过滤为最相关的结果并传递给语言模型至关重要。

此过程中的每一步都可以映射到流水线中的一个组件，如下链接所示：<https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch4/jupyter-notebooks/images/hybrid_rag_pipeline.png>。

让我们在下一节看看这个过程是如何映射的。

## 高级 RAG 流水线

此示例使用了客户端侧融合模式，展示了 Haystack 的模块化架构如何轻松地组合组件。流水线被设置为一个图，用户的查询通过两个并行分支，一个用于稀疏检索，一个用于稠密检索。然后将结果进行合并、为了精确性进行重排序，最后发送到生成器以创建一个：

- **稀疏检索分支：** 这是一个关键词精度引擎。它执行 BM25 搜索以找到包含查询中精确术语的文档。
- **稠密检索分支：** 该分支处理语义搜索。嵌入器（embedder）将查询转换为向量，基于嵌入的检索器使用该向量寻找概念上相似的文档。
- **融合组件：** 该组件获取两个分支的文档列表，合并它们并移除任何重复项。
- **重排序组件：** 这是精度引擎。它使用交叉编码器模型同时查看查询和每个候选文档，产生最相关文档的最终、高精排序。
- **生成阶段：** 最终的高质量文档列表由提示词构建器组件格式化为提示词（prompt）。然后，该提示词被发送到 LLM 生成组件，该组件使用 LLM 综合信息并生成最终答案。

我们将在下一章中深入探讨如何导入和初始化现有组件，并将它们作为流水线的各个连接起来。让我们探索另一个 Haystack 2.0 特别适用的用例：智能体应用和 RAG。

## 开发智能体系统

Haystack 2.0 中智能体（agents）和工具的引入代表了应用开发的根本转变，无论你选择哪种架构模式。它将开发者的角色从人类的严格流水线构建者转变为智能体的能力提供者。

对于利用 Haystack 原生智能体能力的开发者，这种转变允许你创建一组稳健、描述清晰的工具，并信任内部由 LLM 驱动的单智能体智能体系统（<https://haystack.deepset.ai/tutorials/45_creating_a_multi_agent_system>）来动态编排它们。这种方法简化了复杂性，允许系统处理意外查询，而无需为每个边界情况显式编程。

对于喜欢 LangGraph 粒度控制的开发者，通过序列化 Haystack 流水线并通过 Webhooks 或 MCP 服务器部署，可以将 Haystack 从本地库转换为稳健的确定性微服务引擎。

在两种架构中，结果都是相同的：你正在构建可靠的、自包含的智能单元，这些单元可以由推理引擎发现并产生。

然而，这种转变引入了新的挑战和机遇。调试不再仅仅是检查组件之间的数据流；而是分析 LLM 的推理轨迹（思维和工具选择），以理解它为何以某种方式运行。提示词工程的艺术也扩展了；它不再仅仅是为最终生成编写提示词，而是编写清晰且有效的工具描述来引导智能体的推理过程。

## 作为工具的 RAG

Haystack 2.0 的组合性使得从流水线到智能体的转换异常容易。上一节构建的顶尖 RAG 流水线可以封装成单个工具。这是通过创建一个工具实例并提供以下内容的：

- **名称：** `internal_knowledge_search`
- **描述：** 使用此工具搜索我们的内部文档数据库。它是获取我们产品、内部政策和历史项目数据的最佳来源。输入应该是清晰、具体的问题。
- **实例：** `hybrid_rag_pipeline`对象本身。

通过这种方式，复杂的多步骤混合检索过程被抽象为智能体的一项强大能力。我们可以将其扩展为多工具智能体系统。

## 构建多工具智能体

当智能体拥有多个不同工具的访问权限时，真正的威力得以实现。我们可以创建一个第二个工具来搜索实时网页：

1. 使用 Serper Web 组件创建一个简单的流水线，该组件是低成本 Google Search API 的集成。
2. 将此流水线封装另一个工具：
   - **名称：** `web_search`
   - **描述：** 使用此工具从公共互联网查找最新信息。它最适用于时事、竞争对手新闻或非我们公司特定的通用知识问题。
   - **实例：** `web_search_pipeline`对象。

当给这个智能体一个复杂的查询时，例如“总结我们的内部 Q3 报告，并将主要产品的功能与竞争对手的最新发布进行对比”，它可以推理如何解决：

1. **思考：** 查询包含两个部分。第一部分关于内部报告。`internal_knowledge_search`工具描述为最适合。
2. **行动：** 智能体调用`internal_knowledge_search`工具，输入 Q3 性能报告。
3. **观察：** 它从 RAG 流水线收到了报告摘要。
4. **思考：** 现在我需要查找有关竞争对手最新发布的信息。这是外部的最新信息。`web_search`工具最适合这个。
5. **操作：** 代理调用`web_search`工具，输入为`competitor X latest release features`。
6. **观察：** 接收到了网页搜索结果列表。
7. **思考：** 我现在拥有了所需的所有信息。我将综合内部报告摘要和竞争对手功能列表来找到最终答案。
8. **答案：** 代理生成了一个全面的回复，回答了用户原始查询的两个部分。



这种方法可以通过设计代理系统在 `Haystack` 之外进一步推广，使得多代理系统中的每个代理都与一个或多个部署为 `REST` 端点或通过 `MCP` 服务器服务的 `Haystack` 流水线配对。现在让我们来看看为了将 `Haystack` 整合到你的 `RAG`、高级 `RAG` 和代理 `RAG` 工作流中可以采取的具体步骤。为了构建作为工具或部署端点的健壮流水线，你需要牢牢掌握 `Haystack` 提供的特定现成组件。

## 深入研究 Haystack 2.0 架构

本节将概述 `Haystack 2.0` 中可用的数据结构和现成组件。关键数据结构包括表示要存储文档的 `Python` 对象、存储图像、音频和基于 `HTML` 的数据的字节流对象，以及表示人类与 `LLM` 之间消息的对象。这些数据结构可以通过文档存储（`document stores`）和自定义组件进行组织和管理。我们将从基础构建块开始——定义和自定义组件以及流水线：数据类。

### 数据类

在深入研究组件之前，理解在它们之间流动的核心数据结构非常重要。`Haystack` 提供了一系列定义良好的数据类，用于表示流水线中处理的对象：

- **Document（文档）**：最基础的数据类。它封装了可以被处理和存储的数据。`Document` 对象可以包含文本（`content`）或二进制数据（`blob`），以及唯一 ID、自定义元数据、相关性评分、密集向量嵌入（`dense vector embedding`）以及用于混合检索场景的稀疏嵌入（`sparse_embedding`）。

- **ByteStream（字节流）**：代表转换为文本之前的原始二进制数据，例如图像或 `PDF` 文件。它包含字节形式的数据和相关的元数据，包括 `mime_type`（例如 `image/png`）。

# 3 

- **ChatMessage（聊天消息）**：用于构建对话式应用程序和代理。与简单的文本字符串不同，`ChatMessage` 是结构化对象，可以包含多态内容，包括文本、图像（`ImageContent`）、工具调用（`ToolCall`）和工具结果（`ToolCallResult`）。它显式定义了发送者的角色（用户、助手、系统或工具）。

- **StreamingChunk（流式块）**：用于处理实时响应的专用类。它封装了流式内容的单个片段（如 token）以及相关的元数据和潜在的工具增量（`tool deltas`），实现了流畅、低延迟的用户体验。

- **Answer（答案）**：表示流水线最终输出的类族：

  - **GeneratedAnswer（生成答案）**：表示由 `LLM` 生成的答案，包含答案字符串和用于生成的源 `Document` 对象列表。

  - **ExtractedAnswer（提取答案）**：用于抽取式问答系统，代表直接从源文档提取的特定文本范围。

核心数据类（`Document`、`ByteStream`、`ChatMessage`、`StreamingChunk`、`Answer`）是 `Haystack` 流水线的基石，代表了在组件之间严格流动的标准化信息。任何 `RAG` 系统中最关键的组件之一就是持有这些文档的组件。

### 文档存储（Document stores）

`DocumentStore` 是存储 `Document` 对象以供检索的数据库后端。`Haystack` 的架构明确将存储层（`DocumentStore`）与访问层（`retriever`）分离开。`DocumentStore` 在流水线之外进行配置和管理，而相应的 `retriever` 在流水线内部被用于获取数据。`Haystack` 支持广泛的文档存储，包括用于快速原型开发的内存选项和生产级向量数据库：

- **In-memory（内存型）**：

- **Vector databases（向量数据库）**：`ChromaDocumentStore`、`PineconeDocumentStore`、`WeaviateDocumentStore`、`QdrantDocumentStore` 和 `MilvusDocumentStore`

- **Traditional search engines（传统搜索引擎）**：`ElasticsearchDocumentStore` 和 `OpenSearchDocumentStore`

- **Other databases（其他数据库）**：`AstraDBDocumentStore` (Cassandra)、`MongoDBAtlasDocumentStore` 和 `Neo4jDocumentStore` (Graph)

根据项目的规模和现有基础设施选择正确的文档存储是关键架构决策。有了数据类和存储定义，我们现在可以调查执行工作的组件类别。可以在它们的集成页面（https://haystack.deepset.ai/integrations?type=Document+Store）下找到支持的文档存储列表。`Haystack` 还支持创建自定义文档存储。可以在此处找到指南：https://docs.haystack.deepset.ai/docs/creating-custom-stores。

### 组件类别

让我们探索 `Haystack 2.0` 中的现有组件。这些组件是构建任何流水线或代理的构建块。它们根据图 3.4 所示的在典型端到端工作流中的功能被组织成逻辑类别。

| 数据预处理 | 数据嵌入 | 数据检索 | LLM 生成 | 路由 | 自定义组件 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 提取 | 使用嵌入 | 使用密集、稀疏 | 注入查询和 | 分支 | 合并 |
| 标准化模型来自 | 各提供者 | 检索器来获取 | | | |
| 格式转换 | | | | | |
| 分割 | | | | | |

预定义组件分类为数据预处理、嵌入、检索、路由和代理功能的组件。我们可以通过自定义组件扩展 `Haystack` 的功能。我们将在第 5 章深入探讨这一点。现在让我们详细扩展关键类别。

### 数据预处理组件

这些组件通常是索引流水线的第一步，负责摄取原始数据并为索引做准备：

- **File converters（文件转换器）**

- **Web component（网页组件）**：该组件从 `URL` 列表中检索内容，返回 `ByteStream` 对象（https://docs.haystack.deepset.ai/linkcontentfetcher）。

- **Preprocessing components（预处理组件）**：准备你的数据、规范化空格、删除页眉页脚、清理文档中的空行，或将其分割成更小的片段（https://docs.haystack.deepset.ai/processors/）。

- **Audio components（音频组件）**：将音频文件转换为文本（https://docs.haystack.deepset.ai/audio）。

这些转换器预处理组件对于任何 `RAG` 流水线的“摄取”阶段至关重要。一旦文档经过清理和分块，下一步是将它们的语义转换为数据库可以搜索的格式。

### 数据嵌入组件

`Embedder` 是使用深度学习模型将文本转换为数值表示的组件。`Haystack` 提供了用于嵌入文档（用于索引）和查询（用于搜索）的独立组件，因为某些模型对每个任务有不同的最佳设置：

- **OpenAI**: `OpenAIDocumentEmbedder`

- **Sentence Transformers**: `SentenceTransformerDocumentEmbedder` 和 `SentenceTransformersTextEmbedder`。从 `Hugging Face Hub` 加载任何兼容模型，提供了巨大的灵活性。

- **其他提供者**：`Haystack` 与其他嵌入模型提供者的集成列表不断增加，例如 `Cohere` 和 `Hugging Face`。完整列表可以在此处找到：https://docs.haystack.deepset.ai/docs/embedders。

`Embedder` 组件为索引和搜索创建向量表示。一旦文档经过处理和嵌入，就需要它们保存到 `DocumentStore`。

`DocumentWriter` 是索引流水线的最后一步。它接收 `Document` 对象列表并将其写入指定的 `DocumentStore`。它包含了处理重复文档的策略（例如：跳过、覆盖或失败）。它处理索引流水线的最后步骤。

现在我们转向查询流水线使用的组件，从获取数据的组件开始。

## 数据检索组件

检索器（`Retrievers`）是任何搜索或 `RAG` 应用的核心。它们负责根据查询从 `DocumentStore` 中获取一小部分相关文档。它支持基于嵌入（`embedding`）和基于关键词的搜索。`Haystack 2.0` 支持针对多种数据库提供者的检索器。可以在此处找到完整的检索器列表：https://docs.haystack.deepset.ai/docs/retrievers。

检索器执行寻找潜在相关文档的关键第一步。在高级 `RAG` 流水线中，这些初始结果集会经过精炼以提高质量。

虽然检索器获取相关信息，但排序器（`rankers`）用于通过重新排列检索器返回的文档来提高搜索质量。它们是高级 `RAG` 流水线的关键组件。排序器有助于确保将最相关的文档传递给最终步骤。可以在此处找到支持的排序器列表：https://docs.haystack.deepset.ai/docs/rankers。

接下来让我们看看使 `LLM` 使用检索到的上下文来增强响应的组件。

## LLM 生成组件

这些组件负责与 `LLM` 交互以生成文本，并构建引导它们的提示词（`prompts`）：

- **生成器（Generators）**：包括 `OpenAIGenerator`、`HuggingFaceIGenerator` 和 `AnthropicGenerator`。这些组件处理到所有 `LLM` 提供者的 `API` 调用，接收提示词并返回生成的文本响应。可以在此处找到支持的提示词列表：https://docs.haystack.deepset.ai/docs/generators。

- **构建器（Builders）**：这些组件帮助我们构建与流水线连接的有效提示词：

  - `PromptBuilder`：用于从模板创建提示词的灵活组件。它可以将查询和检索到的文档等变量替换到预定义的提示词结构中，确保提示的一致性和有效性。

  - `AnswerBuilder`：用于从生成器的原始输出中解析结构化 `Answer` 对象的工具，通常使用正则表达式。

  - `ChatPromptBuilder`：一种用于类聊天应用的特殊构建器。

这些组件处理 `RAG` 流水线的最后的生成部分。为了构建更复杂的非线性的工作流（例如我们讨论过的混合检索流水线），`Haystack` 提供了逻辑和控制流的组件。

## 路由组件

这些组件对于构建由 `DAG` 架构支持的复杂非线性流水线至关重要。路由器（`Routers`）是一组组件，将查询或文档路由到其他可以处理它们的组件，而连接器（`joiners`）允许我们合并文档、数据结构甚至流水线。

- 连接器：https://docs.haystack.deepset.ai/docs/joiners

- 路由器：https://docs.haystack.deepset.ai/docs/routers

路由器和连接器是实现复杂 `DAG` 的“管道”。最后，我们还有高层组件，它们能够实现我们之前通过智能体工作流探索的基于推理的动态系统。

## 智能体组件

这些是构建智能体系统的高层组件：

- 智能体（`Agent`）：核心推理引擎。它接收查询和工具列表，并编排工具以产生答案（https://docs.haystack.deepset.ai/docs/agent）。

- 工具调用器（`ToolInvoker`）：用于由语言模型执行工具调用（https://docs.haystack.deepset.ai/docs/toolinvoker）。

这些组件构成了从简单搜索到复杂智能体的构建完整工具包。在对所有可用构建块有清晰理解的基础上，让我们概述一个在你的项目中使用它们的结构化流程。

## 将 Haystack 纳入你的工作流

将像 `Haystack 2.0` 这样的框架集成到你的项目中需要结构化的方法。本节提供了分步指南，帮助你从初始评估到设计和调试你的 `NLP` 应用。

## 评估你当前的数据系统

在编写任何代码之前，第一步是对你的项目需求和现有基础设施进行彻底评估。这种基础分析将指导你的设计选择并确保无缝集成：

1. **定义 `NLP` 目标**：清晰地阐述你想实现什么。你是在为内部文档构建问答系统、为客户支持构建聊天机器人，还是构建一个自动化业务流程的自主智能体？你的目标是确定需要构建的流水线的复杂度。

2. **评估数据源**：识别你数据的类型和来源。它是数据库中的结构化数据，还是 `PDF`、`Word` 文档和 `HTML` 文件中的非结构化文本？理解数据格局对于选择正确的转换器（`converter`）和预处理组件至关重要。

3. **评估基础设施和兼容性**：考虑你的部署环境。应用程序将在本地还是云端运行？绘制出现有的数据库系统，并检查它们与 `Haystack` `DocumentStore` 集成的兼容性。如果你计划使用 `Pinecone` 或 `Weaviate` 等向量数据库，请确保你的基础设施能够支持它。

4. **考虑数据动态和治理**：你的数据变化频率是多少？基于静态数据构建的系统其索引策略与处理实时数据流的系统完全不同。此外，评估数据的敏感性。预先建立的数据治理和安全程序对于管理敏感信息、规划成本和维护用户信任至关重要。

初始评估对于定义项目的范围和技术要求至关重要。一旦你有了明确的目标并了解数据，就可以开始设计流水线本身。

## 使用 Haystack 2.0 设计你的 NLP 流水线

在清晰理解需求后，你可以开始设计你的流水线了。`Haystack 2.0` 显式的、基于图的特性有利于视觉优先的设计流程。

以下步骤可以帮助你将项目目标映射到 `Haystack 2.0` 框架中：

1. **绘制草图**：在编写代码之前，在白板或图表中画出所需的工作流。将每个处理步骤表示为节点（组件），并画箭头以显示它们之间的流。例如，混合 `RAG` 查询将显示查询分为两条并行路径，然后并在连接器处合并，流经排序器，最后进入生成器。

2. **将草图转换为代码**：以视觉图为指南，将其翻译为代码是一项简单的任务：

   - 定义图中的每个节点/组件。
   - 实例化流水线。
   - 对于每个节点，使用 `add_component()` 添加相应的组件。
   - 对于草图中的每个箭头，使用 `connect()` 将组件连接起来。

3. **使用 `draw()` 验证**：在代码中定义流水线后，立即运行 `draw("pipeline_design.png")`。将生成的图像与你的初始草图对比。这个简单的验证步骤可以尽早发现逻辑错误，并确保你的代码准确反映了设计。

这种视觉优先的迭代设计过程有助于确保代码符合预期逻辑。对于新接触 `Haystack 2.0` 的团队，一次处理所有复杂度可能会令人敬畏，因此建议遵循渐进式的开发路径。

## 推荐的开发路线图

对于初接触 `Haystack 2.0`，增量地构建复杂度是有益的。推荐的学习路径如下：

1. **构建索引流水线**：从关注数据准备开始。创建一个能够成功获取源文件、处理它们、嵌入它们并将它们写入 `DocumentStore` 的流水线。正确完成这一点是任何 `RAG` 系统成功的基础。

2. **构建朴素 `RAG` 流水线**：一旦数据被索引，就构建最简单的查询流水线：嵌入查询、通过向量相似性检索文档并生成答案。这将为你提供一个端到端的基准。

3. **增强为混合 `RAG` 流水线**：通过实现混合检索策略改进你的基准。添加并行的 `BM25` 检索路径，合并结果，并添加重排序器以提高提供给 `LLM` 的上下文质量。在这个阶段，你将拥有一个先进的 `RAG` 系统。

4. **评估原生方法与混合方法的质量**：一种常用方法是使用如 `Ragas` 等框架从知识库生成图，创建角色（`personas`）并选择查询策略来构建问答对，针对 `RAG` 流水线的实际响应进行系统性评估。我们将将其作为实操项目的一部分。

5. **毕业到适用于智能体的工具**：作为最后一步，将你的高级混合 `RAG` 流水线封装成一个工具。你可以通过将其交给标准的 `Haystack` 智能体立即在本地进行验证。一旦通过验证，最终的生产环境迁移（也是本书的架构重点）是将流水线序列化，使用 `Hayhooks` 将其部署为 `REST` 微服务，并将该端点交给 `LangGraph` 等有状态的编排器。

这种增量方法允许你构建并验证系统的每个部分，从数据索引到功能完备的智能体。与任何复杂软件一样，你会在过程中不可避免地遇到问题。

## 常见问题故障排除

即使是设计良好的系统，问题也会出现。有效的故障排除是构建健壮应用的关键。表 3.3 概述了常见问题及可能的修复。

| 常见问题 | 可能的原因 | 可能的修复方法 |
|---|---|---|
| 数据摄取和处理错误 | 预处理组件中的文件路径不正确或分块大小配置不合理 | 密切关注转换器和预处理器组件的日志以识别错误。确保文件正确并调整分块大小。 |
| 流水线连接错误：`PipelineConnect` | 尝试使用不兼容的数据类型连接接口，或连接不存在的接口 | 使用 `.draw()` 方法视觉化检查连接，并确保它们与组件定义匹配。 |
| 模型和性能瓶颈 | 重型模型（如嵌入器和排序器）在处理之前未初始化，或 `LLM` 生成延迟天然较高 | 在处理请求之前对重型组件调用 `warm_up()` 方法。对于 `LLM` 延迟，考虑使用流式传输以改善用户体验。 |
| 智能体行为异常 | 工具描述编写不当或模糊，混淆了智能体的推理 | 在日志中检查智能体的推理轨迹（思考、工具选择和观察）。精炼定义工具能力的自然语言提示词。 |
| 数据安全和隐私风险 | 硬编码 `API` 密钥/凭据，或忽略数据驻留和合规性要求 | 通过环境变量或密钥管理系统安全地管理凭据。在使用云提供商时要留心。 |

表 3.3 - 常见错误、可能的原因及可能的修复方法

有效调试确定性流水线和动态智能体是构建可靠应用的关键技能。最后，支撑所有开发的一个关键方面是确保你的数据安全和隐私。遵循故障排除和安全最佳实践将帮助你构建健壮的、生产就绪的系统。现在让我们通过总结本章涵盖的核心概念来结束学习。

## 总结



在本章中，我们介绍了 deepset 及其旗舰开源框架 Haystack。我们追溯了它的演变过程：从在 FARM 等框架时代对 BERT 等模型进行微调的根源，到作为 Haystack 2.0 的完全架构重生——这是一个专为现代 LLM 景观设计的框架。

我们将注意力放在了 Haystack 2.0 上，它是构建端到端 NLP 应用的顶级 Python 包。我们了解到，它的功能远超简单的搜索，能够构建复杂的问答系统、先进的 RAG 流水线以及动态的自主智能体。Haystack 2.0 强大的核心在于其新的构建块：

- **组件（Components）**：使用`@component`装饰的简单、可重用的 Python 类，执行单一且定义良好的任务。
- **流水线（Pipelines）**：显式的、Python 原生的有向无环图（DAG），将组件编排成复杂的非线性工作流。
- **超级组件（SuperComponents）**：预包的、可重用的流水线，可以在更大的流水线内被视为单个组件。
- **智能体和工具（Agents and tools）**：最高层次的抽象，由 LLM 驱动的智能体从一组可用工具（封装的组件或流水线）中动态选择，以推理出解决方案。

deepset 提供的 Haystack 简介

我们探索了如何将 Haystack 整合到真实工作流中，从对数据基础设施需求的彻底评估开始。我们详细介绍了实用的、视觉优先的流水线设计方法，并概述了清晰的开发路线图，从构建基础索引流水线到部署多智能体工具。通过这些探索，我们学会了 Haystack 2.0 如何为旨在构建下一代 LLM 驱动应用的开发者提供透明、灵活且强大的基础。在下一章中，我们将深入研究实际实现，将这些组件组成为功能完备的流水线，并结合实际代码示例探索常见案例。

## 延伸阅读

- Jiao, X., Yin, Y., Shang, L., Jiang, X., Chen, X., Li, L., Wang, F., & Liu, Q. (2020). TinyBERT: Distilling BERT for Natural Language Understanding. https://aclanthology.org/2020.findings-emnlp.372/
- Devlin, J., Chang, M.-W., Lee, K., & Toutavan, K. (2019). BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding. https://arxiv.org/abs/1810.04805.
- Wiggers, K. (2022, April 28). Deepset raises $14M to help companies build NLP apps. TechCrunch. https://techcrunch.com/2022/04/28/deepset-raises-14m-to-help-companies-build-nlp-apps/
- Liu, Y., Ott, M., Na, D., Du, J., Joshi, M., Chen, D., Levy, O., Lewis, M., Zettlemoyer, L., & Stoyanov, V. (2019). RoBERTa: A Robustly Optimized BERT Pretraining Approach. arXiv:1907.11692. https://arxiv.org/abs/1907.11692.
- Model Context Protocol (MCP). What is the Model Context Protocol (MCP)? https://modelcontextprotocol.io/getting-started/intro.
- Source. (2025, June). BM25 and Its Role in Document Relevance Scoring. https://www.sourcery.net/resources/bm25-and-its-role-in-relevance-scoring.

## 获取本书的 PDF 版本及额外内容

这种增量方法允许你构建并验证系统的每个部分，从数据索引到功能完备的智能体。与任何复杂软件一样，你会在过程中不可避免地遇到问题。

### 设计流水线（Pipeline）的步骤

在本章中，我们将通过实践学习如何通过流水线连接组件，以满足各种用例。在本节中，我们将概述初始化 Haystack 流水线和连接组件的关键步骤。我们在这里概述主要的顺序步骤：

1. 选择并初始化组件：为流水线识别合适的组件；例如，索引流水线需要一个预处理器，以及拆分、嵌入和写入组件。带有增强生成的的检索器流水线则需要一个查询嵌入器、一个检索器和一个提示词生成器组件。
2. 创建流水线对象：通过使用`Pipeline()`类初始化流水线。这创建了你数据流的骨架，准备好填充组件。
3. 添加组件：使用`.add_component(name, component)`将单个组件添加到流水线中。此步尚未定义组件之间的数据流，但为集成各种功能为流水线做了准备，从数据提取、清洗到与 LLM 交互等功能。
4. 连接组件：使用`.connect("producer_component.output_name","consumer_component.input_name")`在组件之间建立连接。这一关键步骤定义了数据流，并确保上一个组件的输出正确地馈给下一个组件。Haystack 的设计允许复杂的数据流，包括分支和循环，以处理复杂的数据处理需求。
5. 运行流水线：使用`.run({"component_1":"mandatory_inputs": value})`执行流水线并指定初始输入。此操作将激活通过连接组件的数据流，根据定义的结构处理数据。
6. 可视化流水线：你可以使用`pipeline.draw(paths="path/to/image.png")`方法生成流水线的 Mermaid 图表。

上述工作流可以应用于各种用例。流水线的功能取决于所选组件以及它们连接的顺序。如果你的工作流需要重复或根据给定条件采取特定路径，Haystack 的流水线提供了分支功能。

# 4 

### 利用分支

分支功能使你的流水线能够同时处理多种数据类型或处理需求。通过创建分支，你可以将特定数据引导到专门的组件进行目标处理。流水线内部的循环允许迭代处理，例如错误修正或数据精炼，增强输出的准确性和可靠性。

创建分支的关键组件是路由器（routers）。分支的场景示例可以在此处找到关于探索不同类型路由器的笔记本：

- 条件路由器：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch4/jupyter-notebooks/routers/conditional_router.ipynb
- 元数据路由器：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch4/jupyter-notebooks/routers/metadata_router.ipynb
- 文本语言路由器：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch4/jupyter-notebooks/routers/text_language_router.ipynb

我们可以在图 4.1 中看到条件路由器是如何分支的。

### 整合组件——针对不同用例的 Haystack 流水线

![](img/502c06ed872898cf88f18f9c142647a5_125_0.png)

图 4.1 - 将提示词分类为事实性、语义性或复杂的条件路由器

**图 4.1** 中的的流水线使用 Haystack 的 **ConditionalRouter** 来检查每个入站查询并动态选择最佳处理路径。路由器评估一系列 Jinja2 条件，这些条件在查询文本中寻找模式（例如：针对事实性的 *when/who/what is the for*、针对语义性的 *how does/compare/difference* *between*，以及包含 *and* 的复杂查询）。若要了解更多关于 Jinja 和 Jinja 模板中条件的知识，我们建议你熟悉以下教程中的核心概念：https://pushmetrics.io/learn/jinja/what-is-jinja/。根据第一个匹配的条件，查询被路由到多个输出之一（**事实性、语义性或复杂性**），并连接到专用的 PromptBuilder，该生成器会生成不同风格的提示词（直接回答、详尽解释或详细分析）。最后的默认路由（条件：`{{ True }}`）确保每个查询都能得到处理，并且当我们运行流水线时，我们可以看到为每个查询选择了哪条路由以及生成了哪条提示词。

### 确保数据流的有效性

Haystack 流水线强调受控的数据流，只有连接的组件才会交换数据。这种方法不仅优化了处理速度，还通过隔离数据路径简化了调试。准确映射每个组件的输入和输出至关重要，以确保流水线内部无缝集成和数据交换。

Haystack 规划了确保组件能够逻辑连接的流，确保我们将文档转换为适当的数据结构和向量时，LLM 可以检索并返回适当的信息。核心假设是，当数据通过组件流过流水线时，每个组件将接受特定的数据结构作为输入并返回另一个作为输出。因此，连接组件的顺序非常重要。

例如，负责转换来自 PDF、网站和 Markdown 文件文本的组件将接受每个相应类型的文件（例如文档路径）作为输入，并返回 Haystack 文档对象（document objects）。这些文档对象随后可以作为负责文档内容向量化并将其存储在文档存储中的组件的输入。

当我们构建检索器流水线时，Haystack 假设流水线的输入是一个问题（字符串）和/或一个表示向量格式问题的浮点数列表。我们将在构建流水线的章节中概述不同场景的预期输入输出。

为了便于理解流水线内部的数据流，Haystack 集成了`.draw()`方法，可以创建可视化表示。我们将注意力转向下一个。

### 整合组件——针对不同用例的 Haystack 流水线

![](img/502c06ed872898cf88f18f9c142647a5_127_0.png)

图 4.2 - 将音频向量的流水线 Mermaid 图

### 可视化与验证

可视化你的流水线（例如使用 Mermaid 图表）可以提供结构和数据流的清晰度，帮助你识别任何潜在问题。Mermaid 图是一种使用语法来可视化系统、流程和流的图表类型，常用于创建流程图、序列图、类图、状态图和甘特图。

在 Haystack 中，当组件连接时会自动进行验证，检查兼容性和配置，这有助于故障排除和优化流水线设计。通过`.draw()`方法，我们可以轻松创建数据流的 Mermaid 图。

例如在图 4.2 中，我们看到了一个音频文件索引流水线的数据流。该流水线的目标是获取音频文件并将内容的向量表示存储在文档存储中。

此流水线涉及的组件如下：

- RemoteWhisTranscriber：该组件使用 OpenAI Whisper 入口将音频转换为文本。
- DocumentCleaner：该组件从转录文本中删除异常字符。在处理过程中，异常字符通常指非标准、特殊或意外字符，可能会导致编码、显示或数据处理问题。
- DocumentSplitter：该组件将文本拆分为块，为向量化做准备。
- SentenceTransformerDocumentEmbedder：该组件将文本块转换为数值表示，保留语义含义。
- DocumentWriter：该组件将嵌入写入文档存储。

如果文档存储需要更新或需要添加新文档，此流水线流可以执行一次或多次。通过这一特定的流，我们可以自动化将音频转换为向量格式的过程，并让 LLM 从中检索信息。现有的组件还通过 Haystack 集成（integrations）得到了补充。

为了确保流水线能够连接到各种包、LLM 和嵌入提供商以及评估和观测框架，Haystack 开发了一个框架用于连接甚至开发 Haystack 流水线的集成。

可以集成到 Haystack 流线中的技术列表可以在 https://haystack.deepest.ai/integrations 找到。我们可以在这里找到关于如何实现每个集成的详细信息、其优势，以及为你的项目选择合适集成方案的指导。这种共同努力促进了 Haystack 生态系统的创新和持续改进，使用户能够突破信息检索及其他领域的可能边界。

在本节中，我们概述了在设计和构建 LLM 驱动的流水线时 Haystack 流水线的概要。这些包括：确保我们的流水线能够轻松适配并解决问题的灵活性；确保数据在组件之间进行转换和移动的高效数据流；用于处理和迭代文档的流水线分支和循环；确保我们能够通过 Python 脚本保存和共享流水线的序列化；以及引入尖端模型、向量数据库和增强流水线功能的集成方案。

至此，我们现在准备好按复杂度水平研究常见的 LLM 驱动的用例了。在下一节中，我们将概述 LLM 在不同领域的应用，重点关注 Haystack 流水线的特征是如何实现此类工作流的。

### 使用 Haystack 构建流水线：朴素 RAG 索引和混合 RAG

任何有效的 RAG 系统的核心都是两个不同但相互依赖的工作流：负责准备知识库的**离线索引流水线**，以及利用这些数据实时回答问题的**在线查询流水线**。本节提供了构建这两个基础支柱的蓝图。我们将首先构建一个多功能的索引流水线，它能够从多个源和格式摄取数据，然后构建我们的第一个查询流水线（朴素 RAG 系统），它作为后续所有内容（带排序的混合 RAG 系统）的功能基准。

#### 索引流水线：准备你的知识库

索引流水线是一个关键过程。我们的首要目标是处理来自各种来源的非结构化或半结构化数据，将其转换为标准格式，并加载到 DocumentStore 中，以便进行高效搜索。一个设计良好的索引流水线是高性能 RAG 系统的基石，因为采集数据的质量直接影响检索质量，并最终影响生成的答案质量。

我们将构建一个可以同时处理多种混合数据源的索引流水线：实时网页、本地文本和 PDF 文件以及来自 CSV 文件的结构化表格数据。这是通过使用 FileRouter 实现的，该组件将不同数据类型重定向到相应的转换器，从而实现一个统一而专业的摄取工作流。关键步骤如图 4.3 所示：

![](img/502c06ed872898cf88f18f9c142647a5_130_0.png)

图 4.3 - 带有结构化和非结构化文本的索引流水线

我们的流水线将执行以下顺序的操作：

1. 获取并转换：从网络 URL、本地文本文件、PDF 和 CSV 摄取数据，并将每个转换为 Haystack 文档对象。
2. 分类：将原始数据源分为两个流：结构化和非结构化。
3. 预处理：清理文本、空格、特殊字符或正则表达式。
4. 合并：将来自不同转换器的文档流合并为一个单一列表。
5. 预处理：清理并将合并后的文档分割成较小的、语义连贯的块（chunks）以便进行嵌入。
6. 嵌入并写入：为每个文档块生成向量嵌入，并将最终的、丰富的文档写入 DocumentStore。

可以在此处找到探索带有路由的索引流水线实现的 Jupyter notebook：https://github.com/PacktPublishing/Building-Natural-and-LLM-Pipelines/blob/main/ch4/jupyter-notebooks/indexing_pipeline.ipynb。

将 notebook 中的代码想象为为数字图书馆构建一条流水线。主要目标是获取许多不同格式的“书”（实时网页、本地文本文件、PDF 和结构化 CSV 表格），正确地处理所有内容，并将它们归档到特殊的数据库（InMemoryDocumentStore）中。一旦进入该数据库，你就可以构建一个搜索引擎或聊天机器人（RAG 系统），它可以瞬时找到并理解来自所有不同来源的信息。

#### 整合组件——适用于不同用例的 Haystack 流水线

此代码构建了一个基础索引流水线。这是一个关键的离线过程，其工作是准备你的知识库。该流水线的质量直接影响你最终应用程序的运行情况。让我们拆解该流水线是如何运作的：

##### 第 1 阶段——收集和排序 (FileTypeRouter)

首先，我们收集所有的原材料（文件和网页）。然后，它们进入了我们工作流中最重要的组件：FileTypeRouter。该组件将分配输出插槽数据应该去的去向，并有四个不同的流：text/plain、application/pdf、text/html（用于爬取的网页内容）和 text/csv。

当我们调用`indexing_pipeline.run((..))`时，我们所有的本地文件（.txt、.pdf 和 .csv）都被发送到路由器。随后它处理每一个文件并进行“排序，将其发送到正确的处理道道”。这种为不同数据类型创建不同路径的能力被称为分支（branching），它是 Haystack 灵活设计的关键部分。通过这种方式，路由器将每个文件引导到正确的预处理流水线：

- haystack_intro.txt 仅发送到 text/plain 输出
- sample.pdf 仅发送到 text/PDF 输出
- 11m_models.csv 仅发送到 text/CSV 输出

下一阶段是通过分支处理结构化和非结构化数据。

##### 第 2 阶段——处理（两个专门分支）

现在，我们的材料同时沿两条独立的、专门的流水线流动：

###### 分支 1，非结构化数据（Web、TXT、PDF）

此分支用于文本块：

1. 获取并转换：LinkFetcher 抓取网页，HTMLConverter、TextConverter 和 PDFConverter 共同工作，将各自的输入转换为标准的 Haystack 文档对象。




## 流水线细节

### 分支 1，文本数据

#### 合并

`unstructured_doc_joiner` 将来自这些不同转换器的所有文档收集到的一个列表中。

#### 预处理

`text_cleaner` 整理文本，`text_splitter` 将文档切成较小的 150 个词的块。这对于后续步骤至关重要。

### 分支 2，结构化数据（CSV）

此分支专门为表格数据设计：

- **转换**：`csv_converter` 简单地读取整个文件内容到单个文档。
- **预处理**：
  - `csv_cleaner`：该组件处理表格非常智能。它移除了空行和空列（例如我们示例中的三逗号）。
  - `csv_splitter`：这是关键。我们设置了 `split_mode="row-wise"`。它将表格的每一行放入自己的文档中（例如“Company: OpenAI, Model: GPT-4”）。

### 容错机制

现实世界的数据很少完美，因此我们的流水线需要针对意外情况的防御。第一道防线是 `FileTypeRouter`，它作为未知文件格式（如 `.png`）的安全阀。它将未匹配的文件重定向到未分类的路由，有效地忽略它们以防止阻塞机器。

然而，`LinkFetcher` 默认情况下更为严格；它会对损坏的 URL 抛出异常，这可能会导致流水线崩溃。为了防止这种情况，我们将其配置为 `raise_on_failure=False` 以直接跳过坏行。最后，即使是可访问的文件，如果证明是空的，也可能导致 `DocumentSplitter` 崩溃。为了解决这个问题，我们引入了 `DocumentSanitizer`，这是一个自定义关口，检查每个类型（为了健壮性跳过错误，为了质量进行清洗，确保流水线达到生产就绪）。在下一章中，我们将更深入地学习如何定义自定义组件。

## 第 3 阶段——统一、索引和上架

现在，我们将所有内容重新整合：

1. **统一**：`final_doc_joiner` 是重新合并点。它等待并收集所有处理过的文档（来自分支 1 的文本块和来自分支 2 的单独行）形成一个单一的大列表。
2. **索引**：`doc_embedder` 接收此统一列表。它读取每一个数据块（无论是文本还是 CSV 行）并将其转换为向量（捕捉其语义映射的数字列表）。
3. **上架**：最后，`writer` 获取这些已完全处理的、“已索引”的文档（现在已拥有了强大的向量嵌入），并将它们“上架”到 `InMemoryDocumentStore` 中。

大功告成！你的库现在已经完全索引化。博客文章的文本、PDF 文件的内容以及 CSV 文件的每一行都并排存储，根据它们的含义进行理解和检索。这些处理后的数据现在存储在 `DocumentStore` 中，是我们构建问答能力的基础。下一步是构建 RAG 流水线，它将与此知识库交互以生成智能智能体。

生成的流水线可以使用内置的绘图功能创建其 Mermaid 图表进行可视化，可以在此处处找到：`https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch4/jupyter-notebooks/images/indexing_pipeline.png`。

## Naive RAG – 一个基础问答系统

Naive RAG 架构代表了 RAG 最直接的实现。它遵循一个简单的线性的两步过程：首先，检索一组与用户查询相关的文档；其次，根据这些文档中包含的信息生成响应。虽然它是 RAG 最简单的形式，但它本身就是一个至关重要的基准和强大的工具。

即使在这样的基础配置下，Haystack 的设计哲学也提供了显著优势。框架对显式连接（`pipe.connect()`）的坚持创建了一个“玻璃盒”架构。在 Haystack 流水线中的每一步都是透明且可追溯的。从初始的查询嵌入，通过检索器，再到提示词构建器，最后到生成器，数据流都由开发者显式定义。这种透明度对于调试和理解系统行为价值连城，为构建更复杂、更易维护的应用程序奠定了坚实基础。让我们来实现这一工作流。

我们的流水线将执行以下操作序列：

1. **查询向量化**：对用户问题应用嵌入模型。
2. **检索相关上下文**：使用嵌入后的问题从 `DocumentStore` 中检索相关信息。
3. **增强**：通过提示词生成器，指令 LLM 根据用户查询和检索到的上下文来用自然语言回答问题。

探索语义搜索流水线实现的 Jupyter notebook 可以在此处找到：`https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch4/jupyter-notebooks/semantic_search_pipeline.ipynb`。

如果说第一个流水线是我们用于构建智能库的离线装配线，那么语义搜索流水线就是使用它的在线系统。语义搜索流水线是“管理员”，它接收用户查询，搜索已索引的架子，并实时生成感知上下文的答案。让我们拆解这条流水线来回答问题。

## 阶段 1 – 查询向量化

用户的问题是起点。但我们无法将文本问题与 `DocumentStore` 中的向量嵌入进行比较。我们首先需要将问题转换为相同的格式：

1. **操作**：将用户的问题（例如，“什么是 Haystack 2.0？”）发送给 `SentenceTransformersEmbedder`。
2. **组件**：`text_embedder` 组件应用与索引期间使用的完全相同的嵌入。
3. **输出**：这产生了一个“查询向量”，这是一个捕捉问题语义含义的数字列表。

这个初始阶段使得能够通过向量搜索检索语义相关的查询。让我们探索下一个阶段。

## 阶段 2 – 检索相关上下文

现在我们有了查询的向量，可以从库中找到最相关的文档：

1. **操作**：将阶段 1 的查询向量输入到 `InMemoryEmbeddingRetriever`。
2. **组件**：检索器组件获取此向量并将其与存储在 `DocumentStore` 中的所有文档向量比较。它找到含义与问题含义最相似的 `top_k=3` 文档。
3. **输出**：这产生了一个文档对象列表。这些可以是来自网页、文本文件或 CSV 行的块，取决于哪个最相关。

这些文档对象现在与原始查询一起被“注入”到提示词模板中，该模板用于引导 LLM 应如何回答。

## 阶段 3 – 增强并生成

我们获取检索到的事实并使用它们来构建答案。

**操作（增强）**：`PromptBuilder` 组件接收两个输入：原始问题和来自检索器的文档列表。然后它遵循模板来“增强”问题，将检索到的文档内容巧妙地放置在给 LLM 的更大的提示词中。模板如下：

```
Given the following information...
Context:
[来自文档 1 的内容]
[来自文档 2 的内容]
...
Question: [原始用户问题]
Answer:
```

**操作（生成）**：这个最终的增强提示词被发送给 LLM（`OpenAIGenerator`）。LLM 读取它刚刚收到的上下文，并仅使用该信息用自然语言回答问题。

就此了！系统随后打印 LLM 的回复。我们成功构建了一个不仅是猜测答案而是检索事实的流水线。

你可以在此处找到探索带有重排序（reranking）的混合流水线实现的 Jupyter notebook：`https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch4/jupyter-notebooks/hybrid_pipeline.ipynb`。

## 混合检索 RAG

为了构建更健壮的问答系统，我们使用了一种名为混合检索（hybrid retrieval）的高级技术。该流水线通过并行两种不同的搜索方法来创建一个更通用的搜索系统：

- **稠密（语义）检索**：理解含义、意图和概念的专家。
- **稀疏（词法）检索**：关键词匹配的专家，非常适合寻找特定的字面术语。

混合检索流水线的 Mermaid 图表可以在以下 URL：`https://github.com/PacktPublishing/Building-Natural-language-and-LLM-Pipelines/blob/main/ch4/jupyter-notebooks/images/hybrid_rag_pipeline.png`。

这种混合方法可以被认为是我们原始的“语义管理员”与一位“关键词专家”（`BM25`）共同工作，以找到最好的文档。Haystack 的灵活设计使得运行这些并行任务成为原生能力。但我们又增加了一位专家：“首席管理员”（重排序器），他会对它们的合并结果进行最后的深度分析，为我们提供最完美的文档。让我们来拆解这条先进的流水线。

### 阶段 1 – 并行检索（两位专家）

与之前，用户的问题是起点，但与语义搜索流水线不同，在混合搜索流水线中，用户的问题会同时发送到两条路径——稠密/语义搜索和稀疏/关键词搜索：

**路径 1，稠密/语义搜索**：这将使用语义检索组件并遵循此模式：

1. **动作**：将 `hybrid_question` 发送到 `SentenceTransformersEmbedder`。
2. **组件**：`text_embedder` 将问题转换为查询向量。
3. **组件**：`embedding_retriever` 使用该向量从 `document_store` 中找到语义上相似的 `top_k=3` 文档。

**路径 2，稀疏/关键词专家**：这将使用基于关键词的检索器并遵循此模式：

1. **动作**：将原始用户文本直接发送到 `InMemoryBM25Retriever`。
2. **组件**：`bm25_retriever`（我们的关键词专家）忽略语义含义，通过匹配精确单词（如 Haystack 或 2.）找到它的 `top_k=3` 文档。

一旦通过每个路径检索到相关文档，就是时候合并它们了。

### 阶段 2 – 融合（收集结果）

现在，我们有两个文档列表，它们可能重叠也可能不重叠。我们需要将其“融合”为一个单一的候选列表：

1. **动作**：将两个文档列表（总最多六个）输入 `DocumentJoiner`。
2. **组件**：`document_joiner` 将两个列表合并为一个合并的候选文档列表。
3. **输出**：产生一个单一的合并后的文档列表。

在这里，我们可以引入一种机制对检索到的文档进行排序，以便选择最相关的内容。

### 阶段 3 – 重排序（首席管理员的最终检查）

这个合并后的列表很好，但尚未按照真正的相关性进行排序。我们现在添加了最强大的质量控制步骤：排序器（ranker）：

1. **动作**：将合并后的文档列表和原始用户问题文本均送到 `TransformersSimilarityRanker`。
2. **组件**：排序器是一个计算密集型但准确得多的模型。它通过将查询和每个候选文档视为一对来执行“更深层、更具上下文感知能力的分析”。然后它根据这种深度分析对整个列表进行重新评分和重新排序。
3. **输出**：输出是最终的、精确排序的 `top_k=3` 最相关文档列表，这些文档是从两种检索方法的优中选出的。

一旦排序器选择了前三个文档，我们现在准备好引入我们的 LLM 了。

### 阶段 4 – 增强与生成（最终答案）

最后一个阶段与我们的朴素（naive）RAG 流水线相同，但现在通过更高质量的重排序文档进行了“增强”：

- **动作（增强）**：`PromptBuilder` 接收原始问题和来自排序器排名最高的文档。它巧妙地将这些高度相关的上下文放入提示词模板中。
- **动作（生成）**：将这个最终的增强提示词发送给 LLM（`OpenAIGenerator`）。LLM 读取它收到的优质上下文，并仅使用这些信息生成最终的、具有上下文感知能力的答案。

下一个逻辑步骤是比较我们的朴素 RAG 和混合 RAG 流水线的性能。为了简化评估时每个流水线的调用，我们将它们封装为 `SuperComponents`。这是一种允许我们将整个流水线视为一个组件的抽象。除了能够将复杂的工作流作为组件连接外，抽象还有一个额外好处：简化复杂的输入/输出结构。在*第 6 章*，我们将使用不同的嵌入模型系统地评估我们的朴素和混合 RAG SuperComponents。

## 通过 SuperComponents 简化流水线使用

在本节中，我们将介绍将朴素和混合 RAG 流水线重构为复用 SuperComponents 的过程。这是通过将流水线传递给 `SuperComponent` 类来实现的。我们还将使用输入类型转换和输出映射来定义新组件的公共接口。

保持原始定义的朴素和混合流水线，如果我们的流水线命名为 `naive_rag_pipeline` 和 `hybrid_rag_pipeline`，那么将流水线重构为 SuperComponent 可以通过两种方式完成：

### 方法 1 – 包装现有的流水线实例

第一种方法涉及包装一个实例化的流水线。当你已经定义了一个流水线对象并希望在不修改底层结构的情况下快速将其抽象时，这种方法是理想：

```python
from haystack import SuperComponent
# Naive RAG
native_rag_sc = SuperComponent(
  pipeline=naive_rag_pipeline,
  input_mapping={
  "query": [
    "text_embedder.text",
    "prompt_builder.question"
  ],
}
out_mapping={
  "llm.replies": "replies",
  "retriever.documents": "documents"
}

# Hybrid RAG
hybrid_rag_sc = SuperComponent(
pipeline=hybrid_rag_pipeline,
input_mapping={
  "query": [
    "text_embedder.text",
    "bm25_retriever.query",
    "ranker.query",
    "prompt_builder.question"
  ]
},
out_mapping={
  "llm.replies": "replies",
  "retriever.documents": "documents"
})
```

一旦封装为 SuperComponent，我们可以像下面所示执行查询：

```python
native_rag_sc.run(query=question)
hybrid_rag_sc.run(query=question)
```

这种方法可以作为快速将流水线封装为组件。但它有局限性——如果我们想更改生成器组件或文档存储，必须手动修改流水线本身。对于可以变量作为输入的 SuperComponents，我们看下一种方法。

### 方法 2 – 构建运行时配置的 SuperComponent

运行时配置。我们没有使用硬编码值，而是接受诸如嵌入模型、LLM 模型和 `top-k` 检索设置等参数。这种设计确保了我们的 SuperComponent 不仅仅是一个静态脚本，而是一个灵活的对象，在尝试构建流水线之前就会检查必要的凭据（例如模型 API 密钥）。

一旦设置好，我们就移动到 `build_pipeline` 方法定义的内部组装逻辑。在这里，我们初始化了任务所需的特定组件：用于查询向量化的文本编码器、用于搜索我们 Elasticsearch 存储的检索器、用于格式化上下文的提示构建器以及用于生成最终答案的生成器。注意，你可以选择不同的生成器（例如 `OllamaGenerator`）来使用 OpenAI 以外的模型。

我们创建一个标准的 `Pipeline()` 对象并将这些组件添加到其中。然后按照之前的步骤进行：使用 `pipeline.connect()` 方法显式地连接组件，绘制特定的线条，允许数据从嵌入器的输出流向检索器，从检索器找到的文档流向提示构建器。

最后，我们使用输入和输出映射抽象了这种内部复杂性。这一步将流水线转换为单个、易于使用的组件。我们定义了一个输入映射，接收单个外部查询并将其内部路由到嵌入器和提示构建器。同样，我们定义了一个输出映射，仅暴露用户关心的内容：LLM 的最终回复和用于参考的检索文档。通过这样封装逻辑，我们将组装线复杂的布线隐藏在组件内部，仅向我们应用程序的其他部分暴露一个简单、清晰的接口。

通过使用 `@super_component` 装饰器和专用的类结构，我们将流水线从静态脚本转变为灵活的软件组件，它可以无缝地集成到更大的应用程序中。现在我们来查看多模态流水线。

# 构建多模态流水线：图像、文本和音频

在之前的章节中，我们建立了用于处理和检索文本的健壮流水线。然而，现实世界的知识通常锁定在非文本格式中，如图像、图表和音频录制。本节将我们的架构扩展到**多模态 RAG**。

与基于文本的 RAG 不同（后者的查询和文档共享单一模态（文本）和同质向量空间），多模态 RAG 必须跨越**模态差距**。我们无法直接在数学上将文本查询与二进制图像文件进行比较。为了解决这个问题，我们采用了两种主要策略：**联合嵌入**（将不同模态映射到共享的向量空间）和**模态翻译**（在索引之前将视觉或音频信号转换为文本）。

我们将通过一系列笔记本来探索这些策略，从基础的图像处理进展到复杂的视听推理。

# 策略 1 – 使用 CLIP 进行联合嵌入

第一种方法使用如对比语言-图像预训练（CLIP）等模型模型等模型，这些模型经过训练，可以将图像和文本投影到相同的潜在向量空间。这允许我们使用自然语言搜索图像，而无需任何显式标记。



此笔记本介绍了`sentence-transformers`文档图像嵌入 Haystack 组件。通过使用`sentence-transformers/clip-ViT-L-14`（https://huggingface.co/sentence-transformers/clip-ViT-L-14）模型，我们可以将本地图像文件转换为向量表示，这些向量在几何上与其文本描述的向量接近。通过此笔记本，我们可以从查询创建文本嵌入，从示例图像创建图像嵌入，计算余弦相似度得分，并查看哪个图像匹配文本描述。

为了在大规模上应用，我们必须构建一个能够处理混合数据流的索引流水线。

该流水线使用`FileTypeRouter`组件根据其 MIME 类型分离传入的文件：

- **文本分支**：PDF 被路由到标准的`PyPDFToDocument`转换器和文本嵌入器。

- **图像分支**：图像被路由到专门的`ImageToFileDocument`转换器和图像嵌入器。

- **胶水**：我们使用了一个自定义组件`ImagePathFixer`。标准转换器可能只存储文件名，但嵌入组件通常需要绝对路径。该组件在向量化之前解析这些路径。

我们可以在以下 URL 以 Mermaid 图表的形式可视化该流水线：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch4/jupyter-notebooks/multimodal/images/multimodal_indexing_clip.png。

结果是一个统一的`InMemoryDocumentStore`，其中图像和文本共存，可以通过单个查询嵌入模型进行搜索。让我们看看第二种策略。

# 策略 2 – LLM 内容提取和视觉 RAG

CLIP 在视觉相似性方面表现出色，但它缺乏读取图像内详细文本或分析复杂图表的能力。针对这一点，我们转向了视觉 LLM（如 GPT-4o）。

在此工作流中，我们用`LLDocumentContentExtractor`替换了图像嵌入器。该组件将图像发送到视觉 LLM 以生成详细的文本描述。然后我们使用高性能文本模型如`mixedbread-ai/mxbai-embedded-large-v1`（https://huggingface.co/mixedbread-ai/mxbai-embedded-large-v1）对此描述进行嵌入。这有效地将图像转换为文本，允许我们使用标准的、精度检索工具。

可以在这里找到探索完整索引流程的 Jupyter Notebook：https://github.com/PacktPublishing/Building-Natural-language-and-LLM-Pipelines/blob/main/ch4/jupyter-notebooks/multimodal/multimodal_indexing.ipynb

与之前一样，我们可以用路由器路由不同类型的文档，以便我们使用文本嵌入模型处理 PDF 文件，使用视觉模型处理图像文件。Mermaid 图表可见：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch4/jupyter-notebooks/multimodal/multimodal_indexing.png（注：此处根据上下文逻辑推断原文可能指向图表路径）。

让我们拆解此索引流水线的关键：

- **文本分支**：PDF 被路由到标准的`PyPDFDocument`转换器。

- **图像分支**：图像被路由到视觉模型。

- **胶水**：我们使用`DocumentJoiner`组件连接处理后的文档。

一旦完成索引过程，我们就可以像开发简单和混合 RAG 那样构建流水线。这将允许我们使用自然语言从文本和图像中检索信息。

带有视觉模型且完整 RAG 流程的 Jupyter Notebook 可以在这里找到：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch4/multimodal/4_vision_llm.ipynb

该流水线代表了多模态 RAG 的先进水平。它采用了代理搜索、源回答逻辑：

- **检索**：我们使用索引期间生成的文本搜索文档存储。

- **生成**：找到相关文档后，流水线将原始图像（在元数据中）传递给生成器。

在这种场景下，视觉 LLM 查看原始照片来回答问题。它不是依赖代理描述，而是对源材料进行视觉推理。

这种架构提供了最高的保真度。文本描述仅作为索引键，而生成步骤保留了原始模态的全分辨率。我们可以在此 URL 查看流水线的 Mermaid 图表：https://github.com/PackPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch4/jupyter-notebooks/multimodal/images/multimodal_rag.png。

如果不解决主要的音频类型：音频，我们对多模态的探索将是不完整的。从会议到客户互动，大量的企业智能锁定在 `.wav` 和 `.mp3` 文件中。

# 处理音频数据

与空间图像不同，音频必须线性化（即转录为文本）才能兼容向量搜索。

演示音频集成的 Jupyter Notebook 在这里：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch4/jupyter-notebooks/multimodal/S_audio_transcription_whisper.ipynb。

该流水线使用`RemoteWhisper`转录器将音频文件（如 `.wav` 或 `.mp3`）转换为文本文档。一个关键步骤是使用配置为`split_by="sentence"`的`DocumentsSplitter`进行分割，如图 4.2 所示。这会将长转录分割成语义连贯的块（例如 10 个句子），确保检索系统可以确定对话中的特定时刻，而不是检索整个小时的录音。该流水线作为一个简单的音频转录器，但可以扩展以处理本章中的各种文件。我们将通过对流水线并行和异步用法的简要概述来结束本章。

## 并行化与异步流水线

在之前的章节中，我们构建了结构严谨且功能丰富的流水线。然而，单凭结构本身并不能保证性能。当我们从本地原型转向生产环境时，系统的延迟变成了一个关键限制。现实世界的 RAG 应用很少受限于 CPU 速度，而是受限于输入/输出（I/O）操作。它们的大部分执行时间都花在了等待上，例如等待嵌入 API 返回向量、等待数据库执行搜索或等待 LLM 生成标记。

在标准的同步流水线中，这些等待时间是累积的。如果一个混合搜索需要一次稠密检索（0.5 秒）和一次稀疏检索（0.5 秒），用户需要等待 1.0 秒。为了解决这个问题，Haystack 引入了并行化和异步流水线。这种高级能力允许我们解耦独立操作，通过并发执行它们来大幅降低延迟。

你可以在此处找到实现此异步架构的 Jupyter notebook：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch4/jupyter-notebooks/async_hybrid_pipeline.ipynb。

异步流水线的构建方法与标准版本几乎相同，我们只需实例化`AsyncPipeline`类而不是`Pipeline`类。

虽然它在添加和连接组件方面与`Pipeline`类共享类似的 API，但其内部执行图完全不同。

`AsyncPipeline`类通过分析有向多图来识别独立分支。在线性流水线（A -> B -> C）中，没有并行空间；B 依赖于 A。在分支流水线（A -> B 和 A -> C）中，节点 B 和 C 共享对 A 的依赖，但彼此独立。`AsyncPipeline`会识别这种拓扑结构并将 B 和 C 作为并发的`asyncio.Task`对象调度。

`AsyncPipeline`提供了专门的方法来处理不同的应用需求，特别是对于感知延迟至关重要的聊天应用：

- `run_async()`：此方法异步执行执行流水线。它是非阻塞的，是 Web 服务器（如 FastAPI）在并发处理多个用户查询时的理想选择。

- `run_async_generator()`：这是一个流式方法。它会在组件完成时产生部分输出。

这种流式能力对于调试和用户反馈特别有用。我们不需要等待整个过程结束，可以在检索器完成工作之前看到 LLM 才开始生成。

通过掌握并行化，开发者可以确保他们的流水线不仅智能，而且能够响应实时用户交互的严苛需求。这标志着在 Haystack 2.0 中整合组件的全面指南到此结束。我们已经从图的基础布局移动到了异步执行的复杂优化，为现代 NLP 架构提供了清晰的蓝图。

## 总结

在本章中，我们从 Haystack 2.0 的理论概念转向具体的实现。我们探索了有向多图的核心设计哲学，它允许分支和灵活的数据流。

我们构建了三种不同类型的流水线：

- **索引流水线（Indexing pipelines）**：我们使用`FileRouter`构建了一个健壮的接入系统，同时处理结构化（CSV）和非结构化（PDF、Web）数据，并将它们归一化到统一的文档存储中。

- **RAG 流水线（RAG pipelines）**：我们从仅依赖语义嵌入搜索的原始 RAG 系统进化到了混合 RAG 系统。混合方法引入了分支，将稀疏检索（BM25）的精确与稠密检索的上下文理解相结合，并通过重排序器（reranker）进行优化以获得最佳准确率。

- **多模态流水线（Multimodal pipelines）**：我们弥合了文本、图像和音频之间的差距。我们使用 CLIP 实现联合嵌入以通过文本搜索图像，并使用视觉 RAG 允许 LLM 直接看到并推理图像。

最后，我们使用`SuperComponents`对这些工作流进行了模块化优化，并使用异步流水线进行并行执行。在下一章中，我们将深入研究自定义组件的创建，特别是将开发自定义组件从文档语料中生成知识图谱和合成数据。然后，在第 6 章，我们将自定义组件与原始和混合 RAG `SuperComponents`结合，系统地评估这两种方法并在更改嵌入模型时衡量性能。

## 订阅免费电子书

新框架、演进中的架构、研究发布、生产环境分析——AI_Distilled 将噪音过滤掉，为亲手操作 LLM 和生成式 AI 系统的工程师和研究人员提供每周简报。现在订阅即可获得免费的电子书，以及帮助您保持专注和掌握资讯的每周见解。

访问 https://packt.link/80z6Y 订阅或扫描下方的二维码。

![](img/502c06ed872898cf88f18f9c142647a5_148_0.png)
