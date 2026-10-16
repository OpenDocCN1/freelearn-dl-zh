## 5 使用自定义组件开发 Haystack 流水线

在之前的章节中，重点是将预定义组件组装成流水线，以实现特定目标，从数据接入到带有智能体的高级 **RAG**。这一章标志着熟练程度的性转变：从 Haystack 框架的用户转变为能够扩展它的架构师。其核心目标是学习如何构建自定义组件并将其集成到复杂的流水线，从而释放 Haystack 解决独特领域特定挑战的全部潜力。

为了将 RAG 开发从一种艺术转变为成熟的工程实践，量化的、数据驱动的方法是至关重要的。在本章中，我们将涵盖以下主题：

- 如何在 Haystack 中定义自定义组件

- 高级自定义组件功能的实现

- 测试和调试自定义组件

## 技术要求

我们将继续使用 Jupyter notebook 和虚拟环境，正如仓库中 ch5 文件夹的**安装说明（Setup Instructions）**部分所示。你可以在仓库文件夹中找到安装说明的完整步骤分解：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/tree/ch5#setup-instructions

## 如何在 Haystack 中定义自定义组件

在本章中，我们将关注自定义组件的创建以及如何将其整合到 Haystack 流水线。Haystack 的自定义组件提供了实现特定功能的活性，无论是过滤结果、与外部交互还是执行标准组件未涵盖的任务。随着 Haystack 2.0 的引入，创建、复用和共享这些自定义组件变得更加高效，增强了 NLP 流水线的定制和扩展性。

### 自定义组件的关键要求

在本节中，我们将概述构建集成到 Hay 流水线的自定义组件所需的要求。在 Haystack 中定义自定义组件需要以下内容：

- `@component`装饰器：这是将 Python 类注册到 Haystack 框架的主要机制，表明该类可以实例化并在流水线中使用。此装饰器使类对 Haystack 可见，使其能够管理生命周期并调度执行。

- `__init__`方法：标准的 Python `__init__`方法作为组件的构造函数。它的主要作用是配置和依赖注入。对于在运行之间不会更改的静态参数（如 API 密钥、模型名称或与外部服务的连接），应在此传入并存储为实例属性。通过`__init__`使组件变得可配置是其复用性的关键。

- `run()`方法：每个组件都必须有一个`run()`方法；它是组件逻辑的入口点也是功能的核心。该方法包含了执行其特定任务的代码，无论是获取数据、转换文本还是调用 LLM。一个关键规则是`run()`方法必须始终返回一个字典。字典的键对应组件的输出插槽，值则是传递给下游其他组件的数据。

- `@component.output_types`装饰器：此装饰器添加在`run()`方法上。它指定了组件产生的输出类型和名称。

声明的名称和类型必须与 `run()` 方法返回的字典保持一致，确保数据流的一致性和可预测性。

通过这些要求，我们可以在 Haystack 中定义自定义组件，使其成为可以在带有预定义 Haystack 组件的管道中运行的类。

Haystack 2.0 最重大的架构改进之一是从 1.x 版本的隐式字典传递数据流，转变为基于“插槽”（sockets）的显式、类型安全模型。这一设计选择是框架透明度和健壮性的基石。组件的合约（即它接受什么输入以及产生什么输出）直接在代码中被显式地定义：

- **输入（Inputs）**：输入插口通过在 `run()` 签名中将其声明为类型参数来定义。Haystack 引擎会自动验证进入插口的数据符合指定的类型，从而捕获潜在的集成错误。

- **输出（Outputs）**：输出插口使用直接放置在 `run()` 方法上方的 `@component.output_types()` 装饰器定义。装饰器接收关键字参数，其中键是输出插口的名称（必须与 `run()` 返回的字典中的键匹配），值是预期的数据类型。

这种显式的“数据合约”相比 Haystack 1.x 隐式的字典传递是一个巨大的改进，后者可能难以追踪和调试。它反映了软件工程向显式接口发展的更广泛趋势，使 RAG 开发不再是一项“黑盒”NLP 任务，而是一门标准的、可调试的软件工程学科。框架的 `draw()` 方法生成管道的可视化图，是这种哲学的终极体现，将抽象代码转换为具体的、可验证的图表。让我们来实现第一个自定义组件并将其集成到管道中。

## 组件拆解：Prefixer

我们将使用一个包含自定义 `Prefixer` 组件的 Jupyter 笔记本，该组件展示了构建健壮且可预测组件的关键设计模式。该组件简单地为 Haystack `Document` 对象列表中的每个文档添加用户指定的前缀。

你可以在这里找到探索简单 `Prefixer` 自定义定义的 Jupyter 笔记本：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch5/jupyter-notebooks/prefixed_custom-component.ipynb。

让我们拆解它的工作原理。

`run()` 方法内部的核心逻辑遍历输入列表中提供的每个 `Document` 对象。对于每个文档，它执行两个关键操作：

- **不可变处理（Immutable processing）**：它会创建一个新的 `Document` 对象。这是一种最佳实践，避免了原地修改原始数据，这可能会在复杂的管道中导致意外的副作用。

- **元数据保留（Metadata preservation）**：它显式地将原始文档元数据复制到新文档中。这确保了重要信息（如来源、ID 等）在转换过程中不会丢失。

