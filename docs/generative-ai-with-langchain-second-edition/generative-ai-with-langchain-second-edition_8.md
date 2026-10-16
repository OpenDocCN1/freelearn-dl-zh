# 附录

本附录作为与 LangChain 集成的主要 LLM（大语言模型）提供商的实操参考指南。当你使用书中介绍的技巧来开发开发应用时，你将需要连接到各种模型提供商，每个提供商都有自己的身份验证机制、功能和集成模式。

我们将首先介绍主要 LLM 提供者的详细设置说明，包括 `OpenAI`、`Hugging Face`、`Google` 等。对于每个提供商，我们将详细介绍创建账户、生成 API 密钥以及配置开发环境以通过 LangChain 使用这些服务的流程。最后我们将以一个实际实现示例结束，该示例演示了如何处理超过 LLM 上下文窗口的内容——具体来说是使用 LangChain 的 `map-reduce` 技术总结长视频。这种模式可以适用于各种场景，即你需要处理海量文本、音频转录或其他或其他无法放入单个 LLM 上下文中的内容。

## OpenAI

`OpenAI` 仍然是最受欢迎的 LLM 提供商之一，提供了适用于不同任务的各种能力水平的模型，包括 `GPT-4` 和 `GPT-o1`。LangChain 提供了与 `OpenAI` API 的无缝集成，支持其传统的补全模型（`completion models`）和聊天模型（`chat models`）。这些模型中的每一个都可以通过 LangChain 调用。

为了使用 `OpenAI` 模型，你首先需要获取一个 API 密钥。要获取 `OpenAI` API 密钥，请遵循以下步骤：

1. 在 `https://platform.openai.com/` 登录。
2. 设置账单信息。
3. 在个人密钥页面查看 API 密钥。
4. 点击创建新的密钥。

获取密钥后，你应该在环境变量中设置 `OPENAI_API_KEY`。或者，在初始化类时也可以将密钥作为参数传递。

`OpenAI` 提供了一套全面的功能，与 LangChain 集成：

## Hugging Face

`Hugging Face` 是自然语言处理（NLP）领域非常著名的参与者，提供了预训练模型、数据集和空间（`spaces`）。LangChain 与 `Hugging Face` 集成，允许你使用数以千计的模型。

要使用 `Hugging Face`，你需要一个 API 令牌。你可以通过 `https://huggingface.co/` 创建账户并导航到设置来获取。一旦你有了令牌，请将其设置为环境变量 `HUGGINGFACE_TOKEN`。

LangChain 提供了多种与 `Hugging Face` 交互的方法，包括：

- `HuggingFacePipeline`：用于本地运行模型。
- `HuggingFaceChat`：用于与 `Hugging Face` 上的模型交互。
- `HuggingFaceEmbeddings`：用于生成嵌入。

## Google

Google 提供了两个主要平台来访问其 LLM，包括最新的 `Gemini` 模型：

### 1. Google AI

`Google AI` 平台为开发者和用户提供了简单的设置，并可以访问最新的 `Gemini` 模型。通过 `Google AI` 使用 `Gemini` 模型：

- **Google 账户**：标准的 Google 账户即可进行身份验证。
- **API 密钥**：生成 API 密钥进行身份验证。
  - 访问此页面创建 API 密钥：`https://ai.google.dev/gemini-api/docs/api-key`

获取 API 密钥后，在环境中设置 `GOOGLE_API_KEY`。

### 2. Google Cloud Vertex AI

对于企业级功能和集成，`Google` 的 `Gemini` 模型可以通过 `Google Cloud Vertex AI` 平台使用。通过 `Vertex AI` 使用模型：

1. 创建 `Google Cloud` 账户，这需要接受服务条款并设置账单。
2. 安装 `gcloud` CLI 以与 `Google Cloud` 服务交互。参考安装说明 `https://cloud.google.com/sdk/docs/install`。
3. 运行以下命令进行身份验证并获取令牌：`gcloud auth application-default login`
4. 确保你的 Google Cloud 项目已启用 `Vertex AI` API。
5. 你可以设置 Google Cloud 项目 ID - 例如，使用 `gcloud` 命令：

