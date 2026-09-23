# 高效提示 Codex CLI

*第 1 章* 和 *第 2 章* 介绍了入门与设置：Codex CLI 是什么、它如何定位、如何安装。本章开始讲述决定本书其余部分是否物超所值的两项技能。第一项是提示词工程，本章介绍：非提示词工程理论，而是适用于能读取代码库、执行命令、一次运行数分钟的智能体编码工具的具体模式。第二项是 `AGENTS.md`，见 *第 4 章*。为 Codex CLI 编写提示词，不像填写搜索框，更像在把办公室钥匙交给承包商前对其进行简报。

### 学习目标

你将能够：

1.  编写包含目标、上下文、约束和完成标准的提示词
2.  设置可跨会话持久传递上下文的 `AGENTS.md` 文件
3.  识别导致浪费运行的提示词反模式，并应用相应修复
4.  使用 Codex CLI 生成和维护项目文档

### 为何 Codex CLI 提示词与众不同

单个提示词可触发一系列工具调用——读取文件、运行测试、编写代码——运行数分钟后才返回供审查的内容。当模糊指令显现为问题时，Codex CLI 可能已触及十几个文件。³⁰

这改变了清晰度的经济学。在对话式 AI 中，模糊可恢复：你可以廉价迭代。在智能体 AI 中，模糊代价高昂。Codex CLI 还可能自信地做出技术上合理但对你的项目错误的选择，因为你从未告诉它此处“正确”是什么样子。

*高效的 Codex CLI 提示词在于在执行开始前消除歧义。*

#### 模糊运行的代价

```
### 提示词 A
改进订单处理代码。
### 提示词 B
重构 src/orders/process.ts，将嵌套回调链替换为 async/await。不要更改公共函数签名。每次文件更改后运行 npm test，仅当所有测试通过时才继续。
```

提示词 A 让 Codex CLI 自行决定“改进”意味着什么、触及哪些文件以及何时停止。提示词 B 明确指定了这三点。差别在于：数分钟的算力浪费和数十个不想要的文件更改。

### 高效提示词的构成

以下四要素可靠地出现在产生良好结果的提示词中：

**目标**：用一个具体句子描述“完成”是什么样子。使用结果导向动词：“添加”、“修复”、“迁移”、“替换”。

**上下文**：语言版本、框架及一个类比代码片段：“遵循 `src/api/users.ts` 中的模式”。

**约束**：Codex CLI 不应做什么。不引入新依赖。不修改数据库模式。不触碰 `src/legacy/` 中的文件。

**完成标准**：一个具体的、可运行的检查：“`GET /api/posts?page=2&limit=20` 返回正确的 JSON，且 `npm test` 以代码 0 退出”。

```
目标：为 /api/posts 端点添加分页。
上下文：Express.js 应用，通过 pg-pool 使用 PostgreSQL，遵循 src/api/users.ts 中现有的模式（基于游标的分页）。
约束：不修改数据库模式。不添加新 npm 包。不更改 /api/users 端点。
完成标准：GET /api/posts?cursor=<id>&limit=20 返回正确的页面和 next_cursor 字段。现有测试仍通过。运行 npm test。
```

提示词中的一个具体示例胜过一段抽象解释。

四要素提示词也隐含了范围。最佳切入点是同事能在一个下午完成、具有明确前后状态的任务。更小的任务浪费结构化提示的开销；更大的任务应拆解为里程碑或交给规划模式（本章后述）。

### 推理力度

Codex CLI 允许你调整模型在行动前思考的深度。`model_reasoning_effort` 设置以延迟和 Token 成本换取推理深度。更高力度并非总是更好：对于单行重命名，`high` 仅消耗 Token；对于跨模块重构，`low` 产出的浅层变更会遗漏架构隐患。

**表 3-1. 推理力度级别**

| **级别** | **配置值** | **最佳适用场景** |
| --- | --- | --- |
| `minimal` | `model_reasoning_effort = "minimal"` | 大规模运行的微小转换：格式化工具、批量重命名、CI 中的文件模板生成 |
| `Low` | `model_reasoning_effort = "low"` | 快速编辑、重命名操作 |




### 推理工作量配置

| 值 | 配置 | 适用场景 |
|---|---|---|
| `medium` | `model_reasoning_effort = "medium"` (默认) | 大多数交互式编码任务 |
| `high` | `model_reasoning_effort = "high"` | 复杂算法、跨模块重构 |
| `xhigh` | `model_reasoning_effort = "xhigh"` | 架构决策、安全审查 |

