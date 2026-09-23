# 技能生态系统：使用与编写技能

*第 9 章* 展示了钩子如何拦截代理的生命周期：在会话开始时注入上下文、对完成做出反应、阻止违反策略的提示词。钩子表达的是控制逻辑，而非可复用知识。一旦您连接了一个强制执行团队提交信息约定的钩子，如果不手动复制脚本和配置文件，就无法将该工作流共享给不同项目上的同事。这种摩擦指向了另一种基元：一种设计用于不拦截代理做什么，而是教会它如何做新事情的基元。技能就是这种基元。

本章涵盖技能生态系统的两方面：消费者侧（查找、安装和组合社区技能）和生产者侧（编写自己的技能、编写有效的描述、控制调用策略，以及发布它们以便他人也能安装）。

### 学习目标

您将能够：

1.  使用 `$skill-installer` 和 `npx skills` 安装、配置和禁用技能
2.  编写包含正确前置元数据和分层支持材料的完整、有效的 `SKILL.md` 文件
3.  使用参考验证器构建并端到端测试一个真实的技能




### 配置高影响工作流

4. 为必须永不自动触发的高影响工作流配置 `allow_implicit_invocation: false`。

5. 通过 Codex 插件市场创建并分发插件，包括带有安装策略的本地市场配置。

6. 将技能和插件组合成可复用的 Agent 工作流。

### 消费者视角：使用与浏览生态系统

#### 什么是技能及其工作原理

技能是一个包含 `SKILL.md` 文件的目录。该文件使用 YAML 前置元数据声明名称和描述，随后是自由格式的 Markdown 说明。Agent 将这些说明作为加载的上下文进行读取，并在说明指示时调用存储在 `SKILL.md` 旁边的脚本或参考文档。没有任何内容会自动执行；Agent 读取说明并根据你的提示词决定如何对其采取行动。

设计理念是**渐进式披露**。启动时，Codex CLI 仅从每个可用技能加载 `name` 和 `description` 字段，每个技能约 100 个 token。完整主体仅在 Agent 判定技能相关时加载，要么是因为你的提示词隐式匹配了描述，要么是因为你按名称引用了该技能。`scripts/`、`references/` 和 `assets/` 子目录中的文件仅在 `SKILL.md` 说明引用它们时才加载。

一个提交了二十个技能的团队在启动时大约消耗 2,000 个 token，这完全在任何现代模型的上下文预算范围内，并且仅为与当前任务相关的技能加载详细说明。

最小目录结构如下：

```
incident-triage/
├── SKILL.md        # 必需；下面的每个条目均为可选
├── scripts/        # 运行手册命令、告警查询、呼叫助手
├── references/     # 事后分析模板、严重程度剧本
└── assets/         # 事件 JSON Schema、消息模板
```

#### agentskills.io 标准

Agent Skills 规范由 agentskills.io^([69]()) 管理，是打包 Agent 知识的可移植格式。Anthropic 开发了该格式，现已成为开放标准。^([70]()) 采用范围跨越数十个 Agent 产品，包括 CLI Agent、IDE Agent、桌面和云产品以及特定领域平台；agentskills.io 客户端目录记录了当前列表。^([71]()) agentskills.io 注册表是技能发现和分发的中心枢纽。

跨平台采用是该标准最具影响力的特性：提交到你仓库的单个 `SKILL.md` 无论开发者使用哪种 Agent 都能完全一致地工作。为 Codex CLI 编写的技能无需修改即可移植到 Claude Code、Cursor、Roo Code、Gemini CLI 以及其他每个采用该标准的产品。^([72]()) 无需移植步骤，无需工具特定方言，无需每个 Agent 的配置文件。这是该规范刻意极简设计的实际回报：足够严格以确保互操作性，足够简单以便每个 Agent 运行时都能实现它。

与 `AGENTS.md`（*第 4 章*）一样，`SKILL.md` 是开放标准而非 Codex 专有文件格式。两者均由 Linux 基金会的 Agentic AI Foundation 托管，因此使 `AGENTS.md` 安全可靠的相同治理保证也适用于技能。这两种格式是互补的：`AGENTS.md` 承载项目的常规规则和约束，而 `SKILL.md` 承载特定工作流的程序性知识。它们共同形成一个一次编写、到处部署的层，可在实现任一标准的任何 Agent 运行时上运行。

安全研究人员已识别出滥用 `scripts/` 目录或在 `SKILL.md` 中嵌入提示词注入载荷的恶意技能。请像对待第三方代码一样对待已安装的技能：在激活前审查每个文件，并避免从不可信来源安装。

#### 目录与安装

规范目录是 OpenAI 官方仓库 `github.com/openai/skills`^([73]())。它包含两个可见层级：**系统技能**内置于 Codex CLI（`$skill-creator`、`$skill-installer`、由 `codex-auto-review` 模型支持的内置代码审查技能）和一个经过 OpenAI 内部审查的**精选目录**可安装技能。精选示例包括 `gh-address-comments`、`gh-fix-ci`、多个 Notion 集成技能以及 `$define-goal`，后者可在编写任何代码前将模糊意图转化为具体、可衡量的目标。系统技能已可用（带 `$` 前缀）；精选技能需要单一安装步骤。

内置的 `$skill-installer` 是从官方目录获取的标准方式：

```
$skill-installer gh-fix-ci
```

你也可以将安装器指向显式 URL 或本地路径，这对于跨机器共享开发中技能很有用：

```
$skill-installer install https://github.com/openai/skills/tree/main/skills/.curated/gh-fix-ci
```

除了官方目录，社区技能还托管在第三方注册表中，如 agentskills.io^([74]())、skills.sh^([75]()) 和 Tessl 注册表。^([76]()) skills.sh 提供的跨平台 `npx skills` CLI 可通过单个命令在各 Agent 产品中安装社区技能：