```bash



`gcloud config set project my-project`

其他方法是在初始化 LLM 时传递构造函数参数、使用`aiplatform.init()`或设置 GCP 环境变量。

如果你尚未启用相关服务，你将收到一条实用的错误信息，引导你前往正确的网站，并在那里点击“启用”。你需要根据偏好和可用性启用 Vertex 或 Generative Language API。

LangChain 提供了与 Google 服务的集成，例如语言模型推理、嵌入、从不同源摄入数据、文档转换和翻译。

主要有两个集成包：

- `langchain-google-vertexai`
- `langchain-google-genai`

我们将使用`langchain-google-genai`，这是 LangChain 推荐给个人开发者使用的包。设置非常简单，只需要一个 Google 账号和 API 密钥。对于较大的项目，建议迁移到`langchain-google-vertexai`。该集成提供了企业级功能，如客户加密密钥、虚拟私有云集成等，需要开通计费的 Google Cloud 账户。

如果你遵循了上一节中提到的 GitHub 上的说明，你应该已经安装了`langchain-google-genai`包。

![](img/b4e68161fea00e7befce1e978c814dda_725_0.png)

## 其他提供者

- **Replicate**: 你可以使用 GitHub 凭据在 https://replicate.com/ 上进行身份验证。然后点击左上角的用户图标，即可找到 API 令牌——只需复制 API 密钥并设置为环境变量`REPLICATE_API_TOKEN`。若要运行更大的任务，你需要设置信用卡（在计费下）。

- **Azure**: 通过 GitHub 或 Microsoft 凭据进行验证，我们可以在 https://azure.microsoft.com/ 创建 Azure 账户。我们可以在 **Cognitive Services | Azure OpenAI** 下创建新的 API 密钥。

- **Anthropic**: 你需要设置`ANTHROPIC_API_KEY`环境变量。请确保你已经在 https://console.anthropic.com/ 设置了计费并向控制台充值。

## 总结长视频

在*第 3 章*中，我们演示了如何使用 map-reduce（映射-还原）方法总结长视频（无法放入上下文窗口的视频）。我们使用 LangGraph 设计了这样的工作流。当然，你也可以将相同的方法用于类似的情况——例如总结长文本或从长音频中提取信息。现在让我们仅使用 LangChain 来同样的操作，因为这将是一次对我们有帮助的练习。

首先，`PromptTemplate`不支持媒体类型（截至 2025 年 2 月），因此我们需要手动将输入转换为消息列表。为了使用参数化链，作为替代方案，我们将创建一个 Python 函数，该函数接收参数（始终通过名称提供）并创建一个待处理的消息列表。每条消息都会指示 LLM 总结视频的某部分（通过偏移量间隔进行拆分），这些消息可以并行处理。输出将是一个字符串列表，每个字符串总结原始视频的一个子部分。

当你在 Python 函数声明中使用额外的星号（`*`）时，意味着星号之后的参数只能通过名称提供。例如，让我们创建一个带有许多参数的简单函数，我们可以在 Python 中通过仅传递少数（或不传递）参数以不同的方式调用它：

```python
def test(a: int, b: int = 2, c: int = 3):
    print(f"a={a}, b={b}, c={c}")
    pass

test(1, 2, 3)
test(1, 2, c=3)
test(1, b=2, c=3)
test(1, c=3)
```

但如果你更改了签名，第一次调用将抛错误：

```python
def test(a: int, b: int = 2, *, c: int = 3):
    print(f"a={a}, b={b}, c={c}")
    pass

# 这样再不起作用了: test(1, 2, 3)
```

如果你查看 LangChain 的源码，可能会经常看到这种写法。这就是为什么我们决定详细解释一下它。

现在，回到我们的代码。如果我们想将`video_uri`作为输入参数，仍然需要运行两个独立的步骤。当然，我们可以将这些步骤封装成 Python 函数，但作为替代方案，我们将所有内容合并到一个链中：

```python
from langchain_core.runnables import RunnableLambda