新文档的内容被设置为格式化后的`f"prefix{doc.content}"`字符串，并将新文档添加到`modified_documents`列表中。

在集成到管道之前，笔记本通过独立运行组件来验证其功能。以下是初始化和验证独立组件的关键步骤：

1. **初始化（Initialize）**：创建组件：

```
prefix_instance = Prefixer()
```

2. **执行（Execute）**：直接调用 `run()`：

```
result = prefix_instance.run(
  documents=documents, prefix="This is a prefixed document: ")
```

3. **验证（Verify）**：检查结果，确认它包含一个字典`{'documents': [...]}`，其中每个文档的内容已成功添加前缀，且元数据保持完好。

虽然组件定义允许创建根据我们定制的组件，但自定义组件的真正价值在于能够无缝集成到更大的 Haystack 管道中。笔记本通过构建一个简单的管道演示了这一点，将我们的 `Prefixer` 组件的输出直接馈给 `DocumentWriter` 组件。该管道执行了以下操作序列：



# 初始化管道和组件

首先，初始化必要的组件。这包括我们自定义的 `Prefixer`、用于存储最终数据的 `InMemoryDocumentStore` 类，以及配置为使用该存储器的 `DocumentWriter` 组件。以下是初始化代码块：

```python
# 创建一个内存文档存储器
document_store = InMemoryDocumentStore()

# 初始化我们自定义的组件
prefixer = Prefixer()
# 创建一个写入存储器 store=
writer = DocumentWriter(document_store=document_store)
# 创建管道
pipeline = Pipeline()
```

## 添加并连接组件

接下来，将组件添加到管道中并连接它们的插槽。这建立了数据流。`prefixer` 自定义组件的输出插槽（命名为 `documents`）连接到 `writer` 组件的输入插槽（也命名为 `documents`）。以下是将组件添加到管道的代码：

```python
# 将组件添加到管道
pipeline.add_component("prefixer", prefixer)
pipeline.add_component("writer", writer)

# 将 prefixer 的输出连接到 writer 的输入
pipeline.connect("prefixer.documents", "writer.documents")
```

## 执行管道

使用`run()`方法执行管道。关键在于，输入是以字典形式提供的以馈给 `prefixer` 组件。以下是用自定义组件运行管道的代码：

```python
pipeline.run(
  {
    "prefixer": {
      "documents": documents,
      "prefix": "This is a prefixed document: "
    }
  }
)
```

此操作激活了数据流。`prefixer`接收文档和前缀，并将将其输出（修改后的文档）传递给`DocumentWriter`，后者将其保存到`InMemoryStore`中。

## 验证最终输出

最后，笔记本查询`.filter_documents()`。这确认了数据库中的文档已通过我们自定义的 `Prefixer` 组件处理，验证了整个工作流的成功。

在示例笔记本中，我们创建了一个为对象添加前缀的自定义组件。现在，我们将转向在自定义组件内部添加功能，当一个计算密集密集型步骤（如加载模型）需要其仅加载一次时。

# 管理状态和重型资源：`warm_up()` 方法

构建 AI 管道时的一个常见挑战是管理昂贵的、一次性的初始化任务，例如下载语言模型或建立数据库连接。在`__init__`方法中执行这些任务会减慢初始管道定义，而在`run()`方法中执行则会非常低效。

为了解决这个问题，Haystack 提供了`warm_up()`方法。该方法是一个特殊的钩子，管道引擎会在每个组件实例上调用一次，在第一次调用`run()`方法之前。它是执行重型操作的理想位置。

在本节中，我们将突出一个简单的示例，该示例包含了最少的要求，以确保我们的自定义组件兼容并可以作为 Haystack 管道的一部分引入。

在构建组件时，你会面临一个挑战：你在哪里加载资源？如果你在`__init__`构造函数中加载，组件的初始化就会变慢。仅仅创建类的实例（例如`my_embedder = MyEmbedder()`）就会触发长时间的下载或模型加载过程，这既不灵活又低效。另一方面，如果你在`run()`方法中加载，你的组件将极其低效。`run()`方法每处理一批数据就会调用一次。这意味着你每次都会从磁盘重新加载整个模型，为每次操作增加了巨大的延迟。

为了解决这个问题，Haystack 通过`warm_up()`方法提供了一个标准的解决方案。这种模式将组件的逻辑清晰地分为三个阶段：

- **配置 (Configuration) -- `__init__(self, ...)`**：此方法应该是轻量级且快速的。它的工作只是存储配置参数，例如模型名称或 API 密钥。它不应该加载任何重型资源。
- **初始化 (Initialization) -- `warm_up(self)`**：这是执行重负载的地方。此方法在管道启动时由 Haystack 调用一次。它的任务是加载模型、建立数据库连接，或执行任何其他一次性设置。它将加载的资源存储在组件实例上。
- **Processing – `run(self, ...)`**：这是你组件的核心逻辑。它在流水线执行期间被反复调用，并且设计目标是快速运行。它假设`warm_up()`方法已经过调用，并可以直接使用预加载的资源。

让我们来看一个示例，我们将加载一个 Transformers 模型并应用它将文本转换为向量格式。

你可以在这里找到探索自定义组件定义的 Jupyter notebook：https://ngithub.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch5/jupyter-notebooks/warm_up_component.ipynb。