```
npx skills add vercel-labs/agent-skills --skill frontend-design -a codex
```

#### 安装、管理与版本控制技能

##### 范围与目录解析

Codex CLI 按以下优先级顺序（从高到低）扫描技能目录：

```
$CWD/.agents/skills       # 项目级
$CWD/../.agents/skills    # 父目录
$REPO_ROOT/.agents/skills # 仓库根目录
$HOME/.agents/skills      # 用户级
/etc/codex/skills         # 系统/管理员
内置系统技能              # 最低优先级
```

同名的项目级技能会覆盖用户级技能，用户级技能覆盖系统级技能。

| **范围** | **目录** | **控制者** |
| --- | --- | --- |
| 项目 | `$REPO_ROOT/.agents/skills/` | 代码仓库（提交至源码控制） |
| 用户 | `$HOME/.agents/skills/` | 个人开发者 |
| 管理员 | `/etc/codex/skills/` | 组织平台团队 |
| 系统 | 内置 | OpenAI |

项目级是团队最实用的层级。将团队的项目专用技能提交到 `$REPO_ROOT/.agents/skills/`，克隆仓库的每位开发者都能自动获得一致的 Agent 行为，无需手动安装步骤。

##### 禁用而非删除

如果你想阻止技能加载而不删除其目录，请在 `~/.codex/config.toml` 中添加条目：

```
[[skills.config]]
name = "gh-address-comments"
enabled = false
```

修改 `config.toml` 后重启 Codex CLI。当技能导致意外的隐式激活时，此方法效果很好：禁用它，调查描述，调整后重新启用。

##### 调用策略

默认情况下，当你的提示词匹配描述时，Codex CLI 会隐式激活技能。通过在技能目录内添加 `agents/openai.yaml` 文件可覆盖此行为：

```
policy:
  allow_implicit_invocation: false
```

禁用隐式调用后，技能仅在你按名称引用时激活。对敏感或昂贵的技能使用此设置，此时你需要明确意图而非自动模式匹配。

#### 选择合适的基元

技能是 Codex CLI 提供的六个配置接口之一。每个接口解决不同的问题：

1.  **AGENTS.md** 承载静态项目上下文：约定、命令、文件边界（*第 4 章*）。
2.  **Hooks** 强制执行生命周期护栏：控制 Agent 是否运行（*第 9 章*）。
3.  **技能** 编码程序性知识：如何做 X。
4.  **MCP 服务器** 暴露实时集成：授予 Agent 对 Y 的实时访问权限（*第 8 章*）。




5. **子代理** 委派执行：在隔离的上下文窗口中并行工作（*第 11 章*）。
6. **插件** 将其余部分打包为可分发的单元（本章后文介绍）。

这些层级是累加的。典型的成熟配置会使用 `AGENTS.md` 管理项目上下文，使用两三个技能处理团队工作流，使用一两个 MCP 服务器进行外部集成，使用子代理进行编排，并使用插件进行分发。从能解决问题的最底层开始；只有当下层力有不逮时，才向上迁移。

其中，最棘手的选择在于技能、MCP 和钩子之间：这三种原语往往看起来像是完成同一项工作的替代方案。*表 10-1* 对它们进行了直接对比。

##### 表 10-1. 技能 vs. MCP vs. 钩子 对比矩阵

| **维度** | **技能** | **MCP 服务器** | **钩子** |
| :--- | :--- | :--- | :--- |
| 定义 | 加载到代理窗口中的基于文本的上下文 | 暴露类型化工具端点的实时服务器 | 生命周期事件触发的 Shell 回调 |
| 加载模式 | 渐进式：启动时加载名称和描述，激活时加载主体 | 预加载式：启动时加载所有工具架构 | 事件驱动式：在 `SessionStart`、`Stop` 或提交提示词时运行 |
| 执行模式 | 代理读取指令并使用自己的工具执行 | 代理调用结构化端点，针对运行中的进程执行 | 运行您的脚本；输出/退出码反馈给代理 |
| 最适用场景 | 程序性知识、团队约定、工作流模式 | 实时数据访问、认证 API、有状态操作 | 上下文注入、提示词拦截、会话边界副作用 |
| 分发方式 | 提交到仓库的文件或从注册表安装 | 作为守护进程运行；配置引用主机和端口 | 提交到 `.codex/` 的脚本 |
| 跨平台 | 一个 `SKILL.md` 可在多个代理产品间运行（Codex CLI、Claude Code、Cursor、Roo Code、Gemini CLI 等） | 除非平台支持 MCP，否则为平台专用 | 仅限 Codex |
| 何时使用 | “教会代理如何做 X” | “给代理提供对 Y 的实时访问” | “控制代理何时、是否运行” |

如果您正从变得难以驾驭的 `AGENTS.md` 文件迁移，技能通常是正确的重构目标。详见*第 4 章* 的完整膨胀分析。因为这两种格式都受 AAIF 管辖，迁移路径有标准支撑：项目上下文保留在 `AGENTS.md`，可复用的程序性知识迁移到可移植的技能中。

### 生产者视角：编写您自己的技能

#### SKILL.md 格式

技能是一个目录，而非单个文件。它必须包含一个 `SKILL.md` 文件；其他皆为可选。^(⁷⁷)

```
my-skill/
├── SKILL.md ← 必需：前置元数据 + 指令
├── agents/
│ └── openai.yaml ← Codex 专用：调用策略、MCP 依赖
├── scripts/ ← 可选：可执行辅助工具
├── references/ ← 可选：参考文档，按需加载
└── assets/ ← 可选：模板、数据文件
```

`SKILL.md` 由 YAML 前置元数据分隔符分隔为两个部分。开始 `---` 和结束 `---` 之间的所有内容为结构化元数据。之后的所有内容为 Markdown 散文，即代理在技能激活后作为指令读取的内容。

下面是一个完整的带注释示例：