在 `~/.codex/config.toml` 中设置推理工作量，按配置文件区分，或在会话中通过 `/model` 命令修改：

```
#### ~/.codex/config.toml — 用户级默认配置
model_reasoning_effort = "medium"
#### ~/.codex/refactor.config.toml — 一个提高难度工作工作量的配置文件
model_reasoning_effort = "high"
```

使用 `codex --profile refactor` 激活该配置文件。

### 规划模式

对于模糊或多步骤的任务，请先调用规划模式。按 `Shift`+`Tab` 切换模式，或输入 `/plan`。

```
/plan Propose a migration strategy for moving our auth from sessions to JWT
```

规划模式将 Codex CLI 置于上下文收集状态：它会读取代码库，识别歧义，并在更改任何文件之前生成结构化计划。

| 场景 | 使用方式 |
|---|---|
| 单文件、明确定义的变更 | 直接执行 |
| 跨模块变更 (3+ 模块) | 规划模式 |
| 需求模糊 | 规划模式 |
| 紧凑的调试循环 | 直接执行 |

规划模式的“无写入”约束在提示词层面强制执行，而非通过运行时沙箱。对于高风险工作，请使用临时分支。

### 用于多小时任务的 PLANS.md

对于跨越数小时或多个会话的任务，创建一个持久化的 `PLANS.md` 并在 `AGENTS.md` 中引用：

```
#### ExecPlan: <功能名称>
##### 目的 / 全局视角
##### 进度
- [ ] 步骤 1 --- 描述 (开始于: 2026-03-27T09:14Z)
- [x] 步骤 2 --- 描述 (完成于: 2026-03-27T09:32Z)
##### 决策日志
| 日期 | 决策 | 理由 |
##### 具体步骤
##### 验收与验证
```

关键属性是 **自包含性**：ExecPlan 必须使智能体能够在无外部上下文的情况下端到端地实现该功能。

这种规范可以形式化为 **PLANS.md 技术**：在编写任何代码之前，在 `PLANS.md` 文件中编写基于里程碑的计划。详见 *第 4 章*。

### 检查点模式

使用中间验证步骤构建长提示词：

```
将遗留认证模块迁移到新的认证服务。
每步完成后：(1) 所有测试通过，(2) 无 TypeScript 错误，(3) 变更已提交。

步骤：
1. 将令牌验证逻辑提取到 src/auth/validate.ts
2. 更新中间件以使用新模块
3. 删除 src/auth/legacy.js
4. 更新代码库中所有的导入
```

### 迭代提示与会话中纠偏

如果 Codex CLI 走偏了，请立即中断。按 `Esc` 停止当前步骤（REPL 底部的状态指示器显示 `esc to interrupt` 表示步骤正在运行）。然后输入纠正指令。会话中纠偏遵循与初始提示词相同的结构：“停止。撤销对 `src/db/schema.ts` 的更改，改为在 `src/db/migrations/` 中处理迁移。”

当累积历史挤占近期决策时使用 `/compact`。使用 `/fork` 从同一起点探索两种方法。当会话因提示词有两种有效解释而产生错误结果时，停止并重写提示词，而非在纠偏上迭代。*第 7 章* 深入介绍了 `/compact` 和 `/fork`，包括上下文窗口如何填充、压缩保留与丢弃的内容，以及何时在冒险操作前进行分叉。

### 常见任务的提示词模式

#### Bug 修复

包含观察到的行为、预期行为和复现步骤：

```
Bug: 结账页面因卡片过期抛出未处理的 Promise 拒绝。
观察到: 服务器日志中出现 "CardError: card_expired"。用户看到 500 页面。
预期: 用户看到 "Your card has expired. Please update your payment method."
复现: 运行 __tests__/checkout/payment.test.ts "handles expired card" 用例。
修复位置: src/checkout/payment.ts。包含回归测试。
不要修改 src/payments/stripe.ts。
```

#### 重构

明确要求保留行为：

```
重构 src/notifications/sender.ts：将 .then()/.catch() 替换为 async/await。
公共 API 必须不变。每个函数修改后运行 npm test。不要修改其他文件。
```

#### 可并行化工作

明确告知 Codex CLI 任务相互独立：

```
并行为这三个模块编写单元测试：
- src/auth/validate.ts
- src/payments/process.ts
- src/notifications/sender.ts
使用 __tests__/helpers.ts 中的辅助函数和 __tests__/auth/token.test.ts 中的模式。
```

### 文档生成

