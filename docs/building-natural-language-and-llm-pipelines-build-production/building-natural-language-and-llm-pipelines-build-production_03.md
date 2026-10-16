## 深入研究大语言模型

上一章帮助我们建立了通过流水线和生成式 AI 项目生命周期使用`LLM`解决问题的概念基础。

在本章中，我们将深化对`LLM`的理解，追踪它们从 2023 年的基础基准到 2025 年专门的智能体架构的演进。我们将从回顾定义了上一代模型和框架的核心原则开始，包括 **transformer** 架构以及`RAG`、提示（`prompting`）和微调（`fine-tuning`）的初始定义。随后，我们将探索 2024-2025 年的大专业化：从单体模型向两个主导类的范式转移：**小语言模型 (SLMs)** 和强大的**推理语言模型 (RLMs)**。这一分析将包括对 2025 年`DeepSeek-RI`发布的深度解析，这是一项向整个开发者社区开源高级推理能力的技术事件。对于开发者来说，这意味着一刀切时代的终结；架构师现在必须根据成本、延迟和认知深度特定权衡来选择模型。

接下来，我们将追踪开发者的实际演进。在本章结束时，你将准备好进行特定部署和优化。

在本章中，我们探讨了从通用模型向专用模型的转变，以及这种转变对开发者选择的影响。我们涵盖了以下内容：

- `LLM`概述
- 与模型交互
- 数据存储
- 推理

## 技术要求

在本书中，我们还需要：

- `Python` 3.11 或更高版本
- `uv`
- `VS Code`
- `Jupyter Notebook`
- `GitHub`账户
- `Miniconda`（可选）

要获取代码和练习，请按照如下方式克隆仓库：

1. 打开`VS Code`，点击“文件 | 新窗口”，然后点击“终端 | 新建终端”。确保你的终端是`Bash`或命令行类型。

2. 在终端中，输入以下每个命令（以`$`符号标识的命令）并按回车：

```bash
git clone https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines.git
cd building-natural-language-and-LLM-pipelines/
cd ch2
uv sync
source .venv/bin/activate
```

这将创建并激活一个包含执行`Jupyter` notebook 中代码所需的依赖的虚拟环境。每个文件夹将有不同的依赖项。每章推荐的工作流是在`VSCode`中打开一个新窗口，打开章节目录，初始化终端，运行`uv sync`安装依赖，然后运行

```bash
source .venv/bin/activate
```

此环境将作为`Jupyter` notebook 内核使用。

3. 通过`VS Code`扩展市场启用`Jupyter Notebook`扩展。打开笔记本时，点击“选择内核”并点击“Python 环境”，然后选择本地虚拟环境。它将被命名为`rag-haystack-ch2`并指向路径`./.venv/bin/python`。

4. 此外，我们将创建一个`.env`文件。在你的`ch2`目录中，点击文件选项卡创建新文件，并命名为`.env`。此`.env`文件包含`OpenAI API`等其他服务的凭据。作为示例，请查看 https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/ch2/.env.example。