```
---
name: deploy-to-staging
description: |
 将当前服务部署到暂存环境。当被要求部署、推送到暂存、
 切割发布候选版本或更新正在运行的暂存服务时使用。
 请勿用于生产环境部署或本地测试环境。
license: Apache-2.0
compatibility: Requires Docker, AWS CLI ≥ 2.x
allowed-tools: Bash Read
metadata:
 author: platform-team
 version: "2.1"
---
#### Instructions
1. 运行 `scripts/preflight.sh` 以验证环境变量和 Docker 守护进程的可达性。
2. 构建并标记 Docker 镜像：`docker build -t $SERVICE:$SHA .`
3. 推送到 ECR：`scripts/push-ecr.sh`
4. 更新 ECS 任务定义并注册新版本。
5. 等待服务稳定；报告成功或失败。
```

前置元数据受代理技能规范管辖。两个字段为必填（`name` 和 `description`）；其他四个为选填。*表 10-2* 列出了每个字段及其约束。请遵守这些约束，否则参考验证器将拒绝该技能。

##### 表 10-2. SKILL.md 前置元数据字段参考

| **字段** | **必填** | **约束** |
| :--- | :--- | :--- |
| `name` | 是 | 1 至 64 字符。仅限小写字母数字和连字符。必须与父目录名称完全一致。不能有前导/尾随连字符，不能有连续连字符。 |
| `description` | 是 | 1 至 1,024 字符。每次会话开始时加载。充当路由规则：描述技能做什么、何时使用，包含有助于代理识别相关任务的关键词。 |
| `license` | 否 | 许可证名称或捆绑许可证文件的路径。 |
| `compatibility` | 否 | 最多 500 字符。仅指定真实需求（Python 版本、所需二进制文件、网络访问等）。 |
| `metadata` | 否 | 任意键值对（字符串键、字符串值）。用于版本、作者、团队、变更日志指针等。 |
| `allowed-tools` | 否 | 撰写时为实验性。空格分隔的预批准工具名称，使代理跳过逐操作权限提示。 |

`name` 约束是大多数初次作者的绊脚石。参考验证器会将失败显示为单行错误，但提前识别这些模式会更快。*表 10-3* 将有效名称与四个最常见的拒绝原因进行了对比。

##### 表 10-3. 常见 name 字段陷阱

| **名称** | **有效** | **原因** |
| :--- | :--- | :--- |
| `deploy-to-staging` | 有效 | 不适用 |
| `pr-review` | 有效 | 不适用 |
| `CodeReview` | 无效 | 包含大写字母 |
| `pr_summarizer` | 无效 | 包含下划线 |
| `-deploy` | 无效 | 以连字符开头 |
| `deploy--v2` | 无效 | 包含连续连字符 |

#### 三层加载模型

Codex 通过三个加载层级实现渐进式披露：^(⁷⁸)

会话开始时，Codex CLI 扫描每个配置的技能位置，仅读取 YAML 前置元数据。您可以安装 100 个技能，发现开销约为 8,000 个令牌，相当于一个冗长的 MCP 服务器工具清单。请将 `SKILL.md` 正文控制在 500 行以内（约 5,000 个令牌）；如果指令超出该限制，请将内容拆分到单独的文件中。按主题聚焦的 Markdown 文件（每主题一个，例如 `REFERENCE.md`、`FORMS.md` 或特定领域文件如 `legal.md`）放在 `references/` 下。可执行脚本（Python、Bash 或 JavaScript）放在 `scripts/` 下。代理仅在 `SKILL.md` 指令指示时按需加载它们。

这是技能与 MCP 的根本架构差异。MCP 在会话开始时注入所有工具架构；一个包含五个服务器的生产级 MCP 配置，在第一条用户消息到达前可能消耗 55,000 个令牌。渐进式披露模型颠倒了这一点：按需付费。

#### 编写 description 字段

`description` 字段一份双职：既是人类文档，也是隐式调用的路由规则。模糊的描述会导致漏调；过宽的描述会导致误触。

```
##### 太模糊 — 会漏掉调用
description: Manages deployments.
##### 太宽泛 — 会在无关任务上触发
description: Handles any infrastructure or cloud task.
##### 写得好 — 具体触发条件，明确排除项
description: |
 将当前服务部署到暂存环境。当被要求部署、推送到暂存、
 切割发布候选版本或更新正在运行的暂存服务时使用。
 请勿用于生产环境部署或本地 Docker Compose 测试环境。
```

写得好的示例包含三个结构要素：



#### 核心设计原则

1.  **清晰的主要操作**：明确界定技能的目的。
2.  **触发同义词**：覆盖用户表达同一意图的常用词汇。
3.  **显式排除项**：防止危险的误触发。

该字段限制为 1,024 字符：足以容纳同义词和排除项，而无需写成长篇大论。

**先写排除项**。当你明确阐述技能*不应*做什么时，包含标准几乎会自行成型。

#### `agents/openai.yaml` 文件：策略与 MCP 依赖

`agents/openai.yaml` 文件（可选）用于配置 UI 表现、调用策略和 MCP 服务器依赖。[⁷⁹]

```
interface:
 display_name: "Staging Deploy"
 short_description: "Deploy containerised services to the staging environment"
 brand_color: "#FF9900"
 default_prompt: "Deploy the current service to staging"
policy:
 allow_implicit_invocation: false
dependencies:
 tools:
 - type: "mcp"
 value: "awsInfrastructure"
 description: "AWS infrastructure MCP server for ECS and ECR access"
transport: "streamable_http"
 url: "https://mcp.internal.example.com/aws"
```

`allow_implicit_invocation` 默认为 `true`。对于涉及生产基础设施、运行状态修改测试、删除文件或调用影响计费 API 的技能，请将其设置为 `false`。

包含“deploy”一词的杂散提示绝不应静默触发部署工作流。对于高影响力技能，务必设置 `allow_implicit_invocation: false`。