Codex CLI 不仅是代码编写工具，也是高效的文档生成器。其读取整个代码库并推理代码结构的能力，使其适合生成和维护项目文档。

#### 代码文档

Codex CLI 可以为整个代码库生成符合语言规范的文档注释：

```
为 src/api/ 中所有导出的函数添加 JSDoc 注释。
遵循 src/api/users.ts 中的现有风格。
包含 @param、@returns 和 @throws 标签。
不要修改任何实现代码。
```

对于 Python 项目，相同模式适用于 docstrings：

```
为 src/services/ 中所有公共函数添加 Google 风格的 docstrings。
包含 Args、Returns 和 Raises 部分。
遵循 src/services/auth.py 中的模式。
```

关键在于指定文档风格（JSDoc、Google 风格、NumPy 风格、rustdoc）并指向现有示例。没有风格参考时，Codex CLI 生成的文档虽正确但不一致。

#### 架构决策记录 (ADR)

**架构决策记录** (**ADR**) 记录重大技术决策背后的推理。Codex CLI 可以根据代码上下文和简短提示起草 ADR：

```
为从 Express 迁移到 Fastify 的决策编写 ADR。
阅读 src/api/ 中的现有 Express 路由和 src/api-v2/ 中的新 Fastify 路由。包含背景、决策、后果及考虑的替代方案。保存至 docs/adr/0012-fastify-migration.md。
遵循 docs/adr/0001-template.md 中的格式。
```

#### README 与引导文档

对于新项目或文档滞后的项目，Codex CLI 可以通过读取仓库结构、依赖和配置来生成引导文档：

```
为该项目生成 README.md。阅读 package.json、src/ 目录结构和现有的 AGENTS.md。包含：
- 项目概述（一段话）
- 先决条件和安装步骤
- 开发工作流（构建、测试、Lint）
- 项目结构概览
- 贡献指南
不包含徽章或盾牌。
```

#### 将文档时效性作为 CI 任务

文档腐烂——即文档描述的代码已不复存在——是常见问题。Codex CLI 可检测此问题：

```
codex exec --sandbox read-only \
 "对比 docs/api.md 中记录的公共 API 与 src/index.ts 中的实际导出。列出文档中记录但已不存在的函数，以及存在但未记录的函数。" \
 -o /tmp/doc-audit.md
```

将其作为定期 CI 检查运行，在文档漂移累积前发现它。

### 将持久化上下文移出提示词

#### AGENTS.md

`AGENTS.md` 在每个会话开始时自动读取。将约定、约束、构建命令和禁修改文件放在此处：

```
### 项目: 支付服务
#### 构建与测试
npm run build && npm test
##### 约定
- 全程使用 async/await；禁止 .then() 链
- 错误消息使用句首大写，无尾随句号
- 所有货币金额以整数（分）存储，而非浮点数
#### 禁止修改
- src/legacy/chargebee.ts (已弃用；只读)
- 任何匹配 *.generated.ts 的文件
```



文件构成：`~/.codex/AGENTS.md` 设置个人默认值，仓库根目录设置项目默认值，子目录会覆盖两者。³² `/init` 斜杠命令会在会话中引导生成一个初始文件：这是一种轻松上手的方式，但很少能产生最佳结果。自动生成的文件往往过度描述显而易见的内容，而对真正改变智能体行为的项目特定决策描述不足。请将输出视为待修剪的草稿，而非待提交的文件。第 4 章 全面介绍了 `AGENTS.md` 的模式与陷阱。

#### 通过配置文件使用自定义指令

`AGENTS.md` 适用于项目中的每个会话。配置文件仅在你请求时才会应用。当你在不同模式下工作时，这种区别很重要：同一个仓库早上可能是修复 Bug 的目标，下午可能变成架构评审的对象，发布前又可能成为安全审计的靶子。每种模式都需要不同的推理姿态、不同的输出风格和不同的常设指令。如果没有配置文件，你就必须在每次提示词中重新建立这种姿态。

在各自的每配置文件文件 `~/.codex/<name>.config.toml` 中定义一个命名配置文件，其中 `model_instructions_file` 指向你的自定义指令：

```toml
#### ~/.codex/bugfix.config.toml
model_instructions_file = "~/.codex/prompts/bugfix.md"
model_reasoning_effort = "high"
```

使用 `codex --profile bugfix` 激活。指令文件可能包含：

```text
你是一名一丝不苟的调试员。用一句话阐述根本原因。
包含回归测试。不做投机性更改。
```

### 输出控制：塑造智能体的沟通方式