提供的 notebook 构建了此自定义组件的简单示例：名为`LocalEmbedderText`的文本嵌入，然后它被适配为`LocalEmbedderDocs`以与 Haystack Documents 配合。该组件加载了一个 SentenceTransformer 模型来生成嵌入。让我们拆解它的“流水线”来理解它是如何运作的：

## 阶段 1 – 配置 (`__init__`)

首先，初始化组件。这一步是瞬间完成的，因为它只存储了要使用的模型名称；并没有加载它：

```python
class LocalEmbedderDocs:
    def __init__(
        self, model_name:
            str = "sentence-transformers/all-MiniLM-L6-v2"

        self.model_name = model_name
        # 模型尚未加载
        self.model: Optional = None
```

## 阶段 2 – 初始化 (`warm_up`)

这是关键的准备步骤。`warm_up()`方法检查模型是否已经加载。如果尚未加载（如果`self.model`不为 None），它将继续将大型 SentenceTransformer 模型加载到内存中并赋值给`self.model`。如果再次调用`warm_up()`，它会发现`self.model`不再是`None`并将什么都不做，从而防止不重新加载。

我们可以在这里看到一个热身初始化示例：

```python
def warm_up(self):
    """
    加载 SentenceTransformer 模型。
    在第一次运行之前
    仅被调用一次。
    """
    if self.model is None:
        self.model = SentenceTransformer(self.model_name)
```

## 阶段 3 – 处理 (`run`)

这是组件的主要“工作”。当带有`Document`对象列表调用`run`方法时，它会执行两个操作：

- **安全检查：** 它通过检查`self.model`是否为`None`来验证`warm_up()`方法已运行。如果模型未加载，它将抛出`RuntimeError`。
- **执行：** 它从每个`Document`中提取文本内容，使用预加载的`self.model`生成嵌入，并返回标准的输出字典。

当该模式用于流水线时，其真实价值就显而易见了。notebook 通过构建完整的预处理流演示了这一点：从 PDF 文件到 Haystack 文档，流水线内部的数据流应用了文档拆分器并将本地嵌入器应用于拆分后的文档。这里是最重要的部分：你不需要自己调用`warm_up()`。

当你执行`pipeline.run(...)`时，Haystack 设计用于为你管理这一切。在任何数据开始流动之前，Haystack 会自动检查流水线中的所有组件。每个具有`warm_up()`方法的组件，它都会调用一次该方法。

这确保了在你的 PDF 数据被转换、清理和拆分时，`LocalEmbedder`组件已经“预热”，其模型已加载到内存中，准备好以最高速度生成嵌入。这种关注点分离使得 Haystack 流水线既具有高度灵活性又具有极高性能。

在本节中，我们学习了自定义组件定义的知识。现在，我们将所学应用于一个高级案例：构建一系列自定义组件以从 PDF 文件或爬取的网站创建知识图谱，然后使用该知识图谱从文档中生成问答对，稍后这些可以用这些评估 RAG 系统。

# 高级自定义组件功能实现

在之前的章节中，我们学习了自定义组件定义的知识，包括`run()`方法以及用于管理重型资源的`warm_up()`方法。现在，我们将所学应用于一个高级案例：构建一系列自定义组件以从文档中创建知识图谱，然后使用该图谱生成问答对，用于评估 RAG 系统。

在深入研究代码之前，理解我们为什么要使用知识图谱作为中间步骤至关重要。虽然直接从文本块生成查询似乎更简单，但这种方法有显著的局限性。依赖于对孤立文本块进行向量搜索的标准 RAG 系统难以处理复杂的、多跳查询（Su et al. 2020）；Neo4j (2025)，这些查询需要分散在多个文档或上下文中的信息。

知识图谱在向量搜索失败的地方表现优异。它们将数据存储为节点（人、公司或概念等实体）和边（它们之间的关系）组成的网络（Neo4j, 2025）。这种显式的、连接的结构允许复杂的遍历和推理。通过表示信息是如何连接的，而不是仅仅是连接了，知识图谱为生成真正测试 RAG 系统推理能力的问题提供了必要的支架，超越了简单的事实检索（Linders et al. (2025)）。这种方法通常被称为图 RAG (Neo4j, 2025)，是构建更智能系统的尖端技术。

# Ragas 框架：RAG 评估的基础

这正是 Ragas 框架旨在解决的挑战。Ragas 是一个开源 Python 库，为评估 RAG 流水线提供了全面的工具包。可以在此处找到完整文档：https://docs.ragas.io/en/stable/

它的核心设计理念是评估 RAG 系统的两个主要组件，即检索器和生成器，既独立评估也作为一个整体评估。Ragas 框架的一个主要优势是能够使用基于 LLM 的指标提供高质量评估，许多指标不需要大量的人工标注“标准答案”数据集。当我们为了产品化而构建混合流水线时，仅仅断一个流水线比另一个好是不的。我们必须从轶性的检查转向数据驱动的工程实践。这需要一个专门工具包来量化我们 RAG 组件的性能。

虽然 Ragas 提供了许多指标，但它的评估围绕 RAG 质量的关键维度：