`dependencies.tools` 数组连接技能与 MCP：技能编码*如何运行工作流*；MCP 服务器提供*对外部系统的实时访问*。当 Codex CLI 加载带有 MCP 依赖的技能时，会提示用户安装任何缺失的服务器。

#### 构建与发布技能

##### 本地验证

在 Codex 中测试前，请运行参考验证器。`skills-ref` 是一个位于规范仓库中的 Python 工具，从源码安装：[⁸⁰]

```
git clone https://github.com/agentskills/agentskills.git
cd agentskills/skills-ref
pip install -e .
skills-ref validate ~/code/your-repo/.agents/skills/deploy-to-staging/
```

如果你使用 `uv`，`uv sync` 是 `pip install -e .` 的替代方案。验证器会检查前置元数据约束，确认 `name` 与目录名匹配，并报告行级错误。

##### 在 Codex 中测试

在技能所在的仓库中打开 Codex CLI 会话。输入使用你描述中同义词的提示，确认技能被激活。然后验证 `allow_implicit_invocation: false` 能阻止隐式激活。

##### 发布与分享技能

agentskills.io 是一个规范和客户端目录，而非托管注册表；没有“发布”按钮能将技能变为单名安装。以分发任何其他开源制品的相同方式分发技能：推送到公共 Git 仓库并告知他人 URL。

1.  将技能目录推送到公共 GitHub 仓库（约定路径：仓库根目录下的 `skills/<skill-name>/`）。
2.  在技能目录中添加 `README.md`，包含简短描述、前置条件和一个示例调用。
3.  （可选）在社区目录（如 agentskills.io 或 skills.sh）上列出你的技能；两者都索引公开托管的技能仓库。

有三种模式供读者安装你发布的内容。选择与你的分发渠道匹配的一种：

```
#### 直接从 Git URL 安装 — 适用于任何主机
$skill-installer install https://github.com/<owner>/<repo>/tree/main/skills/<skill-name>
#### 跨平台社区 CLI，支持多种代理
npx skills add <owner>/<repo> --skill <skill-name> -a codex
#### 通过团队注册的市场安装
codex plugin marketplace add <owner>/<repo>
```

裸名称查找（例如 `$skill-installer gh-fix-ci`）保留给 OpenAI 在 `github.com/openai/skills` 处策展的官方目录中的技能。社区技能始终需要 URL 或 `owner/repo` 命名空间。

#### 技能规模：五层级谱系

并非每个工作流都需要包含脚本、参考资料和 CI 验证的完整生产级技能。技能复杂度落在五层级谱系上，大多数团队因直接跳到上层而过度投入。

**第 1 级：10 字提示。** `AGENTS.md` 中的单句或直接输入。无文件，无 YAML。适合运行一两次的一次性工作流。示例：“对所有已更改文件运行 ESLint 并修复可自动修复的问题。”

**第 2 级：可复用提示。** 存储在文本文件中或仅含描述的简短 `SKILL.md`，无支持文件。适合每周重复的工作流。示例：包含描述和五行生成变更日志指令的 `SKILL.md`。

**第 3 级：团队约定。** 包含描述、触发同义词、排除项及含有一两个辅助脚本的 `scripts/` 目录的 `SKILL.md`。适合跨团队共享的工作流。大多数项目专用技能应止步于此。

**第 4 级：库技能。** 完全规范的 `SKILL.md`，包含 `references/`、`scripts/`、`assets/`、`agents/openai.yaml`、MCP 依赖及 CI 验证。适合作为社区目录条目发布的跨团队或跨组织技能。

**第 5 级：生产制品。** 具有版本锁定、自动化基准测试、回归测试和正式评审流程的技能。适合把关生产部署或强制执行合规策略的技能。

陷阱在于在第 2 或 3 级足矣时却伸手去够第 4 或 5 级。解决今日问题的 10 字提示，胜过为解决明天假设问题而投入三天打磨的生产级技能。

如果编写 `SKILL.md` 耗时超过了它自动化任务本身的耗时，说明你在过度设计。先写提示。用三次后，提取为第 2 级技能。团队采纳后，晋升为第 3 级。让使用量驱动复杂度。

#### 创作工作流：手动构建，再编码化

一种**新兴社区实践**，由 Nate Herk 的 Codex CLI 课程推广，即先手动构建工作流，再将其编码为技能，而非预先设计技能稍后再发现缺口。[⁸¹] 五步流程如下：

1.  **在标准会话中手动构建。** 端到端完成任务。识别步骤、错误处理逻辑、输出格式和 API 端点。
2.  **编写 `SKILL.md`。** 用清晰的描述（路由规则）和分步指令编码流程。
3.  **验证激活。** 确认技能出现在 TUI 选择器中并响应其 `$` 调用。
4.  **关联 `AGENTS.md`。** 如果技能依赖项目特定路径、基础设施或约定，确保项目的 `AGENTS.md` 在会话开始时提供该上下文。
5.  **迭代优化。** 面对真实输入运行。当失效模式出现时，收紧指令。

这自然产出第 2 或 3 级技能，避免了跳到第 4 级的过早复杂性。围绕同一工作流衍生出三种相邻实践：[⁸²]

**全局与项目本地。** 如果技能引用特定文件路径、基础设施端点或项目特定配置，使其成为项目本地（提交至 `$REPO_ROOT/.agents/skills/`）。如果其通用运行（如变更日志生成器、PR 总结技能），全局安装在 `$HOME/.agents/skills/`。

**作为技能步骤的质量门禁。** 部署技能可包含浏览器验证步骤，在未发现严重 Bug 前阻止推送。这将质量把关直接嵌入技能流程，而非依赖单独的 CI 检查，符合第 19 章中的“源头质量”原则。