create_inputs_chain = RunnableLambda(lambda x:
    _create_input_messages(**x))

map_step_chain = create_inputs_chain |
    RunnableLambda(lambda x: map_chain.batch(x, config={"max_concurrency": 3}))

summaries = map_step_chain.invoke({"video_uri": video_uri})
```

现在，让我们所有提供的总结合并到一个单个提示中，并让 LLM 准备一份最终总结：

```python
def merge_summaries(summaries: list[str], interval_secs: int = 600, **kwargs) -> str:
    sub_summaries = []
    for i, summary in enumerate(summaries):
        sub_summary = (
            f"Summary from sec {interval_secs} to sec {(i+1)*interval_secs}:",
            f"\n{summary}\n"
        )
        sub_summaries.append(sub_summary)
    return "".join(sub_summaries)

reduce_prompt = PromptTemplate.from_template(
    "你被给了一份分段视频总结列表。\nSUMMARIES:\n{summaries}\n基于此，请准备一份视频的最终总结。"
)

reduce_chain = (
    RunnableLambda(lambda x: merge_summaries(x["summaries"]))
    | reduce_prompt
    | llm
    | StrOutputParser()
)

final_summary = reduce_chain.invoke({"summaries": summaries})
```



- 精通生成式 AI 和代理系统（agentic systems）的核心原理
- 理解 AI 代理体如何在动态环境中运行、推理和适应
- 使 AI 代理体能够分析自身的行为并进行现场发挥
- 实现 AI 代理体可以利用外部工具并规划复杂任务的系统
- 应用方法增强 AI 的透明度、问责性和可靠性
- 探索 AI 代理体在各行业的真实世界实现

# Packt 正在寻找像你这样的作者

如果你感兴趣成为 Packt 的作者，请访问 `authors.packtpub.com` 并立即申请。我们已经与成千上万像你这样的开发者和技术专业人员合作，帮助他们将见解分享给全球技术社区。您可以进行通用申请、申请我们正在招募作者的特定热门话题，或者提交自己的想法。

# 分享你的观点

现在你已经完成了《基于 LangChain 的生成式 AI》（第二版），我们非常想听听你的想法！如果你是从亚马逊购买的手本书，请[点击此处直接进入该书的亚马逊评论页面](https://www.amazon.com/dp/B08N5K7B6) 并分享你的反馈，或在购买书籍的网站上留下评论。

你的评论对我们以及技术社区至关重要，将帮助我们确保交付高质量的内容。

# 索引

# A

- 自适应系统 (adaptive systems)
  - 构建 248
  - 动态行为调整 (dynamic behavior adjustment) 248
  - 人机回 (human-in-the-loop) 248-250
- 高级记忆机制 (advanced memory mechanisms) 411
- 高级工具调用能力 (advanced tool-calling capabilities) 209, 210
- 代理化 AI (agentic AI) 9
- 代理化架构 (agentic architectures) 224-226
  - 模式 (patterns) 225, 226
- 代理式 RAG 157
- 代理内存 (agent memory) 266
  - 缓存 (cache) 263
  - 存储 (store) 264, 265
- 代理体 (agents) 3, 216
  - 规划与解决代理 (plan-and-solve agent) 217-220
- AI21 Labs Jurassic 30
- AI 代理体 (AI agents) 11, 12
  - 考量 (considerations) 13
  - 重大挑战 (significant challenges) 12

# Amazon Bedrock 31

# Annoy 126

## Anthropic
- 引用链接 431

# Anthropic Claude 30, 287-289

# API key 设置 28-31

# 应用程序默认认证 (ADC) 28

# 应用程序接口 (APIs) 7

# 近似最近邻 (ANN) 126

# 通用人工智能 (AGI) 406

# 人工智能 (AI) 2

# 自动化评估方法 (automated evaluation methods) 320, 321

# 自主代理体 (autonomous agents) 410

## Azure
- 引用链接 431

# Azure OpenAI Service 31

# B

# BERT 7

# 大科技公司 (Big tech)
- 对比中小型企业 (versus small enterprises) 407, 408

# BLOOM 429

# LangChain 构建块 (building blocks, LangChain)
- LangChain 表达式语言 (LCEL) 42-44
- 模型接口 (model interfaces) 32
- 提示词模板 (prompt templates) 40
- 内置 LangChain 工具 (built-in LangChain tools) 192-198

# C

- 链链式提示词 (chaining prompt) 88, 89
- 思维链 (CoT) 90-92
- 对话历史 (chat history)
  - 修剪 (trimming) 97, 98
- Chinchilla 缩放法则 (Chinchilla scaling law) 5
- 分块策略 (chunking strategies) 132
  - 基于代理的分块 (agent-based chunking) 135
  - 特定于文档的分块 (document-specific chunking) 134
  - 固定大小分块 (fixed-size chunking) 132
  - 多模态分块 (multi-modal chunking) 136
  - 递归字符分块 (recursive character chunking) 133, 134
  - 选择 (selecting) 136, 137
  - 语义分块 (semantic chunking) 134, 135
- Claude 7
- 云提供商网关 (cloud provider gateways) 31
- 代码模型 (code LLMs)
  - 基准测试 (benchmarks) 273, 274
  - 演变 (evolution) 271-273
- 结合 LLM 的代码 (code, with LLMs)
  - 代理方法 (agentic approach) 289, 290
  - Anthropic Claude 287-289
  - 文档 RAG (documentation RAG) 290-292
  - Google 生成式 AI (Google generative AI) 282, 283
  - Hugging Face 284-287
  - 仓库 RAG (repository RAG) 293-295
  - 编写 (writing) 282

# Cohere 模型 30

# 通信协议 (communication protocols) 231, 232

# 复杂集成应用 (complex integrated applications) 10

# 概念级建模 (concept-level modeling) 412

# Conda 27

# 共识机制 (consensus mechanism) 229-231

# 上下文处理 (context processing) 145
- 上下文压缩 (contextual compression) 145
- 最大边界相关性 (MMR) 146

## 上下文窗口 (context window)
- 使用 (working with) 93, 94

# 持续集成与持续交付 (CI/CD) 流水线 373

# 受控输出生成 (controlled output generation) 76
- 错误处理 (error handling) 79-81
- 输出解析 (output parsing) 76-79

# 企业文档聊天机器人 (corporate documentation chatbot)
- 开发 (developing) 161, 162
- 文档加载 (document loading) 162-165
- 文档检索 (document retrieval) 166-168
- 评估与性能考量 (evaluation and performance considerations) 177, 178
- 与 Streamlit 集成 (integrating, with Streamlit) 174-176
- 语言模型设置 (language model setup) 165, 166
- 状态图设计 (state graph, designing) 168-173

# 企业文档管理器工具 (Corporate Documentation Manager tool) 161

# 纠错检索增强生成 (CRAG) 155, 156

# 自定义工具 (custom tools) 199
- `BaseTool` 205, 206
- 从 `Runnable` 创建 (creating from Runnable) 202-205
- 函数工具 (function, as tool) 199

# D

## DALL-E 模型
- 使用 OpenAI 55, 56

## 数据质量 (data quality)
- 演变 (evolution) 412, 413
- 技术进展 (via technical advances) 413, 414

# DeepSeek 模型 30

## 依赖项 (dependencies)
- 设置 (setting up) 26, 27

# 有向无环图 (DAG) 68

# 分布式方法 (distributed approach) 410, 411

# Docker 27

# 文档 RAG (documentation RAG) 290-292

## 文档处理 RAG 流水线 (document processing, RAG pipeline)
- 分块策略 (chunking strategies) 132
- 检索 (retrieval) 137

- 动态少样本提示词 (dynamic few-shot prompting) 89, 90

# E

# 效率创新 (efficiency innovations) 410
- 关键技术 (key techniques) 410

## 邮件提取 (email extraction)
- 评估 (evaluating) 344-347

# 嵌入 (embeddings) 109, 114, 115
- 挑战 (challenges) 115
- 迁移搜索 (migrating to search) 113

# 错误处理 (error handling) 79-81, 206-209
- 重试 (retries) 82, 83

- 外部合作伙伴包 (external partner packages) 21

# F

# 回退 (fallback) 84

# Faiss 126

## FastAPI
- Web 框架部署 (for web framework) 354-358

## 少样本提示词 (few-shot prompting)
- 对比零样本提示词 (versus zero-shot prompting) 87, 88

# FizzBuzz 282

# 基础模型编排 (Foundational Model Orchestration, FOMO) 353

# G

## gcloud CLI
- 安装 (installation) 430

## Gemini 1.5 Pro
- 使用 (using) 58-60

## 生成式 AI 应用 (generative AI applications)
- 部署 (deploying) 353

# 生成式 AI 经济与行业转型 (generative AI economic and industry transformation) 415-417
- 竞争动态 (competitive dynamics) 417, 418
- 分布 (distribution) 419, 420
- 公平考量 (equity considerations) 419, 420
- 特定行业转型 (industry-specific transformations) 417, 418
- 职业演变与技能影响 (job evolution and skills implications) 418
- 局限 (limitations) 401
- 对比人类认知 (versus human cognition) 403, 404

# Google AI 平台 (Google AI platform) 430

# Google Cloud Vertex AI (Google Cloud Vertex AI) 330

# Google Colab (Google Colab) 26

# Google Gemini (Google Gemini) 30

- Google 生成式 AI (google generative AI) 282, 283
- Google Vertex AI 31
- GPT-4 7
  - 使用 (using) 61, 62
- Gradient Notebooks (Gradient Notebooks) 26
- 图配置 (graph configuration) 75, 76
- 图 (graphs) 69

# H

- HF 数据集 (HF datasets and Evaluate)
  - 使用基准测试评估 (benchmark, evaluating with) 343
- GPT-II 51
- GPT-4 Vision
  - 使用 (using) 61-63
- 分层可导航小世界网络 (Hierarchical Navigable Small World, HNSW) 121
- `hnswlib` 226
- Hugging Face 50, 284-287, 429
- HuggingFace 推理终端 (HuggingFace Inference Endpoints) 31
- 人类认知对比生成式 AI 模型 (human cognition versus generative AI models) 403, 404
- 人机回 (Human-in-the-Loop, HIL) 232
  - 评估 (evaluation) 321
- 混合检索 (hybrid retrieval)
  - 密集检索方法 (dense retrieval method) 140
  - 稀疏检索方法 (sparse retrieval method) 140
- 假设文档嵌入 (Hypothetical Document Embeddings, HyDE) 144, 145

# I

- 图像理解 (image understanding) 58
  - Gemini 1.5 Pro, 使用 (using) 58-60
  - GPT-4 Vision, 使用 (using) 61-63

## 索引 (indexes)
- 迁移检索系统 (migrating to retrieval systems) 108, 109
- Inflection Pi 30
- 基础设施即代码 (Infrastructure as Code, IaC) 378
- LLM 基础设施考量 (infrastructure considerations, LLMs) 377, 378
  - 部署模型，选择 (deployment model, selecting) 378, 379
  - 模型服务基础设施 (model serving infrastructure) 380-382

# J

- 职业演变与技能影响 (job evolution and skills implications)
  - 长期转变 (long-term shifts, 2045 and beyond) 419
  - 中期影响 (medium-term impacts, 2035-2045) 418
  - 短期影响 (near-term impacts, 2025-3035) 418

# K

- Kaggle Notebooks 26
- KM 缩放法则 (KM scaling law) 5

# L

- LangChain 14
  - 代理开发 (agent development) 16
  - 构建块 (building blocks) 32
  - 集成 (integrations) 281
  - 实现能力 (implementations capabilities) 274
  - 第三方应用 (third-party applications) 22, 23
  - 视觉化工具 (visual tools) 22, 23

# LangChain 与数据集 (LangChain, with datasets)
- 创建 `pandas` DataFrame (`pandas` DataFrame, creating) 301-303
- 问答 (Q&A) 303-306

# langchain-anthropic 20

# LangChain 应用 (LangChain applications)
- 成本管理 (cost management) 391
- 模型选择策略 (model selection strategies) 391
- 监控与成本分析 (monitoring and cost analysis) 395
- 其他策略 (other strategies) 394
- 输出 Token 优化 (output token optimization) 394

# LangChain 架构 (LangChain architecture)
- 优势 (advantages) 19
- 核心结构 (core structure) 20
- 生态系统 (ecosystem) 18
- 探索 (exploring) 17
- 库组织 (library organization) 20
- 模块化设计与依赖管理 (modular design and dependency management) 19

# langchain-core 20

# langchain-experimental 20

# LangChain 表达式语言 (LangChain Expression Language, LCEL) 16, 42-44
- 复杂链示例 (complex chain example) 45-47
- 工作流 (workflows) 44, 45

# langchain-openai 20

## LangChain 检索器 (LangChain retrievers)
- 高级/专门检索器 (Advanced/Specialized Retrievers) 138
- 算法检索器 (Algorithmic Retrievers) 138
- 核心基础设施检索器 (Core Infrastructure Retrievers) 138
- 外部知识检索器 (External Knowledge Retrievers) 138
- 集成检索器 (Integration Retrievers) 138

# LangGraph 21
- 平台 (platform) 247, 370, 371
- 流式模式 (streaming modes) 241-243
- 工作流构建 (workflow building) 95-97

# LangGraph 检查点 (LangGraph checkpoints) 101-103

# LangGraph CLI
- 用于本地开发的使用 (using, for local development) 371-373

# LangGraph 基础 (LangGraph fundamentals) 68
- 受控输出生成 (controlled output generation) 76
- 图配置 (graph configuration) 75, 76
- 减少器 (reducers) 73-75
- 状态管理 (state management) 69-73

# LangSmith 21, 387-389
- 使用基准测试评估 (benchmark, evaluating with) 339-342

# 语言代理树搜索 (Language Agent Tree Search, LATS) 225

# 大语言模型 (large language model, LLM) 1
- 复杂的集成化应用 10
- 局限性 9, 14, 15

# LATS 方法 261

# Llama 2 8

# Llama.cpp 51

# LLM 代理评估
- 最佳实践 323-335
- 能力 316-320
- 方法论与方法 320-323
- 离线评估 336-347

# 用于数据科学的 LLM 代理
- 应用 295-297
- 数据集分析 301
- ML 模型，训练 297

# LLM 应用
- 偏见检测与监控 387
- 持续改进 390
- 部署 353, 354
- 幻觉检测 386
- 可观测性策略 389
- 观察 382
- 运行指标 383
- 响应跟踪 384-386
- 安全考量 350-352

# LLM 应用部署
- LangChain 应用的考量 365-370
- 基础设施考量 377, 378
- LangGraph 平台 370, 371
- 模型上下文协议 (MCP) 375-377
- 使用 Ray Serve 进行可扩展部署 358
- 无服务器部署选项 374
- UI 框架 375
- 使用 FastAPI 进行 Web 框架部署 354-358

# LLM 评估
- 构建共识 315, 316
- 性能与效率 312, 313
- 安全与对齐 311, 312
- 显著性 310, 311
- 用户与利益者价值 313-315

# LLM 家族 30

# LLM 生成的代码
- 验证框架 279-281

# LLMOps 353, 378

# 软件开发中的 LLM 268
- 代码 LLM 基准测试 273, 274
- 代码 LLM 演进 271-273
- 实现考量 269-271
- 工程方法 274-277
- 开发的未来 269
- LangChain 集成 281
- 安全与风险缓解 277-279

# 本地模型
- Hugging Face 模型 50, 51
- Ollama 49
- 运行 48
- 使用 51-54

# 长期记忆 262

# 长视频
- 总结 432-434

# M

# Map 方法 94

# 最大边缘相关性 (MMR) 140

# MCP 客户端 375

# MCP 服务 375

# 记忆 2

# 记忆机制 97
- 聊天历史，保存到数据库 99-101
- 聊天历史，修剪 97, 98
- LangGraph 检查点 101-103

# Miniconda
- 下载链接 26

# Mistral 模型 30

# Mixtral 7

# ML 模型
- 代理，请求构建神经网络 298
- 代理执行与结果 299-301
- 支持 Python 的代理，设置 297

# MLOps 353

# 模型上下文协议 (MCP) 375

# LangChain 模型接口 32
- 聊天模型，使用 34, 35
- 开发测试 33
- LLM 交互模式 32, 33
- 模型行为，控制 38, 39
- 应用的参数选择 40
- 推理模型 36-38

# 模型许可证
- 引用链接 8

# 模型开放框架 (MOF) 63

# 模型缩放法则
- Chinchilla 缩放法则 5
- KM 缩放法则 5

# 模型选择策略，LangChain 391
- 级联模型 393, 394
- 分层模型选择 391-393

# 现代 LLM 现状 2-4
- 许可 7, 8
- LLM 供应商 6, 7
- 模型比较 4-6

# 蒙特卡洛树搜索 (MCTS) 411
- 用于修剪 ToT 261, 262

# 多代理架构 227
- 通信协议 231-241
- 通过共享消息列表 245-247
- 共识机制 229-231
- 交接 (handoffs) 243, 244
- LangGraph 平台 247
- LangGraph 流式处理 241-243
- 角色与专业化 228, 229

# 多模态 AI 应用 54
- 图像理解 58
- 文本图像 55

# 多模态扩散 (MMDiT) 57

# N

# 注意力模型 (NAMs) 411

# 非度空间库 (`nmsib`) 127

# O

- Ollama 49
- OpenAI 427, 428
  - 引用链接 428

# OPENAI_API_KEY 28

# OpenAI GPT-4o 30

# LLM 应用的运行指标
- 可见性 383
- 延迟维度 383
- Token 经济 383
- 工具使用 383

# 输出修复器
- 引用链接 84

# 输出解析 76-79

# P

- 困惑度模型 30
- 规划解决代理 217-220

# 产品量化 (PQ) 126

# 提示词工程 40, 85
- 思维链 (CoT) 90-92
- 少样本提示 (few-shot) 87
- 提示词模板 85-87
- 自一致性 92, 93
- 零样本提示 (zero-shot) 87
- 提示词模板 40, 41, 85
  - 聊天提示词模板 41

# R

# RAG 架构
- 代理方法 226, 227

# RAG 落地模型 411

# RAG 流水线
- 高级技术 140
- 组件 127-129
- 文档处理 130-132
- 增强器 110
  - 组件 110-112
- 评估 336-338
  - 评估 317, 318
- 生成器 110
- 知识库 110
- 检索器 110
- 故障排除 178, 179

# RAG 技术
- 代理 RAG 157
- 上下文处理 145
- 修正 RAG 155
- 混合检索 140
- 查询转换 143, 144
- 重排序 141, 142
- 响应增强 146
- 选择 158-160

# Ray Serve
- 用于可扩展部署 358

# ReACT 188-191

# 推理模型 92

# 推理路径
- 探索 250
- 思维链 (ToT) 技术 250-261
- 使用 MCTS 修剪 261-262

# 减少方法 94

# 还原器 73-75

# 人类反馈强化学习 (RLHF) 3

# Replicate 31
- 引用链接 431

# 仓库 RAG 293-295

# 重排序
- 列表重排序器 142
- 成对重排序器 141
- 点重排序器 141

# 重排序实现
- Cohere 重排序 142
- 基于 LLM 的自定义重排序 143
- RankLLM 142

# 响应增强技术 164
- 自一致性检查 150-154
- 来源归属 147-150
- 重试 82, 83

## 检索器
- LangChain 检索器 138
- 遵循的模式 137
- 向量存储检索器 139, 140

# S

# 使用 Ray Serve 进行可扩展部署 358
- 应用运行 363-365
- 索引构建 359-361
- 索引服务 361-363

# 缩放方法 409
- 训练阶段缩放法则 415
- 缩放局限性 406
  - 大科技公司对比小企业 407, 408
  - 数据质量训练 412, 413
  - 通过技术进步民主化 413, 414
  - 假设挑战 406
  - 训练阶段缩放法则 415

# 缩放局限性，替代方法
- 分布式方法 410, 411
- 效率创新方法 410
- 传统方法 409

# 自一致性 92, 93

# 小企业
- 对比大科技公司 407, 408

# 小语言模型 (SLMs) 4

# 代码片段 108

# 社会影响 420
- 版权与归属挑战 422
- 错误信息与网络安全 421
- 监管与实施挑战 423

# SPTAG 127

# Stable Diffusion
- 使用 57

# 状态管理 69-73

# 结构化生成生成 84

# 填充方法 (stuff approach) 93

# 超步 72

# 系统级评估
- 最佳实践 322, 323

# T

# 测试时计算 410

# 文本嵌入推理 (TEI) 429

# 文本到图像应用 55
- 通过 OpenAI 使用 DALL-E 55, 56
- 使用 Stable Diffusion 57

# 心理理论 (ToM) 404

# 每个输出 Token 时间 (TPOT) 383

# 首 Token 延迟 (TTFT) 383

# Together AI 31

# 工具 2
- 内置 LangChain 工具 192-199
- 自定义工具 199
- 定义 192
- 错误处理 206-209
- 使用 LangChain 185-187

# 工具，整合到工作流
- 控制生成 210, 211
- 供应商提供的控制生成 212, 213
- 工具调用范式 214, 215
- `ToolNode` 213

# 传统方法
- 组件 409

# 传统数据库搜索

# 思维链 (ToT) 模式 223, 250-261
- 使用 MCTS 修剪 261, 262

# TypedDict 69

# U

## UI 框架
- Chainlit 375
- Gradio 375
- Mesop 375
- Streamlit 375

# 通用句子编码器 (USE) 320

# V

## 向量索引
- 策略 121-127

## 向量存储检索器
- 数据库检索器 139
- 词法检索器 139
- API 检索器 139

# 向量存储 115, 116
- 比较 117, 118
- 嵌入 118
- 硬件考量 119
- LangChain 中的接口 119, 120
- 模式 118

# 视觉中心增强 412

# Z

# 零样本提示 85-87
- 对比少样本提示 87, 88

## 下载本书的免费 PDF 副本

感谢购买本书！

你是否喜欢在旅途中阅读，但无法随身携带纸质书？

你购买的电子书是否兼容你选择的设备？

别担心，现在购买每本 Packt 书籍，都可以免费获得该书的无 DRM PDF 版本。

2. 提交您的购买证明。
3. 大功完成！我们将直接将免费 PDF 及其他福利发送到您的邮箱。

# 与 Packt 的生成式 AI 社区保持联系

如需获取有关 AI 最新趋势、工具和突破的每周更新，请订阅 `AI_Distilled`——这是 AI 专业人士、研究人员和创新者的首选通讯邮件，订阅地址：[AI_Distilled](https://packt.link/05Uyu)。如果您对此书有任何疑问，或想深入研究生成式 AI 和大语言模型（LLMs），欢迎加入我们的 Discord 服务器参与讨论：https://packt.link/4Bbd9，读者、爱好者和专家均可以在那里交流想法和见解。

| 通讯邮件二维码 | Discord 二维码 |
| --- | --- |
| ![QR Code 1] | ![QR Code 2] |