- **忠实度 (Faithfulness)：** 衡量生成的答案相对于检索上下文的事实一致性。这是对 LLM 幻觉的直接衡量https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/faithfulness/
- **回答相关性 (Response relevance)：** 评估生成的答案与用户问题的相关性https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/answer_relevance/
- **上下文精确度 (Context precision)：** 衡量检索上下文的信噪比。它评估上下文块与查询是否相关https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/context_precision/
- **上下文召回率 (Context recall)：** 评估检索上下文是否包含了回答问题所需的所有信息https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/context_recall/

通过这些指标，我们可以客观且量化地评估 RAG 流水线。

在创建流水线之前，我们需要一个高质量的测试数据集。手动创建一个多样化的、复杂的多跳问题是一个重大的开发瓶颈。这是 Ragas 提供的第二个、也是对我们而言最关键的能力：**合成测试数据生成**。这些合成数据由基于文档的问答对组成，使用不同的角色和策略生成。我们流水线的自定义组件是这种高级 Ragas 方法论的直接实现。

Ragas `TestsetGenerator`采用了一种解决测试数据的新颖方法：

1. **知识图谱创建**：首先，它导入一组源文档并构建知识图谱。这种基于图的表示法显式地映射了文本中的实体和关系，超越了简单的、孤立的块。
2. **查询合成与合成数据生成**：它利用这个结构化的知识图谱来驱动一组查询合成器。这些合成器旨在生成不同复杂度的查询，这就是为什么生成的 CSV 文件包含`synthesizer_name`列：
   - a. `MultiHopSpecificQuerySynthesizer`：创建直接的基于事实的问题
   - b. `MultiHopSpecificQuerySynthesizer`：生成需要连接文本不同部分特定事实的复杂问题
   - c. `MultiHopAbstractQuerySynthesizer`：生成需要综合多个概念信息的高级推理问题

我们将通过两个映射每个步骤的自定义组件：`KnowledgeGraphGenerator`和`SyntheticTestGenerator`，在我们的自定义组件中模拟这种设计。通过构建我们的`KnowledgeGraphGenerator`和`SyntheticTestGenerator`组件，我们有效地实现了这一 RAG 开创工作流程的操作化。我们正在利用这种先进技术来创建一个健壮、具有挑战性且多样化的评估数据集，以便充分测试我们 RAG 流水线的推理能力。该练习的完整实现可以在两个独立脚本提供的补充代码中找到。

# 实现 `KnowledgeGraphGenerator` 自定义组件

实现`KnowledgeGraphGenerator`自定义组件的脚本可以在以下 URL ：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/chs/jupyter-notebooks/scripts/knowledge_graph_component.py。

`KnowledgeGraphGenerator`自定义组件通过使用 Ragas 框架将平的文档列表转换为结构化的、互连的网络来运行。该过程从导入文档并将其转换为图结构中的初始节点开始。这里采用的关键技术是应用变换（transforms），具体来说使用`apply_transforms`方法，它通过显式映射实体及其连接，将这些孤立节点升级为丰富的数据网。这种结构对于实现复杂推理至关重要，因为它超越了简单的存储，表示了不同信息片段如何在逻辑上关联：

```
kg = KnowledgeGraph()
for doc in documents:
  kg.nodes.append(
    Node(
      type=NodeType.DOCUMENT,
      properties={
        "page_content": doc.page_content,
        "document metadata": doc.metadata}))
default_transforms_config = default_transforms(
  documents=documents, llm=transformer_llm,
  embedding_model=embedding_model)
apply_transforms(kg, default_transforms_config)
```

图的创建很大程度上取决于组件初始化期间提供的模型的双重作用。LLM 服务文档节点的内容以提取特定实体（如人员、概念或组织），并识别连接它们的关系，有效地构建了图的边。同时，嵌入模型提供了分析文本所需的语义理解，帮助系统衡量概念之间的相似性，以确保形成的连接在上下文中是准确的。这些模型共同允许组件生成一种图 RAG 结构，支持为评估目的生成复杂的、多跳的问题。

结果是一个知识图谱，可以使用相同的脚本以 JSON 格式保存，也可以作为 Python 数据结构传递以创建合成问题。接下来我们将查看一个使用知识图谱生成合成数据的自定义组件。

# 实现 `SyntheticTestGenerator` 自定义组件

实现`SyntheticTestGenerator`组件的脚本可以在以下 URL ：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch5/jupyter-notebooks/synthetic_test_components.py。

`SyntheticTestGenerator`自定义组件将结构化的知识图谱转换为多样化的、高质量的评估数据集。该组件不是随机采样文本，而是使用配置的`query_distribution`来实例化特定的合成器，如单跳特定查询、多跳特定查询和多跳抽象查询。这些合成器定义在脚本中，位于我们`SyntheticTestGenerator`自定义定义内一个名为`create_query_synthesizers`的额外方法中。



这些合成器遍历上游`KnowledgeGraphGenerator`组件提供的知识图谱的节点和边，识别能够生成复杂的单跳和多跳问题的关系，这些问题能够真实测试 RAG 系统的推理能力。这种架构选择确保了测试集不仅是事实的集合，而且是对系统连接不同信息片段能力的全面检查。

为了确保我们的自定义组件保持健壮性，我们在组件的执行中实现了带有回退设计模式（fallback design pattern）的优选路径。由于查询的复杂性更优，从知识图谱生成问题是优选方法，但一个健壮的组件在图生成失败或产生空结果时不能崩溃。我们通过将生成逻辑封装在`try - except`块中来实现这一点：如果知识路径遇到异常，组件将自动地降级到基于文档的生成方法。这确保了流水线产生一组可用集合，即使在非理想情况下也能保持数据流流：