**创作时的推理力度。** Medium 或 High 推理力度是迭代优化的最佳平衡点：足够快以便迭代，足够准确以正确编码流程。为调试复杂故障保留 Extra High。

[⁷⁹]: 脚注 79 内容
[⁸⁰]: 脚注 80 内容
[⁸¹]: 脚注 81 内容
[⁸²]: 脚注 82 内容



### 评估技能

使用 `$skill-creator` 脚手架新技能，而不是从头编写 `SKILL.md`。

### 评估技能

`skills-ref` 可以捕捉语法错误，但无法捕捉行为失败。要回答“技能是否真正有效？”，你需要一个评估流水线：一种结构化、可重复的方式，用于根据预期结果衡量技能行为。^[83]

#### 三层评估架构

**第一层：手动冒烟测试。** 通过所有三种调用路径触发技能：TUI 选择器、`$` 前缀，以及通过描述匹配的隐式路由。验证“何时使用”和“何时不使用”的指导原则是否成立。

**第二层：确定性 JSONL 评分。** 运行 `codex exec --json` 以生成结构化事件流（`thread.started`、`turn.started`/`turn.completed`、`item.started`/`item.completed`，类型包括 `command_execution`、`file_change`、`mcp_tool_call`）。以编程方式解析事件，以断言特定行为（文件创建、命令执行、调用正确的工具）。从包含 10 到 20 个代表性用例的 CSV 测试集中提供提示词。

**第三层：模型辅助量表评分。** 使用 `codex exec --output-schema` 配合 Zod 或 JSON Schema 定义来收集结构化评估输出。一个单独的模型实例根据量表（正确性、完整性、指令遵循度）对每个响应进行评分。这可以捕捉确定性检查遗漏的定性失败。

**描述字段是评估的一部分。** 如果描述错误，技能会在不该触发时触发，或在该触发时失败。通过创建触发/非触发提示词对，单独评估描述，确认隐式路由符合预期。一个内部完美但描述误导的技能在实践中仍会失败。

#### 社区评估工具

在官方工具成熟之前，社区评估工具正在填补空白。有两个值得了解：

1.  **Skillgrade**^[84] 将所有三层封装为单个命令：

    ```
    skillgrade run --skill .agents/skills/deploy --tests tests/deploy.csv
    ```

    它运行 CSV 测试用例，执行确定性断言，可选调用模型辅助评分，并为每个用例写入机器可读的通过/失败报告。

2.  **Skill Creator V2**（Nathan Onn）采用不同角度：针对固定测试用例集进行并行变体基准测试，根据输出质量、一致性和指令遵循度对每个变体评分。^[85] 它用定量比较取代了“感觉更好”的判断。基准测试运行消耗大量 Token；请参阅 *第 14 章* 了解成本影响。对于第 4 层或第 5 层技能，定量评分基础设施可以证明投资的合理性。

#### CI 集成

运行 `skills-ref` 进行语法验证，运行评估流水线进行行为验证：

```
- name: Validate skills
  run: |
    pip install git+https://github.com/agentskills/agentskills.git#subdirectory=skills-ref
    for skill_dir in .agents/skills/*/; do
      skills-ref validate "$skill_dir"
    done
- name: Eval skills
  uses: openai/codex-action@v1
  with:
    codex-args: "--json"
    eval-config: .codex/eval/skills.yaml
```

根据评估通过率设置合并门禁：低于阈值的技能回归将阻止 PR。这对技能应用了与应用代码相同的规范。

#### 托管生命周期平台

想要版本控制、分发、评估和治理，又不想自己构建流水线的团队，可以使用托管平台（如 Tessl^[86]），它提供注册表、治理控制和全生命周期工具。托管平台以较少的控制权换取更少的维护和更快的企业级治理路径；当手动管理一切的开销开始占主导地位时（第 4 层或第 5 层），这种权衡最合理。

### 插件：打包与分发

技能编码程序性知识。插件将这些知识连同应用连接器和 MCP 服务器配置打包成可安装、可分发的单元。Codex 插件市场是分发渠道：技能、应用集成和 MCP 服务器配置的托管安装和更新工作流。^[87]

#### 插件架构

插件是一个包含 `.codex-plugin/plugin.json` 清单和可选组件包的目录。^[88] 清单最多声明三种组件类型：

1.  **技能**：`skills/` 子目录中的 `SKILL.md` 文件，通过前文所述的渐进式披露模型按上下文加载
2.  **应用**：外部服务连接器（GitHub、Slack、Gmail、Google Drive），通过 OAuth 或 API Token 提供经过认证的数据访问
3.  **MCP 服务器**：在 `.mcp.json` 文件中声明的模型上下文协议服务，暴露来自外部系统的工具和共享上下文

任意组合均有效。插件将 `AGENTS.md` 片段、`config.toml` MCP 部分、OAuth 设置和支持资产打包成单个可安装单元。

```
my-review-tools/
├── .codex-plugin/
│ └── plugin.json # 必需清单
├── skills/
│ └── security-review/
│ └── SKILL.md # 技能指令
│ └── perf-review/
│ └── SKILL.md
├── .mcp.json # 可选 MCP 配置
├── .app.json # 可选应用连接器
└── assets/
 └── logo.png
```

最小 `plugin.json` 示例：

```
{
 "name": "my-review-tools",
 "version": "1.0.0",
 "description": "Code review patterns for the platform team",
 "skills": "./skills/",
 "mcpServers": "./.mcp.json",
 "interface": {
 "displayName": "Review Tools",
 "shortDescription": "Automated code review against team standards",
 "category": "Code Quality"
 }
}
```

所有清单路径必须以 `./` 为前缀，并相对于插件根目录。`name` 字段必须是稳定的 kebab-case；Codex CLI 在市场操作中将其用作插件标识符。

#### 市场发现与分发

Codex CLI 从三个来源读取插件目录，按优先级顺序解析：

