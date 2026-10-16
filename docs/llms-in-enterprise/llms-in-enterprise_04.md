# 第 2 部分

# 高级设计模式与技术

在本书的第二部分中，我们将超越基础知识，探索高级设计模式和技术，用于自定义、优化以及将 LLM 集成到现有系统和工作流中。

这一部分侧重于模式和最佳实践的实际应用，从掌握提示词工程到解决数据策略以及管理部署。

本部分包含以下章节：

- 第 6 章，*企业级 LLM 的提示词工程艺术*
- 第 7 章，*评估 LLM 应用面临的企业挑战*
- 第 8 章，*数据蓝图：构建有效的 LLM 开发策略*
- 第 9 章，*在生产环境中管理模型部署*
- 第 10 章，*加速与优化的推理模式*

# 6

# 企业级 LLM 的提示词工程艺术

大语言模型（LLMs）已成为企业领域中变革性的工具，重塑了企业与信息交互、自动化流程以及与客户互动的方式。随着这些复杂的 AI 系统日益集成到关键业务职中，能够有效地与这些模型进行通信并引导它们，已成为寻求最大化其潜力的组织的关键技能。这正是提示词工程（`prompt engineering`）——这种通过构建输入内容来引导 LLM 行为的战略性艺术——在企业 AI 工具箱中占据中心地位的地方。

从基础 AI 交互到复杂的 LLM 实现，这一历程揭示了一个基本真理：输出的质量与输入的质量成正比。一个设计良好的提示词可能意味着：它能让产生通用、模糊或不准确回答的 LLM，转化为一个提供精确、与上下文相关且具有可操作性洞察的模型。这种区别在企业环境中尤为为重要，因为基于 AI 生成内容的决策可能会对财务、运营和声誉产生重大影响。

考虑一家使用 LLMs 分析市场趋势并生成投资建议的金融服务公司。构建糟糕的提示词可能会对市场状况产生宽泛的概括，对投资策略师几乎没有价值。相比之下，一个精心设计的提示词——指定了分析的精确参数、包含了相关的上下文因素并定义了预期输出的结构——可以产生详细、细致入微的洞察，直接指导投资决策。区别不在于模型本身的能力，而在于如何通过战略性的提示词工程有效地对其进行引导。

提示词工程既是一门技术学科，也是一项创造性活动。它要求理解 LLMs 的底层架构和能力，同时培养对语言模式的直觉，以激发最佳响应。这种双重性使得提示词工程成为计算思维交汇处的独特技能组合，这种组合在企业背景下特别有价值，因为那里技术精确性和沟通清晰度同样重要。

在本章中，我们将研究支持有效提示词设计的原则，探索优化 LLM 交互的高级技术，并解决如管理幻觉和确保响应一致性等关键挑战。我们还将提供实际示例和代码演示来说明这些概念的实际应用，为在现实业务场景中掌握提示词工程提供实践方法。无论你是在开发面向客户的聊天机器人、自动化内容生成，还是构建复杂的数据分析系统，本章涵盖的提示词工程技术都将增强你引导 LLMs 输出符合你特定业务目标和质量标准的能力。

# 提示词工程的力量

任何 LLM 交互的核心都是`prompt engineering`（提示词工程），这是一种设计提示词的艺术，旨在引导模型产生准确、相关且符合上下文的输出。这不仅仅是输入命令，而是关于构建正确的指令以挖掘模型的巨大能力。

## 理解提示词与输出的关系

`well-engineered prompt`（设计良好的提示词）像是一镜精确的透镜，模型通过它来解释当前的任务。提示词与结果输出之间的关系由模型在训练期间学习的复杂统计推理模式支配。通过理解和利用这些模式，我们可以显著影响 LLM 响应的质量和性质。

LLMs 的运行机制是根据收到的输入预测最可能的下一个标记（`tokens`）。提示词确立了模型尝试延续的上下文和模式。当我们构建提示词时，我们本质上是将模型放置在其广阔学习概率空间中的某个特定点上，引导其向最可能从该起点开始的完成类型对齐。

这种概率基础解释了为什么提示词用词或结构的细微变化会导致截然不同的输出。每个词汇的选择、信息的顺序以及请求的框架构建都会以重要的方式改变模型的预测轨迹。对于一致性和可靠性至关重要的企业应用来说，理解这种关系对于开发稳定的生产级 LLM 实现至关重要。

### 提示词作为控制机制

提示词充当 LLM 行为的主要控制机制。与遵循显式编程逻辑的传统软件系统不同，LLMs 响应于自然语言引导。这种灵活性既是它们最大的优势，也是最大的挑战——它们可以适应用语言描述的几乎任何任务，但确保它们正确执行该任务需要仔细的提示词设计。

考虑提示词如何在不同维度上作为控制机制发挥作用：

提示词决定模型将关注哪些信息、可能忽略哪些信息以及将生成什么种类型的内容。通过显式指令或隐含提示，提示词影响响应的语气、正式程度和风格属性。提示词可以规定响应中信息的组织形式、格式和呈现方式。良好的提示词可以引导模型的推理过程，鼓励分步分析或特定的逻辑方法。提示词在模型响应周围建立边界，限制长度或范围，或指定特定要求。

例如，通过提供清晰的指令并指定预期格式，用户可以减少歧义，并引导模型交付与其目标一致的结果。这在需要高准确性的应用中特别重要，例如数据摘要、技术写作或复杂问题解决。

### 提示词作为模型能力的接口

提示词是我们访问 LLMs 内部嵌入能力的接口。这些模型包含了在训练期间获得的海量知识和技能，但如果没有有效的提示，这些潜力中的很大一部分是无法触及的。设计良好的提示词的作用是一把钥匙，解锁与当前任务相关的特定能力。

这种接口功能在企业背景下尤为重要，因为不同的业务部门可能需要访问模型功能的不同方面：

*   营销团队可能需要模型的创意写作能力。
*   法律部门可能需要模型对合同的分析推理能力。
*   客户服务部门可能侧重于模型的对话能力。
*   研究部门可能利用模型对技术信息的综合能力。

这些用例每一个都需要不同的提示词方法来有效访问相关的模型能力。将提示词理解为专门的接口有助于组织为不同的业务职开发定制策略。

### 提示词工程的经济学

从业务角度来看，有效的提示词工程代表了重大的经济机会。同一个模型，通过不同的提示词访问，可以产生质量和效用完全不同的输出。这意味着，通过巧妙的提示词设计，组织可以在不需要额外的计算资源或更昂贵模型的情况下，从其 AI 投资中提取出大量的价值。

考虑一个金融分析任务中基础提示词与工程化提示词的对比。以下是基础提示词：

```
Analyze the quarterly financial data.
```

这种极简的提示词几乎没有为模型提供任何指导。在没有关于要分析哪些财务数据、关注哪些方面、使用何种格式或需要多深分析的具体信息的情况下，模型必须做出许多假设。生成的输出可能是泛泛化的，可能会遗漏关键洞察，或者未能满足业务的特定需求。此类输出的经济价值有限。

现在，让我们来看这个提示词的工程化版本：

```
Analyze the Q3 2024 financial data for XYZ Corporation with the following parameters:
1\. 与前两个季度对比收入和利润率
2\. 识别驱动盈利能力变化的三个关键因素
3\. 将分析格式化为结构化报告，每个发现使用项目符号
4\. 特定的数值数据以支持结论
5\. 将分析限制在 500 字以内
```

这个工程化的提示词提供了全面的指导，大大增加了获得有用、聚焦且直接解决业务需求的分析的概率。尽管使用了相同的底层模型和相似的计算资源，但其输出的经济价值要高得多。