```
try:
    testset = self._generate_from_knowledge_graph(knowledge_graph)
    method = "knowledge_graph"
except Exception as kg_error:
    logger.warning(f"Knowledge graph generation failed: {kg_error}. Falling back to document-based generation.")
    testset = self._generate_from_documents(documents)
    method = "documents_fallback"
```

你会注意到，我们将通过知识图谱或从文档生成测试的逻辑封装在各自独立的方法中；这确保了每个方法在失败时不会出现问题。

使用知识图谱的合成数据使用了 Ragas 的`TestSetGenerator`类，该类使用 LLM 和嵌入模型生成问题，以从图谱中构建问答对：

```
def _generate_from_knowledge_graph(self, knowledge_graph: KnowledgeGraph):
    """使用知识图谱生成测试"""
    try:
        generator = TestsetGenerator(
            llm=self.llm,
            embedding_model=self.embeddings,
            knowledge_graph=knowledge_graph
        )
```

我们自定义组件中的`_generate_from_documents`回退方法也使用了`TestsetGenerator`类，但它是直接解析文档。使用的 LLM 和嵌入模型可以是 Haystack 支持的生成器。在提供的笔记本中，我们展示了如何使用 OpenAI 以及 Ollama 生成知识图谱。但这些可以很容易地替换为其他生成器和嵌入模型。

## 从 PDF 构建知识图谱和合成数据生成流水线

现在我们将拆分从文档库（在我们的示例中为 PDF）根据 PDF 内容构建知识图谱并生成稍后用于评估 RAG 流水线的合成数据的关键步骤。

我们将这些步骤分为几个关键阶段：

- 1. **摄取与预处理**：此阶段的主要目标是从各种来源获取原始的非结构化数据，并将其转换为适用于下游 NLP 任务的标准化、清洁格式：

    - `PDFToDocument`：这是入口。它的工作是执行初始的“获取”操作。`PDFToDocument`摄取本地 PDF 文件并生成标准的`List[HaystackDocument]`，使得流水线的其他部分可以与特定格式无关。

    - `DocumentCleaner`：该组件执行至关重要的预处理步骤。它移除多余的空格和空行，这些可能会引入噪声并干扰 `DocumentSplitter` 的逻辑。

    - `DocumentSplitter`：该组件配置为按`by="sentence"`且`split_length=5`分文档。这是一个关键的设计决策。通过将大型文档拆分为易于管理的、语义连贯的块，我们确保了 `KnowledgeGraphGenerator` 接收到高质量、集中的输入，这对于构建准确的图谱至关重要。

预处理的初始阶段确保了我们的合成数据基于我们选择的源，并且是高质量的。在我们开始创建知识图谱和合成数据之前，还有一个额外的处理步骤。让我们来看一下。

- 2. **桥接组件（`DocumentToLangChainConverter`）**：构建自定义流水线的一个关键架构要求是确保组件之间的数据兼容性：

    - `DocumentToLangChainConverter`是我们创建的自定义“桥接”组件。Haystack 的原生组件（如 `DocumentSplitter`）输出`List[HaystackDocument]`对象。然而，我们用于驱动核心逻辑的 Ragas 框架期望`List[LangChainDocument]`对象。

该组件的`run`方法简单地遍历 Haystack 文档列表，拷贝它们的`.content`和`.meta`属性，并实例化新的`LangChainDocument`对象。这种无缝转换允许我们把基于 Ragas 的自定义组件集成到 Haystack 流水线中。这确保了数据已准备就，并处于适当的数据结构格式，以创建知识图谱。

- 3. **核心组件拆解 – `KnowledgeGraphGenerator`**：这是我们的第一个自定义组件，使用本章概述的原理构建。它旨在创建一个结构化的知识图谱，作为我们测试生成的基础。

`__init__`方法是轻量级的，快速存储带有 LLM 和`apply_transforms`标志的配置设置，同时准备 Ragas 包装器。主`run`方法随后接收文档、孤立节点，并执行其最关键的步骤：如果`apply_transforms`为 True，它将使用 Ragas 及其 LLM，通过处理节点来提取实体、识别关系并创建它们之间的连接（边），从而构建 RAG 智能。最后，该组件返回一个包含完成的知识图谱的字典，准备传递给流水线的下一阶段。

最后一步是使用知识图谱根据不同的策略创建一个代表性的合成数据集，该数据集由问答对组成。

- 4. **核心组件拆解 – `SyntheticTestGenerator`**：该组件消耗知识图谱并生成最终评估数据集。它通过使用带有回退机制的优路径展示了复杂的流水线设计。`__init__`构造函数配置组件，存储`testset_size`和`llm_model`。最重要的是，它存储了`query_distribution`：

```python
query_distribution=[("single_hop", 0.25),
("multi_hop_specific", 0.25),
("multi_hop_abstract", 0.5)]
```

此配置直接指示组件要构建什么类型的问题，确保我们的测试集具有多样性，并对 RAG 的推理能力提出挑战。让我们拆解不同类型的查询分布合成器：