1.  **openai/skills 精选目录**：官方经过审核的插件目录，启动时获取并本地缓存。这些显示在 `/plugins` TUI 浏览器的“精选”下。
2.  **仓库级市场**：`$REPO_ROOT/.codex/plugins/marketplace.json` 将插件限定在特定仓库。提交此文件，每个协作者自动获得相同的插件集。
3.  **个人市场**：`~/.codex/plugins/marketplace.json` 保存针对单个开发者的插件，适用于个人工作流自动化或开发中的插件。

当插件名称出现在多个市场中时，优先级最高的来源生效，允许组织在需要时用仓库级分支覆盖精选插件。^[89]

插件通过两种调用机制呈现。**隐式调用**：描述你想要的结果（例如“总结我未读的 Gmail 线程”），Codex CLI 自动选择合适的插件工具。**显式调用**：输入 `@` 后跟插件或技能名称，直接针对特定能力。在脚本化工作流和 `AGENTS.md` 指令中，显式调用更佳，因为确定性很重要。

#### 安装策略与企业治理

`marketplace.json` 中的每个插件条目都带有一个 `installation` 策略，直接映射到企业 IT 治理模式：^[90]

| 策略 | 行为 | 企业用例 |
| --- | --- | --- |
| `INSTALLED_BY_DEFAULT` | 对打开仓库的每个团队成员立即激活 | 标准工具推广 |
| `AVAILABLE` | 在 `/plugins` 中列出但需要手动安装 | 批准的可选工具 |
| `NOT_AVAILABLE` | 完全从目录中隐藏 | 未批准、已弃用或受限工具 |





#### 插件认证策略

独立的 `authentication` 字段控制何时与第三方服务交换凭据：

- `ON_INSTALL`：安装期间立即触发认证
- `ON_FIRST_USE`：延迟到插件被调用时才认证

#### 插件市场配置示例

```json
{
  "name": "acme-team-plugins",
  "interface": { "displayName": "Acme Engineering Plugins" },
  "plugins": [
    {
      "name": "internal-review-bot",
      "source": { "source": "local", "path": "./plugins/review-bot" },
      "policy": { "installation": "INSTALLED_BY_DEFAULT", "authentication": "ON_INSTALL" },
      "category": "Code Quality"
    },
    {
      "name": "slack-integration",
      "source": { "source": "local", "path": "./plugins/slack" },
      "policy": { "installation": "AVAILABLE", "authentication": "ON_FIRST_USE" },
      "category": "Communication"
    },
    {
      "name": "untrusted-community-tool",
      "source": { "source": "local", "path": "./plugins/community-x" },
      "policy": { "installation": "NOT_AVAILABLE" },
      "category": "Experimental"
    }
  ]
}
```

这反映了**移动设备管理**（**MDM**）和 **IDE 扩展治理**中的允许列表模式。将 `marketplace.json` 提交到仓库根目录，可将插件策略随其所管辖的代码一起纳入版本控制。

谨慎使用 `INSTALLED_BY_DEFAULT`。默认安装的插件会增加每个会话的上下文负担。仅将其保留给真正通用的团队工作流（代码审查标准、安全策略），而非可有可无的功能。

#### 通过 app-server RPC 进行安装和移除生命周期管理

插件系统支持完整的安装、浏览、移除和更新生命周期：^(⁹¹)

| **生命周期阶段** | **能力** | **CLI 界面** |
| --- | --- | --- |
| **Install** | 注册市场并从 Git、本地路径或 URL 安装插件 | `codex plugin marketplace add`, `/plugin install` |
| **Browse** | 带标签页的市场 TUI、内联切换、跨仓库插件读取（含生成的描述） | `/plugins` |
| **Remove** | 彻底卸载市场源及其缓存的插件 | `codex plugin marketplace remove` |
| **Update** | 自动将配置的市场升级到最新上游版本 | `codex plugin marketplace upgrade` |

移除逻辑位于 `codex_core::plugins::remove_marketplace()`，这是一个可从 CLI 和 app-server RPC 层调用的共享函数。^(⁹²) CLI 是对该核心函数的薄封装，因此任何客户端界面（VS Code 扩展、桌面应用、JetBrains 插件，或基于 app-server 协议构建的自定义集成）都可以通过相同的代码路径安装、浏览和移除插件。

来自远程市场的未安装插件会显示生成的描述，包含源 Git URL、路径、引用和 SHA 详情，以便团队在决定安装前评估插件。^(⁹³)

### 可复用工作流模式：组合技能与插件

当技能和插件组合成可复用的 Agent 工作流时效果最佳：即技能激活、插件调用和 MCP 工具调用的序列，编码了团队的标准操作程序。^(⁹⁴)

#### 模式 1：审查-修复循环

代码审查技能识别差异中的问题。修复技能（或 Agent 原生编码能力）解决每个发现。第二次审查确认修复。该循环可编码为单个复合技能（其指令编排序列），或作为三个独立技能由父级 `AGENTS.md` 指令串联。

#### 模式 2：脚手架-测试-部署流水线

脚手架技能生成样板代码（新服务、新迁移、新 API 端点）。测试技能运行项目测试套件并报告失败。部署技能在测试通过后推送到暂存环境。每个技能独立有用；组合后，它们编码了完整的功能交付工作流。

#### 模式 3：插件即工作流包

当工作流不仅需要指令，还需认证 API 访问和外部工具时，插件打包整个技术栈。一个插件可能包含：用于编排 Terraform 漂移修复工作流的技能、用于读取云提供商状态的 MCP 服务器、用于将结果发送到 Slack 的应用连接器。开发者安装一个插件即获得完整能力。

**通用原则**：从独立技能开始用于仅含指令的工作流。当工作流需要 MCP 服务器、应用连接器或团队级治理控制时，提升为插件。当提交的 `SKILL.md` 即可满足时，抵制将所有内容打包进插件的冲动。