AI 部署平台供应商 Scale AI ([https://go.scale.com/hubfs/Content/Scale%20Zeitgeist%20AI%20Readiness%20Report%202024%204-29%20final.pdf?utm_source=chatgpt.com](https://go.scale.com/hubfs/Content/Scale%20Zeitgeist%20AI%20Readiness%20Report%202024%204-29%20final.pdf?utm_source=chatgpt.com)) 的研究发现，使用优化提示词的企业在实现所需输出的迭代次数上平均减少了 34%，转化为了显著的时间节省和更高效的资源利用。同样，斯坦福大学以人为中心的 AI 研究所的一项研究报告，工程化良好的提示词可以在 62% 的测试企业用例中减少对更贵模型变体的需求，从而大幅节约成本。

## 提示词工程不佳的后果

相反，设计不当的提示词会导致次优的输出，例如无关的、误导性的或不完整的信息。LLM 可能会产生幻觉（自信但错误的回答），或未能理解模糊或歧义查询的上下文。这在企业环境中可能会产生严重影响。

以下是无效提示词带来的业务风险：

- **决策错误**：不准确或不完整的信息可能导致糟糕的业务决策并产生深远影响。例如，一个要求市场分析的模糊提示词可能会遗漏关键的竞争因素，导致策略误导。
- **资源浪费**：通过更好的提示词可以避免在完善或纠正输出上花费时间。组织通常报告，投入了大量的人力重新制作或验证由不提示词生成的 LLM 输出。
- **合规风险**：在受监管的行业中，不准确的信息可能会引发法律风险。例如，金融服务公司使用 LLM 起草客户沟通邮件，如果由于提示词差导致对投资产品产生误导性陈述，可能会面临监管处罚。
- **客户不满意**：在面向客户的应用程序中，无关的回答会损害用户体验。Salesforce 的研究发现，提示词工程不佳的聊天机器人客户满意度比使用优化提示词的机器人低 27%。
- **声誉损害**：持续的糟糕输出会破坏内部和外部对 AI 系统的信任。这可能导致技术的放弃，或拒绝采用具有潜在价值的 AI 工具。

### 案例研究：医疗信息管理

考虑一个医疗场景，使用 LLM 总结患者病历供医生审阅。一个模糊的提示词如`Summarize this patient’s history`可能会产生一个泛概述，遗漏关于药物过敏或近期手术的细节细节。优化的提示词会准确指定要包含和优先考虑哪些医疗信息：

```
Summarize the patient's medical history with a focus on:
1\. 当前药物和已知过敏（对任何严重过敏进行加粗）
2\. 需要持续管理的慢性病
3\. 最近 12 个月内的住院或手术
4\. 过去 30 天内的异常测试结果
5\. 专科医生的当前治疗方案
使用每个类别的清晰标题格式化此摘要，总限制在 400 字以内，确保所有关键安全信息出现在摘要开头。
```

在大型医院系统的一项试点研究中，将医疗摘要从通用提示词切换为特定提示词，使关键信息的遗漏减少了 76%，医生审阅时间缩短了 23%，证明了有效的提示词工程在高风险环境中的显著现实影响。

## 提示词工程的迭代本质

提示词工程不是一项一次性的工作，而是一个不断完善的迭代过程。擅长利用 LLM 的组织通常会开发系统化的提示词开发方法，包含以下特征。

### 提示词开发的结构化方法

定义良好的提示词工程方法可以确保可靠性、一致性和改进模型性能。该过程中的关键步骤包括：

1. **基于任务需求和领域知识的初始提示词设计**：此阶段涉及识别特定的业务需求，并根据对领域和 LLM 能力的理解将其转换为初始提示词结构。
2. **使用多样化输入进行测试**以识别边缘情况和失败模式：对一系列潜在输入进行系统化测试有助于发现提示词设计中的弱点，特别是模型可能误解指令或未能处理某些类型请求的意外方式。
3. **分析输出**以识别优劣模式：通过检查多个输出，工程师可以识别反复出现的问题，如遗漏、幻觉或结构问题，这些问题需要通过完善提示词来解决。
4. **完善提示词结构**以解决已识别的问题：根据测试结果对提示词进行修改，增加清晰度、额外约束、示例或结构元素，引导模型实现更稳定的性能。
5. **与领域专家进行验证**以确保输出符合质量标准：领域专家审查输出，从领域角度验证事实准确性、适用性和实用性，为进一步完善提供反馈。
6. **基于持续性能监控的持续改进**：一旦部署，将根据性能指标、用户反馈和不断变化的业务需求定期审查和更新提示词。

这种迭代方法将提示词工程从一种尝试错误的练习转变为一种持续产生与业务目标一致的高质量输出的系统方法。对于可靠性和一致性至关重要的企业应用，这种结构化方法对于成功实施 LLM 至关重要。

### 衡量与优化

迭代方法的一个关键方面是为提示词性能建立明确的指标。这些可能包括：

- **准确率**：符合事实准确性要求的输出比例
- **完成率**：获得满意回答的请求比例
- **一致性评分**：多次使用相同提示词时输出的变异性
- **相关性评分**：领域专家对输出内容与查询之间相关性的评估
- **效率指标**：所需的计算资源或响应时间

通过在提示词的迭代中追踪这些指标，组织可以量化改进情况，并就提示词设计做出数据驱动的决策。这种以测量驱动的方法有助于证明提示词工程投资的合理性，并展示其业务价值。

## 战略性提示词工程的企业应用

有效提示词工程的战略价值在以下企业应用场景中变得显而易见。让我们详细探讨每一个场景。

### 客服转型

设计良好的提示词可以帮助机器人对客户查询提供更准确、更有帮助的回答，在保持高服务质量的同时减少对人工介入的需求。例如，一家大型电信公司重新设计了其客户服务提示词，包括：

*   用于检测客户情绪的特定指令
*   将回复语气与客户情绪匹配的指南
*   问题诊断的结构化方法
*   针对复杂问题的清晰升级标准

优化后的提示词使得转接人工席的比例降低了 43%，首次接触解决率提升了 28%。客户满意度评分提高了 17%，证明了战略性提示词工程的巨大业务影响。

### 大规模内容生成

对于营销、文档或报告目的，战略性提示词可以确保生成的内容保持品牌语调、包含关键信息点并遵循风格指南。这使得能够大规模生产一致的内容，而这通过纯人工努力很难实现。

一家全球消费品公司实施了一套用于营销内容生成的提示词工程框架，包括：

*   直接嵌入提示词中的品牌语调指南
*   特定于产品的信息需求
*   符合市场需求的文化考量
*   针对监管要求的合规性检查

该框架允许他们同时为 23 个市场生成本地化的营销内容，在保持品牌一致性和监管合规性的同时，将营销活动的上市时间缩短了 64%。

### 数据分析与洞察生成

在分析复杂数据集时，精确的提示词可以引导 LLM 关注特定的模式或关系，提取出可能可能被隐藏的可操作洞察。这种能力对于处理大量非结构化或半结构化数据的组织来说特别有价值。

能源领域的一项分析展示了结构化提示词如何从复杂报告中提取特定洞察：

```
Analyze the attached quarterly oil production reports to:
1\. Identify the top 3 regions showing production decline over two consecutive quarters
2\. Calculate the average percentage decline in these regions
3\. Extract any explanatory factors mentioned in the reports for these declines
4\. Suggest potential mitigating strategies based on successful approaches mentioned elsewhere in the reports
5\. Format findings as a concise executive summary with supporting data
```

这种有针对的方法产生了特定的、可操作的洞察，直接指导了资源分配决策，证明了提示词工程如何将原始数据转化为业务价值。

这些应用中的每一个都证明了提示词工程如何作为企业需求与 LLM 能力之间的关键接口。通过掌握这一接口，组织可以充分利用这些强大模型的全部潜力，同时缓解其固的局限性。

## 理解提示词工程背后的科学

提示词工程是构建有效输入的的艺术与科学，旨在最大化 LLM 输出的相关性和准确性。它使用户能够充分利用 LLM 在各种应用中的潜力，从创意任务到复杂的问题解决。

在深入技术细节之前，了解有效的提示词工程既是科学也是艺术是很重要的。它需要系统性的实验，并结合对 LLM 如何处理和响应输入的深度理解。目标是创建不仅能提取所需信息，而且以一致、可靠且符合企业应用特定要求的方式工作的提示词。

对提示词工程的研究揭示了几个关键原则，这些原则决定了不同 LLM 架构下提示词的有效性。这些原则形成了提示词设计的科学基础。让我们逐一探索这些原则。

## 基础概念

提示词工程的基础原理侧重于理解输入提示词与其生成的输出之间的动态关系。设计良好的提示词利用了 LLM 的训练能力，使其行为对齐满足特定需求的响应。提示词的有效性通常取决于几个核心要素，我们将在接下来讨论。

### 清晰性与具体性

清晰且具体的提示词减少了产生模糊或无关输出的可能性。提示词越精确，模型就越可能生成相关且可操作的响应。例如，“解释数据科学应用”这样的提示词是很宽泛的，而“列出机器学习在零售分析中的三个实际用途，特别关注库存管理、客户细分和需求预测”则既聚焦又提供了丰富信息，促使模型返回与零售特别相关的示例。

Anthropic 对其`Claude`模型的研究（[`https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview`](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)）显示，指定具体的响应格式要求使跨多样化任务的任务完成率提高了 34%。同样，OpenAI 对`GPT`模型的研究表明，带有关于推理步骤明确指令的提示词使复杂问题解决任务的准确性提高了了 28%。

### 上下文线索

上下文是诱发相关响应的关键，特别是对于特定行业的应用。例如，在法律或医学领域，提供额外的上下文（如特定的法律案件或患者场景）有助于模型从这些领域内的相关知识中汲取。



最近的进展强调了领域特定上下文在改进提示词结果方面的价值。例如，`CoCounsel`（原名为 `Casetext` 的法律 AI 平台，在 `Casetext` 于 2025 年 4 月 1 日关闭后正式更名）的研究展示了如何在法律应用中使用 `GPT-4`（由 `GPT-4` 驱动）（[`https://arxiv.org/abs/2212.01326`](https://arxiv.org/abs/2212.01326)），显示了法律专业人士如何通过整合详细案件上下文来提高 AI 生成内容的质量和相关性。虽然 `Casetext` 没有发布确切数字，但《Legal Prompting: Teaching a Language Model to Think Like a Lawyer》等研究表明，使用结构化的法律推理格式（如“问题、规则、应用、结论”`IRAC`）可以显著提升性能，将法律蕴含任务的准确率从 70% 提高到 81% 以上。

同样，妙佑律医学中心（Mayo Clinic）的研究表明，当 AI 系统获得全面的患者数据（如心电图，即记录心脏电活动的图）时，能够以 90% 的准确率检测出房颤，并以 93% 的准确率识别出左室功能障碍风险个体，这凸显了详细输入如何显著提升诊断性能（[`www.mayoclinic.org/departments-centers/ai-cardiology/overview/ovc-20486648?utm_source`](https://www.mayoclinic.org/departments-centers/ai-cardiology/overview/ovc-20486648?utm_source)）。

在客户服务应用中，Zendesk 等公司利用聊天机器人中的大模型（LLMs）提供感知上下文的回应。这些机器人使用客户特定（如购买记录或账户历史）来增强互动并创建个性化的用户体验。

## 有效提示工程的关键见解

基于经验研究和实际应用，有效的提示工程有几个关键见解：

- 提示的特定性与输出质量强正相关。
- 结构化提示（带有编号列表）可以改进输出的组织性。
- 角色分配有助于设定模型处理任务的方式。
- 在提示中包含示例可以指导输出的格式和风格。
- 显式约束（字数、语气和结构）可产生更受控的输出。

这些见解代表了各种大模型应用中研究结果和实践经验的综合。通过系统地应用这些原则，组织可以开发出更有效的提示策略，产生与业务目标一致的一致、高质量输出。

## 提示特定性与输出质量

提示工程研究中最一致的发现之一是提示特定性与输出质量之间的强相关性。如下方的分析所示，更具体的提示通常会产生更可靠、相关且准确的输出：

- **基础提示**：“为一款智能手表写一段产品描述。”

  这个提示太不明确了。它没有包含受众、关键特性或语气，这可能导致泛泛且无重点的回应。

- **改进的提示**：“为一款具有健身追踪和长续航功能的高端智能手表写一段产品描述。让它对潜在买家具有吸引力。”

  这好了一些，因为它提供了一些背景（高端智能手表、健身追踪、电池寿命）和一个通用的语气指令（引人入胜）。然而，它没有结构，并且对于强调哪些点并不清晰。

- **工程良好的提示**：“你是一位营销专家。请为一款具有健身追踪、睡眠监测和两周电池寿命的高端智能手表写一段引人注目的产品描述。使用专业但具有说力的语气，强调关键特性和优势。”

  通过分配角色（“你是一位营销专家”）、指定产品的独特卖点并要求语气，此提示可能获得连贯且有说服力的答案。

- **专家级提示**：“作为一名丰富的电子商务文案作家，为针对健身爱好者的奢华智能手表写一段产品描述。突出尖端的心率监测、GPS 追踪和 AI 驱动的教练功能。语气要信息丰富且具说服力，包含行动号召，并使内容针对在线销售优化。”

  这个提示包含了所有必要的细节：角色识别、受众识别、清晰的结构、关键特性和有说服力的语言。因此，它将产生最高效且精确的响应。

现在我们已经看到了提示特定性的演变，下表根据关键特征比较了它们的有效性：

| 类型 | 有效性 | 关键特征 |
| :--- | :--- | :--- |
| 基础 | 低 | 问题：缺乏目标受众；无产品规格；无语气指导；无字数限制 |
| 改进 | 中 | 问题：缺乏语气指导；无结构规范；无行动号召 (CTA) 要求 |
| 工程良好 | 高 | 优点：特定受众；清晰的产品细节；语气指导；字数限制；特性聚焦；CTA 规范 |
| 专家级 | 非常高 | 优点：角色分配；详细的受众；清晰的结构；特定元素；痛点识别；包含社交证明 |

表 6.1：提示工程有效性比较；参见 https://arxiv.org/abs/2302.11382; https://arxiv.org/abs/2102.09690; https://cookbook.openai.com/articles/techniques_to_improve_reliability; 以及 https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview

这些类别展示了提示工程的精炼程度如何极大地影响模型输出的质量。基础提示提供的指导极少，而工程良好或专家级提示则包含了提高清晰度、相关性和有效性的基本元素。

| 提示类型 | 字数 | 基础分 | 特定性评分 | 总分 | 估计可靠性 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 基础 | 4 | 2 | 0.2 | 2.2 | 14.7% |
| 改进 | 14 | 5 | 0.7 | 5.7 | 38.0% |
| 工程良好 | 42 | 8 | 2.1 | 10.1 | 67.3% |
| 专家级 | 99 | 10 | 5.0 | 14.9 | 99.7% |

表 6.2：提示特征与输出质量之间的关系；参见 https://arxiv.org/abs/2201.11903; https://arxiv.org/abs/2005.14165; 以及 https://arxiv.org/abs/2107.13586

**注意**

这些表格是基于概念提示工程原则的说明性分析，展示了理论分析。

某些结构元素可以持续提升不同模型和任务中大语言模型（LLM）的回复。让我们详细查看它们。

## 层级组织

将复杂的提示词（prompt）分解为清晰的章节和子章节层次，有助于模型理解任务的逻辑结构。例如：

```
Create a business analysis report with the following components:
SECTION 1: MARKET OVERVIEW
- Current market size and growth rate
- Major competitors and their market shares
- Key market trends for 2025
SECTION 2: CUSTOMER ANALYSIS
- Primary customer demographics
- Key customer needs and pain points
- Changes in customer behavior since 2023
SECTION 3: STRATEGIC RECOMMENDATIONS
- Three high-priority action items
- Resource requirements for each action
- Expected timeline and outcomes
```

这种层级结构为信息的组织提供了清晰的指导，使模型更容易生成连贯、结构良好的回复。

## 分隔符与划分标记符

使用分隔符来分离提示词的不同组件，可以显著提高模型区分指令、上下文、示例和实际查询的能力。以下示例使用了常用的分隔符（`<<<` 和 `>>>`）：

```
INSTRUCTIONS: You are an expert financial analyst. Provide a balanced assessment of the investment opportunity described below.
CONTEXT:
<<<
XYZ Corp is a renewable energy startup founded in 2022. They have developed a new type of solar panel that claims 34% efficiency, compared to the industry standard of 22%. They are seeking $5M in Series A funding at a $40M valuation. Their prototype has been independently verified, but mass production has not yet begun.
>>>
QUESTION: Analyze this investment opportunity, covering technological advantage, market potential, valuation reasonableness, and key risks. Provide a final recommendation.
```

分隔符（`<<< 和 >>>`）清晰地将上下文信息与指令和问题隔离开，帮助模型在生成回复时理解应该参考哪些信息。

## 编号列表与符号列表

将指令或要求组织为编号列表或符号列表点，可以显著提高模型对特定要求的遵循程度：

```
Develop a crisis communication plan for a data breach scenario, following these requirements:
1. Include an initial response statement (max 100 words)
2. Outline communication channels to be used in priority order
3. Provide a timeline with specific actions at 1 hour, 24 hours, and 1 week after detection
4. List key stakeholders to be contacted and information to be shared with each
5. Include three potential Q&A responses for media inquiries
```

研究表明，与以段落形式呈现相同要求相比，模型更有可能处理编号列表中的所有项目。

## 关键词优化

通过在提示词中嵌入相关关键词，企业可以使 LLM 的回复与特定领域的需求对齐。这种技术有时被称为 `词汇启动`（lexical priming），有助于引导模型进入正确的知识领域和回复风格。

### 领域特定术语

包含技术或专业词汇可以向模型发出请求的领域背景信号。例如，金融机构在询问“总结季度业绩趋势”时，通过包含“Q3 财务分析”、“同比比较（YoY comparison）”、“ROI 指标”和“EBITDA 性能”等技术术语，可以提高回复的相关性。

来自斯坦福大学 NLP 组（`https://nlp.stanford.edu/pubs/`）的研究显示，提示词中的领域特定术语使医疗、法律和金融领域专业内容生成的准确性提高了 28–42%。这种效应在通用语言可能存在歧的行业特定任务中尤为明显。

### 动作导向型关键词

特定的动词和动作词可以引导模型生成特定类型的回复：

- **分析类动词**：“analyze（分析）”、“compare（比较）”、“evaluate（评估）和 assess（评定）”
- **创意类动词**：“imagine（想象）”、“design（设计）”、“create（创建）和 envision（想象/展望）”
- **指令类动词**：“explain（解释）”、“describe（描述）”、“list（列出）和 outline（概述）”
- **审慎类动词**：“consider（考虑）”、“weigh（权衡）”、“deliberate（审议）和 reflect（思考）”

例如，“分析近期利率变化对贷款申请的影响”通常比“讨论利率和抵押贷款”产生更关注数据、具分析性的回复。

### 措辞与语法

尝试不同的措辞（例如使用主动语态与被动语态或改变句式）可以突出所需的信息。例如，“解释区块链在供应链管理中的主要优势”通常比简单的“区块链如何帮助供应链？”产生更全面的回复。

在直接提问、祈使句或复杂条件结构之间的选择也会影响回复质量：

- **直接提问**：“驱动医疗保健云采纳的三个主要因素是什么？”
- **祈使句**：“列出驱动医疗保健云采纳的三个主要因素。”
- **条件结构**：“如果你正在为医疗保健提供者提供云采纳建议，你会强调哪三个主要因素是驱动因素？”

企业级 AI 平台供应商 Cohere 的测试发现，祈使结构产生了更简洁、更有结构化的回复，而条件提示词通常会产生更细致、更感知上下文的答案。直接提问通常会引发更直接且具教育意义的回复。

## 现实应用：企业政策分析

考虑一个企业用例：公司需要分析新的监管框架如何影响其合规性要求。以下是优化结构和关键词的方法。

这是一个欠佳的提示词：

```
How does the new data protection regulation affect us?
```

以下是一个带有结构和关键词的优化提示词：

```
TASK: Provide a detailed analysis of how the EU General Data Protection Regulation (GDPR) impacts our enterprise data management policies.
REQUIRED COMPONENTS:
1. Identify the 3-5 most critical GDPR compliance requirements relevant to our customer data processing activities
2. For each requirement, analyze:
  a. Current compliance status (compliant/partially compliant/non-compliant)
  b. Technical changes needed to achieve full compliance
  c. Estimated timeline and resource requirements
3. Prioritize requirements based on:
  a. Potential penalty risk (Article 83 of GDPR)
  b. Implementation complexity
  c. Business impact
FORMAT: Structure the analysis as a formal compliance assessment report with executive summary, detailed findings, and prioritized recommendations.
CONSTRAINTS: Focus specifically on Articles 5-7, 12-17, and 32-36 of GDPR that relate to data processing principles, subject rights, and security measures.
```

优化后的提示词使用了领域特定术语（`GDPR`、`compliance` 和 `Article 83`）、清晰的结构元素和明确的格式要求，以引导模型生成全面的分析。

通过应用这些结构和词汇技术，企业可以开发出针对特定业务用例持续产生高质量、相关的输出。

根据上述示例，以下是有效提示词的准则：

- **明确化**：清晰地说明问题的意图。例如，与其问“什么是 AI？”，不如使用“提供 AI 的简洁定义及其在医疗保健领域的主要应用”。
- **提供上下文**：包含相关的背景信息以引导模型。例如，“作为老师，我想设计一个关于 AI 的趣味课程——列出三个我可以使用互动活动。”
- **使用约束**：指定输出格式或关注领域，例如，“用三个要点总结区块链的优势。”
- **通过示例迭代**：通过在提示词本身中添加示例来向模型展示你的预期。例如，“重写句子‘AI 正在改变行业’以使其听起来更正式。示例：‘人工智能正在彻底性地变革各个领域。’”

结合这些策略可以确保提示词构建具有系统化且可扩展的方法，将该过程从试错法转化为定义良好的方法论。

让我们使用 `GPT-Neo` 重新构建上述示例，它可以通过 `Hugging Face` 访问。通过编写更具针对性的提示词，此设置将引导模型关注特定领域，例如“供应链管理”和“区块链优势”。

让我们来看代码安装和解释。

### 初始化流水线

我们使用 Hugging Face `transformers` 库中的 `text-generation` 流线，并指定 `gpt-neo-1.3B`，它是开源的，适用于各种文本生成任务：

```python
# 导入必要的库
from transformers import pipeline, set_seed
# 使用 gpt-neo 初始化 Hugging Face 流线，这是一个免费的开源文本生成模型
generator = pipeline("text-generation", model="EleutherAI/gpt-neo-1.3B")
```



![img/3.png](img/3.png)

**快速提示**：使用 **AI Code Explainer** 和 **Quick Copy** 功能增强你的编码体验。在下一代 Packt Reader 中打开此书。点击 **Copy** 按钮（`1`）快速将代码复制到你的编码环境中，或点击 **Explain** 按钮（`2`）让 AI 助手向你解释一段代码。

![白色背景上的黑色文字，AI 生成的内容可能不准确](img/image_%282%29.png)

![img/21.png](img/21.png)

购买此书将免费赠送**下一代 Packt Reader**。扫描二维码或访问 [packtpub.com/unlock](https://packtpub.com/unlock)，然后使用搜索栏通过名称查找此书。双击检查显示的版本以确保你获得的是正确的版本。

![白色背景上的二维码，AI 生成的内容可能不准确](img/Latest_QR.png)

### 提示词（Prompt）

提示词提供了诸如“透明度”、“安全性”和“追溯性”等关键词，引导模型生成关注供应链管理中区块链优势的响应：

```python
# 设置随机种子以保证可复现性
set_seed(42)
# 定义具有清晰结构和关键词的提示词
prompt = """
描述区块链技术对供应链管理的具体优势。
至少提到三个侧重于透明度、安全性和溯性的用例。
"""
```

### 输出生成

通过设置`max_length=100`，我们限制了响应长度以确保答案简洁，而`num_return_sequences=1`指定了我们只需要一个响应：

```python
# 生成模型响应
output = generator(prompt, max_length=100, num_return_sequences=1)
# 打印响应输出
print(output[0]['generated_text'])
```

### 预期输出与解释

模型输出通常会按照提示词中的指令描述区块链在供应链管理中的优势，涵盖透明度、安全性和追溯性。

**示例输出**：

```
"描述区块链技术对供应链管理的具体优势。至少提到三个侧重于透明度、安全性和溯性的用例。至少分配两个不同视角的用例。识别供应链中的每个主要利益相关者。列出带有主要参与者名称的供应链主要利益相关者。详细分析并描述供应链流程。描述供应链对特定行业的影响"
```

让我们分析一下此输出：

* **透明度**：模型讨论了区块链如何实现对供应链中每个阶段货物的追踪。
* **安全性**：它强调了区块链的安全账本能力，能够防止数据篡改。
* **追溯性**：模型提供了关于区块链记录如何为货物创建轨迹的见解。

在构建带有关键词的结构化提示词的基础上，下一节将通过确保模型保留之前的交流来进一步推进交互，从而增强多步任务或对话驱动任务的连贯性。

## 结合上下文和连续性

在持续交互中，保持上下文的连续性对于确保响应的连贯和相关至关重要。这涉及许多技术，如引用先前的交流或逻辑化地组织提示词，以有效地连接后续交互。对于企业级应用，特别是涉及客户支持系统或虚拟助手等多轮对话的应用，跨交互保持上下文的能力对于提供无缝用户体验至关重要：

* **上下文嵌入**：在提示词中包含之前交流的关键细节或相关背景信息（通常通过附加标记或结构化分段实现），有助于保持模型响应的连续性。然而，这种方法必须考虑模型的局限性，例如上下文长度，它限制了在模型性能下降之前可以包含多少先验信息。高效地对相关细节进行排序对于平衡完整性和连贯性至关重要。

* **对话链**：设计引用早期交流的提示词可以实现更连贯、持续的对话。例如，在医疗助手场景中，提示词可以保留病史或之前的建议，以便在后续交互中在此基础上进行。

让我们来看一个真实示例。Babylon Health 实施了上下文敏感的 LLM，在其虚拟健康应用中与患者提供持续交互。通过在整个用户交互过程中保持上下文，Babylon Health 平台中的 LLM 提供了一致且相关的用户体验，根据对话的早期部分定制响应。

### 持续改进

与模型交互的任何方面一样，评估和完善提示词都是一个过程。企业可以实施 A/B 测试和反馈循环等策略来不断完善提示词，根据实际输出提高响应质量：

* **A/B 测试**：通过测试不同版本的提示词，企业可以识别哪些方案产生结果。例如，比较使用不同技术术语或指令格式的提示词，可以发现哪种方法对特定用例产生更准确相关的响应。

* **反馈循环**：将用户或领域专家反馈直接整合到周期中可以驱动持续改进，特别是对于对术语或准确性有要求的应用。这些反馈可以指出模型不足的边缘情况，为完善提示词提供宝贵的见解。

* **响应变异分析**：检查模型在相同提示词的不同实例下的响应变化，可以识别稳定性问题。如果关键任务的响应显示出高变异，调整模型参数（如将`temperature`设置为`0`）可以确保输出更加一致。

* **纵向评估**：随着模型通过更新演进以及使用模式的变化，组织应该实施持续监控系统来跟踪性能。这有助于检测并解决响应质量的任何下降，确保持续的可靠性。

`Zero-shot`（零样本）和`few-shot learning`（少样本学习）是大型语言模型（LLM）背景下的一种提示工程技术，无需进行大量的微调即可执行任务。零样本学习依赖模型的预训练知识，根据极少的输入处理新任务；而少样本学习则在提示中提供少量示例，以帮助更准确地引导模型的响应。这些技术利用了模型的固有能力，使其能够在极少额外训练的情况下适应新挑战：

* `Zero-shot learning`（零样本学习）：给模型一个不包含预期输出示例的提示。它依靠从预训练获得的通用理解和提示中的细节来推断答案。

* `Few-shot learning`（少样本学习）：在提示定义中给模型提供一个或多个示例，引导其响应格式。通过为模型提供上下文提示，可以提高准确性。

## 使用 Hugging Face 的零样本和少样本学习提示示例代码

使用 Hugging Face 的`GPT-Neo`模型，我们将创建一个零样本提示和一个少样本提示来演示这些技术。以下是它们的实现方法：

**步骤 1：导入必要的库：**

```
# Import necessary libraries
from transformers import pipeline, set_seed
# Initialize Hugging Face pipeline with the GPT-Neo model
generator = pipeline("text-generation", 
    model="EleutherAI/gpt-neo-1.3B")
# Set a seed for reproducibility
set_seed(42)
```

**步骤 2：零样本学习提示：**

对于`zero-shot learning`，我们将提供一个简单的提示让模型解释一个概念，且没有先前的示例：

```
# Define a zero-shot prompt asking the model to define a term
zero_shot_prompt = "Explain the concept of blockchain technology in supply chain management."
# Generate the output
zero_shot_output = generator(
    zero_shot_prompt, max_length=100, num_return_sequences=1
)
# Print the output
print("Zero-shot Output:")
print(zero_shot_output[0]['generated_text'])
```

在这个示例中，模型尝试仅根据其预训练知识回答问题，对所需的格式或深度没有额外的引导。以下是预期的输出：

```
"Blockchain technology in supply chain management refers to the use of distributed ledger systems to ensure transparency and traceability across the entire supply chain. By recording each transaction securely, it helps stakeholders track goods from origin to delivery, enhancing security and trust."
```

**步骤 3：少样本学习提示：**

对于`few-shot learning`，我们将在提示中提供一或两个示例来引导模型。以下是我们的设置方法：

```
# Define a few-shot prompt with examples included
few_shot_prompt = """
System: You are an expert in blockchain technology, specializing in supply chain management. Respond to the following user inquiries with clear and detailed explanations.
Example 1:
User Prompt: "What is the role of blockchain in supply chain management?"
Model Response: "Blockchain technology in supply chain management enables secure tracking of goods across each stage. It helps reduce fraud and improves transparency by providing a decentralized, tamper-proof ledger of transactions."
Example 2:
User Prompt: "How does blockchain help in ensuring ethical sourcing?"
Model Response: "By using blockchain, companies can ensure that products are sourced ethically and securely. Every transaction is recorded, making auditing straightforward and helping to confirm that suppliers meet ethical standards."
Your Turn:
User Prompt: "How does blockchain impact supply chain transparency?"
"""
# Generate the output
few_shot_output = generator(few_shot_prompt, max_length=150, 
    num_return_sequences=1)
# Print the output
print("Few-shot Output:")
print(few_shot_output[0]['generated_text'])
```

这是我们少样本学习示例的预想输出：

```
"Blockchain technology in supply chain management improves traceability, allowing stakeholders to verify the origin and journey of goods. It provides a permanent record that can help companies prevent counterfeits, enhance trust, and ensure compliance with regulations."
```

正如我们在之前的示例中看到的：

* `Zero-shot prompt`（零样本提示）：模型完全根据问题生成响应，没有额外的引导，聚焦于区块链的通用益处。

* `Few-shot prompt`（少样本提示）：在这里，通过提供示例，模型使其响应与示例的风格、结构和内容更加对齐。这种方法有助于生成符合用户所需的输出格式或详细程度的响应。

使用零样本和少样本提示允许企业更灵活地利用 LLM，而不需要进行大量的微调。零样本提示有助于快速获取广泛主题的见解，而少样本提示则帮助针对更特定的应用完善响应，特别是在存在首选响应格式时。

这些技术使得 Hugging Face 的开源模型适用于广泛的场景，从回答通用查询到为更具针对性的应用程序生成结构化响应。

## 管理和缓解 LLM 中的幻觉

在 LLM 的语境下，`Hallucinations`（幻觉）是指这些模型生成错误或具有误导性信息的情况。这种现象构成了重大挑战，特别是在准确性和可靠性至关重要的应用中。对于企业环境，LLM 可能用于生成内部报告、面向客户的通信或决策支持系统的内容，幻觉可能会对组织的信誉和运营完整性产生严重影响。

本节探讨了 LLM 中幻觉的本质，概述了减轻其发生的策略，并分析了现实案例研究以说明有效的管理实践。对于任何在企业环境中实施 LLM 的人员来说，理解如何检测和缓解幻觉是一项关键技能，特别是在法律服务、医疗保健或金融分析等事实准确性至关重要的领域。

### 理解 LLM 中的幻觉

LLM 中的幻觉通常源于模型依赖训练数据中的模式而非外部事实。这导致生成看似合理但错误或无意义的信息：

* `Types of hallucinations`（幻觉类型）：幻觉的范围从微小错误到完全虚构不等。例如，LLM 可能会自信地断言一个完全不真实的历史事实，或将虚构事件视为事实。

* `Causes`（原因）：常见原因包括训练数据的偏差、对噪声数据的过拟合或对复杂数据分布的欠拟合。此外，模型内部缺乏严格的事实检查机制会导致这个问题。

### 减少幻觉的方法

缓解幻觉对于增强 LLM 输出的可靠性和信任至关重要。以下策略旨在减少幻觉的发生和影响：

* `Data quality and diversity`（数据质量和多样性）：确保微调或检索增强生成（`RAG`）使用的数据具有多样性、代表性且高质量，可以显著减少模型输出中的偏差和不准确性。例如，研究发现，在多样化数据集上训练的模型比在同质数据上训练的模型产生的幻觉更少。在企业环境中，这可能涉及策划特定领域的数据集，反映与业务相关的术语、标准和事实知识。

- **监控与反馈**：实施持续监控系统来标记潜在的幻觉以供人工审核，这有助于维护输出的完整性。像 OpenAI 的`ChatGPT`等系统利用用户反馈来随时间提高响应的准确性。对于企业而言，这可能涉及实施一种审查工作流，让领域专家在关键模型输出用于决策过程之前对其进行验证。

- **提示工程**：优化提示词的结构方式可以更有效地引导模型，最大限度地减少生成无关或错误响应的概率。例如，指定预期答案的格式可以帮助模型保持专注。诸如显式指示模型承认不确定性（“如果你不确定，请直说”）等技术，可以减少自信但错误的断言。

- **超参数调优**：调整`temperature`等模型参数可以影响创造性与事实可靠性之间的平衡。将`temperature`设置为`0`可以确保响应的确定性，这对于需要高一致性的应用（如客户支持或技术文档）至关重要。

- **后处理检查**：在生成后采用自动事实检查和验证层，根据可信数据源验证输出的准确性。一个示例是使用诸如`International Fact-Checking Network`类的验证框架，它将生成的断言与事实数据库进行比较。对于处理敏感信息的企业应用，实施多阶段验证过程可以为防止虚假信息提供额外的安全保障。

### 幻觉缓解的深度分析：一些假设性场景

本小节深入探讨了已有效实施幻觉管理策略的特定案例研究，提供了宝贵的见解和实践示例：

- **假设性场景 1——新闻生成**：

    - **问题**：一个人工智能驱动的新闻生成系统经常产生包含事实错误陈述的文章。

    - **缓解策略**：集成多级验证流程，在发布前将初始输出与可信数据库进行交叉比对。

    - **结果**：事实错误显著减少，用户信任度增加，证明了稳健验证方法的有效性。

- **假设性场景 2——医疗建议系统**：

    - **问题**：一个基于`LLM`的系统提供的医疗建议偶尔包含错误的药物剂量信息。

    - **缓解策略**：实施受限的响应框架，将剂量信息的生成限制在医学专家验证过的范围内，确保建议的剂量处于特定患者情况的安全且合适的范围内。

    - **结果**：增强了系统提供的医疗建议的安全性和可靠性，展示了在敏感领域专家监督的重要性。

## 管理幻觉的工具与框架

以下是管理幻觉的一些最佳实践：

- **自动事实检查工具**：自动将生成的内容与可信数据库或事实存储库进行交叉比对的工具，有助于实时验证`LLM`输出。例如，如`ClaimBuster`等工具可以分析`LLMs`提出的断言并对照事实陈述数据库进行检查。企业应用可以受益于与内部知识库或行业特定数据库集成的自定义验证系统，以验证专业内容。

- **模拟**：使用控制环境模拟潜在的幻觉并测试缓解策略，可以为系统在现实世界的更可靠表现做好准备。例如，Google 的对话研究项目`AirDialogue`使用模拟对话来对其模型针对各种输入进行压力测试。组织可以开发反映其特定用例和潜在边缘情况的场景测试框架。

- **稳健的内部检查**：实施版本控制系统和严格的部署前测试协议，有助于确保模型更新不会无意中增加幻觉率。这对于面向用户的应用程序（如客户服务机器人）尤为重要，因为不一致或错误的信息可能会损害品牌声誉和客户信任。

- **反馈集成系统**：建立结构化机制来收集、分析并纳入终端用户对模型输出的反馈，有助于识别反复出现的幻觉模式。反馈循环对于持续改进至关重要，允许组织根据真实性能数据精炼提示词和模型。



- **领域特定评估框架**：开发针对特定行业或应用的专业化评估标准，可以更有效地识别语境中的幻觉。例如，法律服务公司可能会实施专门评估`LLM`生成内容的法律准确性和上下文相关的框架。

通过理解幻觉产生并实施稳健的缓解策略，组织可以显著提高其`LLM`输出的可靠性。先进监控工具的集成结合有效的训练数据管理，确保用户能够信任 AI 系统提供的信息。这对于企业应用尤为关键，因为错误可能会导致重大的业务、法律或声誉后果。

# 总结
本章探讨了企业级`LLMs`的提示工程艺术，重点关注通过精心设计的提示词来优化模型交互的实用策略。我们研究了提示工程的基础概念，强调清晰性、特定性和上下文提示，以引导`LLMs`生成准确且相关的输出。通过掌握这些技术，企业可以确保其`LLM`实现交付准确、相关且可信的输出。

我们研究了进阶技术，包括结构化提示词、关键词优化以及零样本和少样本学习方法，并通过实际示例展示了它们的实现。本章探讨了`LLMs`中的幻觉问题，提供了通过改进数据质量、监控系统和验证技术来缓解的策略。

下一章将探索企业背景下`LLM`性能的评估框架和指标。

# 参考文献
- **思维链提示 (Chain-of-thought prompting):** Wei, J., Wang, X., Schuurmans, D., Bosma, M., Ichter, B., Xia, F., Chi, E., Le, Q., & Zhou, D. (2022). Chain-of-Thought Prompting Elicits Reasoning in Large Language Models. [`arxiv.org/abs/2201.11903`](https://arxiv.org/abs/2201.11903)

- **少样本提示 (Few-shot prompting):** Brown, T., Mann, B., Ryder, N., Subbiah, M., Kaplan, J. D., Dhariwal, P., Neelakantan, A., Shyam, P., Sastry, G., Askell, A., et al. (2020). Language models are few-shot learners. [`arxiv.org/abs/2005.14165`](https://arxiv.org/abs/2005.14165)

- **提示工程模式 (Prompt engineering patterns):** White, J., Fu, Q., Hays, S., Sanborn, M., Olea, C., Gilbert, H., Elnashar, A., Spencer-Smith, J., & Schmidt, D. C. (2023). A Prompt Pattern Catalog to Enhance Prompt Engineering with ChatGPT. [`arxiv.org/abs/2302.11382`](https://arxiv.org/abs/2302.11382)

- **提示校准 (Calibrate through prompting):** Zhao, Z., Wallace, E., Feng, S., Klein, D., & Singh, S. (2021). Calibrate Before Use: Improving Few-Shot Performance of Language Models. [`arxiv.org/abs/2102.09690`](https://arxiv.org/abs/2102.09690)

- **关于提示工程的实证研究：** Liu, P., Yuan, W., Fu, J., Jiang, Z., Hayashi, H., & Neubig, G. (2023). 预训练、提示与预测：自然语言处理中提示方法的系统综述。[`arxiv.org/abs/2107.13586`](https://arxiv.org/abs/2107.13586)

- **OpenAI 的提示工程指南：** OpenAI. (2023). OpenAI 菜谱：提高可靠性的技术。[`cookbook.openai.com/articles/techniques_to_improve_reliability`](https://cookbook.openai.com/articles/techniques_to_improve_reliability)

- **Anthropic 关于提示工程的研究：** Anthropic. (2023). 提示指南。[`docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview`](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)

- **Choi, J., Liao, Q.V., Yuan, Y., et al. (2023).** “用于法律任务的大语言模型评估。”发表于 2023 年自然语言处理实证方法会议集。获取地址：[`aclanthology.org/2023.emnlp-main.892/`](https://aclanthology.org/2023.emnlp-main.892/)

- **Henderson, P., Sinha, K., Angelard-Gontier, N., et al. (2022).** “法律之海：从法律中学习负责任的数据过滤以及 256GB 开源法律数据集。”[`arxiv.org/abs/2207.00220`](https://arxiv.org/abs/2207.00220)

- **Singhal, K., Azizi, S., Tu, T., et al. (2023).** “大语言模型编码临床知识。” *自然（Nature）*，620, 172-180。[`www.nature.com/articles/s41586-023-06291-2`](https://www.nature.com/articles/s41586-023-06291-2)

- **Thirunavukarasu, A.J., et al. (2023).** “医学中的大语言模型。” *自然医学（Nature Medicine）*，29, 1918-1932。[`www.nature.com/articles/s41591-023-02448-8`](https://www.nature.com/articles/s41591-023-02448-8)

- **汤森·路世（Thom Reuters）。** (无日期). 法律服务中的 AI。获取自汤森·路世（Thomson Reuters）。[`legal.thomsonreuters.com/en/insights/artificial-intelligence`](https://legal.thomsonreuters.com/en/insights/artificial-intelligence)

- **Gartner. (2021).** 客服中的 AI 聊天机器人。获取自 Gartner。

- **JAMA Network Open. (2021).** 人工智能在医疗保健中的实施：现状与未来方向。获取自 JAMA Network Open。[`jamanetwork.com/journals/jamanetworkopen/fullarticle/2787744`](https://jamanetwork.com/journals/jamanetworkopen/fullarticle/2787744)

- **HoloAssist (一个用于人机协作的数据集)**：[`holoassist.github.io/`](https://holoassist.github.io/)

- 一个由 EleutherAI 维护的、用于训练语言模型的大规模、多样化数据集。适用于构建先进 AI 系统的研究人员。[`pile.eleuther.ai/`](https://pile.eleuther.ai/)

- 一家提供法律服务和见解的全球性法律事务所，包括关于法律领域 AI 和数字化转型的思想领导力观点。[`www.cliffordchance.com/home.html`](https://www.cliffordchance.com/home.html)

- 关于医疗保健领域 AI 应用的更新和研究文章，涵盖梅奥医学中心（Mayo Clinic）的创新和研究。[`newsnetwork.mayoclinic.org/category/research/ai-and-digital-health/`](https://newsnetwork.mayoclinic.org/category/research/ai-and-digital-health/)

- Google 官方 AI 研究与开发平台，展示了与人工智能相关的项目、工具和资源。[`ai.google/`](https://ai.google/)

- 微软对伦理 AI 开发的承诺，提供原则、工具和框架以确保负责任的 AI 使用。[`www.microsoft.com/en-us/ai/responsible-ai`](https://www.microsoft.com/en-us/ai/responsible-ai)

- 摩根大通（JPMorgan Chase）的背景和概述，美国最大的金融机构之一，包括其技术和创新计划。[`en.wikipedia.org/wiki/JPMorgan_Chase#:~:text=JPMorgan%20Chase%20is%20the%20fifth,largest%20U.S.%20corporations%20by%20revenue`](https://en.wikipedia.org/wiki/JPMorgan_Chase#:~:text=JPMorgan%20Chase%20is%20the%20fifth,largest%20U.S.%20corporations%20by%20revenue)

- Google 关于创建能够进行自然灵活对话的对话式代理的研究见解。[`research.google/blog/towards-a-conversational-agent-that-can-chat-aboutanything/`](https://research.google/blog/towards-a-conversational-agent-that-can-chat-aboutanything/)

- GPT 模型后的 AI 研究机构，提供工具、API 和研究以安全且广泛地推进人工智能。[`openai.com/`](https://openai.com/)

- 一份分析各行业在采用 AI 技术方面的准备情况和挑战的全面报告。[`go.scale.com/hubfs/Content/Scale%20Zeitgeist%20AI%20Readiness%20Report%202024%204-29%20final.pdf?utm_source=chatgpt.com`](https://go.scale.com/hubfs/Content/Scale%20Zeitgeist%20AI%20Readiness%20Report%202024%204-29%20final.pdf?utm_source=chatgpt.com)

我们将探讨企业级环境中大语言模型（LLM）评估的多维度特性，从基础指标到考虑到准确性、相关性、稳健性和伦理考因素的进阶评估模式。通过真实的案例研究和实践示例，我们将展示领先组织如何开发创新方法，以确保其`LLM`应用提供一致、可信且有价值的结果。

理解这些评估框架不仅对于实施`LLM`的技术团队至关重要，对于寻求在管理相关风险的同时最大化 AI 投资回报的业务领导者也同样重要。随着`LLM`越来越多集成到核心业务运营中，正确评估其性能的能力成为区分成功实施与昂贵失误的竞争优势。

# 技术要求
在实施本章中讨论的`LLM`数据策略之前，请确保你已经准备好必要的硬件和软件。

## 硬件要求
你可以在以下环境中运行本章的代码示例：

* Google Colab（推荐使用，便于访问 GPU）

* 本地机器（如果你有所需的硬件）

对于本地运行的用户，推荐的配置如下：

* CPU：`Intel i7`/`AMD Ryzen 7`（或同等设备）

* RAM：至少 `16 GB`（大型数据集推荐 `32 GB`）

* GPU：可选，但为了更快的分词和处理建议配备

* Google Colab 提供免费 GPU（`T4`、`P100` 或 `A100`，取决于可用性）

* 本地使用：`NVIDIA GTX 1080` 或更高版本（优先选择 `RTX` 系列）

* 存储：至少 `10 GB` 空闲空间用于模型和数据

## 软件要求
这些是软件要求：

* 操作系统：`Ubuntu 20.04+`/`Windows 10+`/`macOS 11+`

* Python 版本：`3.8` 或更高

* 关键库和依赖项：

    * `transformers`（用于分词器和模型）：

      ```
      pip install transformers
      ```

    * `torch`（用于 PyTorch 实现）：

      ```
      pip install torch
      ```

    * `pandas`（用于数据处理）：

      ```
      pip install pandas
      ```

    * `numpy`（用于数值计算）：

      ```
      pip install numpy
      ```

    * `openpyxl`（用于 Excel 文件处理）：

      ```
      pip install openpyxl
      ```

    * `beautifulsoup4`（用于网页爬虫示例）：

      ```
      pip install beautifulsoup4
      ```

    * `scrapy`（用于结构化数据提取）：

      ```
      pip install scrapy
      ```

对于`DeepSeek`模型示例，你需要额外的磁盘空间（约 `14 GB`）来下载和存储模型权重。

你可以在书籍附带的 GitHub 仓库中找到本章的代码示例：[`github.com/PacktPublishing/LLMs-in-Enterprise`](https://github.com/PacktPublishing/LLMs-in-Enterprise)

# 评估模式的角色
虽然提示工程（Prompt Engineering）塑造了输入，但`LLM`性能的评估确保了输出符合预期的质量标准。鉴于`LLM`能够针对同一提示生成多种响应，评估`LLM`是一个复杂且多维过程。因此，一个稳健的评估框架不仅必须评估输出的准确性，还要评估其在不同语境下的相关性、连贯性和适应性。

评估`LLM`的性能需要评估其输出质量的多个方面。最关键的评估模式之一是检查响应的准确性和相关性，特别是因为`LLM`的输出通常比传统的机器学习任务需要更多的语境解释。以下章节将探索你应该考虑的一些不同评估角度。

## 输出准确性与相关性
评估`LLM`时最直接关注的是其响应是否准确且相关。然而，与具有明确答案的传统机器学习任务不同，`LLM`输出通常需要上下文解释。

该领域的评估指标包括：

* 任务特定准确率：对于翻译或事实检索等结构化任务，可以使用`BLEU`或`ROUGE`等传统指标来衡量模型输出与预期结果的匹配程度。如果你还不熟悉`BLEU`和`ROUGE`，不用担心，我们将在本章中详细探讨。

* 上下文相关性：在创意写作或对话系统等开放式任务中，相关性变得更加主。在这里，评估必须确定模型对上下文的理解程度，以及响应是否连贯且有意义。

# 响应变异性与稳健性
`LLM`的内在特性之一是响应变异性，即对同一提示生成不同响应的能力。虽然这可以作为促进创意或适应性的优势，但在尝试确保输出的一致性和可靠性时也引入了挑战。

在深入评估之前，微调关键模型参数以使模型符合目标任务至关重要。例如，将`temperature`设置为`0`可确保确定性的响应，为相同的提示提供可重复的输出。同样，设置最大 token 限制（例如对于二分类响应任务设置为`1`）可以限制特定用例的输出（例如在系统提示中定义的是/否回答）。超参数调优是基础性步骤，为根据不同标准对模型进行评估做好准备。

该领域的有效评估必须考虑：

* 稳健性测试：应该使用各种输入对模型进行测试，包括边缘情况或模糊的提示，以评估它们处理异常或意外之外查询的可靠性。

* 提示敏感性：提示词辞的细微变化有时会产生截然不同的输出。例如，询问模型“可再生能源的处处什么？”可能会得到详细列表，而将其重述为“解释为什么可再生能源具有优势”则可能导致更叙述性的响应。评估模型对这些变化的敏感性可以确保在不同变体下的稳定性。

* 多样性指标：对于创意应用，鼓励输出多样性是重要的，但这种多样性必须保持在相关性和准确性的接受范围内。

## 定性衡量：连贯性、流畅度和用户体验
定性评估在确定`LLM`输出的整体质量方面起着关键作用，特别是在交互式应用程序中。它不仅关乎正确性，响应的流、自然度和逻辑结构同样重要。

该领域的常见定性指标包括：

* 连贯性：模型生成逻辑一致且响应的能力。

* 流畅度：衡量语言的自然程度；这涉及评估语法、句法和语气，以确保响应像人类说话或写作。

* 用户满意度：在面向用户的应用程序中，直接反馈或满意度评分可以洞察模型满足用户预期的程度。该指标通常考虑响应的正确性和感知实用性。

## 伦理考虑与偏见检测
`LLM`评估的另一个关键维度是偏见和伦理问题的检测与缓解。在海量多样数据集上训练的`LLM`可能会无意中复制或放大数据中存在的偏见，导致产生问题的输出。

该领域的评估模式包括：



# 偏见审计与伦理评估

## 偏见审计

- **偏见审计**：在不同人口统计或文化背景下对模型的响应进行定期审查，以确保输出生成的公平性和公正。

- `Ethical filters`（伦理过滤器）：扫描输入或输出中有害或不当内容的自动化系统，特别是在医疗或金融等敏感领域。

- `Fairness metrics`（公平性指标）：衡量模型在不同用户组中表现的工具，有助于确保某些人口统计群体不会被系统的响应不公平地不利或排除。例如，实现人口统计学均衡（demographic parity）可以确保模型的结果在性别或年龄等不同群体之间平衡，从而最小化偏见并促进公平。

## 纵向与自适应评估

大语言模型（LLM）的评估不是一次性活动，而是一个持续的过程。随着模型部署到真实环境中，需要随时间对其进行监控和评估。随着新用例的出现以及底层数据或应用的演变，这一点变得尤为关键，因为可能会导致特征漂移——输入特征的特性发生变化，可能影响模型的准确性和可靠性。监控这些变化有助于缓解任何负面影响，并确保模型与其预期任务保持一致。

纵向评估跟踪模型在不同版本或长时间内的表现，而自适应评估则涉及将用户反馈和现实结果整合到评估循环中，确保模型能够适应不断变化的需求和背景。

## 评估中的常见错误

在 LLM 评估过程中出现的常见问题包括：

- `Over-reliance on quantitative metrics`（过度依赖定量指标）：准确率或速度等指标虽然很有用，但往往无法捕捉全貌。将定性评估与定量指标结合是进行全面评估的关键。

- `Ignoring variability`（忽略变异性）：LLM 本质上具有变异性，在评估中忽略这一点会导致对一致性产生不现实的预期。在各种输入和提示词下测试模型的响应至关重要。

- `Inconsistent human feedback`（不一致的人类反馈）：人类评审员的主观评估可能会存在差异，从而为评估过程引入偏见。标准化评估标准并使用多名评审员（如领域专家）有助于缓解这一问题。

在继续之前，需要注意的是，LLM 评估是一个持续的过程，受到真实世界使用、反馈和变异性的影响。我们已经涵盖了关键策略和需要注意的常见陷阱。

接下来，我们将观察这些评估原则如何在企业环境中应用，在环境中，规模、复杂性和合规性带来了独特的挑战。

# 评估 LLM 应用的企业特定挑战

在企业应用中评估和理解 LLM 的性能是一项多方面的挑战。随着企业日益依赖这些模型处理从客户服务机器人到复杂数据分析的各种应用，对稳健、可扩展且有意义的评估方法的需求变得至关重要。LLM 与企业工作流的不断整合，对如何最好地评估其在现实世界的有效性和可靠性提出了关键问题。

## 评估的可扩展性

评估 LLM 时最紧迫的挑战之一是评估过程的可扩展性，特别是对于生成长、开放式输出的任务。传统的评估指标（如准确率或情感评分）在评估较长输出的质量时往往力不从心。例如，利用 LLM 起草详细报告或提议的跨国公司必须确保生成的文档不仅事实准确，而且保持连贯性并符合特定的监管指南。这种复杂性需要一套全面的指标，能够评估数段或数页内容的文本质量。

在实践中，评估生成财务报告的 LLM 涉及分析多种因素：数据呈现的准确性、语言清晰清晰、对金融法规的遵守以及整体连贯性。评估不能仅仅是定量的；定性评估也至关重要。因此，公司通常部署自动评分系统和人类评审员的结合来确保评估的彻底。然而，实施这种双重系统增加了计算需求和评估复杂性，这可能是许多企业的阻碍。

评估的可扩展性还与 LLM 在企业内部服务的应用范围密切相关。每个应用可能都需要独特的评估标准，这使得建立标准的评估框架变得复杂化。例如，在医疗保健领域用于临床文档记录的 LLM 必须遵循一套与用于营销文案生成的 LLM 不同的标准。因此，组织必须分配大量资源来开发定制的评估方法，这可能会减少其他关键业务功能的资源。

## 涌现能力与基准创建

另一个重大挑战在于 LLM 的涌现能力，随着规模的扩大和使用的的演变，这些模型往往会展现出意想不到的能力。现有的基准可能无法充分捕捉这些新能力，因此需要创建新的评估框架和基准数据集。开发这些基准的过程可能非常耗时，并需要领域专家的参与以确保相关性和严谨性。

例如，一家部署 LLM 起草合同的法律服务公司可能会遇到模型开始有效解释复杂法律条款的情况。评估这种能力需要专门为法律背景设计的基准，不仅评估解释的准确性，还评估模型对法律细微差别的理解。开发这些基准通常涉及 LLM 工程师与法律专家之间的协作，这可能会缓慢评估过程。

新兴任务也可能揭示 LLM 之前未测试的能力。例如，当 LLM 表现出高准确率总结长法律文档的能力时，组织必须创建新的基准来有效测试这种涌现行为。这不仅需要时间，还需要对领域复杂性的深入理解，强调了跨学科协作的必要性。

基准创建的一个显著例子是 `The Pile` [1]，它的开发目的是评估 LLM 在信息检索任务中满足约束的能力。`KAB` 数据集说明了传统的评估框架对于新兴的 LLM 能力可能是不足的，特别是在需要精确和上下文查询响应的领域，如医疗保健或法律服务。组织必须准备随着 LLM 能力的演变不断调整其评估策略。

考虑一个使用大语言模型（LLM）作为客服聊天机器人的电子商务平台。如果机器人的响应因模型更新而在一天与一天之间产生巨大差异，可能会导致客户沮丧并产生不可靠的印象。例如，某大型电商公司在更新后，由于聊天机器人提供的回答过于简且帮助性较小，导致客户满意度指标下降。这种不一致性会直接影响公司的盈利，因为糟糕的客户体验通常意味着收入流失和客户忠诚度的降低。

为了应对这些挑战，企业必须在更新环节实施稳健的内部检查。这可能涉及严谨的部署前测试、用于跟踪更改的版本控制系统，以及跨模型版本的一致性监控。这些措施有助于确保模型更新不会破坏服务质量，尽管它们需要大量的规划和投入。此外，组织需要与终端用户建立反馈机制，以获取他们对模型的体验。这些用户反馈可以为未来的迭代和更新提供参考，从而实现持续改进并与用户预期保持一致。

## 与现实任务的交互

在企业环境中，LLM 与现实任务的交互引入了额外的复杂性。在交互式和多代理系统中，LLM 与用户界面的集成程度、与其他系统的协作能力以及辅助完成任务的有效性，可能与其独立的准确性或流畅性同样重要。评估这些交互需要一种更全面的评估方法，以考虑现实应用中的动态性质。

由微软开发的 HoloAssist 数据集（`https://holoassist.github.io/`）展示了一个系统，它提供了一个数据集来评估模型在协助用户完成现实任务和纠正错误方面的有效性。通过评估 LLM 在实际场景而非受控测试环境中运行情况，企业可以获得模型在日常任务中用途的见解。这一评估过程可以揭示模型在实际应用中的优缺点，为未来的开发和部署策略提供参考。

例如，在医疗领域，LLM 可以通过生成患者报告或建议治疗方案来协助医疗专业人员。评估模型在这些语境下的性能，需要评估它理解医学术语的准确性、与电子健康记录系统的集成程度，以及辅助从业者做出明智决策的有效性。为了全面评估这些交互，组织可能会采用模拟现实应用的虚拟环境，以便其在真实条件下评估 LLM 的性能。

## 企业 LLM 应用的现实案例

为了更好地理解 LLM 如何应用于现实企业环境，请考虑以下来自不同行业的示例，每个示例都强调了独特的挑战和评估方法：

*   **法律服务公司**：Clifford Chance（`https://www.cliffordchance.com/home.html`）是一家全球领先的律所，它利用 LLM 协助起复杂的法律文件和合同。这些模型提高了效率，但也面临解读复杂条款的挑战。为了应对这些挑战，Clifford Chance 开发了专门的评估框架，侧重于法律准确性和上下文相关性，并且通常与其内部法律专家合作完成。这种严谨的方法确保了 LLM 符合律所的高标准，并在其工作流中保持可靠工具属性。此类计划反映了法律行业的更广泛趋势，正如 Thomson Reuters 等研究强调的那样，这些研究讨论了 AI 在重塑法律服务方面的变革性作用，以及对稳健评估策略的迫需求。

*   **电子商务平台**：Amazon 将 LLM 集成到其客服聊天机器人中。更新后，模型开始提供不一致的响应，影响了客户满意度。该公司实施了严格的测试方案并建立了版本控制，以确保更新不会损害聊天机器人的质量。根据 Gartner 的报告，利用 LLM 的客服机器人可以显著提高效率，但需要持续评估以维持质量和可靠性。

*   **医疗机构**：医疗提供机构梅奥医学中心（Mayo Clinic）利用 LLM 协助生成患者报告并建议治疗方案。该机构持续评估模型理解医学术语的准确性及其在辅助临床决策方面的有效性。他们在模拟环境中进行定期评估，以确保 LLM 符合医疗标准和实践。发表在《JAMA Network Open》上的一项研究讨论了 AI 在医疗领域的实施，并强调了临床应用中彻底评估的重要性。

*   **微软的 HoloAssist**：微软开发了 HoloAssist 来评估 LLM 在各种背景下协助用户完成任务和纠正错误的程度。该平台提供了 LLM 在现实场景中性能的见解，增强了模型在不同应用中的用途。微软研究的一篇论文讨论了在现实场景中评估 AI 的挑战，以及 HoloAssist 如何通过提供性能评估数据集来应对这些挑战。

为了说明企业如何在实际场景中评估 LLM 输出，请考虑以下代码示例，它展示了如何使用 BLEU 分数进行文本生成和评估。

*   **导入语句**：导入了必要的库。使用 `Transformers` 库加载 `DeepSeek Coder` 模型，而 `nltk.translate` 提供了计算 BLEU 分数以评估生成代码的功能。

*   **模型和分词器初始化**：使用预训练的 `DeepSeek Coder` 模型初始化 `AutoModelForCausalLM` 和 `AutoTokenizer`，设置专门为代码生成的模型架构和分词器。

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
from nltk.translate.bleu_score import sentence_bleu
#加载预训练的 DeepSeek 模型和分词器
model_name = "deepseek-ai/deepseek-coder-6.7b-base"  # 你可以需要选择其他变体
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(model_name)
```

*   **文本生成函数**：`generate_text` 函数接收提示输入，对其进行编码，生成响应并将其解码回可读文本。

```python
# 根据给定提示生成文本函数
def generate_text(prompt, max_length=50):
    # 对输入提示进行编码以为模型准备数据
    input_ids = tokenizer.encode(prompt, return_tensors='pt')
    # 根据编码的提示从模型生成文本，指定最大长度
    output = model.generate(input_ids, max_length=max_length, 
        num_return_sequences=1)
    # 将生成的输出标记解码为可读文本
    generated_text = tokenizer.decode(output[0], 
        skip_special_tokens=True)
    return generated_text
```

*   **文本生成示例**：提供了一个示例提示来生成并打印文本输出，展示了模型根据提示生成内容的能力。

### BLEU 评分计算

为了评估生成的文本，首先定义一个参考文本，并将生成的文本分词为单词。然后通过计算 `BLEU` 评分来衡量生成文本与参考文本之间的相似性，这提供了一种量化的评估方法。

```
# 定义一个用于文本生成的样本 prompt
prompt = "The future of AI in healthcare"
# 使用 prompt 生成文本并打印输出
generated_text = generate_text(prompt)
print("Generated Text:", generated_text)
# 定义一个参考文本，用于评估模型的输出
reference = ["The future of AI in healthcare includes advancements in diagnostics and treatment."]
# 将生成的文本分词为单词以计算 BLEU 评分
candidate = generated_text.split()
# 计算 BLEU 评分，衡量生成文本与参考文本之间的相似性
bleu_score = sentence_bleu([reference], candidate)
```

由于 `BLEU` 是基于 n-gram（长度为 `n` 的短语）计算相似性的，它会在生成文本和参考文本之间寻找匹配的单词或短语。生成的文本缺乏参考文本中关键短语，如“advancements”（进展）、“diagnostics”（诊断）或“treatment”（治疗）。由于生成输出与参考文本之间没有共享的 n-gram，`BLEU` 评分最终结果为 `0`。

```
Generated Text: The future of AI in healthcare is uncertain. The future of AI in healthcare is uncertain.
The future of AI in healthcare is uncertain. The future of AI in healthcare is uncertain.
The future of AI in healthcare is uncertain. The
BLEU Score: 0
```

现在我们已经了解了评估大语言模型（LLM）应用时面临的企业挑战，下一部分将提供如何更有效地进行 LLM 评估的建议。这包括构建新的基准和工作流，不仅能解决当前的挑战，还能预测 LLM 技术的未来发展。通过专注于负责任的 AI 实践并强调交互性和多智能体评估，组织可以创建可持续的评估策略，驱动持续改进和快速创新。

# LLM 评估方法的建议

随着企业将 LLM 集成到复杂的系统中，有效的评估策略对于确保模型在真实环境中具有一致、符合伦理且准确的性能至关重要。传统的基准往往无法涵盖企业应用面临的细微挑战，特别是在将模型行为与特定用例、伦理指南以及不断变化的用户预期对齐方面。以下是开发应对这些挑战评估策略的关键建议。

## 构建新的基准和工作流

为了有效评估 LLM，企业必须开发反映其独特运营需求的自定义基准和工作流。与通用的基准（其可能侧重于广泛的语言或理解任务）不同，企业特定的基准可以评估模型在其预期执行的精确任务中的熟练程度。这可能包括创建来自私有数据源的自定义，以便在相关的场景中对模型进行评估。

例如：

*   **法律文档处理**：律所可能会创建一个专门定制的基准数据集，用于评估 LLM 在法律任务上的表现。例如，Clifford Chance 探索了 AI 驱动的解决方案，用于审查复杂的法律条款和监管合规性。同样，Latham & Watkins 实施了系统来总结案件简报并对大量诉讼文件进行分类。这些基准通常包含合同、判例摘要或法律文本的精选数据集，能够对 LLM 识别关键信息、总结密集的法律材料以及确保符合司法管辖要求的能力进行重点评估。

*   **客户支持机器人**：对于电信公司，基准可以包含为了隐私保护而匿名化的真实对话，用以评估 LLM 在处理复杂查询、正确升级问题和保持对话清晰度方面的表现。

### 自定义基准工作流的示例代码

下面是使用 Python 结合 Hugging Face 的 Transformers 库在私有数据集上创建和运行自定义评估的基本设置。这可以根据不同的企业领域进行调整。

此示例使用 Hugging Face 的 Transformers 库在代表特定企业任务的自定义数据集上评估 LLM。

*   **模型加载**：我们加载了一个针对情感分类进行微调的预训练 `DistilBERT` 模型作为示例。你可以将其替换为适用于你企业需求的模型（例如，针对法律或医疗分类微调的模型）。

```
# 从 Hugging Face Transformers 和 pandas 导入必要的库
from transformers import (
    pipeline, AutoModelForSequenceClassification, AutoTokenizer
)
import pandas as pd
# 加载一个用于序列分类的预训练模型（在情感分析上微调的 DistilBERT）
model_name = "distilbert-base-uncased-finetuned-sst-2-english"
model = AutoModelForSequenceClassification.from_pretrained(
    model_name
)
tokenizer = AutoTokenizer.from_pretrained(model_name)
```

*   **数据集创建**：我们创建一个包含文本和预期输出的样本数据集。在实际应用中，该数据集将来自私有数据，并根据公司的特定任务定制。




# 模型评估与微调综合指南

## 使用自定义数据集进行评估

以下代码演示了如何通过 `pandas` 创建一个包含文本样本及其预期情感标签的数据集。

```
data = pd.DataFrame({
    "text": [
        "Customer complaint about billing",       # 可能被分类为 NEGATIVE
        "Legal clause on data privacy",           # 可能被分类为 NEUTRAL 或 NEGATIVE
        "Technical issue with software",          # 可能被分类为 NEGATIVE
        "User feedback praising the interface",   # 可能被分类为 POSITIVE
        "Inquiry about account balance",          # 可能被分类为 NEUTRAL
        "Successful resolution of ticket"         # 可能被分类为 POSITIVE
    ],
    "expected_output": [
        "NEGATIVE",  # 预期匹配典型的模型情感标签
        "NEGATIVE",  # 调整以与模型的 NEGATIVE 标签对齐
        "NEGATIVE",  # 技术问题通常与负面体验相关
        "POSITIVE",  # 赞赏反馈与 POSITIVE 情感对齐
        "NEUTRAL",   # 一般查询可能是中性的
        "POSITIVE"   # 成功案例应与 POSITIVE 情感一致
    ]
})
```

- **流水线设置：** 建立了一个文本分类流水线，为每个文本输入生成预测。

- **评估函数：** 对于每个输入文本，模型预测一个标签，并将预测标签与预期输出进行比较。该函数随后计算准确率并以百分比显示。

```python
# 定义一个函数在自定义数据集上评估模型。
def custom_evaluation(data, model, tokenizer):
    # 创建用于推理的文本分类流水线
    nlp_pipeline = pipeline("text-classification", model=model, 
        tokenizer=tokenizer)
    correct = 0

    # 遍历数据集中的每一行并获取预测
    for _, row in data.iterrows():
        output = nlp_pipeline(row["text"])[0]["label"]
        print(f"Text: {row['text']} | Predicted: {output} | Expected: {row['expected_output']}")

        # 通过匹配模型输出与预期输出来统计正确预测的数量
        if output == row["expected_output"]:
            correct += 1
    # 计算百分比形式的准确率
    accuracy = correct / len(data)
    print(f"Custom Evaluation Accuracy: {accuracy * 100:.2f}%")
# 自定义评估
custom_evaluation(data, model, tokenizer)
```

**输出解释：**

对于数据集中的每一行，输出显示了输入文本、预测标签和预期标签，允许你检查模型是否准确。

以下是示例输出：

```
Text: Customer complaint about billing | Predicted: Complaint | Expected: Complaint
Text: Legal clause on data privacy | Predicted: Legal | Expected: Legal
Text: Technical issue with software | Predicted: Technical | Expected: Technical
Custom Evaluation Accuracy: 100.00%
```

在这种方法中，稍后根据评估结果触发模型微调，从而实现迭代改进的反馈循环。

如果所有预测都符合预期，此输出将显示 100% 的准确率；如果模型误分类任何示例，准确率则较低。

## 关注交互性和多智能体评估

多智能体评估测试了大语言模型（LLM）与其他代理或系统有效交互的能力。这种方法通过模拟模型与其他系统协作的真实工作流，超越了静态评估。以下是一些示例：

- **客户服务**：与 CRM、知识库和工单系统交互的 LLM 应该根据其跨系统传递相关信息和解决查询的能力进行评估。

- **零售**：电子商务应用需要 LLM 回答查询、推荐产品并与库存系统接口以确认库存情况。评估应测试模型在处理实时库存变化和管理动态客户交互方面的性能。

## 在评估中以负责任 AI 为中心

负责任 AI 评估侧重于确保公平性、最小化偏见并防止有害输出。对抗性测试、公平性评估和毒性筛选等技术允许企业主动应对社会影响。

使用`IBM AI Fairness 360`或`Google's What-If Tool`等工具检查 LLM 如何处理不同的人口群体。例如，可以对招聘模型进行公平性测试，确保对职位的建议在不同性别和种族群体之间保持一致。

一个真实世界的案例是 Google 的负责任 AI（[`newsnetwork.mayoclinic.org/category/research/ai-and-digital-health/`](https://newsnetwork.mayoclinic.org/category/research/ai-and-digital-health/)），它将公平性评估作为负责任 AI 实践的一部分来评估语言模型，确保输出在不同语言和文化中都没有有害刻板印象。

## 桥接评估与改进之间的差距

评估见解应直接指导模型的改进。分析模型错误、注意力模式和偏见，为完善模型的准确性、相关性和安全性提供可操作的数据。

例如，一个在事实准确性方面存在持续问题的 LLM 可能需要针对特定领域经过验证的数据集进行针对性微调，以提高其上下文理解和事实召回。这一过程通常涉及优化模型权重，这是微调的精髓，而不是调整注意力头等低级机制。后者通常在预训练或高级技术如低秩自适应（`LoRA`）中更为相关，用于高效地适配大模型而无需重新训练。

通过专注于微调验证的数据集并利用参数高效训练方法，开发者可以迭代增强特定用例的模型可靠性，同时避免可能使过程过于复杂的必要的低级干预。

### 桥接评估与改进差距的示例代码

此代码演示了如何使用 Hugging Face 的`Trainer` API 使用企业特定数据对模型进行微调，根据评估见解对其进行改进。

首先，我们需要来自 Hugging Face 的基础库，它们为我们的模型训练和评估提供了结构。

```python
# 从 Hugging Face 导入库
from transformers import (
    Trainer, TrainingArguments, AutoModelSequenceClassification
)
from datasets import load_dataset

# 加载 SST-2 数据集
dataset = load_dataset("glue", "sst2")
train_data = dataset["train"]
eval_data = dataset["validation"]

# 加载预训练模型
model = AutoModelSequenceClassification.from_pretrained(
    "distilbert-base-uncased"
)

# 定义训练配置
training_args = TrainingArguments(
    output_dir="./results",        # 保存模型输出的目录
    evaluation_strategy="epoch",   # 每个 epoch 后评估模型
    save_strategy="epoch",         # 每个 epoch 后保存模型检查点
    logging_dir='./logs',          # 保存日志的目录
    logging_steps=10,              # 每 10 步记录一次指标
    num_train_epochs=1,            # 为了快速示例减少为 1 epoch
    learning_rate=2e-5,            # 优化器的学习率
)

# 初始化 Trainer API
trainer = Trainer(
    model=model,
    args=training_args,
    train_dataset=train_data,
    eval_dataset=eval_data
)

# 训练模型
trainer.train()
# 评估模型
eval_results = trainer.evaluate()
print("Evaluation Results:", eval_results)
```

- **评估结果**：训练完成后，将根据验证集计算评估指标。由于 SST-2 是一个二分类数据集，输出通常包括：

  - `eval_loss`：验证集上的平均损失

  - `eval_accuracy`：模型在情感分类上的准确率

```
{
    "eval_loss": 0.420,
    "eval_accuracy": 0.84,
    "eval_runtime": 2.31,
    "eval_samples_per_second": 125.6
}
```

在这种方法中，可以根据评估结果触发模型微调，这将为迭代改进提供反馈循环。

## 遵循 LLM 应用最佳实践的企业真实案例

以下是一些在企业级应用中体现 LLM 评估最佳实践的公司示例：

- **Microsoft Azure AI Foundry:** 微软整合了用于负责任 AI 的评估工具和自定义模型评估功能，允许客户调整 LLM 响应以符合企业标准。通过使用负责任 AI 原则，`Azure AI Foundry` 确保了企业应用保持准确且符合伦理。

- **Amazon Alexa AI:** 亚马逊通过多智能体测试评估 Alexa 的 LLM，评估其在购物、智能家居控制和客户服务等集成应用中的性能。这确保了`Alexa AI`保持可靠、安全并对动态用户需求做出响应。

- **摩根大通（JPMorgan Chase）在金融领域的应用:** 摩根大通采用自定义评估工作流，确保模型输出符合金融法规，增强了模型在欺诈检测和交易监控等领域的准确性和合规性。

通过遵循这些结构化的评估方法，并结合公平性检查和多智能体测试等工具，企业可以确保其 LLM 既是高性能，又符合组织价值观和用户预期。

此外，这些建议和真实案例强调了量身评估方法以满足企业应用特定需求的重要性，并强调了交互式、负责任和迭代的方法。当组织力争最大化 LLM 潜力时，不断演进评估框架对于确保模型有效且负责任地服务用户至关重要。

严谨评估的关键结果是能够通过提示词工程（prompt engineering）完善与 LLM 的交互。从评估模型性能中获得的洞察（例如理解其优势、局限性和行为）可以直接应用于优化提示词。通过利用这些评估发现，企业可以设计出更有效的提示词，引导模型产生精确、与上下文相关且可靠的输出。

# 桥接评估与指标：一种数据驱动的方法

评估 LLM 需要严谨且多方面的方法，因为这些模型的复杂性要求超出简单准确率评分的指标。有效的评估指标对于理解 LLM 在各种任务中（从生成连贯文本到准确回答问题）的表现至关重要。本节将探讨基本的评估技术，解释不同的指标类型，并介绍每种指标的真实世界应用。

## 理解 LLM 评估指标

在实施高级指标之前，理解 LLM 评估的基础原理至关重要。评估指标的目标不仅是量化地衡量性能，还为了衡量模型输出的定性方面，如流畅性、相关性和适应性。

此外，评估框架涵盖了安全相关指标、传统的基于数学的 NLP 指标以及其他工程考虑（如成本、延迟效率）。这种全面的方法确保了从多个维度对模型进行评估，以确保在真实世界应用中的整体有效性和安全性。

评估 LLM 性能时，考虑到所有类型的指标非常包括：

1. **质量指标：** 评估模型响应的流畅性、相关性和适应性。

2. **安全性指标：** 衡量模型遵循安全指南的程度，最大限度地减少有害或偏见的输出。

3. **基于数学的指标：** 包括传统的指标如`perplexity`（困惑度）、`BLEU`和`ROUGE`：

   1. **Perplexity (困惑度):** 衡量 LLM 预测词序列的能力，反映了其整体流畅性和连贯性。

   2. **BLEU (双语句子评估辅助):** 常用于机器翻译，`BLEU`分数可以衡量生成文本与一组参考文本之间的相似性。

   3. **ROUGE (基于召回率的文本摘要评估研究):** 主要用于摘要任务，衡量模型输出与参考摘要之间词汇或短语的重叠程度。

### 有效评估的关键指标和技术

特定任务指标专门用于衡量特定类型 LLM 任务的性能，例如文本摘要、机器翻译或对话生成。特定任务指标提供了一个集中视角，展示模型处理单个任务的效果如何。

- **摘要与 ROUGE 分数：** 在摘要任务中，`ROUGE`分数评估模型摘要与参考摘要之间的相似性。例如，OpenAI 的`GPT`模型已通过`ROUGE`测试，以评估其在`CNN/Daily Mail`和`XSum`等数据集上的摘要性能，其中`ROUGE-L`经常被用于衡量语言重叠。

那么，让我们来看一个指标评估示例。在开始之前，您需要安装以下包：

```
! pip install evaluate
```

- **BLEU 指标：** `evaluate.load("bleu")`函数从 Hugging Face 的评估库加载`BLEU`指标。

- **ROUGE 指标：** `evaluate.load("rouge")`函数加载`ROUGE`指标。

```python
# 必要的模块
import evaluate
# 加载指标
bleu = evaluate.load("bleu")
rouge = evaluate.load("rouge")
```

现在，通过对每个指标调用`.compute()`，我们根据提供的预测值和参考值计算分数。

```python
# 预测和参考示例
predictions = ["The future of AI in healthcare looks promising, enhancing diagnostics and treatment."]
references = [["AI in healthcare is advancing with potential in diagnostics and treatments."]]
# 计算 BLEU 分数
bleu_score = bleu.compute(predictions=predictions, references=references)
print("BLEU Score:", bleu_score)
# 计算 ROUGE 分数
rouge_score = rouge.compute(predictions=predictions, references=references)
print("ROUGE Score:", rouge_score)
```

输出将显示两个指标：`BLEU`分数和`ROUGE`分数，为文本生成的质量提供洞察。

`BLEU`分数的输出可能如下：

```
BLEU Score: {'bleu': 0.0, 'precisions': [0.42857142857142855, 0.23076923076923078, 0.08333333333333333, 0.0], 'brevity_penalty': 1.0, 'length_ratio': 1.1666666666666667, 'translation_length': 14, 'reference_length': 12}
```

这表明生成的文本与参考文本之间没有显著重叠，如`BLEU`分数为 0 所示。精确率值代表 n-gram 的重叠情况，其中第一个值（0.428）显示了匹配的单词比例。

`ROUGE`分数的输出可能如下：

```
ROUGE Scores: {'rouge1': 0.43478260869565216, 'rouge2': 0.28571428571428564, 'rougeL': 0.43478260869565216, 'rougeLsum': 0.43478260869565216}
```

这表明生成的文本与参考文本之间存在中等重叠，`ROUGE-1`得分为 0.435，说明单词匹配尚可，而`ROUGE-2`为 0.286 则反映了匹配的双词组较少。`ROUGE-L`分数指示了最长公共序列的相似性。

在本示例中，`BLEU`分为生成的文本与参考文本之间的对齐程度提供了定量衡量，而`ROUGE`分数则帮助量化生成的摘要与原始内容的对齐程度，为摘要的有效性提供了清晰的衡量。然而，重要的是，`BLEU`主要关注生成文本与参考文本之间精确的 n-gram 匹配。例如，即使句子在语义上相似，如果精确的词序列不匹配，`BLEU`分数也可能返回低值，正如当两个句子之间只有“AI in healthcare”和“diagnostics and”是共同词汇，但其余短语不同的情况所示。

这种区别强调了，虽然`BLEU`通过比较词序列（n-grams）来衡量精确率（precision），但`ROUGE`评估的是重叠术或短语的召回率（recall），使其更侧于捕捉有意义的内容重叠。这两项指标都为模型性能提供了有价值但不同的见解。




# 引言

在摘要评估之外，相关性和连贯性评估变得至关重要，特别是对于对话式 AI 或聊天机器人。这些系统在交互中必须保持上下文和连贯性，因此评估其响应的相关性和流利度至关重要。

评估技术通常采用语义相似性衡量，例如`cosine similarity`，或更高级的指标，如`BERTScore`，来量化生成的响应与预期上下文及对话流的对齐程度。相关性和连贯性作为 AI 辅助质量指标，有助于确保模型的输出不仅准确，而且在上下文中恰当且逻辑一致的。

## 示例 – 对话系统与 BERTScore

`BERTScore`利用来自 transformer 模型（如`BERT`）的嵌入，评估生成响应中的词与人类响应相比的相似性。这种方法允许开发者在不依赖精确短语匹配的情况下评估对话的连贯性，这对于开放式对话至关重要。例如，可以使用`BERTScore`评估`ChatGPT`以检查客户服务交互中的响应是否连贯，确保响应在上下文恰当并符合用户的提问。

```
# Import necessary libraries
from bert_score import BERTScorer
# Sample generated and reference responses
generated_responses = [
    "I am looking for information on my account.",
    "Can you help me reset my password?"
]
reference_responses = [
    "I need help with my account.",
    "I want to reset my password."
]
# Initialize BERTScorer
scorer = BERTScorer(lang="en", rescale_with_baseline=True)
# Calculate BERTScore
P, R, F1 = scorer.score(generated_responses, reference_responses)
# Print the results
print("Precision:", P.mean().item())
print("Recall:", R.mean().item())
print("F1 Score:", F1.mean().item())
```

### 预期输出说明

当你运行上述代码时，可以预期输出显示生成响应与参考响应相比的精确率、召回率和 F1 分。

```
Precision: 0.4680666923522949
Recall: 0.587756872177124
F1 Score: 0.5280133485794067
```

- **精确率（Precision）** 表示在参考文本的上下文中，生成的词中有多少是相关的。
- **召回率（Recall)** 衡量了参考响应中的相关词中有多少被生成响应所捕获。
- **F1 分数** 代表精确率和召回率的调和平均数。

因此，真正的企业级应用需要更广泛的评估视角，而不仅仅是质量。评估还应该考虑安全性、模型效率（包括大语言模型`LLM`与小语言模型`SLM`之间的权衡，这会影响成本和延迟）以及与偏差检测等伦理原则的对齐。此外，如第 6 章所示，管理和缓解`LLM`中的幻觉成为确保这些应用既可靠又可信的关键部分。

正如我们所知，`LLM`的幻觉发生在模型依赖训练数据中的模式而非经过证实的事实，从而生成虚假或误导性信息。这些错误范围从轻微的不准确到完全虚构的陈述，通常源于有偏差或多样性不足的数据、过拟合或缺乏事实核查。为了缓解幻觉，策略包括提高数据质量、采用提示工程、添加后处理验证层以及整合用户反馈机制。来自新闻和医疗建议等领域的案例表明，应用这些方法（例如多级验证和专家评审限制）可以显著减少事实性错误。自动事实检查器和模拟环境等工具进一步支持了提高`LLM`可靠性和建立用户信任的持续努力。

# 评估 LLM 性能的指标

有效评估`LLM`对于理解其效能和改进领域至关重要。通过建立稳健的指标并定期评估性能，组织可以确保其`LLM`部署交付最佳结果，并根据新兴需求和技术不断进化。本节详细介绍了评估`LLM`性能的指标，涵盖了关键性能指标（`KPI`）、定量与定性衡量的结合，以及持续改进的策略。

## 关键性能指标 (KPIs)

`KPI`对于系统跟踪`LLM`的性能至关重要。它们提供了可衡量的值，组织可以用它们来评估模型在现实应用中的有效性：

- **准确率（Accuracy）**：衡量模型在任务上下文中正确输出的百分比。对于需要精确输出的任务（如事实报告或技术支持），高准确率至关重要。
- **速度/响应时间（延迟）**：评估模型生成响应的速度。这对于及时响应至关重要的面向用户的应用尤为重要。
- **鲁棒性（Robustness）**：评估模型处理边缘情况或异常输入的能力，而不会失败或产生无意义的输出（或幻觉）。
- **用户满意度**：通常通过调查或反馈机制收集，该指标反映了模型满足用户预期和需求的程度。

## 定量与定性衡量

虽然定量指标因其客观性和易于追踪性而极具价值，但定性衡量为模型性能的细微差别提供了深度洞察，而这些是数字本身可能会忽略的：

### 定量衡量

- **精确率和召回率（Precision and recall）**：对于信息检索等任务非常重要，检索信息的关联性与其全面性同样重要。
- **F1 分**：精确率和召回率的调和平均数，适用于精确率和召回率平衡至关重要的情况。
- **BLEU 分**：常用于机器翻译，以衡量模型输出与一组高质量参考翻译之间的相似性。

### 定性衡量

- **用户反馈分析**：来自用户的直接评论可以提供模型上下文适用性和交互易用性的见解。
- **专家评审**：领域专家可以对需要高水平领域知识的任务对模型输出进行评估，提供自动化指标无法捕捉的洞察。

## 持续改进流程

`LLM`运行的环境不断变化，需要持续的调整和优化以维持其有效性：

- **迭代反馈循环**：建立持续收集和分析用户反馈以及模型性能数据的机制。这些信息应直接反馈到开发周期，以完善和改进模型。
- **A/B 测试**：定期测试不同版本的模型或组件，以确定在特定条件下的最佳表现。
- **更新和重训练协议**：开发定期更新训练数据并重新训练模型的协议，以适应新信息和不断的使用模式。这对于避免模型过时至关重要，防止性能随数据分布或用户预期的变化而下降。可以使用微调和蒸馏等技术来优化重训练过程，确保模型在不需要从头完全重训练的情况下保持相关性和有效。
- **性能基准测试**：定期对照行业标准或竞争模型对模型进行基准测试，以确保其处于技术前沿并符合行业基准。

维护`LLM`的性能和可靠性是一个超出初始部署的持续过程。持续改进机制（如迭代反馈循环、`A/B`测试和定期重训练）确保模型保持有效、自适应并符合不断变化的企业需求。通过与行业标准进行性能基准测试并随着时间的推移完善策略，组织可以主动应对数据漂移、模型过时和新兴用户预期等问题。

当企业完善其评估和优化策略时，理解`LLM`的更广泛影响变得至关重要。这不仅包括效率，还包括与幻觉相关的风险以及模型生成错误或误导性信息的情况。

# 总结

本章探讨了`LLM`的评估指标，包括`BLEU`和`ROUGE`分，这些被认为是评估生成内容质量的重要工具。本章还讨论了幻觉这一关键问题，即`LLM`生成错误信息的情况。缓解这些幻觉的策略包括提高数据质量、实施严格的模型监控以及利用自动事实检查。

在过程中，使用了现实案例研究来说明这些策略的应用，突显了包括新闻生成和医疗建议系统在内的各个领域的有效实践。

在下一章中，我们将探索在现实环境中部署`LLM`的关键阶段。它将涵盖部署的技术方面、对监管要求的合规性，以及通过详细案例研究提供的实用策略，为`LLM`从开发到实时业务运营的过渡提供全面指南。

# 参考文献

- The Pile: 一个包含 800GB 多样文本的语言建模数据集（`https://pile.eleuther.ai/`）
- Microsoft 的 HoloAssist（`https://holoassist.github.io/`）
- Mayo Clinic 医疗（`https://newsnetwork.mayoclinic.org/category/research/ai-and-digital-health/`）
- Microsoft Azure OpenAI Service（`https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-approach-gen-ai`）

## 相关资源

- [迈向可以聊天“任何事情”的对话代理之路（`https://research.google/blog/towards-a-conversational-agent-that-can-chat-aboutanything`）](https://research.google/blog/towards-a-conversational-agent-that-can-chat-aboutanything)
- [摩根大通银行（`https://en.wikipedia.org/wiki/JPMorgan_Chase#:~:text=JPMorgan%20Chase%20is%20the%20fifth,largest%20U.S.%20corporations%20by%20revenue`）](https://en.wikipedia.org/wiki/JPMorgan_Chase#:~:text=JPMorgan%20Chase%20is%20the%20fifth,largest%20U.S.%20corporations%20by%20revenue)
- [OpenAI 的 ChatGPT（`https://openai.com/`）](https://openai.com/)

| 立即解锁本书的专属福利 | 扫描此二维码或访问 [packtpub.com/unlock](https://packtpub.com/unlock)，然后按书名搜索本书。 | ![](img/Unlock-01.png)![白色背景上的二维码，AI 生成的内容可能不准确。](img/Unlock2.png) |
| :--- | :--- | :--- |
| **注意**：在开始之前请准备好您的购买发票。 | | |