- `single_hop`（单跳）：该合成器生成直接的事实性查询。这些是最简单类型的问题，通常可以从单条信息或上下文回答。25% 的问题将属于此类类型。

  例如：“在 ChatGPT 的背景下，Christopher Ong 是谁？”

- `multi_hop_specific`（特定多跳）：该合成器生成需要连接特定实实的问题。为此，RAG 系统必须从源数据的多个不同部分中检索并合成信息。25% 的问题将属于此类类型。

  例如：“与 Haystack 1.0 相比，Haystack 2.0 有哪些关键改进，特别是在处理循环和自定义组件方面？”

- `multi_hop_abstract`（抽象多跳）：该合成器生成需要跨概念进行更广泛推理的问题。50% 的问题将属于此类类型。

  例如：“ChatGPT 的主要功能是什么？它是如何利用用户交互的？”

## 连接知识图（KG）到测试生成器

```python
pipeline.connect("kg_generator.knowledge_graph",
    "test_generator.knowledge_graph")
```

```python
# 将测试生成器连接到保存器
pipeline.connect("test_generator.testset", "test_saver.testset")
```

这种连接策略是一种关键的设计模式。`test_generator`接收来自`doc_converter`（作为备回回方案）和`kg_generator`（作为首选方法）的输入，使得流水线更加健壮。

- **执行流水线：** 最后，使用`run()`方法执行流水线，并为第一个组件指定输入：

```python
pdf_sources = [Path("/data_for_indexing/howpeopleuseai.pdf")]
result = pipeline.run({"pdf_converter": {"sources": pdf_sources}})
```

这会激活整个数据流。PDF 将被转换、清洗、切分并转换成知识图，然后用该知识图生成测试集，最后保存到 CSV 文件中。现在，让我们来看看该流水线生成的合成数据。

## 分析生成的测试数据 (PDF)

在 PDF 文档上运行此流水线会生成`synthetic_tests_10_from_pdf.csv`文件，位于：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch5/jupyter-notebooks/data_for_eval/synthetic_tests_10_from_pdf.csv。它包含了完整的合成数据集。表 5.1 包含了所使用的两种查询策略示例。请注意，每次执行脚本时，由于知识图和合成数据生成过程的非确定性，查询将会发生变化：

| 问题 | 答案（参考） | 策略 |
| --- | --- | --- |
| “在 ChatGPT 研究背景下，Christopher Ong 是谁？” | “Christopher Ong 是 NBER 第 34255 号工作论文《How People Use ChatGPT》的合作者之一。他隶属于哈佛大学和 OpenAI。” | 单跳特定查询合成器 |
| “不同教育水平的人在使用 ChatGPT 获取实际指导方面的方面有何不同，特别是在工作相关消息和请求类型方面？” | “使用情况……显示出显著差异……36% 的实际指导消息是辅导或教学请求……在工作相关消息方面，37% 是由未获得学学位的用户发送的，相比之下……具有某些研究生教育的用户这一比例为 48%。” [cite: 985, 987, 988] | 多跳特定查询合成器 |

表 5.1 – 包含问题、答案和合成器的合成数据示例

第一个问题是一个**单跳**（single-hop）查询；可以通过单条信息回答。第二个是**多跳**（multi-hop）查询；它要求系统合成三个不同概念的信息：实际指导使用、教育水平和工作相关消息。这正是知识图旨在产生的复杂、高价值的问题。现在我们将看到如何轻松地调整此流水线，以而从爬取的网站生成合成数据。

## 从爬取的网站组装知识图谱和合成数据生成流水线

这是 Haystack 组件架构的模块化和灵活性大放异之处。为了处理实时网页而不是本地 PDF，我们更换了初始的数据摄取组件。流水线的核心逻辑（包括`DocumentCleaner`、`DocumentSplitter`、`DocumentToLangConverter`、`KnowledgeGraphGenerator`和`SyntheticTestGenerator`）保持不变。我们只对流水线的“头部” 了以下更改：

- **移除：** `PDFToDocument`
- **添加：** `LinkContentFetcher`（用于从 URL 获取内容）和`HTMLToDocument`（将原始 HTML 转换为 Haystack Documents）

一个探索如何从网站 URL 生成知识图谱和合成数据的流水线实现的 Jupyter notebook 可以在此处找到：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch5/jupyter-notebooks/web_knowledge_graph_pipeline.ipynb

这证明了我们工作流的可重用性。知识图的生成过程与内容无关；它处理来自 Haystack 2.0 博客文章的处理后的 HTML 内容，其有效性与处理 PDF 内容时相同。

## 分析网页生成的数据 (HTML)

| 问题 | 答案（参考） | 策略 |
| --- | --- | --- |
| “HyDE 在 Haystack 中起什么作用？” | “HyDE 是一种优化技术，可以用于带有自定义组件的 Haystack 流水线。” | 单跳特定查询合成器 |
| "Haystack 2.0 相比 Haystack 1.0 有哪些改进，特别是在处理循环和可定制组件方面？” | "Haystack 2.0 引入了显著改进……在 Haystack 1.0 中，流水线图是无环的……Haystack 2.0 允许流水线图中存在循环……此外，Haystack 2.0 强调……” | 多跳特定查询合成器 |

表 5.2 – 包含问题、答案和合成器的示例合成数据