提示工程不仅关乎你对模型说什么；还包括配置模型说什么回复。`config.toml` 中的多个键控制着冗长度、推理可见性和 Token 管理。这些邻近提示词的控制项在不改变任务指令本身的情况下，塑造了智能体的沟通风格。

#### 冗长度

`model_verbosity` 键接受三个值：

```toml
model_verbosity = "low" # 以代码为主，最少文本
model_verbosity = "medium" # 平衡（默认）
model_verbosity = "high" # 详细演示、架构评论
```

在仅关注代码输出的自动化流水线中使用 `low`。在你希望模型用文本解释其推理过程的架构探索会话中使用 `high`。

冗长度也是成本的直接杠杆。输出 Token 的计费费率高于输入 Token，而文本解释的体量可能远超其伴随的代码。在每天运行数百次的 CI 任务上设置 `low` 冗长度，可将模型的评论压缩到产生结果所需的最小程度，使成本主要由输入提示词决定，而非运行时的叙述。在长会话中还有次级效应：每一轮助手对话都会成为对话历史的一部分，并在下一轮重放，因此冗长的早期轮次会复合成每次后续调用中更大的前缀。在文本是交付物的地方保持高冗长度；在不是交付物的地方降低它。

#### 推理摘要

`model_reasoning_summary` 键控制模型的内部推理链是否在输出中被总结。结合 `hide_agent_reasoning`（完全抑制推理）和 `show_raw_agent_reasoning`（显示未经编辑的推理链），这三个键让你能将推理可见性从完全隐藏、总结到原始三个层级进行调节。

#### Token 预算感知

两个键影响智能体管理自身上下文的激进程度：`tool_output_token_limit` 限制单个工具结果消耗的 Token 数，`model_auto_compact_token_limit` 设置触发自动压缩的阈值。设置较低的 `tool_output_token_limit` 可防止单次大文件读取占用不成比例的上下文窗口份额，这在提示跨多文件工作时尤为相关。

#### 常见工作流的配置文件

```toml
##### ~/.codex/ci.config.toml — 简练的自动化
model_verbosity = "low"
hide_agent_reasoning = true
model_reasoning_effort = "low"
tool_output_token_limit = 8000
```

```toml
##### ~/.codex/explore.config.toml — 详细的探索
model_verbosity = "high"
show_raw_agent_reasoning = true
model_reasoning_effort = "high"
```

```toml
##### ~/.codex/budget.config.toml — 成本敏感的长会话
model_verbosity = "medium"
model_reasoning_effort = "medium"
model_auto_compact_token_limit = 80000
tool_output_token_limit = 12000
```

使用 `codex --profile ci` 激活。这些配置文件可与模型选择配置文件组合，因此你可以将 `ci` 输出配置文件与快速模型结合，以实现最大的成本效益。

### 元提示：利用智能体改进自身指令

元提示使用一次 LLM 调用来改进后续调用所使用的提示词。³³ 在 Codex CLI 中，这意味着让智能体分析自身的失败，并针对三个跨会话持久化的指令界面提出具体的编辑建议：`AGENTS.md`、技能（`SKILL.md`）和 `config.toml`。³⁴

这不是一个自我修改的智能体循环。人类仍处于审批路径中。Codex CLI 提出建议；你审查并提交。这种区别很重要：研究表明，未经人工筛选的自我生成指令改进无法产生可衡量的收益，而人工筛选的改进能显著提高通过率。³⁵

#### 模式 1：会后复盘

最简单的元提示模式运行在一个出现偏差的会话结束时：

```text
你刚完成了一个任务，但犯了两个错误：
1. 你运行了 `npm test` 而不是 `pnpm test`
2. 你在一个纯 ESM 项目中创建了一个 CommonJS 模块
请审查当前的 AGENTS.md 并提出一个最小差异补丁，
以防止这两个错误在未来的会话中再次发生。
以围栏式 apply_patch 块的形式输出差异。
```

#### 模式 2：通过 codex exec 进行批量分析

对于在许多仓库中运行 Codex CLI 的团队：

```text
codex exec "读取 AGENTS.md 和 ~/.codex/sessions/ 中最近 5 次会话的记录。\
找出我覆盖你初始方法的三个最常见修正模式。针对每个模式，\
提出一个具体的 AGENTS.md 补充内容来消除该修正。\
输出为单个 Markdown 文件。" \
 --sandbox read-only \
 -o /tmp/metaprompt-report.md
```

#### 模式 3：技能细化

技能使用渐进式披露：Codex CLI 最初只能看到名称和描述，仅在激活时才加载完整指令：