5. 在动手练习期间，你有权选择使用`Ollama` (https://ollama.com/) 使用本地`LLM`或使用托管在`OpenAI`的模型。要获取本地模型，请下载并安装`Ollama`客户端（https://ollama.com/download），并在终端上运行`ollama pull <model-name>`从其模型中进行选择。

你可以在 https://ollama.com/search 找到可用模型。如果你倾向于使用`OpenAI API`，请创建一个帐户和 API 密钥（https://openai.com/api/）。

## LLM 概述

大语言模型（`LLMs`）是一类深度学习模型，专门用于以类似于人类交流的方式处理和生成文本。这些模型通常使用深度学习架构，如`transformer`（Vaswani 等人，2017, 2023），具有从数百到数十亿计的参数。它们通常在多样化的数据集上进行训练，包括书籍、网站或文章的合集。这使得它们能够生成句子、回答问题、撰写文章以及创作故事。训练使它们能够结合上下文并进行推理。由于其规模巨大，`LLMs`在训练和推理过程中需要大量的计算资源。

在接下来的子节中，我们将详细探讨`LLM`的常见应用，并深入理解它们的工作原理、存在的不同架构类型，以及我们可以采取哪些步骤来评估结果。

## LLM 的用例

我们概述了`LLM`应用的四个主要类别：理解、生成、检索和交互。

- **理解 – 经典 NLP**：`LLMs`可用于分析客户评论、社交媒体帖子或调查报告，以确定情感（积极、消极和中性）。它们还可以被用于文本分类（垃圾邮件检测、主题分类）以及命名实体识别。即通过识别并将文本中的实体分类为人员、组织和地理位置的过程。



- **生成 – 文本生成**：在这种情况下，内容创建包括目标是辅助生成不同形式的书面内容的任务，例如营销材料、社交媒体帖子等。它还包括诸如通过从自然语言描述生成代码片段来协助开发者等任务。`GitHub Copilot` 是由 `LLMs` 驱动的应用的典型范例，它协助开发者生成代码片段和文档。然而，在近年来，`Copilot` 已经从简单的代码补全演变为一种复杂的代理系统（`GitHub`, 2025）。这种演变是被称为“代理型 DevOps”（`agentic DevOps`）（`Microsoft`, 2025）的更广泛趋势的一部分，在这种范式中，智能代理与开发者协作，以自动化和优化软件生命周期的每个阶段。

另一个主要的模型家族是 `Anthropic` 的 `Claude`，包括 `Claude 3.5` 系列（`Sonnet`、`Opus` 和 `Haiku`）。例如，`Claude 3.5 Sonnet` 模型在研究生级推理（`GPQA`）和编码能力（`HumanEval`）方面设定了新的行业基准，在特定评估中通常优于 `GPT-4o` 等竞争对手模型（`Anthropic`, 2025; `Galileo`, 2025; `Dev`, `Nik L`, 2025）。

- **检索 – 将模型置于数据**：在 `LLMs` 应用于信息检索的情况下，应用包括通过搜索引擎理解查询的上下文来增强搜索，以及实现基于文档合集的问答系统。此外，`LLMs` 可能被用于从文档集中提取信息并对其进行转换，例如填充结构化数据库。

- **交互 – 对话代理和助手**：在 `LLMs` 应用于对话代理的情况下，流行的用例包括聊天机器人（如 `ChatGPT`）、虚拟助手和用于内容审核的机器人。这些应用可用于提供客户支持、回答常见问题（`FAQs`）并用自然语言与用户进行互动。它们还可以用于自动检测和过滤文本中的不当内容。

现在让我们来看看 `LLM` 在 2023 年至 2025 年之间的关键变化，以理解 `AI` 从简单的文本生成器到复杂的推理编排器的转变。这一时期标志着从单一的、“一刀切”模型向专业化格局的战略性转变，开发者必须在用于成本效益型边缘部署的超高效小语言模型（`SLMs`）和用于复杂多步逻辑推理的强大大模型（`RLMs`）之间做出选择。

## 2023 基准：基础 LLM 原理回顾

为了理解 2024 年到 2025 年之间发生的巨大变化，首先必须建立 2023 年的基础知识基准。

`LLMs` 是一类深度学习模型，旨在以类似于人类交流的方式处理和生成文本。驱动 `LLM` 革命的引擎是 `transformer` 架构。该架构在 2017 年的论文《Attention Is All You Need》（`Vaswani` 等人，2017，2023 修订）中引入，其设计从递归序列处理转变为标记（`tokens`）的并行处理。通过舍弃递归，`transformer` 允许在训练期间实现多得并行化，在输入和输出之间建立全局依赖，而无需逐个处理每个符号。

图 2.1 – Transformer 架构，受 Vaswani 等人概述流程的启发

以下是该架构的组件：

- 1. **输入**：准备一个要进行分词的自然语言序列；其标记被分配唯一的 ID。这些 ID 通过编码层进行向量化。

- 2. **位置编码**：`Transformer` 模型本身并不理解序列中单词的顺序，因此必须将位置信息显式地整合到标记嵌入中。位置编码注入了每个标记在 ID 序列中的相对或绝对位置信息，使模型能够利用序列结构。位置编码可以是绝对或相对的。**绝对位置编码**是在原始 `transformer` 架构中引入的。**相对编码**后来引入（`Shaw` 等人，2018），以提高 `transformer` 的性能。

- 3. **注意力机制**：这些分为两种类型：

    *自注意力（点积注意力）*：该技术的核心的核心是使用自注意力，它允许模型根据特定的编码来权衡每个词的相关性。这有助于获取每个词相对于所有词的上下文。

    *多头注意力*：这是多次运行自注意力的过程。对于每个“头”（自注意力层），都会使用不同的注意力权重。然后，对结果进行拼接。这允许关注序列的不同部分。

- 4. **前馈神经网络**：每个层都包含一个全连接前馈网络，对每个位置独立且相同地通过 `ReLU` 激活函数进行处理。该子层有助于进一步完善对输入的理解。

- 5. **层归一化**：这种技术用于稳定学习过程并加速学习。此步骤应用于每个多头注意力和前馈之后。随着信息流过这些层，模型完善了对输入的理解。

- 6. **输出层**：线性层将解码器输出投影到更大的 `logits` 向量中，而 `softmax` 函数将这些值转换为预测标记的概率。这些层共同用于确定模型词表或类别的最终概率分布。输出为向量形式，每个向量对应输出序列中的一个单词。输出格式取决于应用类型。例如，当预期结果是文本生成时，结果是词表的概率分布。当预期输出是分类时，结果是类别的概率分布。

- 7. **后处理**：情况下，最后步骤涉及将向量中的标记 ID 转换为可读语言。

这种架构设计导致了三个主要的模型家族，它们主导了 2023 年的格局：

- 生成式预训练变换（`GPT`）是由 `OpenAI` 开发的模型，包括参数量为 1.1 亿的 `GPT`、15 亿参数的 `GPT-2`、1750 亿参数的 `GPT-3`（`Radford` 等人，2018, 2019 和 2020），以及参数量为 1.7 万亿的 `GPT-4`（`OpenAI`, 2023），其特点是自回归模型。它被训练用于预测句子中的下一个单词。`GPT` 在聊天机器人应用中的用例以 `OpenAI` 的 `ChatGPT` 为例。这些对于具有生成性质的任务非常有用，即故事生成、摘要生成和代码生成。

## 基于 Transformer 的双向编码器表示 (BERT)

-   **BERT** 是由 `Google Research`（`Devlin` 等人，2018）开发的一种模型。它是一个多层双向编码器，其架构与 `GPT` 相似，但它包含了双向掩码，并将下一句预测作为一项额外的预训练任务。`BERT` 类模型对于需要理解输入的任务（例如问答应用）非常有用。

-   **双向和自回归的 Transformer (BART)** 由 `Facebook AI`（`Lewis` 等人，2019）提出，它将自回归的 `Transformer` 与双向 `Transformer` 结合起来。作者观察到，`BERT` 的双向和自动编码器特性有利于分类等下游任务，但不利于生成任务，因为在生成任务中，生成的词应该只取决于之前生成的词。另一方面，虽然 `GPT` 的单向自回归方法有利于文本生成，但不利于需要整个序列信息的任务（如分类）。`BART` 结合了这两种方法。

表 2.1 总结了流行的 `Transformer` 模型、其架构以及用途。

| Transformer 类型 | 架构 | 类似模型 | 关注点 | 示例 |

|---|---|---|---|---|

| 自回归 | 解码器 | 类似 GPT | 生成式任务 | 聊天机器人 |

| 自动编码 | 双向编码器 | 类似 BERT | 理解输入 | 问答 |

| 序列到序列 | 编码器-解码器 | 类似 BART | 需要输入的生成式任务 | 语言翻译 |

表 2.1 – 流行的 Transformer 模型

当人们开始尝试这些模型时，一个问题变得非常普遍：使模型适应特定需求。我们接下来来看这一点。

## 2023 交互工具包：提示工程、微调和 RAG

在 2023 年，处理这些大型单体模型面临的主要挑战有两个：幻觉（生成在表面上合理但在事实上是错误或无意义的内容）以及训练、微调和推理的巨大计算成本。为了应对这些挑战，出现了许多极具说服力的策略：提示工程（`prompting`），定义为提供特定的输入或指令（“提示词”）来引导大语言模型（`LLM`）的回答；微调（`fine-tuning`），即在较小的、特定领域的指令-输出对数据集上对预训练的 `LLM` 进行进一步训练；检索增强生成（`RAG`）（`Lewis` 等人，2021），这是一种通过带有外部文档（非参数化）稠密向量索引的预训练（参数化）模型来提高性能和事实准确性的技术。

以下是微调 `LLM` 的流行技术列表。

-   **指令微调 (Instruction tuning, IT)：** `Zhang` 等人（2023; https://arxiv.org/abs/2308.10792）对截至 2023 年 8 月关于指令微调过程现有文献进行了综述。该方法通过指令-输出对对 `LLM` 进行进一步训练，其中指令包含人类指令，而输出是对该指令的预期响应。

-   **基于人类反馈的强化学习 (RLHF)：** `Ouyang` 等人（2022; https://arxiv.org/abs/2283.02155）将 `RLHF` 作为一种微调技术应用于 `GPT-3`。该技术遵循三个步骤：收集演示数据以训练有监督策略，收集比较数据以训练奖励模型，以及通过强化学习针对奖励模型对策略进行优化。

为了在保持计算成本在合理范围的同时提高 `LLM` 的性能，人们探索了以下技术：

-   **参数高效微调 (PEFT)：** `Lialin` 等人（2023; https://arxiv.org/abs/2303.15647; https://huggingface.co/docs/peft/index）对 `PEFT` 方法进行了系统的比较。该技术旨在寻找高效的方法，在模型参数的子集而非所有参数上适配并微调 `LLM`。在 `LLM` 中，参数的总数可以达到数十十亿个，因此微调每个参数在计算上是非常昂贵的，且并非必须的。因此，通过 `PEFT`，我们可以在降低调整参数计算成本的同时，使模型适应我们的用用例。

-   **低秩自适应 (LoRA)：** `Hu` 等人（2021; https://arxiv.org/abs/2106.09685）提出了这种技术，其目标是冻结 `Transformer` 架构的预层（`Vaswani` 等人，2017, 2023），从而减少下游任务的可训练参数数量。这意味着通过 `LoRA`，我们可以在消耗内存的同时加速具有 `Transformer` 架构的 `LLM` 的微调。该技术通过低秩分解使用矩阵表示权重更新。新矩阵经过训练以适应新数据，其优势是实施的更改总数较少。最终结果由原始权重和适配权重的组合组成。更多信息请访问 https://huggingface.co/docs/main/en/conceptual_guides/lora。

-   **模型蒸馏 (Model distillation)：** 其背后的思想是将 `LLM` 视为可以训练学生模型的老师。最终目标是让学生模型的行为与老师模型相似。

-   **RAG：** 如 `Lewis` 等人（2021）在论文中介绍（https://arxiv.org/abs/2005.11401）的，检索增强生成（`RAG`）是一种微调配方，它将预训练的参数化内存生成模型与非参数内存相结合。在架构中，参数化内存是带有外部文档（非参数化）稠密向量索引的预训练模型，这是一种提高性能和事实准确性的技术。

在这些选择时，开发者通常查看以下内容：

-   **RAG：** 用于模型需要访问最新的、事实性的数据时，我应该使用 `RAG`，还是应该微调我的模型？

-   **PEFT 和 LoRA：** 用于模型需要适配特定的风格、格式或以适应特定领域。

专门设计用于执行多步、逻辑化且深思熟虑的推理，超越了传统大模型（`LLMs`）的快速模式匹配。

`Besta` 等人（2025）概述了推理语言模型（`RLM`）的架构蓝图，它结合了三个不同的组件：

- **LLM（作为知识库）：** 传统大型 `LLM` 的权重作为基础，提供世界知识和语言流利度。

- **强化学习 (RL)（作为策略/价值模型）：** 使用 `RLM` 框架来引导生成过程。`RLM` 模型并非像普通 `LLM` 那样优化下一个标记预测（`next-token prediction`），而是针对长期策略或结果进行优化。



# 搜索启发式算法与 2025 年开源领域重大事件

- **搜索启发式算法（作为探索者）**：集成了如蒙特卡洛树搜索（`MCTS`）或束搜索（`Beam Search`）等算法，允许模型探索思维树或推理图。模型可以生成多个潜在的解决方案路径，对它们进行评估，并从没有前景的路径中回溯。

2025 年开源领域最重大的事件是`DeepSeek 时刻`。2025 年 1 月 20 日，中国公司 `DeepSeek` 发布了其 RLM 模型（https://api-docs.deepseek.com/news/250128），在 AI 界引起了巨大震动。

其意义体现在两个方面：

- **性能**：`DeepSeek-RI`在推理性能上达到了与世界顶尖闭源模型相当水平，包括`OpenAI-o1`等（DeepSeek et al., 2025）。

- **可访问性**：它在宽松的 `MIT` 许可证下发布，据报道其训练成本具有极高的成本效益（低于 600 万美元），DeepSeek et al., 2025。

这一事件从根本上使高级推理变得民主化，证明了 LLM 并非万亿级大企业的专属领域。`DeepSeek-RI`的成功并非简单的规模扩张（`scaling`）结果，而是算法和架构上的突破，正如其技术报告中所述的。

首先，该模型构建在 2024 年 12 月发布的 `DeepSeek-V3` 基础模型之上。其训练流水线中的真正创新在于：

核心算法突破是**组相对策略优化 (`GRPO`)**（DeepSeek et al., 2025; Zhao et al., 2024）。`GRPO`是一种新颖的 RLM 算法，它通过计算采样输出组内奖励的平均值和标准差来计算基准（`baseline`）和优势分（`advantage scores`），而不是使用独立的评论模型（`critic model`）。在 LLM 的典型强化学习（`RL`）设置中，通常使用评论模型（通常是神经网络）来估计基准并为策略模型提供反馈。这个基准对于计算特定输出的“优势”是必要的，它能有效确定一个答案与平均水平相比是好是坏。由于评论模型的大小通常与策略模型相当，这会将训练的内存和计算需求增加倍。`GRPO`通过用基于组的评分取代神经网络基准，消除了对独立评论模型的需求。

对于训练过程中提出的每一个问题，算法都会采样一组不同的输出。系统不再询问独立的评论模型回答有多好，而是简单地比较该特定组内输出的奖励。答案的优势是通过计算其奖励相对于组内同伴的平均值和标准差来得出的。通过使用这些内部组统计数据作为基准，`GRPO`实现了与传统 RL 相同的优化信号，显著降低了训练流水线的计算和工程复杂度。

其次，最终的 `DeepSeek-R1` 模型是一个多阶段训练流水线的产物：

- **阶段 1，纯强化（RL）**：在基础模型上仅使用 RL（`GRPO`）训练了第一个 `DeepSeek-R1-Zero`（https://huggingface.co/deepseek-ai/DeepSeek-R1-Zero）。尽管实验是成功的，但它面临着可读性差和语言混杂的问题。

- **阶段 2，多阶段**：为了创建最终的精炼版 `DeepSeek-R1`，他们设计了一个多阶段流水线：过程首先对一小部分长、高质量**思维链（chain-of-thought）**示例集进行**监督微调（SFT）**（https://huggingface.co/learn/11m-course/en/chapter11/），为模型提供结构化推理的初始脚架。接下来，团队应用 `GRPO` 推动模型超越模式模仿，鼓励深思熟虑的逐步解决问题。在此基础上，他们使用拒绝采样（`rejection sampling`）从 RL 增强的模型中提取最强输出，汇出一个规模更大、更干净的合成数据集，用于第二轮 SFT 以进一步精炼模型。在最后阶段，他们在所有任务场景下运行了额外的全局 RL 过程，作为一种润色阶段，对齐了模型的推理深度、格式一致性和整体性能。

这次开源发布证明了高级推理是一个算法和架构挑战，可以通过新颖的 RL 技术和工程化的训练流水线来解决，而不仅仅是规模挑战。对于开发者来说，这一事件标志着推理的力量现在已成为开放、可用的工具，可以集成到他们的应用程序中。这引出了我们章节的下一部分：与语言模型交互。

# 与语言模型交互

从 2023 年到 2025 年模型架构的演变不仅重塑了 AI 格局；从根本上改变了开发者与语言模型工作的方式。随着小型语言模型（`SLM`）和推理语言模型（`RLM`）形成两个专门分支，早期 LLM 时代指导交互设计的假设不再成立。深思熟虑推理模型、智能体流水线（`agentic pipelines`）和长上下文系统的出现引入了新的失败形式、新机会和新的工程挑战。

在快速创新和专门模型爆炸为特征的环境中，传统的提示工程（`prompt engineering`）概念已不再足够。开发者的角色已从编写孤立的指令扩展到编排整个上下文：提示、检索知识、工具定义、智能体状态和中间推理步骤的动态混合，这些元素驱动着现代 AI 系统。随着模型获得更深层次的推理能力，智能体开始在决策链上运行，上下文窗口不再仅仅是一个被动的输入缓冲区，而是一个活跃的工程界面。

这种从编写巧妙的提示到构建可靠信息流的转变，标志着 2025 年 AI 开发中最重要的概念跨越之一。为了在 `SLM`、`RLM` 或任何智能体系统之上构建健壮的应用，开发者必须精通上下文工程（`context engineering`），即对策展、结构化、压缩、隔离和管理模型随时间消耗的信息的学科。

精通上下文工程为 2025 年构建生产级 AI 系统的开发者提供了几个关键益处：

- **增强可靠性和事实性**：通过策展上下文窗口内的信息，开发者使模型建立在经过验证的数据之上。

- **缓解上下文腐化**：系统性地管理活跃的工程界面可以防止长会话中的性能下降。

- **运营成本效率**：压缩等技术允许将大型数据集提炼为 Token 高效的表示，降低推理的反复成本。

通过将上下文窗口视为动态内存层，开发者可以构建能够记忆交互并更新行为的智能体。

在探索实现这一学科的实用框架和技术之前，我们首先需要理解为什么提示提示失效，以及上下文工程是如何出现的。为了帮助你开始交互 LLM，我们创建了一系列笔记本：

- **使用 Ollama 对本地模型进行提示**：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch2/jupyter-notebooks/01_prompt-ollama-model.ipynb

- **使用 Ollama 使用本地模型创建智能体**：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch2/jupyter-notebooks/02_create-simple-agent.ipynb

- **探索 RAG 介绍**：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch2/jupyter-notebooks/03_document-qa-langchain.ipynb

这些资源说明了今天可以通过 Python 生态系统创建智能体、增强 LLM 并利用 LLM、SLM 和 RLM。虽然这些很简单，但它们展示了未来的趋势：上下文工程。

## 2025 年开发者交互的演进：从提示到上下文工程

为了构建可靠的应用，理解从提示到上下文工程的概念跨越至关重要：

- **提示工程，2024 年的手工艺**：2024 年，提示工程被定义为编写有效提示的艺术。它是编写精确指令（提示）以从 LLM 获得特定预期响应的实践。这种关注调整单个单词的方法被更准确地描述为“提示词制作（`prompt crafting`）”。

- **上下文工程，2025 年的学科**：这是 2025 年的新正式学科。它是提示工程的超集。重点从仅仅是提示转向了在有限的上下文窗口中填充正确信息的精细艺术和科学。



这种转变是由智能体系统（`agentic systems`）中简单提示词的失败驱动的。在循环中运行的智能体会产生越来越多的数据（Anthropic, 2025）。如果没有正式的工程规范，这会导致关键性失败，例如“上下文污染”，即某一个步骤的幻觉会影响所有后续步骤的上下文，导致智能体追逐无意义的策略（Datacamp, 2025）。即时的字符串管理是脆弱、难以调试且不可扩展的（Thatipalli, 2025），而且模型性能在过长且嘈杂的上下文中会退化。其他问题还包括上下文破坏、上下文融合和上下文冲突（Kubiya AI, 2025）。

接下来，我们将探索围绕上下文工程（`context engineering`）的策略。这是解决这些挑战的新兴实践。

## 上上下文工程的核心策略 (2025)

上下文工程为开发者提供了一个管理上下文窗口的实用工具包，这在现代 AI 系统中已成为最有限且最有价值的资源之一。到 2025 年，四种核心策略成为有效上下文管理的支柱：写入、选择、压缩和隔离（Anthropic, 2025; Kubiya AI, 2025; Gibson AI, 2025; LangChain, 2025）。

- **写入 (Write)**：将信息存储在即时窗口外部以备日后检索。这是智能体记忆的基础。技术包括草稿板（`scratchpads`），即用于智能体思考或计算的中间工作内存；以及长期记忆（`long-term memories`），即将用户偏好、之前对话中的事实等存储在外部数据库（通常是向量数据库）中。

- **选择 (Select)**：在需要时动态地将最相关的信息提取到上下文窗口中，即“即时”检索策略。技术包括 `RAG`（检索增强生成）、内存选择（从写入上下文存储器中检索相关项）以及动态工具选择（即仅选择与任务相关的工具）。

- **压缩 (Compress)**：将大块信息提炼为更小的、Token 高效的表示，以压缩上下文并避免上下文腐烂（`context rot`）。技术包括总结和工具结果清理。例如，智能体调用一个工具并接收到一个大型 `JSON` 块。在提取出必要信息后，该技术会从上下文中移除大型原始 `JSON`，并仅用提取出的事实进行替换。

- **隔离 (Isolate)**：这是一个关键的高级架构模式。它涉及将任务分成独立的隔间，每个隔间都有一个干净、隔离的上下文窗口。使用的关键技术是子智能体架构（`sub-agent architectures`），给主主管智能体一个复杂的目标（如深度研究）。为了完成子任务，它会启动一个运行在自身干净上下文窗口中的子智能体，执行深度研究，并仅向主管返回最终的详细摘要。这防止了主管智能体的主要上下文被子智能体详细且混乱的工作所污染。这是构建稳健、复杂智能体的关键。

这四种策略共同构成了现代上下文工程的核心，使智能体和推理模型能够在复杂的、多步骤的现实应用中可靠地运行。

> 注意
>
> [高级] 实践创建动态多智能体派发
>
> 探索多智能体创建：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch2/Jupyter-notebooks/advanced-langchain-langgraph/03_multi-agent-workflow.ipynb

各种智能体架构的兴起使得需要上下文工程作为一种正式学科，以使其变得可靠。这引出了我们能够实现这一新学科的框架。

## 2025 年框架架构的演进

2023 年时代的框架诸如 `LangChain` (https://www.langchain.com/)、`LlamaIndex` (https://www.llamaindex.ai/) 和 `Haystack` (https://haystack.deepset.ai/)，它们在 2024 年和 2025 年继续保持专业化。`LangChain` 的即时链（`chain`）概念已随着 `LangChain 1.0`、`LangGraph` 和 `LangSmith` 的引入而被正式的智能体工程栈所取代：这是一套用于开发、评估和部署智能体的完整组件。`deepset` 将其开源框架从 `Haystack 1.0` 升级到 `Haystack 2.0`，发布了用于视觉化、管理和部署流水线的 `Deepset`，并引入了智能体能力。本书认为，每个框架的专业化（即 `LangChain` 和 `LangGraph` 用于智能体工作流定义，`Haystack 2.0` 用于流水线编排）将促进以下关注点分离：用于智能体控制流的编排层和用于稳健数据流的工具层。在本书中，我们将关注上述两个框架：`LangChain` 的 `LangGraph` 和 `deepset` 的 `Haystack`。虽然这两个框架都提供了处理 `RAG`、工具和智能体的重要功能，但在本书中，我们将重点关注将 `Haystack` 作为我们的工具层，并将将 `LangChain` 的 `LangGraph` 作为我们的编排层使用。

## LangChain/LangGraph 1.0：智能体编排层

2025 年 10 月，`LangChain` 发布了其 v1.0 版本（LangChain, 2025），标志着一次重大的战略转向，巩固了智能体工程栈。这次转向的核心在于，`LangChain 1.0`（高级 API）是基于 `LangGraph 1.0`（低级运行时）重新构建的。让我们更深入地探索这些内容。

`LangGraph` 是一个用于智能体编排的低级、基于图的运行时。它的完全目的是构建高度定制、可控且持久的智能体。

与其原始的链概念（https://api.python.langchain.com/en/latest/langchain/chains.html）不同，`LangGraph` 提供了一个状态化的图，支持：

- **循环 (Loops)**：智能体的基础模式。

- **条件分支 (Conditional branching)**：允许智能体做出决策。

- **持久状态和检查点 (Durable state and checkpointing)**：对于长时间运行的任务和人类回路（`HITL`）审批至关重要。

`LangGraph` 是处理复杂状态控制流的 2025 年领导者（Trix, 2023）。`LangChain 1.0` 定位为构建智能体的最快方式，其定义性特性是`create_agent`循环，一种对智能体的抽象，通过一组钩子（`hooks`）中间件，允许在智能体采取的每一步中进行细粒度的控制（LangChain, 2025）。

> **[高级] 实践在智能体内定义状态并引入中间件**
>
> 探索带有状态和中间件的多智能体系统：https://github.com/PacketPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch2/Jupyter-notebooks/advanced-langchain-langgraph/03_multi-agent-workflow.ipynb

各种智能体架构的兴起使得需要上下文工程作为一种正式学科，以使其变得可靠。这引出了我们能够实现这一新学科的框架。这种中间件是我们刚刚讨论的上下文策略的直接实现：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch2/Jupyter-notebooks/advanced-langchain-langgraph/03_multi-agent-workflow.ipynb

## Haystack 2.0：工具层

`Haystack` 已经进化，巩固了其在 2025 年作为构建稳健、可衡量且可审计流水线的“流水线优先”框架的市场地位（AlphaCorp, 2025）。其架构是一种具有严格输入输出的显式有向图（`DG`）流水线，非常适合复杂的数据流。

2025 年的功能包括：

- **企业级 RAG**：`Haystack` 的主要优势是稳定性（Trix AI, 2025）。它为混合检索（结合稀疏/`BM25` 和稠密/嵌入）、重排序以及连接所有主要的向量数据库提供了丰富的、经过测试的生态系统。

- **可衡量性与审计**：由于其流水线是显式的，因此是透明且可调试的。关重要的是，`Haystack` 内置了跟踪 `RAG` 指标的评估节点，使其成为对审计有要求的合规行业的首选。

- **部署 (Hayhooks)**：`Haystack 2.0` 提供了 `Hayhooks`，这是一个可以通过单个命令将任何 `Haystack` 流水线部署为生产就绪 `REST API` 或 `MCP` 服务器的框架 https://hayhooks.deepset.ai

# 2 

| 特性 | LangGraph 1.0: 编排层 | Haystack 2.0: 工具层 |
| :--- | :--- | :--- |
| 核心架构 | 有状态、基于图的运行时。支持循环、持久化、人类参与。 | 显式的基于 `DAG`（有向无环图）的流水线。 |
| 优势 | 复杂的多步骤有状态工作流。可控代理循环。持久的状态和检查点。 | 可衡量、可审计、高性能的 `RAG`。企业级稳定性。混合检索。可靠且可扩展微服务的自定义开发。 |
| 理想用场景 | 构建复杂智能体的大脑。客户支持机器人和多步骤研究智能体。 | 构建供智能体调用的可靠、可部署工具（例如：*`RAG` 即服务*微服务）。 |
| 2025 v1.0 创新 | `LangGraph` 运行时 | `Hayhooks` 部署框架 |
| 上文管理 | 通过 `LangChain 1.0` 中的中间件实现 | 通过显式流水线组件实现 |

表 2.2 – 2025 年框架分工 (LangGraph vs. Haystack)

你问“*为什么不只用一个框架呢？*”很好。2025 年及未来的开发者会为任务选择合适的工具。在本书中：

- 我们使用 `Haystack` 构建工具层，因为它的显式 `DAG`、混合检索组件和评估节点是为创建可审计、高性能且可靠的 `RAG` 流水线而专门构建的。它是数据流领域的顶级框架。最重要的是，`Haystack` 允许将通用和自定义流水线部署为微服务，使其成为任何需要严谨性、数据溯源和可扩展性的代理框架的强大补充。

- 我们使用 `LangGraph` 构建编排层，因为它的有状态、循环图运行时是专为满足呈现上下文工程需求的复杂、持久且可控的控制流而构建的。



尝试强行让一个框架去做另一个框架的工作会导致冲突。在 `LangChain` 和 `LangGraph` 中从零构建可衡量的混合 `RAG` 流水线是可行的，但它忽略了 `Haystack` 提供的经过实践检验的顶级组件。在 `Haystack` 中构建复杂的、有状态的、持久代理循环也是可能的，但这不是该框架的核心专长（它针对 `DAG`/数据流进行了优化），虽然它确实支持循环图，但其内部的组件-流水线框架使得代理工作流比 `LangGraph` 的对应产品更难管理。本书认为混合架构将成为一种成熟的设计模式，反映了成熟的工程规范。

本书将主要关注工具层，并在工具作为 `MCP` 服务器或 `REST` 端点微服务暴露时，会将工程化作为编排层的一部分。我们将从精通 `RAG` 流水线开发、自定义组件开发、流水线评估和流水线部署开始。本章提供了使用 `LangGraph` 创建智能体的高级笔记本作为参考，并在书的后续章节中再次回顾。

> **注意**

**练习学习状态（`state`）**

使用 `LangGraph` 创建一个简单的智能体：`https://github.com/PacktPublishing/Building-Natural-language-and-LLM-Pipelines/blob/main/ch2/jupyter-notebooks/advanced-langchain-langgraph/04_understanding-state-graph.ipynb`

**练习定义有状态的代理**

使用 `LangGraph` 创建一个带工具的简单有状态智能体：`https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/ch2/jupyter-notebooks/advanced-langchain-langgraph/05_graph-based-agent-with-tools.ipynb`

## [高级] 练习使用状态和中间件创建多智能体系统

使用 `LangGraph` 探索带有中间件的多代理创建：`https://github.com/PacktPublishing/Building-Natural-language-and-LLM-Pipelines/blob/main/ch2/jupyter-notebooks/advanced-langchain-langgraph/06_multi-agent-systems-middleware.ipynb`

一个结合两个框架的示例端到端工作流如下。

1. 用户提问。该问题发送到我们由大语言模型驱动的主智能体。

2. 智能体判断它需要外部信息，因此调用一个可以搜索知识库的工具。

3. 工具将问题发送到我们的 `API`（部署的 `Haystack` 流水线）。该 `API` 在后台运行一个完整的 `RAG` 流水线。

4. `RAG` 流水线（部署为微服务）执行工作：

   - a. 它将问题转换为嵌入（`embedding`）

   - b. 找到最相关的文档

   - c. 构建一个有用的提示词（`prompt`）

   - d. 并要求另一个 `LLM` 创建基于事实的答案

5. `API` 以整洁的 `JSON` 格式返回结果。

6. 工具将此结果返回给智能体。

7. 智能体现在拥有了两个东西：

   - a. 原始用户问题

   - b. 从 `RAG` 工具检索到的上下文

8. 结合这两者，智能体写出最终答案并完成对话。

这可以通过简单的 `LangChain` `Python` 对象实现，但使用 `LangChain` 与使用 `Hayhooks` 和 `Haystack` 的核心区别在于：我们的 `RAG` 工具不是一个从内存中读写文档的简单 `Python` 对象，而是一个可扩展的服务，能够查询和处理成千上万种格式的文档，包括文本、表格、音频和图像。这为我们讨论 `RAG` 和智能体架构的关键部分：向量存储（`vector stores`）提供了完美的过渡。

## 数据存储、检索和内存：向量存储的作用

正如我们探索了 2025 年建议的混合架构，我们现在必须处理向量存储问题。虽然语言模型本身持有训练期间学习的参数化知识，但向量存储提供了外部的、最新的知识。这使得语言模型能够基于事实数据进行推理。

5. **查询嵌入**：当用户提问时，必须使用与索引期间相同的嵌入模型将问题转换为向量格式，以便检索过程能够准确地检测到语义上相似的向量。

6. **检索**：此阶段使用嵌入后的问题，并在向量存储中提取与查询最接近的嵌入。此阶段返回包含最相关信息的分块（`chunks`）。

7. **增强**：一旦返回分块，它们将作为上下文与原始查询一起传递给语言模型，由模型以自然语言提供最终答案。

值得注意的是，分块策略（`chunking strategy`）和嵌入模型的选择是重要的架构决策，它们永久地定义了知识库的语义分辨率和检索过程。根据定义，`RAG` 过程检索的是文本块而非原始文档。生成步骤中的 `LLM` 只能看到这些检索到的块。这产生了一个根本性的设计困境。

如果分块过大（例如，整份五十页的文档），它包含过多的噪声和无关信息，会污染上下文窗口，这个问题类似于智能体循环（`agent loops`）中的上下文污染。相反，如果分块太小（例如，单个句子），它缺乏让 `LLM` 理解其包含事实的周围上下文。因此，分块策略（例如，200 个字加 15% 的重叠）硬编码了整个知识库的语义尺寸。这正是为什么玩具层（`toy layer`）必须是一个健壮的数据流水线，为这些复杂且高风险的摄取和提取过程提供治理和管理。在检索过程中还会暴露出额外的额外复杂性。我们将在接下来探讨这一点。

## 转向高级检索模式

纯语义检索（稠密向量）有一个关键且广为人知的缺点：它无法找到精确的关键词。如果周围的语义上下文不是强匹配，基于嵌入的检索往往无法检索到包含特定产品 `ID` 的文档。传统的关键词搜索（使用稀疏向量，例如 `BM25`/`TF-IDF`）恰恰相反：它擅长寻找精确匹配，但对查询内容一无所知。

一种成熟的解决方案是[混合搜索（`hybrid search`）](https://www.elastic.co/what-is/hybrid-search)，这是一种结合了语义搜索和关键词搜索的架构。这种方法利用关键词搜索的匹配能力和语义搜索的上下文理解能力，提供更优的混合结果。

该机制涉及并行运行两个查询：一个针对稠密向量索引，另一个针对稀疏向量索引（`BM25`），每个查询都有相关性评分，然后被智能地融合为一个单一的、优排名。最常用且有效的[倒数排名融合（`RRF`）](https://www.elastic.co/docs/reference/elasticsearch/rest-apis/reciprocal-rank-fusion) 算法根据合并结果在原始列表中的位置对合并结果进行重新排名，从而避免了归一化不兼容评分的需求。

这种模式是程序级 `RAG` 的良好基准要求。定义良好的工具使我们能够在数据上实现基于关键词、基于向量或混合搜索，并使用性能评估来选择最佳方法。第 5 章和第 6 章将涵盖一系列深入的示例，从成本和性能两个维度评估并为给定的数据集选择最佳方法。

`Wayback 2.0` 框架用于将整个数据流关注（摄取、分块、嵌入、索引和检索）封装到两个可部署的工具中：索引流水线和检索流水线，每个流水线都可以通过 `Hayhooks` 的 `REST` 端点作为独立的微服务部署。编排层不需要知道复杂的检索是如何工作的；它只需调用 `RAG` 工具并检索一个包含经过验证的（`grounded`）上下文的干净 `JSON` 对象。

将智能体的状态持久化计算过程与向量存储的状态化持久化数据解耦，是构建可靠、可审计且可扩展的 `AI Agent` 的关键。为了说明这一点，我们将扩展介绍当前如何使用向量存储来管理智能体中的内存和状态。

## 从 RAG 索引进化到智能体内存

向量存储的角色随着我们分析的模型和框架同步演进。这种演变代表了从静态只读数据源到动态读写内存层的根本性转变。

在 2023 年的基准中，`RAG` 主要是对抗幻觉的缓解策略。在这种架构中，向量存储的作用是被动的且只读的。它作为外部静态文档（例如，技术手册、公共网站）的非参数索引。其目的是让 `LLM` 的输出基于经过验证的、特定上下文的数据，这种做法一直持续到 2025 年的 `RAG` 系统（`Hasan et al.`, 2025）。

然而，2025 年的智能体系统运行在状态循环中，每个周期都会产生额外数据（`Anthropic`, 2025）。这引入了一个新的关键挑战：`LLM` 本质上是无状态的，它们的上下文窗口无法容纳长时间任务的完整历史或知识。解决方案包括在不同层级中构建内存，分类为短期内存（即时上下文窗口）、工作内存（智能体的“草稿纸”）和长期内存（持久的、外部存储器）（`GibsonAI`, 2025）。向量数据库被确定为这种长期内存层的载体（`CortexFlow`, 2024）。

这种基于向量的长期内存重新利用了 `RAG` 机制（`DEV, Cesar`, 2025）。智能体不再检索外部因素，而是检索它过去的经历。根据 `Cesar` 的说法，它将对话历史、知识和任务视为可搜索的向量。这允许智能体召回之前的交互，从中学习，更新知识，并随着时间的推移保持一致的行为。这种在早期实验性智能体中开头的模式，现在是现代智能体设计的基础组件（`Silicon Agent`）。

## 向量存储作为上下文工程的实现者

正如定义的，2025 年提出的上下文工程结合了写入、选择、压缩和隔离技术，以控制进入 `LLM` 有限上下文窗口的信息。向量存储是写入和选择策略的物理实现。

- **写入策略**涉及外部存储信息以供检索。在实践中，这是 `Anthropic` (2025) 所说的结构化笔记或智能体记忆。在交互后，智能体将关键信息（如新学习的用户偏好、进行中的任务或中间结论）作为嵌入写入向量数据库。这使得知识持久化，独立于 `LLM` 的上下文窗口。

- **选择策略**动态地将最相关的信息拉入上下文窗口。这就是内存检索系统（`DEV Cesar`, 2025）。当智能体开始下一个推理步骤时，它使用当前目标查询向量存储（其长期内存）。数据库选择并返回语义最相关的内存，然后这些内存作为上下文注入提示词。这种检索策略确保了智能体拥有运行所需的内存。

这种演变标志着根本性的转变。向量存储的角色已经从被动的只读索引（2023 年 `RAG` 模型）成熟活跃的读写内存层（2025 年智能体模型）。2023 年的模式是写一次读多次。相比之下，2025 年的智能体在每次交互后都会将自己的经历和对话历史写入存储器。这从根本上将数据库从只读转变为读写，引入了新的复杂的工程挑战。让我们来看看 `AWS` 探索的这种高级内存架构（`AWS, Sehw et al.`, 2025）。

## 高级内存架构与整合

这种智能体内存的读写模式产生了一个新的三级问题：内存策展（`memory curation`）。如果智能体不断写入向量内存，存储器将迅速充满冗余、冲突和琐碎的信息。这会污染内存，导致选择策略开始检索低质量、无用的上下文，导致智能体失败，这正是该学科旨在解决的上下文污染问题。

# 2 

探索的 2025 年 `AWS` 解决方案并非简单的写入，而是实现一个整合过程（`AWS, Sehw et al.`, 2025）。这种高级架构模式将智能体的内存视为一种经过策划管理的资产，而不仅仅是数据转储。正如 `Amazon AgentCore` 等架构所详细描述的那样（`AWS, Sehw et al.`, 2025），这种整合过程是一个递归循环：

- 当智能体策略生成新的内存时，它不会立即提交到数据库。相反，系统首先执行检索，查询向量存储以找到语义上最相似的现有内存。

- 新的待处理内存和检索到的旧内存被封装在一起，并带有整合提示词（`consolidation prompt`）发送给大语言模型（`LLM`）。

- 该 `LLM` 充当数据库管理器，评估新旧信息。根据提示词，它决定正确的操作：`ADD`（如果信息是新颖）、`UPDATE`（如果信息精炼或与现有内存合并）或 `NO-OP`（如果信息是冗余）。

- 只有随后，才会执行操作（例如更新向量存储）。

这类智能体系统是递归的。它们利用 `LLM` 结合类似于 `RAG` 的检索过程（可以是作为工具部署的 `RAG` 流水线），智能地管理作为其长期内存的向量存储内容。这种自策划循环是 2025 年智能体的标志。此时，与所用工具解耦的专用编排层变得特别有用。中间件和基于图的智能体工作流定义可以实现这种模式。

在概述了向量存储及其在经典 `RAG` 和智能体短期/长期内存中的作用后，我们准备好探索本章的最后一个章节，并讨论基于 `LLM` 的应用的成本。

## 理解 LLM 部署的经济学：推理成本

在建立数据层之后，我们现在必须分析查询这些数据的模型运行情况。模型专业分为 `LLMs`（大语言模型）、`SLMs`（小语言模型）和 `RLMs`（检索语言模型）不仅是架构的转变，更是深层次的经济转变。为了构建可靠的系统，架构师必须正式定义训练和推理的成本，并理解决定 `AI` 应用总成本（`TCO`; `https://en.wikipedia.org/wiki/Total_cost_of_ownership`）的市场动态和技术驱动因素。

该领域初学者中一个普遍的误解是，语言模型的主要成本是训练。对于 2025 年的应用开发人员和企业来说，这是错误的。占据主导地位、循环且具有战略意义的成本是推理。让我们来看看两者的区别：

- **训练成本：** 这是创建基础模型所需的大额、一次性资本支出（`https://en.wikipedia.org/wiki/Capital_expenditure`）。一个实用的汽车类比定义了构建汽车所投入的所有内容：设计、工程、生产等（`Mangusta Capital`, 2024）。这一过程需要耗资数十亿美元的超级计算机 `GPU` 集群。这是 `OpenAI`、`Google` 和 `Anthropic` 等 `AI` 巨头所享有的专有领域。

- **推理成本：** 这是使用该模型的持续运营费用（`https://en.wikipedia.org/wiki/Operating_expense`）。在同样的类比中，推理是运行汽车的成本，比如汽油费（`Capital`, 2024）。对于大多数 `AI` 消费者（而非生产者）组织来说，这是决定应用经济可行性的经常性成本。

2025 年推理的经济格局由一个深层悖论定义：虽然训练模型的成本不断飙升，但推理成本几乎是免费的。这种市场动态被称为“2025 年的 `AI` 大压缩”。这是一种由既有者（`OpenAI`, `Google`）、开源攻势方（`Meta`, `Mistral`）以及新一代推理即服务初创公司（`Skywork`, 2025）之间的激烈竞争所驱动的 `API` 价格。

对于系统架构师来说，`AI` 模型的价格下降只是故事的一部分。真正重要的是 `TCO`，它捕捉了运行模型的全部成本，而不仅仅是购买或调用的成本。对于基于 `API` 的模型，`TCO` 是直观的，因为你按 `Token` 计费，公式本质上是输入 `Token` 成本加上输出 `Token` 成本。对于自托管模型，`TCO` 变得复杂得多，涉及硬件基准测试、能量消耗、网络以及持续的工程支持费用（`Latitude, Miguelanes C`, 2025）。

`TCO` 的一个主要因素是延迟与吞吐量的权衡（`Nvidia`, 2025）。架构师在延迟（用户体验的速度）和吞吐量（系统必须处理的请求量）之间取得平衡。增加吞吐量（例如批处理请求）总是增加延迟。因此，架构师的工作是定义一个最大可接受的延迟，并在该约束内最大化吞吐量。这一选择决定了所需的模型实例数量，直接塑造了总成本。

`SLM` 在控制 `TCO` 方面起着重要作用。这些模型不仅在学术上有趣，在经济上也具有优势。研究表明，一个 70 亿参数的 `SLM` 服务成本比具有 70-175 亿参数的大得多模型低 10-30 倍（`Nvidia Research, Belcat et al`, 2025）。这种成本优势来自于更高的性能、更低的能耗和大幅减少计算需求。由于这些优势，`SLM` 市场正在迅速扩大。

最后，目前的 `API` 价格可能会忽视深层次的经济现实。虽然 `API` 模型前期成本低，但其基于使用的定价成本会持续增长，并在规模化时可能变得不可预测。自托管模型最初成本较高，但提供了稳定且可预测的长期成本。在特定的使用水平下，托管 `API` 用的累计成本最终将超过拥有和运行自有基础设施的成本。这个 `TCO` 交叉点或收平衡点是现代成本分析的核心，帮助架构师确定何时投资自托管系统在财务上是合理的（`Pan et al.`, 2025）。

架构师的最终决策将技术和财务整合为战略。在私有自托管模型和托管 `API` 之间的选择是主权与便利之间的复杂权衡。

这一决策由三个主要因素驱动：数据安全、模型定制和性能。对这些驱动因素的分析支持了所提混合架构的适用性，作为私有部署（自托管语言模型）和托管部署（调用外部 `API`）选项之间的一种可行模式。

| 决策标准 | 私有部署 | 托管部署 |

| :--- | :--- | :--- |

| 数据安全与合规 | 完全控制。数据永不离开环境。符合 `HIPAA`、`GDPR` 和 `SOC 2` 要求。 | 风险。数据发送给第三方。不适用于受高度监管的数据。 |

| 模型定制 | 完全控制。通过 `PEFT`/`LoRA` 进行深层参数级微调。创建特化模型的能力。 | 有限。仅限于提示词工程（成本高昂）或供应商提供的 `API`。 |

| 性能（延迟） | 低且可预测。毫秒级。边缘 `AI` 和实时 `UI` 所需。 | 高且多变。数秒级，取决于网络和提供商负载。 |

## 总结

在本章中，我们追踪了大语言模型（`LLM`）领域从 2023 年基准到 2025 年专门的代理技术栈的快速演进。我们学习了过去的单体模型（`BERT`, `GPT-3`）如何让位给全新的、专门化的频谱——从用于边缘计算的高效小语言模型（`SLM`）到像 `DeepSeek-R1` 的先进强化语言模型（`RLM`）。

我们看到开发者交互如何从提示词工程（`prompt engineering`）成熟为正式的上下文工程（`context engineering`），这是管理代理信息环境的一策略（编写、选择、压缩、隔离）。我们还分析了框架市场如何应对这一现实。`LangGraph 1.0` 已成为代理控制流的领导者，而 `Haystack 2.0` 则巩固了其作为可靠、可审计数据流领导者的地位。

我们研究了这一新技术栈的经济现实，明确了开发者主要的、持续性的成本是推理而非训练。这要求我们关注总拥有成本（`TCO`），它揭示了一种战略权衡：托管 `API` 的按需付费便利性，与自托管带来的长期经济、安全和低延迟优势之间的对比，而高性价比 `SLM` 使得这种选择更具可行性。



最后，我们将这些轨迹合成为所提的混合架构，其中`LangGraph`编排器调用部署为`Hayhooks`微服务的`Haystack RAG`流水线。这种模式将在第 8 章的实战练习中构建，它范例了定义现代、可靠且生产就绪代理系统的关注点分离和上下文隔离。

在下一章中，我们将提供`Haystack`库的广泛概述，并开始对工具层进行深入探讨。

## 延伸阅读

- Vaswani, L., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., Kaiser, L., and Polosukhin, I. (2017, 修订 2023). Attention Is All You Need. https://arxiv.org/abs/1706.03762

- Zhang, S., Dong, L., Li, X., Zhang, S., Sun, X., Wang, S., J., Hu, R., Zhang, T., Wu, F., Wang, G. (2023). Instruction Tuning for Large Language Models: A Survey. https://arxiv.org/abs/2308.10792

- Ouyang, L., Wu, J., Jiang, X., Almeida, D., Wainwright, C., Mishkin, P., Zhang, C., Agarwal, S., Slama, K., Ray, A (2022). Training language models to follow instructions with human feedback. Advances in Neural Information Processing Systems, 35:27730–27744. https://arxiv.org/abs/2203.02155

- Christiano, P. F., Leike, J., Brown, T., Martic, M., Legg, S., Amodei, D. (2017). Deep reinforcement learning from human preference. (2017). Advances in Neural Information Processing Systems, pages 4299–4307. https://papers.nips.cc/paper/7017-deep-reinforcement-learning-from-human-preferences

- Stiennen

- Lewis, P., Perez, E., Piktus, A., Petroni, F., Karparkhin, V., Goyal, N., Kuttler, H., Lewis, M., Yih, W.-T., Rochahel, T., Riedel, S., Karp, D. (2021). Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks. https://arxiv.org/abs/2005.11401

- Lianin, Y., Deshpande, V., Allen-Zhu, Z., Li, Y., Wang, S., Wang, L. (2023). Scaling Up: A Guide to Parameter-Efficient Fine-Tuning. https://arxiv.org/abs/2303.15647

- Hu, E., Shen, Y., Wallis, P., Allen-Zhu, Z., Li, Y., Wang, S., Wang, L., Chen, W. (2021). LoRA: Low-Rank Adaptation of Large Language Models. https://arxiv.org/abs/2106.09685

- Shaw, P., Uszkoreit, J., & Vaswani, A. (2018). Self-Attention with Relative Position Representations. Proceedings of the 2018 Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies. https://anthology.org/NACA18-2074/

- Radford, A., Narasimhan, K., Salimans, T., Sutskever, I. (2019). Improving language understanding by generative pre-training. https://paperswithcode.com/paper/improving-language-understanding-by

- Radford, A., Wu, J., Child, K., Luan, D., Amodei, D., Sutskever, I. (2019). Language models are unsupervised multitask learners. OpenAI blog, 1(8). https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf

- Brown, T. B., Mann, B., Ryder, N., Subbiah, M., Kaplan, J., Dhariwal, P., & Amodei, D. (2020). Language models are few-shot learners. arXiv preprint:2005.04173

- OpenAI. (2023). GPT-4 Technical Report. https://arxiv.org/abs/2303.08778

- Devlin, J., Chang, W., Lee, K., Toutanova, K. (2018). BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding. https://arxiv.org/abs/1810.04805

- Lewis, M., Wu, J., Jiang, X., Almeida, D., Wainwright, C., Mishkin, P., Zhang, C., Agarwal, S., Slama, K., Ray, A (2022). Training language models to follow instructions with human feedback. Advances in Neural Information Processing Systems, 35:27730–27744. https://arxiv.org/abs/2203.02155

- Hinton, G., Vinyals, O., Dean, J., Dean, E. H. (2015). Improving Language Understanding by Generative Pre-training. https://arxiv.org/abs/1503.02273

- Jiao, X., Yin, Y., Wang, S., Chen, X., Li, Y., Wang, L. (2020). TinyBERT: Distilling Knowledge in Language Models. https://arxiv.org/abs/2007.01312

- Radford, A.,.,,,,,,,,,,,,,,,,,,,,222222).)..------..https://arxiv.org/abs/2211.0411


- * ,,,,,,,,,,, *,,, T. (2022). Efficient Fine-tuning with PEFT. https://arxiv.org/abs/2211.04378

- Lin, C.-Y. (2004). ROUGE: A Package for Automatic Evaluation of summaries. Proceedings of the ACL Workshop: Textization. https://anthology.org/w004-1913

- Papineni, K., Roukos, S., Ward, T., & Zhu, W. (2002). A Method for Automatic Evaluation of Machine Translation. Proceedings of the 40th Annual Meeting of the Association for Computational Linguistics, pages 311–318. Philadelphia, USA. Association for Computational Linguistics. https://anthology.org/P02-1040

- Lavie, A., & Agarwal, A. (2007). METEOR: An automatic metric for MT evaluation with levels of correlation with human judgments. 228–231. https://anthology.org/w07-534

- Meister, C., & Cotterell, R. (2021). Language Model Evaluation Beyond Perplexity. Proceedings of the 59th Annual Meeting of the Association for Computational Linguistics and the 11th International Joint Conference on Natural Language Processing (Volume I: Long Papers), 5328-5339. https://aclanthology.org/2021.acl-long.414

- Zhang, T., Kishore, V., Wu, F., Weinberger, K. Q., & Artzi, Y. (2020). BERTScore: Evaluating Text Generation with BERT. https://arxiv.org/abs/1904.09675

- Chung, H. W., Hou, L., Longpre, S., Zoph, B., Tay, Y., Fedus, W., Li, E., Wang, X., Dehani, M., Brahma, S., Webson, A., Gu, S., Dai, Z., Suzgun, M., Chen, X., Chowdery, A., Narang, S., Mishra, G., Yu, A., Zhao, V., Huang, Y., Dai, A., U, H., Petrov, S., Chi, E. H., Dean, J., Devin, J., Roberts, A., Zhou, D., Le, Q. V., & Wei, J. (2022). Scaling Instruction-Tuned Language Models. https://arxiv.org/abs/2210.11416

- Anthropic (2025). Claude 3.5 Sonnet 具有：https://www.anthropic.com/news/claude-3-5-sonnet

- Galileo (2025). Claude 3.5 Sonnet vs GPT-4o：AI 模型 模型对比。https://galileo.ai/blog/claude-3-5-sonnet-vs-gpt-4o-enterprise-ai-model-comparison

- Dev Nik Claude 3.5 Sonnet vs GPT-4o (2025) https://dev.to/nik/claude-35-sonnet-vs-gpt-4o-49lm1

- Coradari, F., Leonesi, M., & Piangerelli, M. (2025) 小型语言模型的现状与方向：系统性综述。Big Data and Cognitive Computing: 9(7): 189. https://doi.org/10.3390/bdcc9770189

- Sakib Th, Hosain MT, Morol MK. (2025年5月26日) 小型语言模型：架构、技术、评估与未来适应。arXiv 预印 arXiv:2505.19529v1 https://arxiv.org/html/2505.19529v1

- Boston Institute of Analytics (2025). 周报（10月25日 – 11月1日）：语言模型（SLMs）在2025年是如何超越巨头的？（11月1日）https://bostoninstituteofanalytics.org/blog/weekly-wrap-up-25th-oct-1st-nov-how-language-models-slms-are-outperforming-giants-in-2025/

- Besta, M., Barth, J., Schreiber, E., Kubicki, A., Catarino, A., Gerenberg, R., Nyczyk, P., Iff, P., Li, Y., Hausten, S., Sternal, T., Copik, M., Kwasniewski, G., Müller, J., Flis, Ł., Eberhard, H., Wieliadomi, H., & Hoefler (2025) 推理语言模型：蓝图。arXiv 预印 arXiv:2501.11223v1 https://arxiv.org/html/2501.11223v1



- DeepSeek-AI, Guo, D., Yan, D., Dang, H., Song, J., Zhang, R., Ru, Q., Zhou, M., Wang, P., Bi, X., Zhang, Y., Xu, W., Wu, Y., Z, F., Gou, Z., Shao, Z., Li, J., Gao, L., Dai, D., Chen, J., Ji, D., Li, E., Lin, F., Dai, F., Luo, F., Hao, G., Chen, G., Li, G., Zhang, H., Bao, H., Hu, Y., Wang, D., Ding, H., Han, H., Q, H., Hu, H., Li, Q., Guo, J., Li, J., Wang, J., Chen, J., Yuan, J., Qian, J., Li, J., Cai, L., Ni, J., Liang, J., Chen, D., Kong, H., K, A., Guo, G., Guan, K., Yu, K., Wang, L., Zhang, L., Zhao, L., Wang, L., Zhang, X., Xu, L., Xia, L., Zhang, M, Zhang, N., Tang, M., Li, W., Wang, M., Li, T., Tian, N., Huang, H., Wang, P., Wang, Q., Chen, D, Q, Ge, R., Zhang, R, Pan, R., Wang, R, Chen, J., Jin, R, L., Chen, R., Lu, S., Zhou, S., Chen, S., Chen, Y., Wang, S., Wang, Y., Zhou, S., Pan, S., Li, S., S., Zhou, Wu, S., Yu, S., Sun, Y., Tun, P., Pei, T., Sun, T., Wang, T., Zeng, W., Zhao, W., Liu, W., Liang, W., Gao, W., Yu, W., Zhang, W., Xiao, W., L., An, W., Liu, X., Xu, X., Li, X., Li, X., X, Q., Jin, X., Chen, X., Shen, X., Cheng, X., Li, X., Liu, X., Yang, X., Li, X., Wang, X., Zhou, X., Wang, X., Shan, X., Li, Y, X., Li, Y, K., Wang, Y, Q., Wei, Y. X., Zhang, Y., Xu, Y., Li, Y., Zhao, Y., Sun, Y., Wang, Y., Yu, Y., Zhang, Y., Shi, Y, Xiong, E., He, Y., Miao, Y., Wang, Y., Tan, Y., Ma, Y., Liu, Y., Guo, O., Ou, Y., Wang, Y, Gong, G., Zou, Y., He, Y., Xiong, Y., Luo, Y., You, Y., Liu, Y., Zhou, Y., Zhu, Y., Xu, X., Wang, Y, Huang, Y., Li, Y., Zheng, Y., Zhu, Y., Ma, Y., Tang, Y., Za, Y., Yan, Y., Ren, Z., Ren, Z, Sha, F., Fu, Z., Xu, Z., Xie, Z., Zhang, Z., Hao, M., Ma, Z., Yan, Y., Wu, G., Zhu, Y., Li, L., Li, Z., Xie, Z., Pan, Z., Huang, X., Xu, Z., Zhang, G (2025) DeepSeek-Ri: 通过强化学习增强大语言模型的推理能力。arXiv 预印 arXiv:2501.12948 https://arxiv.org/abs/2501.12948

- Shao Z, Wang P, Qu, R, Song J, Zhang H, Zhang M, Li WK, Guo D. DeepSeek-Math：推向开放语言模型数学推理的极限，清华大学

- DataCamp. 什么是提示工程？2025年详细指南（1月）https://www.datacamp.com/blog/what-is-prompt-engineering-the-future-of-ai

- Nearform (2025) 超越提示工程：向上下文工程的转变https://nearform.com/digital-shift-to-context-engineering/

- Llamaindex (2025) 上下文工程——什么是它，以及需要考虑的技术https://www.llamaindex.ai/blog/context-engineering-what-it-is

# 订阅获取免费电子书

新框架、演进中的架构、研究发布、生产环境分析——AI_Distilled 将嘈杂信息过滤，为实际操作 LLM 和生成式 AI 系统的工程师和研究人员提供每周简报。现在订阅，即可获得一本免费电子书，以及帮助你保持专注并掌握资讯的每周见解。

请访问 https://packt.link/80z6Y 订阅，或扫描下方的二维码。