与 PDF 情况类似，答案分布在不同的来源部分。正如我们在第 4 节中学到的，我们可以使用路由器来合并来自 PDF 以及来自 URL 的文档。让我们通过一个示例来结束本节。

## 高级流水线架构：统一数据源

在之前的章节中，我们成功地构建并将自定义组件集成到单源流水线中。我们旅程的最后一步是展示 Haystack 2.0 架构的真正灵活性：能够同时摄取、处理并统一多种数据类型。

Jupyter notebook 创建了从 PDF 生成知识图和合成数据集的流水线：https://github.com/PacktPublishing/Building-Natural-language-and-LLM-Pipelines/blob/main/ch5/jupyter-notebooks/advanced_branching.ipynb

提供的脚本创建了一个复杂的“分支”流水线。这种设计对于数据分散在不同孤岛（例如内部文档和公共网站）的应用至关重要。

这种架构创建一个“漏斗”效应，分为三个阶段：

1. **并行摄取：** 当流水线执行时，它接收两个分支的输入。`file_router`将 PDF 发送到 `PDFToDocument`，同时，`link_fetcher`将流发送到 `HTMLToDocument`。
2. **统一：** `DocumentJoiner`收集来自 PDF 和网页的 Document 对象。
3. **统一处理：** 从这里开始，流水线与之前的版本相同。合并后的文档列表传递给`doc_cleaner`，然后是`doc_splitter`以及我们的自定义`doc_converter`。

下游流组件 `KnowledgeGraphGenerator` 和 `SyntheticTestGenerator` 完全不知道这些数据源自不同的来源。由于我们设计中的关注点分离，它们仅接收一个统一的文档列表并使用其创建一个全面的知识图和高质量的测试数据集。

### 第 5 章

我们再次在表 5.3 中看到了由不同合成器生成的多种类型问题的健康混合，但这次这些问题来自两个不同的来源。与之前一样，每运行一次流水线时，生成的问题都会各不相同：

| 问题 | 答案（参考） | 策略 |
| --- | --- | --- |
| “Alexa 如何在其功能中利用人工智能？” | “Alexa 是一个语音控制的虚拟助手，它利用人工智能来处理海量数据、识别模式并遵循详细指令。” | 单跳特定查询合成器 |
| “在提高 AI 开发数据访问权限的背景下，法律责任的意义是什么？” | “在提高 AI 开发数据访问权限的背景下，法律责任的意义是巨大的。一系列判例表明，责任取决于案件的事实和情况，这可能导致各种惩罚，包括民事罚款或监禁……” | 多跳抽象查询合成器 |

表 5.3 – 包含问题、答案和合成器的示例合成数据

与 PDF 示例一样，该流水线成功地生成了简单的单跳问题和复杂的多跳问题。这里的多跳问题需要连接“Haystack 2.0 改进”、“循环处理”和“可定制组件”等概念，这些信息分布在源文本的不同部分。

通过这三个使用知识图谱生成器和合成数据生成器自定义组件的 Notebook，我们展示了如何从简单的 `Prefixer`（前缀器）组件进化到一个健壮的多源摄取和测试生成系统，这一切都是通过创建和连接模块化、可重用的自定义组件实现的。接下来，我们将通过用于测试自定义组件的技术概述来结束本章。

## 测试和调试自定义组件

在本章中，我们已经从 Haystack 框架的用户转变为能够扩展框架的架构师。这一转变的关键部分——将 RAG 开发从“艺术”转向“成熟的工程实践”，是编写健壮、可预测且易于调试代码的能力。

> 注意
> **练习为自定义组件实现测试**
> 可以在此处找到在本章中探索的组件的一系列测试：https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/tree/main/ch5/tests。

我们的自定义组件，特别是在复杂的多阶段流水线中，与任何其他软件并无异：它们必须经过严格的测试。

提供的测试脚本如下：

- `test_warmup_components.py`: https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch5/tests/test_warmup_components.py



- `test_synthetic_test_components.py`: <https://github.com/PacktPublishing/Building-Natural-language-and-LLM-Pipelines/blob/main/ch5/tests/test_synthetic_test_components.py>
- `test_knowledge_graph_component.py`: <https://github.com/PacktPublishing/Building-Natural-and-LLM-Pipelines/blob/main/ch5/tests/test_knowledge_graph_component.py>

这些脚本共同展示了验证我们自定义组件每个部分的示例策略。让我们详细分析这些测试中的关键指导原则，你可以将这些原则应用于你自己的组件中。

## 组件测试的关键原则

我们的测试策略（如提供的文件所示）不仅仅是检查正确性。它是为了确保我们的组件具有健壮性和高效性，并与 Haystack 流水线正确交互。我们将概述组件测试的关键原则：

1. **使用模拟（Mocking）隔离组件逻辑**：测试组件最重要的原则是隔离逻辑单元。我们的组件通常依赖于外部服务（如 OpenAI API）或重型资源（如加载数 GB 的模型）。我们的测试不应该是缓慢的、昂贵的或因网络错误而失败的。我们使用模拟来模拟这些依赖项，允许我们孤立地测试组件的内部逻辑。

   - **工作原理**：在 `test_synthetic_test_components.py` 中，`@patch('synthetic_test_components.TestsetGenerator')` 装饰器将真实的 `TestsetGenerator`（它会调用 LLM）替换为"模拟"对象。

   - **最佳实践**：同样，`test_synthetic_test_components.py` 中的 `@patch.dict(os.environ, {'OPENAI_API_KEY': 'test-key'})` 模拟了 API 密钥的存在，允许组件在不需要真实密钥的情况下初始化。这确保了我们的测试是快速且重复的，并可以在任何环境中运行。

