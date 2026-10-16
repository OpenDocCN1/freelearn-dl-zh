# 9 未来趋势及展望

在前面的章节中，我们从构建基础的 RAG 流水线演进到将其部署为容器化微服务。最近，第 8 章建立了我们的基准架构模式：清晰的关注点分离，使用 Haystack 处理专门的工具流水线，使用 LangGraph 进行代理体编排。

这种“工具对比编排”的范式是未来自治 AI 系统的基础。本章将超越基准，审视塑造下一代 NLP 的技术和伦理进展。我们将探索硬件限制、行业标准和自我改进记忆如何将孤立的代理体转变为互操作的、智能生态系统。

在本章中，我们将涵盖以下主题：

- 硬件的局限性与机遇

- LLM 的当前局限性

- 代理体的未来：协议、编排与上下文

- 代理体记忆的演进：代理体上下文工程

- 关键新兴领域

在本章结束时，你将具备应对当前技术局限性、并在快速演变的基于 LLM 开发格局中把握新兴机遇的知识。

## 硬件局限性与机遇

驱动我们代理体系统的 LLM 的开发和部署，与底层硬件的能力和限制紧密不可分。这种硬件挑战不仅是模型初始训练的问题，更是未来推理和部署的关键驱动力，直接影响我们的容器化 Haystack 工具和 LangGraph 编排器如何在生产环境中运行。

以下是一些常见的硬件局限性：

- **计算需求**：训练 LLM 需要通过具有数十亿参数的模型处理海量数据集。传统的 CPU 无法胜任这些任务，必须采用专门的加速器，如图形处理器 (GPU) 和张量处理单元 (TPU)，它们是为高效的并行处理而设计的。

- **内存限制**：大模型的规模可能会超过标准硬件的内存容量，在训练和推理过程中产生严重的瓶颈。这需要使用高内存 GPU 或分布式计算环境来容纳这些模型。

- **能耗**：训练和运行这些模型的能量需求是巨大的，引发了人们对环境影响和财务可持续性的严重关注。

为了缓解上述硬件局限性，我们可以关注 NVIDIA 生态系统提供的关键新兴机遇，特别是 NVIDIA AI 基础模型、NVIDIA NeMo 和 NVIDIA 推理微服务 (NIM)：

- **NVIDIA AI 基础模型**：该类别涵盖社区贡献和 NVIDIA 构建的、在 NVIDIA 加速基础设施上运行的 LLM，为模型选择提供了稳健的基准（https://www.nvidia.com/en-us/ai-data-science/foundation-models/）。

- **NVIDIA NeMo**：被认为是开发生成式 AI 模型的关键新兴平台，NeMo 提供了一个可扩展的训练和定制框架（https://www.nvidia.com/en-us/ai-data-science/products/nemo/）。

- **NIM**：NIM 是一组预构建的容器化微服务，专门针对 AI 模型的优化推理（https://www.nvidia.com/en-us/ai-data-science/nim-microservices/）：

  - **使用 TensorRT 优化**：NIM 模型使用 NVIDIA TensorRT 进行优化，这是一个基于 CUDA 并行编程模型的 API 生态。通过量化、层和张量融合以及内核调优等技术，TensorRT 确保了从边缘设备到数据中心的所有 NVIDIA GPU 上实现高性能推理。

  - **架构相关性**：NIM 模型与本书建立的架构相关。它们镜像了第 7 章和第 8 章中的部署模式，即我们使用 Hayhooks 将 Haystack 流水线封装为微服务。NIM 代表了下一个逻辑步骤：将基础模型本身作为可调用的微服务部署。

  - **未来集成**：在代理体系统中，编排器不会区分这些服务；它将简单地调用 Haystack/Hayhook 工具执行专门的 RAG 任务，并调用 NIM“工具”执行原始生成任务。

Haystack 已通过 NVIDIA 集成实现了能够构建 RAG 应用的模型访问（https://haystack.deepset.ai/nvidia）。该集成提供了以下组件：

- **NVIDIATextEmbedder**：用于嵌入字符串的组件，使用 NVIDIA Foundation 和 NIM 嵌入模型。对于区分查询和文档输入的模型，该组件将输入字符串作为查询。

- **NVIDIADocumentEmbedder**：用于嵌入文档的组件，使用 NVIDIA AI Foundation 和 NIM 嵌入模型。

- **NVIDIAGenerator**：使用 NVIDIA AI Foundation Endpoints 提供的生成模型生成文本的组件。