设计组合工作流时，保持每个技能专注于单一职责。审查代码的技能不应同时部署代码。组合发生在编排层（`AGENTS.md` 指令、父技能或插件清单），而非单个技能内部。

#### 插件捆绑钩子

插件可随技能和 MCP 服务器一起携带自己的生命周期钩子，扩展 *第 9 章* 所述的核心钩子系统。^(⁹⁵) 插件捆绑钩子在与用户定义钩子相同的生命周期点触发（`PreToolUse`、`PostToolUse`、`PreL1-Bullet`、`PostL1-Bullet`），但作用域限定在插件上下文中。安全插件可捆绑 `PreToolUse` 钩子以阻止危险的 Shell 命令，无需用户编写自定义钩子脚本。钩子启用状态按插件跟踪，因此切换插件开/关会同时切换其捆绑钩子。

### 扩展优先架构

Codex CLI 采用**扩展优先架构**：核心能力作为可插拔扩展而非硬连线的运行时组件。自动审查器（检查建议变更的安全性和正确性的模型）本身就是可插拔扩展，团队可替换、定制或补充：面向受监管环境的更严格策略引擎，或面向内部工具项目的轻量级检查。插件 ID 规范化保持共享工作区整洁（`my-org/linter` 和 `My-Org/Linter` 注册为同一插件而非两个），插件可发现性分离让管理员能独立控制市场浏览器中可见的插件与可安装的插件。对技能和插件作者的实用启示：治理、审查和策略执行技能插入的是核心功能使用的相同扩展点。

### 案例研究

这些模式在生态系统中普遍存在。四个示例展示了不同层级、不同来源的技能：构建系统集成、OpenAI 团队官方的一级项目技能、编排完整工作流的社区元技能，以及主要平台供应商的精选目录。

#### Nx 单体仓库 Agent 技能

Nx 构建系统的官方 Codex CLI 集成展示了技能与 MCP 分离的实践。`@nx/codex` 包提供六个核心技能（`nx-project-details`、`nx-affected`、`nx-generators`、`nx-dependency-graph`、`nx-migrate` 和 `nx-ci-pipeline`），每个编码为带有调用 Nx CLI 命令的辅助脚本的 `SKILL.md`。除了技能，一个 MCP 服务器（`nx-mcp`）暴露实时工作区元数据：项目图、任务流水线和生成器模式。

技能编码**程序性知识**（“添加新库时，按以下约定运行 `nx g @nx/js:library`”），而 MCP 服务器提供**实时数据**（“这是工作区中的 47 个项目及其依赖关系”）。同时使用两者的 Agent 可推理单体仓库结构（通过 MCP）并正确操作（通过技能），无需开发者在每次提示中解释 Nx 约定。对于大型 Nx 单体仓库，这将 `AGENTS.md` 的篇幅从数页说明压缩为指向已安装技能的几行指向。

#### codex-pr-body：OpenAI 团队官方编写的首个项目级技能




##### `codex-pr-body` 技能：第三层参考实现

`codex-pr-body` 技能是 OpenAI 团队编写的首个正式的项目级技能，直接提交到了 Codex CLI 仓库本身。与 `$skill-installer` 或 `$skill-creator` 等作为内置原语随产品发布的系统技能不同，`codex-pr-body` 位于 `.agents/skills/codex-pr-body/` 并遵循本章提倡的相同约定。其用途：生成考虑了项目特定工作流模式的拉取请求描述。

三个设计选择使其成为一个有用的参考实现：

##### 随代码库一同发布的技能

该技能被提交到仓库中，而非外部发布。克隆仓库的每位贡献者都能自动获得一致的 PR 描述行为：无需安装步骤，无版本漂移。这验证了第三层团队约定模式：复杂到足以保证结构化说明，简单到足以跳过外部发布基础设施。

##### 仅读取 Sapling CLI 以保障安全

该技能将 `sl` 限制为只读操作（`sl log`、`sl status`、`sl diff`）并显式禁止写入。技能需要仓库历史来生成准确的描述，但作为副作用绝不能修改工作树。此模式可推广至任何在不冒险导致意外变更的情况下检查版本控制状态的技能。

##### 感知堆叠 PR 的描述

Codex CLI 团队使用堆叠拉取请求（通过 Sapling 和 Graphite），其中单个功能可能跨越多个依赖的 PR。该技能检查特定于当前堆叠层的提交范围，而非简单地对比 `main` 分支，从而生成反映每个 PR 增量变更的描述。使用堆叠差异工作流的团队面临相同问题；`codex-pr-body` 演示了如何将堆叠感知编码为可移植的技能格式。

更广泛的意义在于：构建智能体的团队已采用本章提倡的模式。当你在权衡是否投资团队专用技能时，`codex-pr-body` 既是参考实现，也是该模式可扩展的证据。

#### aigorahub/elves：编排工作流的元技能

迄今讨论的技能为单一任务编码知识：迁移流程、PR 描述格式、审查清单。**Elves**（aigorahub/elves）则完全不同：一种编排完整的多批次自主开发工作流的技能，拥有自己的记忆管理、治理模型和战略性遗忘。^(⁹⁶)

Elves 通过**两次调用交接模式**运行：一个 `Stage`（暂存）调用用于准备工作计划（解析需求、生成批次定义、创建治理结构），随后是一个 `Launch`（启动）调用开始自主执行。这种分离是刻意设计的，在智能体投入可能长达数小时的自主工作前，给人类一个审查检查点。

四项架构创新使 Elves 对技能设计者意义重大：

##### 分层记忆

Elves 跨批次边界维护四个不同的知识层：一个**计划**（整体工作规范）、一个**生存指南**（来自先前批次的惨痛教训，必须持久化）、一个**心得**（可能被丢弃的软性观察）和一个**执行日志**（已采取行动的持续记录）。每层有不同的保留规则：计划不可变，生存指南仅追加，心得在批次间审查和修剪，执行日志在每批次完成后归档。