模拟使我们能够在不需要执行昂贵操作（如调用 LLM）的情况下断言功能。一旦隔离了它，我们就可以验证组件。

2. **验证组件生命周期**：正如我们在 `warm_up()` 方法中看到的，组件可以具有不同的生命周期：配置、初始化（预热）和运行。我们的测试必须验证这一生命周期。

`test_component_initialization` 中的测试策略确认模型在创建时没有加载（`embedder.model` 为 `None`）。`test_run_with_valid_tests` 中的安全检查确保了在调用 `warm_up()` 之前调用 `run()` 会正确抛出 `RuntimeError`。接下来，`warm_up_loads_model` 验证 `warm_up()` 调用成功加载了模型。通过再次调用 `warm_up()` 并断言模型加载代码未再次调用，这也确认了幂等性，防止了无效的重复加载。

验证生命周期确保组件在运行的任何时候都能按预期正常工作。这引导我们进入下一个原则。

3. **测试配置和初始化**：组件设计为可配置的。`test_component_initialization` 中的测试策略确认模型在创建时没有加载（`embedder.model` 为 `None`）。`test_run_with_valid_tests` 中的安全检查确保了在调用 `warm_up()` 之前调用 `run()` 会正确抛出 `RuntimeError`。接下来，`warm_up_loads_model` 验证 `warm_up()` 调用成功加载了模型。通过再次调用 `warm_up()` 并断言模型加载代码未再次调用，这也确认了幂等性，防止了无效的重复加载。

4. **验证桥接组件（bridge components）**：我们看到了如何使用流水线。我们创建了桥接组件来转换数据。

5. **测试边界情况和优雅失败**：一个健壮的流水线是不会崩溃的。我们实现了以下内容：

   - `test_error_components`: <https://github.com/PacktPublishing/Building-Natural-Language-and-LLM-Pipelines/blob/main/ch5/tests/test_error_components.py>
   - `test_empty_handling`: <https://github.com/PacktPublishing/Building-Natural-language-and-LLM-Pipelines/blob/main/ch5/tests/test_empty_handling.py>

这些测试检查了组件接收到空文档列表的情况。组件应该干净地退出。

## 总结

本章标志着我们从 Haystack 框架的用户转变为能够扩展框架的架构师的关键转变。我们从组装预定义组件转向构建自己的组件，掌握了自定义开发的核心原则。你学习了使用 `@component` 装饰器注册类、使用 `__init__` 方法进行轻量级配置以及使用 `run()` 方法处理组件的核心逻辑。我们还介绍了至关重要的 `warm_up()` 生命周期方法，这是高效管理资源的标准，它将初始化与重复执行隔离开。

我们应用这些原则构建了一个复杂的多阶段流水线，用于一个实用的高级场景：生成高质量的基于图的测试数据集。你学习了如何创建 `KnowledgeGraphGenerator` 从原始文档中提取结构化实体和关系。通过该图，我们的 `SyntheticGenerator` 组件产生了多样化的问题，包括简单的分块生成无法创建的多跳查询。这种方法允许我们充分测试 RAG 系统更深层的推理能力，而不仅仅是简单的事实检索。

最后，我们通过将自定义组件集成到高级架构中，展示了 Haystack 模块化的力量。你看到了如何创建桥接组件以确保数据兼容性，以及如何构建分支流水线，将多种数据源（如 PDF 和网站）统一到单一的无缝工作流中。为了完成从概念到生产的旅程，我们建立了测试的工程规范。我们涵盖了模拟外部服务、验证组件生命周期以及测试边界情况的关键策略，确保我们的自定义组件是健壮、可预测且易于维护的。

## 延伸阅读

- Branić, T. (n.d.). 如何通过知识图谱和 LLM 改进跳跃推理。https://neo4j.com/blog/genai/knowledge-graph-11m-multi-hop-reasoning/
- Linders, J., & Tomczak, J. M. (2025). 用于问答的知识图谱扩展检索增强生成。埃因霍芬理工大学数学与计算机科学系，De Za, Eindhoven。https://arxiv.org/html/2504.08893v1
- Branić, T. (n.d.). 从法律文档到知识图谱。https://neo4j.com/blog/developer/from-legal-documents-to-knowledge-graphs/
- Su, D., Xu, Y., Dai, W., Ji, Z, Yu, T., & Fung, P. (2020). 图卷积网络的多跳跃问题生成。Findings of the Association for Computational Linguistics: EMNLP (2020) (pp. 4636–4647). Association for Computational Linguistics. https://doi.org/10.18653/v1/2020.findings-emnlp-416

## 获取本书的 PDF 版本及独家额外内容

扫描二维码（或访问 packpub.com/unlock）。通过书名搜索此书，确认版本，然后按照页面上的步骤操作。

![](img/502c06ed872898cf88f18f9c142647a5_177_0.png)

注意：请妥善保管发票。直接从 Packt 购买的不需要发票。