- **NVIDARanker**：使用 NVIDIA NIM 进行文档排序的组件。

不想或无法通过云提供者的 API 调用利用 LLM 的公司或行业，在将模型作为 RAG、NLP 和代理体工作流的一部分时面临独特挑战。与云消费者不同，这些团队必须直接管理之前讨论的硬件限制：

- **基础设施开销**：他们必须提供高内存 GPU 来本地处理大模型的参数规模和计算需求。

- **运营复杂性**：他们负责推理层的优化和编排，需要大量的 DevOps 工作来维持低延迟。

为了缓解这些问题，开发者可以利用工具来简化本地模型服务：

- **本地推理提供商**：如 Ollama (https://ollama.com/)、vLLM (https://docs.vllm.ai/en/latest/) 和 LM Studio (https://lmstudio.io/) 允许团队以最少的设置在本地拉取并运行优化模型。

- **框架集成**：通过使用 Haystack 和 LangGraph 中相应的集成，开发者可以将云端 LLM 生成器组件交换为本地对应组件。这允许将模型直接加载到流水线中，而无需外部 API 调用。

- **代理体实现**：一个很好的例子是 LangChain 的本地深度研究员（https://github.com/langchain-ai/local-deep-research），该代理系统能够完全在本地环境中使用各种资源收集、组织并编写报告。

### LLM 的当前局限性

当前这一代 LLM 虽然强大，但存在根本性的局限性。这些局限性不仅仅是学术问题；它们是定义该领域未来的高级智能体（agentic）解决方案的主要动力。下列出的每种局限性都是使“未来趋势”成为“必然选择”的“原因”。我们可以将其分为四个关键领域：真实性问题、上下文问题、黑盒问题以及集成问题。接下来让我们详细分析每一项。

## 真实性问题

LLM 是概率系统，而非事实性数据库。它们容易产生幻觉，生成听起来合理但事实错误的信息。这一问题的之所以出现，是因为它们根据学习到的模式来预测下一个词，而不是基于事实依据。这使得它们在关键用场景中变得不可靠。

这是我们在第 4、5 和第 6 章中学习的 RAG（检索增强生成）旨在解决的根本问题。通过检索外部可验证的文档并为模型的回答提供依据，RAG 缓解了幻觉。然而，静态 RAG 仅仅是部分解决方案。这一局限性是智能体上下文工程（ACE）（Zhang et al., 2025）的主要动力，它代表了进化的化的一步：创建一个动态的、自我纠正的上下文以持续确保事实依据。

## 上下文问题



LLM 受限于 token 限制，这限制了它们一次能处理的输入。它们很难在长交互中维持上下文，并且无法执行超出此窗口的复杂多步推理任务。此外，它们缺乏长期记忆，除非显式提供，否则它们无法将知识从一个会话保留到另一个。

这一局限性是我们在第 8 章中采用的智能体架构的主要动力。我们并不是通过向单个 prompt 中填充数 GB 的文本来解决复杂任务的；而是使用编排器（如 `LangGraph`）将问题分解为一系列步骤，顺序调用工具并管理状态。这一局限性也推动了 ACE，它将上下文从“单个 prompt”重构为“进化的剧本”（playbook），能够随时间推移管理策略和知识。

## 黑盒问题

LLM 通常作为“黑盒”系统运行。特定输出背后的内部推理是不透明的，这使得很难信任模型的决策，特别是在法律、医学或金融等高风险领域。

这种可解释性的缺失是智能体与智能体（A2A）协议的主要动力。在复杂的多智能体系统中，追踪决策至关重要的。A2A 的“结构化任务执行”（Open Protocols for Agent Interoperability, AWS blog）不仅仅是一种消息格式；它是一条可审计的轨迹。它提供了清晰的、分步骤的记录，说明哪个智能体被分配了任务、它做了什么以及结果是什么，形成了一个比单一单体黑盒模型更透明、可解释的推理链。

## 集成问题

最后一个关键局限性不在于模型本身，而在于它的应用。即使是最强大的 LLM，如果它无法访问推理所需的数据，也是徒劳无功。目前，这些数据被困在“信息孤岛和遗留系统中”（Anthropic, 2024）。每一个新工具、数据库或 API 都需要“自定义实现”（Anthropic, 2024），这是一种一次性的、脆弱的连接器，难以构建和维护。

这是模型上下文协议（MCP）明确且公开的动力。MCP 由 Anthropic 等人创建，正是为了解决这一特定问题，提供了一种通用的开放标准，以“用统一的实时协议来替代一次性的补丁”（Edwin Lisowski, 2025）。

我们现在将探索截至 2025 年这些局限性是如何被。

## 智能体未来：协议、编排与上下文

本节构成了我们探索的新核心，直接建立在第 8 章建立的模式之上。但在深入技术实现之前，我们必须定义我们所说的智能体未来。

智能体未来代表了一种范式转变：AI 应用不再是孤立的、单体的聊天机器人，而是去中心化、可操作生态系统中的参与者。在未来，智能体不仅生成文本；它们还积极协作、共享工具并随着时间的推移改进自身的推理策略。

这种转变解决了定制架构中固有的扩展性和集成问题。迈向这一未来，我们解锁了具有以下特征系统：

- **可操作：** 用行业范围内的标准取代脆弱的自定义集成。
- **去中心化：** 允许来自不同团队或供应商的智能体进行安全的协作。
- **自我改进：** 从静态内存进化为从失败中学习的动态上下文系统。

在本节中，我们将看到构建的架构（由 `LangGraph` 智能体编排的 `Haystack` 工具）如何被行业正式化以满足这一愿景。随后我们将探索它如何泛化为现代标准化协议。

## 从智能体到编排器：第 8 章回顾

在第 8 章中，我们确定了本书的核心架构论点——生产级智能体系统由两个不同的层组成：

- **工具层：** 专门的、数据密集型任务封装在 `Haystack` 流水线中（例如 RAG、NER 和情感分析）。这些流水线然后使用 `Hayhooks` 部署为可扩展的、无状态的 REST 微服务
- **编排层：** `LangGraph` 状态机充当智能体的“大脑”。它本身不执行任务；它管理高层逻辑并通过通过调用工具的端点来编排工具，从而完成复杂目标。

我们在第 8 章的 Yelp 多智能体系统是一个完美的例子：一个中央 `LangGraph` 协调器编排了一组“工作”智能体，而这些智能体在底层是我们部署的 `Haystack` 流水线工具的简单客户端。这种双层单体架构是我们的新基准。本节的其余部分将探索如何使该系统具有可操作、去中心化和自我改进的能力。我们将引入两个关键协议来实现这一点：MCP 和 A2A。

## 协议层

我们的 Yelp Navigator 的主要弱点在于它是单体系统。`LangGraph` 智能体和 `Haystack`-`Hayhook` 端点是定制的，仅用于彼此通信。如果我们想添加另一个供应商的工具，或者另一个团队想使用他们的 `Haystack`-RAG 工具，就需要新的自定义集成代码。

协议解决了这个问题。它们是正式的、行业范围的标准，定义了组件应该如何通信，从一个脆弱的自定义代码连接世界转向健壮的即插即用生态系统。我们可以将这些协议应用于两个维度，标准化并扩展我们的 LLM 和智能体应用：水平和垂直。

## 垂直连接：MCP

第一个协议 MCP 标准化了编排器与其工具之间的通信。MCP 是一个开放标准，被比“AI 的 USB-C 端口”（What is Model Context Protocol）。它为 AI 应用（客户端）提供了一个通用的客户端-服务器标准，用于发现并连接外部工具和数据源（服务器）。

# 9 

MCP 定义了客户端-服务器架构（What is Model Context Protocol）。当我们将其映射到栈时，我们学习了以下内容：

- MCP 主机/客户端：这是 AI 应用或编排器。在我们的语文中，这是我们的 `LangGraph` 智能体
- MCP 服务器：这是暴露数据或工具的程序。在我们的语文中，这是我们的 `Haystack` 流水线

MCP 服务器暴露了客户端可以的“原语”。这些包括工具（可执行函数）、资源（数据）和提示（可重用模板）。通信通过特定的 JSON-RPC 数据层进行。

我们可以将其直接映射到我们在第 8 章的 Yelp Navigator 项目架构中。MCP 是我们工具层的正式标准化。`LangGraph` 智能体现在不再是向通用的 `Hayhook` 端点进行简单的 HTTP 调用，而是将与标准化的 MCP 端点进行通信。

这并非遥远的未来；通过我们所学习的框架，今天就可以实现。`Haystack` 内置了 MCP 集成 `mcp-haystack`（Edwin Lisowski, 2025）。

在等式的另一侧（即编排器），`langchain-mcp-adapters` 库和相关教程提供了构建 `LangGraph` 智能体的代码，该智能体可以发现并消费通过 MCP 服务器暴露的任何工具。

## 水平连接：A2A 协议

当 MCP 标准化智能体与其工具之间的“垂直”连接时，A2A 标准化了智能体之间的“水平”连接。（Clarifai, Amdjed Bensalah 和 AKKA）。由 Google 驱动的 A2A 协议是一种专门为智能体间通信、协作和协调设计的对点协议（OneReach AI）。

如果 MCP 标准化了我们的 `LangGraph` 编排器与其 `Haystack` 工具之间的连接，A2A 则标准化了 `LangGraph` 编排器与另一个

## 未来趋势及展望

团队的代理代理编排器（如 `LangGraph`、`CrewAI` 等）。它允许我们的 Yelp Navigator 监控器安全地将任务委托给完全独立的代理，即使这些代理由不同的团队构建并运行在不同的基础设施上。

A2A 构建在两个核心概念之上（代理互操作性开放协议（Open Protocols for Agent Interoperability），AWS 博客）：

- **Agent 卡片 (Agent cards)**：这是该协议的发现机制。Agent 卡片是代理发布的一种元数据“名片”，描述了其能力、联系方式以及安全性要求。
- **结构化任务执行 (Structured task execution)**：这是用于通信的可审计工作流。它允许代理委托任务、跟踪进度、共享中间结果并处理执行结果。这种结构化、可追踪的通信解决了“黑箱”问题。

这项技术在我们技术栈中也可以直接访问。`LangSmith`（https://www.langchain.com/langsmith/observability）作为 `LangChain` 和 `LangGraph` 的生产监控层，内置了对 A2A（https://docs.langchain.com/langsmith/server-a2a）的支持。通过 `LangSmith` 的代理服务器部署 `LangGraph` 代理，开发者会自动创建一个兼容 A2A 的端点。这已被用于演示在完全不同框架构建的代理之间的互操作性，例如 `LangGraph` 代理与 `CrewAI` 代理的通信。

为了巩固这两个截然不同但互补的协议，表 9.1 提供了直接对比。

| 特性 | MCP | A2A 协议 |
|---|---|---|
| 主要目标 | 工具和数据访问 | 代理协作与编排 |
| 通信模型 | 垂直（客户端-服务器） | 水平（点对点） |
| 类比 | 工具的 USB-C 接口 | 代理的商务会议协议 |
| 核心概念 | 原语：工具、资源和提示词 | Agent 卡片（用于发现）和结构化任务（用于执行） |
| 解决的关键 Key问题 | 取代碎片化的、定制定制的工具集成 | 实现去中心化的、跨平台的代理协作 |
| 实现（在我们技术栈中） | `LangGraph` 代理（客户端）到 `Haystack` MCPTool 组件（服务器） | `LangGraph` 代理（对等节点）到 `LangGraph` 代理（对等节点）（通过 `LangSmith`） |

表 9.1 - 代理通信协议对比

在完成了工具到代理再到 MCP 和 A2A 协议的稳健映射后，我们准备好进入下一个改进领域：持续上下文工程。

## 代理内存的演进：代理上下文工程 (agentic context engineering)

现在我们已经标准化了代理与工具（MCP）以及代理之间（A2A）的通信方式，我们必须解决最后的前疆：它们如何“思考”以及如何“学习”。这是代理内存和 RAG 的演进。

代理工程（ACE）是一个框架，它将代理的上下文重新定义为不是静态的、固定的提示词（prompt），而是动态的、“演进的剧本”。这是一种通过不断构建和修改输入（上下文）而不是更改模型权重（微调）来改进行为的方法（Zhang et al., 2025）。

ACE 通过模块化的迭代化的过程工作，允许代理的剧本根据性能进行演进：

1. **生成 (Generation)**：代理使用其当前上下文（剧本）尝试执行任务。
2. **反思 (Reflection)**：代理（或独立的反思代理）检查结果、执行轨迹，并就应如何修订上下文或策略生成自然语言反馈。
3. **策展 (Curation)**：这些反馈被分析并整合到上下文中，增量更新剧本以积累成功策略并丢弃失败策略，从而防止上下文崩溃。

该框架是我们在*第 5 章*和第 6 章中学习的严谨、手动评估过程的*自动化和在线执行*：

- 在*第 5 章*中，我们手动构建了一个流水线来为评估生成合成数据
- 在*第 6 章*中，我们手动使用 `Ragas` 运行离线评估，以测试我们的 RAG 流水线

通过 ACE，*反思*步骤类似于在每个任务后在线运行的自动化 `Ragas` 评估。*策展*步骤是一个自动化的提示词编写过程，能够立即实施发现的结果。这种循环是构建真正自主、自我改进系统的关键。我们现在准备好应对这些进展如何在伦理和运营领域创造了关键的新兴领域。

## 关键新兴领域

本节探讨了随着 LLM 和代理应用兴起而的一些关键新兴领域。我们研究了伦理、法律、运营和决策领域的 AI，突出了 NLP 如何与负责建立公平透明 AI 系统的领域交叉。最后，我们考虑了 NLP 工程师在这一不断变化的格局中的角色，强调了技术专家如何能够为制定政策、促进问责和将公平性嵌入 AI 驱动的解决方案做出贡献。通过这些新兴领域，NLP 社区可以在塑造不仅强大而且符合社会价值和伦理标准的 AI 应用方面发挥至关重要的作用。

## 伦理、法律、运营和决策中的 AI

LLM 的快速采用开始重塑伦理、法律、运营和决策等关键领域，在这些领域，它们处理海量信息和生成上下文相关见解的能力可以显著增强工作流。然而，随着影响力的增加，解决关键挑战的需求日益增长，包括公平性、问责制、透明度和偏见问题。在本节中，我们探索了 LLM 如何应用于这些领域，并结合近期的研究和案例来说明它们在法律文档分析、政策起草、运营优化和数据驱动决策等任务中的作用。通过理解机会和风险，我们的目标是突出确保……的实践和新兴框架。

### 伦理中的 AI

在论文《解构大模型的伦理：从长期问题到新涌的困境：综述》（https://doi.org/10.1007/s43681-024-00797-3）中，Deng 等人（2024）探讨了随着 LLM 快速发展而产生的伦理机会和挑战。作者将伦理关注点分类为长期问题，如数据隐私、版权和公平性。论文强调的一个机会是差分隐私和联邦学习在训练和推理过程中保护用户数据的潜力。此外，保护版权的技术（如水印和后门机制）提供了一条路径……诸如来自人类反馈的强化学习（RLHF）和监督微调的技术可以通过引导……帮助 LLM 生成符合伦理标准和社会价值观的响应。

作者还强调了监管框架（如《欧盟 AI 法案》）在促进问责和合规方面的作用。通过技术进步和监管措施解决伦理问题，AI 领域可以向透明、公平和可靠的生态系统发展。

### 法律中的 AI

在论文《法律领域的大语言模型：综述》（https://www.scencedirect.com/science/article/pii/S22665162400172）中，Lai 等人（2024）讨论了 LLM 在法律领域的变革性影响、可能出现的新机会以及持续的挑战。

一个重大机会涉及利用 LLM 自动处理法律流程，如起草法律文件、审查合同和总结判例法。通过利用 LLM 强大的 NLP 能力，法律执业人员可以显著减少日常任务所需的时间和精力的。例如，Cui 等人（2023）开发的 `ChatBy` 和 Nguyen（2023）开发的 `LawGPT` 等系统被用于为法律查询提供快速响应、相关先例，甚至为法官和律师提供法律推理。

此外，LLM 促成了智能法庭的发展，在法庭中，证词记录、转录和证据分析等司法流程可以通过 AI 驱动的自动化进行简化。这些模型在预测分析方面也显示出潜力，可以帮助根据历史数据预判法律案件。这种预测能力有助于诉讼和和解过程中的风险评估和决策。

然而，尽管这些进步带来了显而易见的处，论文也强调了解决伦理和法律问题的重要性，包括 AI 生成决策的透明度以及训练数据集中偏见的消除。确保公平、维护用户隐私和促进算法问责对于……至关重要。

未来趋势及超越

大语言模型（LLMs）在法律系统中负责任的整合。这强调了 AI 伦理作为一个关键新兴领域的重要性。

## 运营与决策中的 AI

在论文《在运筹学领域评估 LLM 推理能力：ORQA 方法](https://ojs.aaai/index.php/AAAI/article/view/34673)》中，Mostajabdave 等人（2024）强调了利用 LLM 处理运筹学（OR）任务的若干机会。作者引入了 ORQA，这是一项旨在评估 LLM 在制定和回答复杂优化问题时的推理和解决问题的基准测试。

论文中讨论的一个主要机会是使用 LLM 将自然语言问题描述翻译为数学优化模型。这种能力对于降低运筹学的门槛具有特别价值，因为它能让没有正式数学知识的用户也能参与模型构建。翻译任务需要多步推理来识别决策变量、约束条件和目标函数，在这些领域，LLM 可以通过自动化建模过程的初始阶段提供帮助。

此外，论文还强调了 LLM 作为交互式工具引导学生理解复杂的优化概念，从而增强增强运筹学教学和学习的潜力。问答数据集（如 ORQA）的创建为 LLM 支持自学教育提供了机会，通过解释不同建模方法背后的原理并验证用户的回答。

最后，作者们强调了多代理协作框架的潜力，在该框架中，LLM充当领域专家与软件系统之间的中介，简化了生产和交付路由等任务。通过实现更无缝的人机协作，这些应用可以提高运营效率，并缩短手动建模和验证的时间。然而，论文也指出需要提高模型的准确性和一致性，以确保 LLM 能可靠地处理微妙的运筹学任务。

这些机会强调了 LLM 在扩大运筹学工具获取途径和增强各行业决策流程方面的变革性作用，前提是推理准确性和数据稀缺等挑战得到有效解决。

在结束本章之前，我们将讨论通过 MCP 和 A2A 协议实现的工具和代理标准化如何在这些领域体现。

# 9 

### 重新评估自主代理的影响

标准化协议（MCP 和 A2A）和自我改进上下文（ACE）的引入，将代理从实用的工具提升到了自主行动者。这种新力量迫使我们通过一个新的、复杂的视角，重新评估伦理、法律、运营和安全等关键新兴领域。

### 可互操作世界中的伦理与法律

我们最初的讨论关注单个 LLM 的偏见或“黑箱”性质。新的、更复杂的问题是分布式责任和标准化风险问题：

- **A2A 与分布式责任：** 在一个由 A2A 治理的世界里，任务在自主代理网络中被委派。考虑这样一个场景：一个部门的自主金融代理使用 A2A 将任务委托给另一个部门的法律代理。法律代理反过来使用 MCP 从一个恰好有错误的第三方案例法中提取数据。这一系列事件导致了一个缺陷的自动化法律决策，给公司造成了数百万美元的损失。谁负责？金融代理的开发者、法律代理还是第三方 MCP？A2A 通过结构化任务执行创建了清晰的审计追踪，但这条轨迹揭示了一个全新的、复杂的灰色地带，这是孤立 LLM 缺乏解释性时从未触过的。
- **MCP 与数据隐私：** MCP 标准化了数据访问，但也引入了隐私风险。当任何符合 MCP 标准的代理都可以向任何符合 MCP 标准的服务器请求数据时，安全负担就转移到了服务器端。MCP 规范明确指出，实施者应该建立稳健的同意和授权流。构建 `Haystack` 流水线并将其暴露为 MCP 服务器的 NLP 开发者，现在处于管理任何可能连接到它的代理的数据隐私和同意的最前线。

### 大规模运营与决策

随着复杂的代理模式和架构的引入，我们可以专注于完全自律的运营。以下是 MCP 和 A2A 如何影响运营的：

- **企业中的 MCP：** MCP 是解锁被困在遗留系统中数十年价值的关键。企业可以将现有的 API、SAP 系统或内部数据库封装在安全的、管理的 MCP 服务器中。这允许新的、强大的 AI 安全且高效地在这些系统上运行。这已经在发生，例如微软（`Copilot Studio`）（（Microsoft Ignite）等公司已采用 MCP 将代理连接到其工具中。



## 用于业务流程自动化的 A2A

`A2A`是自动化复杂的、跨部门业务工作流的协议。你的 **Yelp 导航员** 变成了 **企业导航员**。可以给一个高级主管代理分配一个目标（如入职新员工），它可以使用 `A2A` 协调 `HR`、`IT` 和财务部门中原本孤立的代理之间的系列任务（Sydney Cost, 2025）。

## 用于可靠决策的 ACE

这种级别的自动化只有在可以信任的情况下才可能。`ACE` 提供了这种信任的机制。通过创建剧本（`playbooks`），自主代理可以从运营失败中学习。如果工作流失败，`ACE` 循环（反思、整理）允许代理修改策略，确保它下次执行任务时更加可靠，而无需人类开发者干预、调试和重新部署。

### 新前沿：代理安全运营 (AgentSecOps)

这是代理未来最关键且最新颖的含义。原始章节中的安全讨论仅限于误信息和偏见。新的代理安全领域解决了一种全新的、复杂的威胁模型，其中攻击向量是构建代理所依赖的自然语言信任（Christian Posta, 2025年5月5日）。

漏洞不再仅仅是用户的提示词（提示词注入）。新的漏洞是工具和代理编排器消耗的元数据：描述和代理卡（`agent cards`）。攻击者可以使用恶意的、被投毒的自然语言描述来欺骗 `LLM`。这产生了一类新的攻击方式，深探讨了 AI 代理攻击向量（Deep Dive Attack Vectors for AI）。

#### 命名攻击（冒充）

攻击者注册一个与合法工具名称相似的恶意 `MCP` 或 `A2A` 代理（例如，金融）。

#### 上下文投毒（间接提示词注入）

攻击者发布一个看似无害的工具，但其描述包含恶意的提示词（例如，计算提示）。

#### 影子攻击

恶意的工具描述可以影响合法工具的行为。例如，一个“症状检查器”工具的描述可能包含向患者发送账单数据。

#### 跑路（Rug pulls）

一个恶意的 `A2A` 代理或 `MCP` 工具建立了良好的声誉并集成到成千上万个工作流中。某一天，它改变了行为，开始操纵结果或收集数据。这种新威胁给开发者带来了巨大的责任。为 `Haystack` 工具或代理卡编写清晰、简洁且安全的描述不再仅仅是文档，而是主要的防线。

## 总结

`LLM` 在伦理、法律和运筹学等领域的应用，强调了 `AI` 在变革决策、简化工作流和扩大复杂系统获取途径方面的巨大潜力。然而，随着 `LLM` 影响力的增长，解决公平、透明度和问责等关键挑战的责任也随之增加。隐私保护技术、监管框架和交互式工具等机会，强调了将技术创新与伦理考虑相结合的需求。在法律领域，`LLM` 可以自动化法律文档并支持预测分析；而在运筹学领域，它们可以协助将自然语言描述翻译为数学模型，并实现更高效的人机协作。

`NLP` 工程师在推动这些领域方面发挥着关键作用，开发对齐方法、创建性能评估基准、设计能够多步推理和动态工具集成的 `AI` 系统。他们的知识对于构建可解释模型、策划高质量训练数据以及提高模型对偏错误的鲁棒性至关重要。通过与跨学科团队合作，`NLP` 工程师可以确保 `AI` 系统既可靠又符合伦理，推动增强信任并在各种应用中产生社会的负责任创新。

作为掌握了整个栈的开发者——从构建 `Haystack` 组件、使用 `Hayhooks` 部署、使用 `LangGraph` 进行编排，到理解领先的协议（`MCP` 和 `A2A`）以及自我改进技术（`ACE`）——我们不再仅仅是 `NLP` 工程师。我们是下一代自主系统的架构师。

在确立了未来的愿景后，我们现在转向我们的目标：对整个旅程进行总结。在*结语*中，我们将从探索转向整合，将*工具与编排*的论点正式化为一份决定性的架构蓝图。我们将

让我们回顾上一次的 Yelp Navigator，将其演变为一个具有韧性、具备状态感知能力的系统，并通过模拟关键基础设施的故障来严格评估其完整性。这项最终分析将弥合我们今天编写的代码与未来自主、主权代理体之间的鸿沟。

## 延伸阅读

- Yang, J., Jimenez, C. E., Wettig, A., Lieret, K., Yao, S., Narasimhan, K., & Press, O. (2024). SWE-agent: Agent-Computer Interfaces Enable Automated Software Engineering. https://arxiv.org/abs/2405.15793
- Deng, C., Duan, Y., Jin, et al. (2025). Deconstructing the ethics of large language models from long-standing issues to new-emerging dilemmas: a survey. AI Ethics S, 4745–4771. https://doi.org/10.1007/s43681-025-00797-3
- Lai, J., Wan, W., Ju, Z., & Yu, P. S. (2024). Large Language Models in Law: A Survey. AI Open. https://www.sciencedirect.com/science/article/pii/S26665102400022
- Cui, J., Li, Z., Yan, Chen, B., & Yuan, L. (2023). ChatLaw: Open-source large language model with integrated external knowledge bases. https://arxiv.org/abs/2306.102400
- Nguyen, H.T. (2023). A brief report on LawGPT 1.0: A virtual legal assistant based on GPT-3. https://arxiv.org/abs/2302.05729
- Mostajabdeh, M., Mu, Y. T., Dash, S. C. B., Ramamonijson, R., Byusa, J. S., Cini, G., Zhou, Z., & Zhang, Y. (2024). Evaluating LLM Reasoning in the Operations Research Domain with ORQA. https://ojs.aai/index.php/AAAI/article/view/34673
- DeepSeek-AI, et al. (2025). DeepSeek-RI: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning. https://arxiv.org/abs/2501.12948
- Zhong, W., Guo, L., Gao, Q., Ye, H., & Wang, Y. (2023). MemoryBank: Enhancing Language Models with Long-Term Memory. https://arxiv.org/abs/2305.10250
- Shao, Z., Wang, P., Zhu, Xu, R., Song, J., Zhang, M., Li, Y., Wu, Y., & Guo, D. (2024). Deepseek: Pushing the limits of mathematical reasoning in open language models. https://arxiv.org/abs/2402.03300
- Huyen, C. (2025). Agents Chip Huyen. https://huyen.com/2025/01/07/agents.html
- Zhang, Q., Hu, C., Upasani, S., Ma, B., Hong, F., Kamanuru, V., Rainon, J., Wu, C., Ji, Li, H., Thakker, U., Zou, J., & Olukotun, K. (2025). Agentic Context Engineering: Evolving Contexts for Self-Improving Language Models. https://doi.org/10.48550/arXiv.2510.04618
- Aldridge, N., & Ward, J. (2025). Open Protocols for Agent Interoperability Part 4: Inter-Communication on A2A. AWS Open Source Blog. https://aws.amazon.com/blogs/opensource/open-protocols-for-agent-interoperability-part-4-agent-communication-on-a2a/
- Anthropic. (2024). Introducing the Model Context Protocol. https://www.anthropic.com/news/model-context-protocol
- Listowski, E. (2025). MCP Explained: The New Standard Connecting AI to Everything. Medium. https://medium.com/@belisowski/mcp-explained-the-new-standard-connecting-ai-to-everything-79c5a1c92828
- Model Context Protocol. (n.d). What is the Model Context Protocol (MCP). https://modelcontextprotocol.io/docs/getting-started/intro
- Latenode. (2025). MCP Integration: LangGraph Complete Model Context Protocol Setup Guide + Working Examples 2025. https://latenode.com/blog/ai-frameworks-technical-infrastructure/langgraph-multi-agent-orchestration/langgraph-mcp-integration-complete-model-context-setup-guide-working-examples-2025
- Kavitkar, A. (2025). Building an Advanced AI: A Step-by-Step Guide to Integrating MCP Servers with LangGraph. Medium. https://medium.com/@amolkavitar/building-an-advanced-ai-agent-a-step-by-step-guide-to-integrating-mcp-servers-with-langgraph-ff519db6e
- Clarifai. (2025). MCP (Model Context Protocol) vs A2A (Agent-to-Agent Protocol) Clearly Explained. https://www.clarifai.com/blog/mcp-vs-a2a
- Bensalah, A. (2025). MCP and A2A: Clearly Explained. Medium. https://medium.com/@djax/mcp-and-a2a
- AKKA. (n.d). MCP, A2A: What does it mean? https://akka.io/blog/a2a-what-does-it
- OneReach AI. (n.d). What is the Model Context Protocol (MCP)? https://onreach.ai/blog/what-is-mcp
- Microsoft. (2025). Extend your agent with Model Context Protocol. https://learn.microsoft.com/en-us/copilot-studio/extend-mcp
- Scott, S. (2025). Building Enterprise: A Guide to AI. https://blog.workday.com/en-us/building-enterprise-a-guide-to-ai
- DeepSeek-AI, et al. (2025). Deep Dive MCP and A2A. https://www.solo.io/deep-dive-mcp-and-a2a
- solo. (2025). The quantitative argument. https://www.solo.io/quantitative-argument
- Medium. (2025). Tracing the evolution of architecture. https://medium.com/@djax/tracing-the-evolution-of-architecture
- Medium. (2025). A case study in microservice. https://medium.com/@belisowski/case-study-in-microservice
- Medium. (2025). The agentic inflection point. https://medium.com/@djax/the-agentic-inflection-point
- Medium. (2025). The tool versus orchestration thesis. https://medium.com/@belisowski/the-tool-versus-orchestration-thesis
- Medium. (2025). Context engineering in the Yelp Navigator V3. https://medium.com/@djax/context-engineering-in-the-yelp-navigator-v3