##### 宪法式判决

每批次结束后，Elves 根据一组人工编写的“底线”规则评估自身输出，这些是硬性约束，一旦违反即中止工作流并升级给人类。这是技能层面的治理而非钩子层面：规则是技能知识的一部分，而非外部执行基础设施。

##### 战略性遗忘

Elves 不让上下文窗口填满直到压缩降低质量，而是主动归档已完成的批次历史，仅将持久化知识提升到其 `.ai-docs/` 目录。工作集在任意长的工作流中保持精简。此模式是第七章讨论的模型级压缩策略在应用层面的补充。

##### 生产级自主比率

该框架专为多批次开发任务上的高智能体-人类时间比而设计：人类在暂存和审查上投入几分钟，而智能体自主工作数小时。这些比率之所以可行，是因为技能的记忆和治理层处理了原本需要人类干预的故障模式。

Elves 证明技能不仅仅是指令集。它们可以是完整的运营框架：记忆管理、治理、自主性和自我修剪，打包为可移植的 `SKILL.md`。对于构建雄心勃勃的自主工作流的团队，Elves 是第五层生产级技能外观的当前参考实现。

#### Google Agent Skills：平台维护的知识包

从个人作者向平台维护包的转变，最好由 **Google Agent Skills**（google/skills）来说明，这是一个涵盖测试、代码审查、API 设计、性能、安全等领域的官方目录。^(⁹⁷) 每个技能均可通过标准分发机制安装：

```
npx skills add google/skills
```

其意义不在于内容（涵盖的是公认的工程实践），而在于分发模式：一家主要平台厂商维护并版本化技能包，可安装到任何支持 Agent Skills 标准的智能体中。这印证了生态系统的可移植性承诺：个人社区作者使用的相同 `SKILL.md` 格式，也适用于官方平台分发。

对企业团队而言，Google Agent Skills 可作为基线。安装官方包以广泛覆盖工程最佳实践，再在其上叠加团队专用技能以适配组织特定约定。这种组合同时提供了广度（平台维护、定期更新）和深度（团队编写、上下文相关）。

### 总结

1.  **技能是一个目录。** `SKILL.md`（包含 `name` 和 `description` 的 YAML 前置元数据，随后是 Markdown 说明），可选地由 `scripts/`、`references/` 和 `assets/` 支持。智能体将说明作为加载的上下文读取，并在说明指示时调用支持文件；无任何内容自动执行。

2.  **可移植的开放标准。** Agent Skills（agentskills.io）在 Linux 基金会的 Agentic AI Foundation 治理下，与 `AGENTS.md` 并列，可在兼容的智能体产品中原样工作。

3.  **三层架构，一套工作流。** 技能用三层渐进式披露编码程序性知识；MCP 服务器通过有状态连接暴露实时集成；钩子在生命周期事件强制执行运行时策略。成熟的智能体工作流同时使用三者，技能承载之前存在于臃肿 `AGENTS.md` 文件中的“如何做 X”知识。

4.  **描述即路由规则。** 它既是人类文档，也是智能体决定何时加载技能的触发器。编写时需包含明确的主要动作、触发同义词和显式排除项。

5.  **`agents/openai.yaml` 桥接。** 声明外部 MCP 服务器依赖，并对永不应在偶然匹配的提示词上触发的技能强制执行 `allow_implicit_invocation: false`。

6.  **解析顺序为项目、用户、管理员、再系统。** 将团队技能提交到 `.agents/skills/` 以确保团队间行为一致。

7.  **五层复杂度谱系。** 从 10 个单词的提示词到带有正式评估的生产制品。从起作用的最低层开始，仅当具体痛点要求时才提升层级。




# 8. 以构建为先的工作流

手动完成任务，将其编码为 `SKILL.md`，针对失败模式进行迭代。这自然会产出第 2 层到第 3 层的技能，且不会过早引入复杂性。

# 9. Codex 插件市场

分发将技能、应用连接器和 MCP 服务器配置捆绑为单个可安装单元的插件。`marketplace.json` 中的安装策略（`INSTALLED_BY_DEFAULT`、`AVAILABLE`、`NOT_AVAILABLE`）提供了企业级治理，其生命周期涵盖通过 `codex plugin marketplace` 子命令和应用服务器 RPC 层进行安装、浏览、移除和升级。

**另请参阅：** *第 4 章*（**AGENTS.md**）。当 `AGENTS.md` 文件变得难以驾驭时，技能是正确的重构目标。*第 4 章*介绍了表明该将程序性知识提取为可移植技能的膨胀模式。

### 练习

1.  **从零编写一个团队约定技能。** 找出团队 `AGENTS.md` 中一个足够专业、适合归入技能的章节（运行数据库迁移的说明、构建集成测试的结构或生成发布变更日志）。将其提取到 `.agents/skills/` 中的一个技能里，编写包含至少两个触发短语和一个显式排除项的描述，使用 `skills-ref` 进行验证，并确认该章节可从 `AGENTS.md` 中移除而不影响代理行为。

2.  **调整过度触发的描述。** 安装一个与你工作相关的社区技能类别。发送五个应激活它的提示词和五个不应激活它的提示词。如果观察到误触发（假阳性），编辑描述以缩小触发条件。记录修改前后的描述，并解释为何修订版本校准得更好。

3.  **创建一个本地插件市场。** 构建一个将两个相关技能（例如，一个审查技能和一个修复技能）捆绑为单个可安装单元的插件。在仓库根目录创建一个带有 `AVAILABLE` 安装策略的 `.codex/plugins/marketplace.json`。验证该插件出现在 `/plugins` 浏览器中，安装它，并测试这两个技能。然后将策略改为 `INSTALLED_BY_DEFAULT`，并确认全新的 Codex CLI 会话会在无需手动安装的情况下自动激活该插件。