```text
我在 ~/.agents/skills/api-scaffold/SKILL.md 处有一个技能。
当我要求创建新的 REST 端点时它应该激活，
但当我在调试现有端点时不应激活。
请审查当前的名称、描述和触发词。
提出更新后的前置元数据以提高激活精确度。
包含三个匹配示例和三个不匹配示例，
供我作为手动测试用例使用。
```

#### 应避免的反模式

元提示可能会生成冗长、过度具体的规则，导致 `AGENTS.md` 膨胀超过 32 KiB 限制。每条规则都应通过一个简单测试：这是防止*反复出现*的失败，还是一次性失误？一次性修正应放在提示词中，而非配置文件中。

### 官方工作流配方：八大规范模式

OpenAI 的开发者文档包含一个专门的工作流页面，将跨软件开发生命周期使用 Codex CLI 的八大规范模式编纂成册。³⁶ 每个配方共享一个通用骨架，该骨架将本章开头的四要素提示词结构形式化：**目标**、**上下文**、**约束** 和 **完成条件**。³⁷

**表 3-2. 八大官方工作流配方**

| # | 配方 | 沙箱模式 | 关键技术 |
| --- | --- | --- | --- |
| 1 | 解释代码库 | `read-only` | `@` 文件模糊搜索 + `/plan` 模式 |
| 2 | 修复 Bug | `workspace-write` | 粘贴错误输出，定义通过标准 |





| 3 | 编写测试 | `workspace-write` | 参考实现 + 现有测试规范 |
| 4 | 从截图生成原型 | `workspace-write` | `-i` 图像输入 + 高推理力度 |
| 5 | 将重构委托给云端 | `workspace-write` | 本地 `/plan` 然后云端提交 |
| 6 | 本地代码审查 | `read-only` | `/review` 斜杠命令 |
| 7 | 审查 GitHub PR | `read-only` | PR 评论中的 `@codex` 或 `codex exec` |
| 8 | 更新文档 | `workspace-write` | 交叉引用代码 + 文档，`PostToolUse` linter 钩子 |

配置文件消除了在不同工作流之间切换时的摩擦：

```
##### ~/.codex/explore.config.toml
sandbox_mode = "read-only"
model_reasoning_effort = "high"

#### ~/.codex/fix.config.toml
sandbox_mode = "workspace-write"
approval_policy = "on-request"

#### ~/.codex/review.config.toml
sandbox_mode = "read-only"
approval_policy = "never"
```

### 总结

1.  **智能体提示要求预先明确。** 模糊的提示会发动一次持续数分钟、可能触及众多文件的运行；而对话式提示可以容忍歧义，这是智能体提示做不到的。

2.  **优秀提示的四要素。** 目标、上下文、约束条件和可运行的完成标准。

3.  **持久化上下文存放在 AGENTS.md 中。** 适用于每个提示的规则应放在文件中，而非在每次调用中重复。

4.  **规划模式与检查点。** 规划模式在代码变更前生成可审查的策略；检查点将长任务构建为可验证的增量步骤。

5.  **重构框架优于持续纠错。** 当会话偏离轨道时，停下来重新构建框架通常比强行纠错更划算。

6.  **元提示需要人工策划。** 让模型改进自身的指令文件，经人工审查后的效果优于自动提交。

7.  **基于配方的提示与配置文件。** OpenAI 的八个官方工作流配方将基于“目标-上下文-约束-完成条件”结构的可复用模式固化下来。命名配置文件无需重新配置即可在配方类别间切换。

8.  **文档生成。** Codex CLI 可以将生成和维护代码注释、ADR、README 及新鲜度审计作为 CI 任务。

### 练习

**练习 3-1：编写一个四要素提示。** 选取一个最近完成的任务，使用目标、上下文、约束条件和完成标准编写一个 Codex CLI 提示。

**练习 3-2：审查并扩展你的 AGENTS.md。** 添加三条未记录在案的团队约定及具体示例。运行一个会话并观察默认行为是否发生变化。

**练习 3-3：在跨领域变更上使用规划模式。** 对涉及五个或更多文件的变更运行 `/plan`。在批准执行前审查计划。

**练习 3-4：标记可并行化的工作。** 编写一个显式标记独立子任务的提示。对比其与顺序执行等效方案的会话时长。

**练习 3-5：生成文档。** 运行 Codex CLI 为未文档化的模块添加文档注释。然后运行一次新鲜度审计，将文档与实际 API 表层进行对比。记录审计发现的任何差异。

