

# 第六章：Headless 模式与 CI/CD 集成

“如果你没有找到一种在睡觉时也能赚钱的方法，你将工作到死。”——沃伦·巴菲特

公司的技术团队只有三名资深工程师，却每天需要审查二十多个 PR。按照惯例，每个 PR 必须至少由一名资深工程师批准才能合并。后果显而易见：PR 堆积如山，发布频率被迫下降，开发人员不得不在等待审查期间切换到其他任务，导致频繁的上下文切换和效率降低。更严重的是，在审查高峰期，人工审查的质量开始下降；疲惫的审查者容易忽略安全漏洞或性能问题，而一旦这些问题进入生产环境，修复成本将成倍增加。

经过一周的调研，小雪提出了一个方案：引入 Claude 作为 PR 的“初审员”，自动执行安全检查、代码质量分析和测试覆盖率评估，并生成结构化的审查报告。人工审查者则基于这些报告做出最终决策，将注意力集中在业务逻辑和架构设计等核心领域。这种方式并非取代人工审查，而是将工程师从重复性的机械检查中解放出来，让他们能够专注于只有人类才能做出的更高层次的判断。

“Claude Code 能在无人值守的环境中自动运行吗？”小冰问道。

“这正是 Headless 模式的核心所在，”Kage 回答道。“在前六章中，我们一直以‘交互模式’使用 Claude：你输入命令，它响应，如此循环往复。但从本章开始，Claude Code 将摆脱人工实时操作，能够在后台、在 CI/CD 流水线中，甚至在凌晨三点无人值守时独立工作。这一转变的意义不亚于 Docker 将应用从‘绑定到特定机器’转变为‘可在任何环境中运行’。”

## 从交互到无人值守：一次关键的架构演进

在交互模式下，Claude Code 充当“现场对话伙伴”的角色：用户输入命令，Claude 立即响应，用户确认结果后继续下一轮交互。这种模式非常适合探索性工作，例如代码调试、架构头脑风暴或增量式模块重构。然而，在自动化场景中，键盘前没有人，没有人检查中间步骤，整个流程必须能够从头到尾独立运行，零人工干预。

Headless 模式正是为此而生。通过使用 `-p`（或 `--print`）参数，用户可以明确告诉 Claude Code：“这是一次非交互式调用；请直接输出结果，任务完成后即退出，无需等待任何用户输入。”

## 最常见的 Headless 模式调用方式

```
claude -p "Analyze security vulnerabilities in the src/ directory"
```

在将其粘贴到流水线之前有一个注意事项：单独使用 `-p` 并不能使运行无人值守。没有权限标志时，打印模式仍然会在 Claude 首次想要读取文件或运行命令时停下来请求批准——这在 CI 中意味着作业会一直挂起直到超时。本章后面每个示例都会添加 `--allowedTools`，正是出于这个原因。

## 等价完整形式

```
claude --print "Analyze security vulnerabilities in the src/ directory"
```

然而，`-p` 只是入口点。真正将 Claude Code 转变为“可编程组件”的，是其全面的参数系统。要完全理解这个系统，首先有必要澄清交互模式与 Headless 模式之间的核心差异（见 *表 7.1*）。

| | 交互模式 | Headless 模式 |
| --- | --- | --- |
| **输入方式** | 多轮实时对话 | 单次调用，一次性传入完整指令 |
| **输出方式** | 终端 TUI，实时渲染 | 无界面，仅标准输出（stdout） |
| **权限确认** | 弹出交互式提示，需要手动确认 | 根据白名单自动跳过或执行 |
| **会话持久化** | 自动保存，支持可恢复传输（resume） | 默认不保存（可按需指定） |
| **成本控制** | 依赖人工观察和中断 | 通过 `--max-turns`/`--max-budget-usd` 进行硬限制 |
| **典型场景** | 开发调试、架构探索 | CI/CD 流水线、定时任务、批处理 |

表 7.1：交互模式与 Headless 模式的核心差异

这种演进的意义远不止“仅仅移除一个界面”。在交互模式中，人类实际上扮演了三个关键角色：权限批准者（实时决定是否允许执行特定命令）、质量检查者（立即判断输出是否符合预期）以及流程控制者（根据中间结果动态调整下一条指令）。当这三个角色都不存在时，系统必须在执行前将所有规则“硬编码”到参数中：授权调用哪些工具？允许的最大对话轮数是多少？成本预算上限是多少？下游程序必须遵循什么输出格式才能解析结果？这就是 Headless 模式庞大参数系统的核心使命：将过去依赖人类直觉和实时判断的流程，转变为一套确定性、可预测且可自动执行的工程规范。

## 核心参数系统：四个维度的控制

Kage 在白板上将参数系统画成四个控制区域，而不是一个矩阵。输出格式、成本、安全性和执行控制是四个相互独立的类别，而不是单一空间的四个象限（*图 7.1*）。

![图 7.1：Headless 模式参数系统的四个维度](img/B39394_7_1.png)

图 7.1：Headless 模式参数系统的四个维度

“看，”Kage 指着白板说，“Headless 模式的参数并不是随机或混乱的，而是可以归类为这四个维度。任何生产级的无人值守调用都必须在所有四个方面做出清晰而果断的选择。”

### 输出格式控制

`--output-format` 参数充当“协议转换器”，将 Claude Code 与下游自动化流水线连接起来。它决定输出数据的结构，并直接影响后续脚本或系统如何消费这些结果。

该参数包含三种核心格式：`text`、`json` 和 `stream-json`。

#### text 格式

最简单的纯文本输出，保留语言的自然流动，适合直接阅读或简单脚本处理。

```
claude -p "List the main issues" --output-format text
```

返回的是纯散文，没有任何包装：

主要问题集中在 `src/auth/session.ts`：

1. 会话令牌使用 `==` 进行比较，而不是常量时间比较（第 47 行）。
2. 刷新路径从不会使旧令牌失效，因此被盗令牌会一直有效（第 92 行）。
3. 登录端点没有速率限制（第 15 行）。

#### json 格式

输出一个完整、严格结构化的 JSON 对象。它不仅包含任务结果，还封装了丰富的执行元数据，是构建健壮 CI/CD 流水线的基石。

```
claude -p "Review PR" --output-format json
```

使用 `json` 格式时，返回对象包含审计和监控所需的全面信息。

```
{
  "type": "result",
  "subtype": "success",
  "session_id": "550e8400-e29b-41d4-a716-446655440000",
  "is_error": false,
  "duration_ms": 12345,
  "duration_api_ms": 10000,
  "num_turns": 5,
  "total_cost_usd": 0.0342,
  "usage": {
    "input_tokens": 5000,
    "output_tokens": 1500,
    "cache_creation_input_tokens": 0,
    "cache_read_input_tokens": 3000
  },
  "result": "Found 2 Critical issues:\n1. SQL injection risk...",
  "structured_output": null
}
```

在自动化运维和成本优化中，以下字段至关重要。

- `total_cost_usd` 可用于精确的成本跟踪。在 CI/CD 流水线中，用户可以将每次运行的成本记录到数据库中，以生成诸如“每个 PR 的 AI 审查成本”之类的报告，甚至可以自动告警或阻止超出预算的部门。
- `num_turns` 可用于监控执行轮数，反映模型交互的实际深度（与配置的 `--max-turns` 限制相比）。
- `usage.cache_read_input_tokens` 反映提示缓存的有效性。在频繁调用相同 System Prompt 或基础代码库的场景中，较高的 `cache_read_input_tokens` 值意味着跳过了更多计算，从而成本更低、速度更快。

#### stream-json 格式

逐行输出 JSON 事件（即 NDJSON 格式），专为实时监控长时间运行任务而设计。示例：

```
claude -p "Analyze code" --output-format stream-json
```

在输出中，每一行都是一个独立的 JSON 对象，并严格按时间排序。

```
{"type":"system","subtype":"init","session_id":"...",
  "model":"claude-sonnet-4-6","tools":["Read","Grep","Glob"]}
{"type":"assistant","message":{"role":"assistant",
  "content":[{"type":"text","text":"Analyzing..."}]}}
{"type":"user","message":{"role":"user","content":
  [{"type":"tool_result","tool_use_id":"...","content":"..."}]}}
{"type":"assistant","message":{"role":"assistant",
  "content":[{"type":"text","text":"Analysis complete."}]}}
{"type":"result","subtype":"success","total_cost_usd":0.03,
  "result":"Final result"}
```

请特别注意第一行的 `system/init` 事件，它会报告当前会话的模型版本、可用工具列表以及 MCP 服务器状态等初始化细节。在调试 CI 配置问题时，这一特性尤其有用。

“三种格式的选择其实很简单，”Kage 总结道。“供人阅读时使用纯文本格式，供机器解析时使用 JSON 格式，而 `stream-json` 格式则用于实时监控。”

### 成本护栏

在交互模式中，你可以随时按 *Ctrl* + *C* 中断 Claude 的执行。然而，在 Headless 模式中，由于没有“人工断路器”机制，你必须通过参数来强制执行控制。参见以下代码示例：

```
# 限制执行轮数（防止过度分析）
claude -p "Review code" --max-turns 5

# 设置硬性成本上限（USD）
claude -p "Analyze logs" --max-budget-usd 0.50

# 双重保护：同时限制轮数和成本
claude -p "Analyze the entire codebase" --max-turns 10 --max-budget-usd 2.00
```

`--max-turns` 和 `--max-budget-usd` 从不同维度进行约束。

- `--max-turns`：限制的是智能体轮次的数量——即助手在循环中自身的轮次——而非工具调用次数。Claude 可以在一个轮次内发起多次工具调用，因此轮次与工具调用并非一一对应；如果你对 `num_turns` 进行监测，会发现它的读数低于工具调用次数。
- `--max-budget-usd`：限制 API 调用的总花费。即使轮次很少（例如只有 3 轮），如果每一轮涉及大量 token（比如读取一个庞大的日志文件），累计成本仍然可能很高。在这种情况下，该参数提供了关键的保护。

这两个参数触发的是根本不同的行为。

- 当达到 `--max-turns` 时，运行会以错误状态终止，并在 JSON 输出中以子类型 `error_max_turns` 报告。可能仍会返回部分文本，但它不是一个完整的答案：请检查退出状态和结果子类型，否则脚本会将一次被截断的运行视为成功的运行。
- 当触发 `--max-budget-usd` 时，执行过程会立即被强制停止，并且不再输出任何后续结论。在 JSON 输出中，此类事件会以子类型 `error_max_budget_usd` 标记。

“在生产环境中，务必同时配置这两个参数，”Kage 强调说。“`--max-turns` 防止 Claude 在处理复杂任务时陷入无限递归，而 `--max-budget-usd` 则防止因意外的大规模分析而耗尽你的 API 预算。这两道防线缺一不可。”

### 安全边界：工具权限

在 Headless 模式下，由于工具调用没有人工确认步骤，你必须预先定义 Claude 的能力边界。这是 CI/CD 场景中最关键的安全设置。参见以下代码示例：

```
# Whitelist mode: only allow read-only tools (safest review configuration)
claude -p "Review code changes" --allowedTools "Read,Grep,Glob"

# Blacklist mode: prohibit specific tools
claude -p "Generate documentation" --disallowedTools "Bash"

# Fine-grained mode: allow specific command patterns
claude -p "Check Git history" --allowedTools "Read,Grep,Glob,Bash(git log *),Bash(git diff *)"
```

`--allowedTools`（白名单）和 `--disallowedTools`（黑名单）具有根本不同的安全语义。

- **白名单基于“封闭世界假设”**：只有明确列出的工具才被允许；其他所有工具都被拒绝。
- **黑名单基于“开放世界假设”**：除明确禁止的工具外，所有工具都被允许。

在自动化场景中，白名单始终比黑名单更安全。这是因为使用白名单时，你无需预想每一个潜在危险的工具，只需列出你实际需要的工具即可。

请注意，`--allowedTools` 支持模式匹配，并且模式中的空白字符是有意义的。`Bash(git log *)`——星号前有一个空格——只允许 `Bash` 执行以“`git log`”开头并后跟一个空格的命令。去掉那个空格，`Bash(git log*)` 也会匹配 `git log-something`；同样的陷阱会把 `Bash(git diff*)` 变成一个能匹配 `git diff-index` 的模式。在白名单中，这是一个安全漏洞，而不是笔误。请写上那个空格。

此外，还有一个更激进的参数 `--dangerously-skip-permissions`，它会跳过所有权限检查。不要忽视其名称中 dangerously 这个词；它并非装饰。在 CI 环境中，除非你在严格隔离的容器中运行，并且确信 Claude 的行为不会影响容器之外的任何事物，否则绝对不要使用这个参数。

### 执行控制：模型、提示词、结构化输出

我们可以通过模型选择、System Prompt 定制、结构化输出等参数来控制 Claude 的“思考过程”。

#### 模型选择

不同的任务需要不同的模型，你需要根据自己的需求进行权衡。参见以下代码示例：

```
# Simple format check (fast and low cost)
claude -p "Check code formatting" --model claude-haiku-4-5

# In-depth security review (using the most powerful model)
claude -p "Security vulnerability analysis" --model claude-sonnet-4-6

# Specify fallback model (automatically downgrade if the primary model is overloaded)
claude -p "Code review" --model claude-sonnet-4-6 --fallback-model haiku
```

`--fallback-model` 是一个在生产中容易被忽视但至关重要的参数。在 API 高峰期，如果主模型不可用并阻塞了 CI 流水线，所有 PR 都会陷入停滞。设置一个回退模型可以确保流水线继续运行，虽然质量会略有牺牲，但能避免工作流中断。

#### System Prompt 定制

通过定制 System Prompt 来定义 Claude 的角色和行为边界。参见以下代码示例。




# 完全替换系统提示词（这将使 Claude 忘记其作为代码助手的默认身份，仅遵循新指令）

```bash
claude -p "Analyze code" --system-prompt "You are an application security expert specializing in the OWASP Top 10."
```

# 追加系统提示词（推荐使用；保留 Claude 的核心能力，同时添加特定约束）

```bash
claude -p "Review PR" --append-system-prompt "Be sure to check for hardcoded sensitive information."
```

# 从文件加载追加的提示词（适用于具有复杂审查标准的大型项目）

```bash
claude -p "Review code" --append-system-prompt-file ./review-guidelines.txt
```

`--system-prompt` 和 `--append-system-prompt` 的关键区别在于它们如何处理 Claude 的默认行为。

- `--system-prompt`：完全覆盖 Claude 的默认 System Prompt。Claude 将不再认为自己是代码助手，也不理解如何正确使用 `Read`、`Grep` 或 `Bash` 等内置工具。它只会机械地遵循你提供的新指令，这很容易导致工具调用失败或行为失控。

- `--append-system-prompt`：将你的自定义内容追加到 Claude 默认 System Prompt 之后。Claude 保留所有核心能力，同时严格遵循你的附加指令。这适用于绝大多数 CI/CD 自动化场景。

## 结构化输出

强制 Claude 输出符合特定 JSON Schema 的数据，实现无缝的程序化集成。参见以下代码示例。

```bash
claude -p "Extract all security issues from the code" \
  --output-format json \
  --json-schema '{
    "type": "object",
    "properties": {
      "issues": {
        "type": "array",
        "items": {
          "type": "object",
          "properties": {
            "severity": {"type": "string",
              "enum": ["critical", "high", "medium", "low"]},
            "file": {"type": "string"},
            "line": {"type": "number"},
            "description": {"type": "string"}
          },
          "required": ["severity", "file", "description"]
        }
      }
    },
    "required": ["issues"]
  }'
```

使用 `--json-schema` 时，输出会根据该 schema 进行校验。若不匹配，Claude 会被重新提示，但仅限固定的重试次数——并非无限重试。如果它仍然无法生成符合要求的内容，本次运行会返回错误结果而非有效数据，因此请编写好失败路径。当校验成功时，数据会出现在 JSON 输出的 `structured_output` 字段中。

# Unix 管道：将 Claude 集成到命令行工作流中

“我注意到一个有趣的设计，”小月说。“看起来 `Claude Code` 支持标准输入（stdin）？”

“没错，”Kage 回答道。“这是 Headless 模式最优雅的功能之一：`Claude Code` 可以无缝集成到 Unix 管道中，成为你工具链中一个灵活的组件。”

Unix 哲学的核心是“每个工具做好一件事，并通过管道组合起来”。`Claude Code` 充分体现了这一设计原则。请看以下代码示例。

```bash
# 分析日志文件
cat server.log | claude -p "找出所有 500 错误并总结根本原因"

# 分析 git 变更
git diff HEAD~1 | claude -p "总结本次提交的变更，并按 Conventional Commits 规范格式化"

# 解析 API 响应
curl -s https://api.example.com/health | claude -p "判断服务是否健康"
```

更有趣的是多阶段管道组合；Claude 既能接收上游数据，也能将处理后的结果传递给下游工具。请看以下代码示例。

```bash
# Claude 分析 → jq 提取 → 发送通知
claude -p "检查安全漏洞" --output-format json | \
jq -r '.result' | \
mail -s "Security Scan Report" security@company.com

# Grep 预过滤 → Claude 深度分析
grep -r "TODO" src/ | claude -p "按优先级对这些 TODO 项进行分类"

# Claude 生成代码 → 直接写入文件
claude -p "生成一个 Express 健康检查路由" --output-format text > routes/health.js
```

在批处理中，这种管道能力将 Claude 变成了一个语义过滤器。经典文本工具——`grep`、`awk`、`sed`——非常擅长精确的模式匹配，但完全不理解含义；Claude 理解文本，但并不是为了将数 GB 的数据流经自身而构建的。将前者通过管道传给后者，你就能同时获得两者的优势。

# 实践示例 1：使用 GitHub Actions 实现自动化代码审查

理论讨论已经足够充分；接下来，让我们用一个完整的工程示例来演示。

Anthropic 提供了两种 GitHub Actions 集成选项：第一种是使用官方 Action（`anthropics/claude-code-action@v1`），封装程度高，易于配置；第二种是直接调用 `CLI`，灵活性更高，控制更精细。

## 使用官方 Action

`Claude Code` 自带一个便捷的安装命令：在交互模式下，运行 `/install-github-app` 即可引导用户完成安装 **GitHub App**、配置 **Secrets** 和创建 **Workflow** 文件的整个过程。

如果你更喜欢手动配置，步骤也很简单。首先，进入你的 GitHub 仓库，然后点击 **Settings** | **Secrets** | **Actions**，添加 **ANTHROPIC_API_KEY**，然后创建以下 Workflow 文件。

```yaml
# .github/workflows/claude-review.yml
name: Claude PR Review
on:
  pull_request:
    types: [opened, synchronize, reopened]
  issue_comment:
    types: [created]

# Automatically cancel running old jobs for new commits on the same PR to save resources
concurrency:
  group: ${{ github.workflow }}-${{ github.event.pull_request.number }}
  cancel-in-progress: true

jobs:
  review:
    runs-on: ubuntu-latest
    # Only run when PR-related events are triggered, or when the comment contains "@claude"
    if: |
      github.event_name == 'pull_request' ||
      contains(github.event.comment.body, '@claude')
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0  # Fetch the full history for diff analysis
      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: |
            Please review the code changes in this PR, focusing on the following aspects.
            1. Security vulnerabilities (such as SQL injection, XSS attacks, hardcoded sensitive information, etc.).
            2. Unhandled exceptions and edge cases.
            3. Performance risks (such as N+1 queries, memory leak risks).

            Please output the report in the following format.
            ## Review Report
            ### Critical (Must Fix)
            ### Warning (Recommended Fix)
            ### Suggestion (Optional Optimization)
          claude_args: >-
            --allowedTools "Read,Grep,Glob"
            --max-turns 10
            --model claude-sonnet-4-6
```

这个配置中有三点值得特别注意。

* **双触发模式**：官方 Action v1 版本能够自动检测触发模式。
  * **代理模式**：当配置中提供了 `prompt` 参数时触发。每次创建或更新 PR 时，Claude 都会自动执行预设的审查任务。
  * **标记模式**：当用户在 PR 评论中提及 `@claude` 时触发。在这种情况下，Claude 会像团队成员一样交互式地回应具体问题。

  这两种模式可以在同一个 Workflow 文件中共存，互不冲突。

* **参数传递机制**：官方 Action v1 版本极大地简化了配置结构。所有 `Claude Code` CLI 参数现在都统一通过 `claude_args` 字段传递。Beta 版本中此前独立的 `max_turns` 和 `allowed_tools` 等字段已被弃用，不再支持。

* **并发控制策略**：通过设置 `cancel-in-progress: true`，可以实现高效的并发控制。当连续向同一个 PR 推送多个提交时，系统会自动取消任何正在运行的旧任务，只保留由最新提交触发的审查流程。这种机制有效避免了冗余执行，并节省了 API 调用成本。

## 直接调用 CLI

当你需要实现更复杂的逻辑时，例如根据审查结果决定是否阻止 PR 合并、将审查报告发布为正式的 PR 评论而不是普通评论，或针对不同类型的文件采用差异化审查策略，直接调用 `Claude CLI` 将提供更大的灵活性。

```yaml
# .github/workflows/advanced-review.yml
name: Advanced AI Review
on:
  pull_request:
    types: [opened, synchronize]
    paths:
      - 'src/**'    # Trigger only when something under src/ changes
concurrency:
  group: ${{ github.workflow }}-${{ github.event.pull_request.number }}
  cancel-in-progress: true
jobs:
  review:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
      issues: write
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0
      - uses: actions/setup-node@v4
        with:
          node-version: "20"
      - name: Install Claude Code
        run: npm install -g @anthropic-ai/claude-code
      - name: Get changed files
        id: changed
        run: |
          FILES=$(git diff --name-only origin/${{ github.base_ref }}...HEAD)
          echo "files=$(echo "$FILES" | tr '\n' ' ')" >> $GITHUB_OUTPUT
          echo "count=$(echo "$FILES" | wc -l)" >> $GITHUB_OUTPUT
      - name: Run Claude Review
        id: review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
          CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC: "1"
          CHANGED_FILES: ${{ steps.changed.outputs.files }}
        run: |
          claude -p "Review the following changed files: $CHANGED_FILES
          Focus on security vulnerabilities, logic errors,
          and performance problems.
          Give the file name and the line number for every issue.
          End your reply with exactly one verdict line:
```



```
<VERDICT>request_changes</VERDICT> or
<VERDICT>approved</VERDICT>" \
--output-format json \
--max-turns 10 \
--max-budget-usd 0.50 \
--model claude-sonnet-4-6 \
--allowedTools "Read,Grep,Glob" > review.json

# Write the review to a file; do not interpolate it
# into a shell or JS string
jq -r '.result' review.json > review.md

echo "cost=$(jq -r '.total_cost_usd' review.json)" >>
$GITHUB_OUTPUT
echo "is_error=$(jq -r '.is_error' review.json)" >>
$GITHUB_OUTPUT
```

```
- name: Post Review Comment
  uses: actions/github-script@v7
  env:
    COST: ${{ steps.review.outputs.cost }}
  with:
    script: |
      const fs = require('fs');
      const result = fs.readFileSync('review.md', 'utf8');
      await github.rest.issues.createComment({
        issue_number: context.issue.number,
        owner: context.repo.owner,
        repo: context.repo.repo,
        body: `## Claude Code Review\n\n${result}\n\n---\n*Cost:
          $${process.env.COST} | Automated by Claude Code*`
      });
```

```
- name: Gate on critical issues
  run: |
    # Match an explicit sentinel, never a bare grep for "critical" —
    # the word appears in almost every review ("no critical issues",
    #"non-critical").
    if grep -qF "<VERDICT>request_changes</VERDICT>" review.md; then
      echo "::error::Claude requested changes"
      exit 1
    fi
```

尽管上述配置比官方 Action 更为复杂，但它为用户提供了三项关键能力：成本透明化（每次审查的具体费用直接显示在评论中）、质量门禁（当检测到 Critical 级别的严重问题时，Workflow 会自动失败并阻止 PR 合并）以及路径过滤（通过 `paths` 参数，仅在 `src/` 目录发生变更时才触发审查）。

此外，还需要关注 `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` 环境变量。这是 CI/CD 流水线中的标准最佳实践，用于在自动化场景中一次性禁用非必要功能，例如自动更新检查、错误上报以及遥测数据传输。

## 实战示例 2：多阶段 CI 流水线

*实战示例 1* 中介绍的是一个单阶段审查的案例。在实际项目中，你可能需要 Claude 参与多个阶段，例如先审查代码质量，再扫描安全漏洞，最后检查测试覆盖率。每个阶段往往有不同的关注重点、工具权限限制和成本预算。

以下是一个多阶段 AI 流水线的示例配置。

```
name: Multi-Stage AI Pipeline
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  # Stage 1: Quick Format Check (using a low-cost model)
  lint-check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Format Check
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
          CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC: "1"
        run: |
          npx @anthropic-ai/claude-code -p "Check if the code under src/ complies with project coding standards" \
            --model claude-haiku-4-5 \
            --max-turns 3 \
            --max-budget-usd 0.05 \
            --allowedTools "Read,Grep,Glob" \
            --output-format text

  # Stage 2: In-Depth Security Review (using a high-performance model)
  security-scan:
    runs-on: ubuntu-latest
    needs: lint-check  # Only runs after format check passes
    steps:
      - uses: actions/checkout@v4
      - name: Security Scan
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
          CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC: "1"
        run: |
          npx @anthropic-ai/claude-code -p "Conduct an in-depth security scan, focusing on OWASP Top 10" \
            --model claude-sonnet-4-6 \
            --append-system-prompt "You are an application security expert. Please focus only on security vulnerabilities." \
            --max-turns 10 \
            --max-budget-usd 1.00 \
            --allowedTools "Read,Grep,Glob" \
            --output-format json \
            --json-schema '{"type":"object","properties":{"vulnerabilities":{"type":"array","items":{"type":"object","properties":{"severity":{"type":"string"},"description":{"type":"string"},"file":{"type":"string"}},"required":["severity","description"]}}},"required":["vulnerabilities"]}' \
            > security-report.json

  # Stage 3: Test Coverage Analysis
  coverage-check:
    runs-on: ubuntu-latest
    needs: lint-check  # Depends on format check, but runs in parallel with security check
    steps:
      - uses: actions/checkout@v4
      - name: Coverage Analysis
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
          CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC: "1"
        run: |
          npx @anthropic-ai/claude-code -p "Analyze which code paths lack test coverage" \
            --model claude-sonnet-4-6 \
            --max-turns 8 \
            --max-budget-usd 0.50 \
            --allowedTools "Read,Grep,Glob" \
            --output-format text
```

上述配置涉及三个关键设计要点。

- **模型分层**：将模型与工作相匹配。机械且定义明确的检查（格式化、lint 分类）无法从更深入的推理中获益，因此更小、更便宜的模型（如 Haiku）就足够了；而需要处理大量上下文并进行推理的工作——安全审查、覆盖率分析——则是更大模型（如 Sonnet）能够体现其成本价值的地方。具体的模型名称会变化，但方法不会变：在两个层级上对十个真实 PR 运行同样的任务，将发现结果与人工审查的基线进行对比，让捕获率的差异来决定更大的模型是否值得其数倍的成本。

- **预算分层**：为每次运行设置 `--max-budget-usd` 上限。合适的上限并不是一个可以从书上照抄的数字——它取决于你的提示词、你的模型，以及你的仓库迫使智能体读取多少上下文。先建立基线：运行任务十次，从 JSON 输出中读取 `total_cost_usd`，并将上限设置到观察到的最大值之上。这里使用的数字（简单检查 $0.05，深度检查 $1）是我们测量得到的数量级，并非建议值。

- **并行执行**：`security-scan` 和 `coverage-check` 都设置了 `needs: lint-check`，意味着它们必须等待第一阶段完成后才能开始。由于这两个阶段之间没有依赖关系，GitHub Actions 会自动并行运行它们。

## 流式输出：实时监控长时间运行的任务

对于长时间运行的分析任务，`stream-json` 格式允许用户实时观察 Claude 的执行进度，而不必等到整个任务完成后才能查看最终结果。参见以下代码示例。

```
claude -p "Analyze the architecture problems across the whole src/ directory" \
  --output-format stream-json \
  --allowedTools "Read,Grep,Glob" | while read -r line; do
    TYPE=$(echo "$line" | jq -r '.type // ""')
    case "$TYPE" in
      "system")
        # Initialization: report which model is running
        echo "[INIT] Model: $(echo "$line" | jq -r '.model // "unknown"')"
        ;;
      "assistant")
        # Pull out the text Claude produced
        TEXT=$(echo "$line" | jq -r '.message.content[0].text // ""')
        [ -n "$TEXT" ] && echo "[CLAUDE] $TEXT"
        ;;
      "result")
        # Task finished: report the turn count and the cost
        COST=$(echo "$line" | jq -r '.total_cost_usd // 0')
        TURNS=$(echo "$line" | jq -r '.num_turns // 0')
        echo "[DONE] Finished in $TURNS turns. Cost: \$$COST"
        ;;
    esac
done
```

`stream-json` 定义了一套完整的事件协议。除了上一个示例中提到的 system（初始化）、assistant（助手响应）、user（用户输入）和 result（结果摘要）事件之外，还有两种其他特殊事件类型值得注意。

- `stream_event`（需要 `--include-partial-messages` 参数）：提供来自 Claude 的增量数据输出，可精细到每个 token 级别的文本片段。这适用于希望实现"打字机效果"的用户界面，但通常在 CI 自动化工作流中不需要启用。

- `system/compact_boundary`：上下文压缩边界事件。当对话历史接近模型的上下文窗口限制时，Claude 会自动触发一种机制来压缩较早的内容以释放空间。在长时间运行的分析任务中，如果检测到该事件，说明当前任务已消耗了大量上下文资源，你应当意识到可能存在上下文丢失的风险。

## 会话管理：跨步骤保持上下文

在 Headless 模式下，系统默认不会持久化会话状态，每次调用都被视为一个全新的上下文。然而，在涉及多轮交互的复杂场景中，往往需要在多次调用之间保持上下文的连续性。参见以下代码示例。

```
# Step 1: Perform initial analysis
# Call Claude to analyze the code structure of src/ and capture the complete JSON output
RESULT=$(claude -p "Analyze module dependencies in src/" \
  --output-format json \
  --allowedTools "Read,Grep,Glob")

# Each stage returns a JSON envelope. The fields that matter downstream:
#
# {
#   "type": "result",
#   "subtype": "success",
#   "session_id": "f3c1a9e2-...",     <- pass this to --resume to continue the same session
#   "result": "Found 3 issues in src/auth/ ...",
#   "total_cost_usd": 0.0412,
#   "num_turns": 6,
#   "is_error": false
# }
#
# A later headless run does not inherit any of this implicitly. Either feed result
# into the next prompt, or pass session_id to --resume so the model keeps its context.

# Step 2: Extract session identifier
# Parse session_id from the returned result for subsequent session restoration
SESSION_ID=$(echo "$RESULT" | jq -r '.session_id')

# Step 3: Continue the conversation based on context
# Use the --resume parameter to load the previous session state and enable continuous reasoning
claude -p "Based on the previous analysis, identify circular dependency issues" \
  --resume "$SESSION_ID" \
  --output-format text
```

`--resume` 参数的核心价值在于允许 Claude 完整"继承"上一次对话的上下文状态，包括已读取的文件、已执行的分析步骤以及已得出的中间结论。与在新提示中手动重述上下文信息相比，这种方式不仅显著提升了交互效率，还确保了模型推理的准确性和连贯性。

除了基本的会话恢复之外，CLI 工具还提供了一系列高级参数来处理复杂的交互场景。参见以下代码示例。



# Auto-continue: resume the most recent session in the current directory
# No need to manually look up the session_id; the system automatically matches and updates it
`claude -p "Continue the previous analysis" --continue`

# Session branching: create a new branch based on an existing session
# Preserve the original session unchanged, and derive a new exploration path from the specified node
`claude -p "What if we switch to a microservices architecture?" --resume "$SESSION_ID" --fork-session`

# Disable persistence: enforce stateless mode
# Suitable for fully isolated CI/CD pipelines, ensuring no session history is left behind
`claude -p "One-time analysis" --no-session-persistence`

`--fork-session` 是一个非常实用的功能，其逻辑类似于 Git 等版本控制系统中的分支机制。它从指定会话的某个特定节点"分叉"出一个全新的会话实例。原会话（主干）保持不变、不受影响；新会话（分支）从分叉点开始独立演进。该功能非常适合"假设性分析"需求。例如，开发者在完成对同一代码库的基线分析后，可以创建多个分支来探索不同的重构方向。

# CI 环境配置：生产级检查清单

到目前为止，所有内容都是关于让运行能够正常工作。接下来的内容则是关于让它能够经受住真实流水线的考验——这些环节容易被忽略，但一旦忽略代价高昂。除了 `-p` 和 `--allowedTools` 之外，生产部署还需要有意识地设置环境变量、界定权限范围，并让失败变得可见。

## 必需的环境变量

必需的环境变量如下：

```
# Authentication
ANTHROPIC_API_KEY=sk-ant-...                         # API key

# Feature disabling
# Option 1: Use an aggregate variable (recommended, can disable multiple non-essential features at once)
CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC=1       # Disable auto-updates, telemetry, and error reporting

# Option 2: Disable individually (for finer-grained control)
DISABLE_AUTOUPDATER=1                               # Disable auto-updates (essential in CI environments)
DISABLE_TELEMETRY=1                                  # Disable telemetry
DISABLE_ERROR_REPORTING=1                           # Disable error reporting
```

`DISABLE_AUTOUPDATER` 是 CI/CD 环境中的一个关键环境变量。如果未设置该变量，Claude Code 可能会在执行过程中尝试自动更新，这可能导致版本不一致或意外的服务中断。相比之下，`CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` 是一个便捷的"总开关"，可以一次性禁用所有非必要的后台流量（包括自动更新、遥测和错误报告），从而简化配置过程。

## 性能调优参数

性能调优参数如下：

```
# Timeout control
# Default timeout for Bash commands (in milliseconds)
BASH_DEFAULT_TIMEOUT_MS=120000                       # Equivalent to 2 min

# Maximum allowed timeout for Bash commands (in milliseconds)
BASH_MAX_TIMEOUT_MS=600000                           # Equivalent to 10 min

# Output control
CLAUDE_CODE_MAX_OUTPUT_TOKENS=32000              # Maximum tokens per model response (default 32,000, upper limit 64,000)
MAX_MCP_OUTPUT_TOKENS=25000                        # Maximum tokens per MCP tool response

# Context management
CLAUDE_CODE_AUTOCOMPACT_PCT_OVERRIDE=50        # Auto-compaction trigger threshold (range 1~100, represents context usage percentage; default 50%)
```

## GitHub Actions 的完整配置模板

将上述各部分整合在一起：以下模板与之前的审查任务相同，但应用了本节中的环境、权限和成本控制。这是值得直接复制使用的版本。

```
- name: Run Claude Analysis
  env:
    # Authentication key (retrieved from repository Secrets)
    ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
    # Performance and environment control
    CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC: "1"     # Disable non-essential background traffic
    BASH_DEFAULT_TIMEOUT_MS: "300000"                 # Default Bash command timeout: 5 min
  run: |
    npx @anthropic-ai/claude-code -p "$PROMPT" \
      --output-format json \
      --max-turns 10 \
      --max-budget-usd 1.00 \
      --allowedTools "Read,Grep,Glob" \
      --model claude-sonnet-4-6 \
      --fallback-model claude-haiku-4-5 \
      --append-system-prompt "Follow the review guidelines in the CLAUDE.md file at the project root."
```

## CI 中的 MCP 服务器配置

如果 CI 任务需要利用 MCP 服务器能力（例如数据库查询、外部 API 调用等），您可以通过 `--mcp-config` 参数加载专用配置文件，并使用 `--strict-mcp-config` 确保环境隔离。请参阅以下代码示例。

```
npx @anthropic-ai/claude-code \
-p "Analyze optimization opportunities in the database schema" \
--mcp-config ./ci-mcp-config.json \
--strict-mcp-config \
--allowedTools "Read,Grep,Glob,mcp__database__query" \
--max-turns 10
```

`--strict-mcp-config` 参数启用严格模式，强制 Claude 仅使用 `--mcp-config` 指定的配置文件。它会忽略用户主目录或项目根目录中的任何其他 MCP 配置。这在 CI 环境中非常重要，因为它可以防止开发人员无意中加载残留的本地 MCP 配置。

# 跨平台 CI/CD 集成

尽管 GitHub Actions 有官方 Action 支持，但 Claude Code 的无头模式本质上与平台无关。只要运行时环境支持 Node.js，它就可以部署在任何 CI/CD 平台上。

## GitLab CI/CD 配置

以下配置演示了如何在 GitLab Runner 中执行代码审查任务并以结构化格式输出结果。

```
# .gitlab-ci.yml
claude-review:
  image: node:20
  variables:
    ANTHROPIC_API_KEY: $ANTHROPIC_API_KEY
    CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC: "1"
  script:
    - npm install -g @anthropic-ai/claude-code
    - |
      claude -p "Review the code changes in this MR" \
        --output-format json \
        --max-turns 10 \
        --max-budget-usd 0.50 \
        --allowedTools "Read,Grep,Glob" > review.json
    - cat review.json | jq -r '.result'
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
```

关于 GitLab 的特别说明：由于上述配置是通用 CI，Anthropic 和 GitLab 一直在深化集成：GitLab 现在提供官方 AI 功能（测试版），并带有专用变量如 `AI_FLOW_INPUT`、`AI_FLOW_CONTEXT` 和 `AI_FLOW_EVENT`，因此提示上下文不再需要手动组装，并且 `gitlab-mcp-server` 通过 MCP（`mcp__gitlab` 工具）向模型暴露 GitLab API。Anthropic 的 GitLab CI/CD 指南记录了当前设置——请查阅该指南而非本页，因为变量名称仍在变动。

## Jenkins 流水线集成配置

在 Jenkins 中，可以使用声明式流水线轻松集成 Claude Code。以下配置演示了如何安全地管理凭据、执行无头审查并解析结构化输出。

```
pipeline {
  agent any
  environment {
    ANTHROPIC_API_KEY = credentials('anthropic-api-key')
    CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC = '1'
  }
  stages {
    stage('AI Code Review') {
      steps {
        sh 'npm install -g @anthropic-ai/claude-code'
        sh '''
        claude -p "Review the most recent code changes, focusing on logical errors and code standards" \
        --output-format json \
        --max-turns 10 \
        --max-budget-usd 0.50 \
        --allowedTools "Read,Grep,Glob" > review.json
        '''
        script {
          def review = readJSON file: 'review.json'
          echo "Review result: ${review.result}"
          echo "Cost: \$${review.total_cost_usd}"
        }
      }
    }
  }
}
```

## 本地自动化脚本

除了 CI/CD 平台之外，Claude Code 的无头模式也非常适合打包为本地脚本，使团队成员能够在提交代码前进行自检，或作为本地自动化工具链的一部分。

以下是 `review.sh` 脚本的增强版本，具备环境检查、动态文件过滤、结构化报告生成和错误处理能力。

```
#!/bin/bash
# review.sh , A Local Code Review Script for Team Sharing
set -e
TARGET=${1:-.}

# Pre-checks
[ -z "$ANTHROPIC_API_KEY" ] && echo "Error: ANTHROPIC_API_KEY not set" && exit 1
command -v claude &> /dev/null || { echo "Error: Claude Code not installed"; exit 1; }
echo "Starting review for: $TARGET"

# Build file list
if [ -d "$TARGET" ]; then
FILES=$(find "$TARGET" -type f \( -name "*.ts" -o -name "*.js" -o -name "*.py" \) | head -20)
else
FILES="$TARGET"
fi

# Run the review. Note: RESULT=$(claude -p ...) would hide a failure even under
# `set -e`, because command substitution masks the inner command's exit code.
if ! claude -p "Review the code quality and security issues in the following files:
$FILES
Output format: a Markdown review report. Give the file name and line number for every issue." \
    --output-format text \
    --max-turns 15 \
    --max-budget-usd 0.50 \
    --allowedTools Read,Grep,Glob > /tmp/review-out.txt 2> /tmp/review-err.txt; then
    cat /tmp/review-err.txt >&2
    exit 1
fi
RESULT=$(cat /tmp/review-out.txt)

# Save the report
REPORT="review-$(date +%Y%m%d-%H%M%S).md"
{
    echo "# Code Review Report"
    echo
    echo "**Date**: $(date)"
    echo "**Target**: $TARGET"
    echo
    echo "---"
    echo
    echo "$RESULT"
} > "$REPORT"
echo "Report saved: $REPORT"
```

# 安全最佳实践：从 RBAC 到容器隔离

"*在 CI/CD 中运行 AI Agent 是一件必须认真对待的安全事项。*"Kage 的语气变得更加严肃。

这不是危言耸听。一个配置不当的无头任务可能导致以下严重风险：

* **机密泄露**：Agent 读取 `.env`、`config.json` 或硬编码的凭据文件并将其打印出来。这不是假设性场景，也不需要恶意提示——一个被要求"检查配置"的审查 Agent 会很乐意照做。最终出现在 CI 日志中的内容看起来是这样的：

```
$ claude -p "Review the configuration for problems" --allowedTools "Read,Grep"
Reading .env ...
The configuration has a few problems. First, the production database
credentials are committed to the repository:
  DATABASE_URL=postgres://admin:S3cr3t!prod@db.internal:5432/orders
  STRIPE_SECRET_KEY=sk_live_51H8xQ2...
These should be moved to a secret store.
```

* **破坏性操作**：执行恶意 shell 命令，破坏构建环境或覆盖代码。



**成本失控**：陷入无限循环或被诱导生成大量无意义内容，导致 API 调用费用过高。

**内容污染**：在 PR/MR 评论中输出幻觉内容、不当言论，甚至注入攻击载荷。

防御策略必须在以下四个层面构建纵深防御体系。

## 最小权限原则

在实际场景中，可以采取以下措施。

```
# Code review tasks: read-only
--allowedTools "Read,Grep,Glob"

# Format fixing tasks: read and write
--allowedTools "Read,Grep,Glob,Edit,Write"

# Git operation tasks: fine-grained Bash whitelist
--allowedTools "Read,Grep,Glob,Bash(git diff *),Bash(git log *)"
```

绝不要授予 Claude 不受限制的 Bash 执行权限。即使在需要 shell 能力的场景中，也必须通过模式匹配严格限制可执行命令的范围。

## 机密管理

对于机密管理，以下对比了正确和错误的做法。

```
# Incorrect practice: hardcoding keys (the key will appear in plain text in the Actions run log)
env:
ANTHROPIC_API_KEY: "sk-ant-xxx"

# Correct practice: referencing repository secrets
env:
ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
```

# 安全防御体系

## GitHub Actions 密钥掩码机制

**GitHub Actions** 会自动掩盖日志中引用的密钥值（显示为 `***`）。

尽管 GitHub 拥有日志掩码机制，但如果你在提示词中指示模型 `list all environment variables` 或 `print configuration details`，模型可能会在其输出文本中重复密钥的具体值。此类模型生成的文本无法被自动掩码机制捕获，从而导致密钥泄露。

切勿在提示词中指示模型访问、读取或输出任何环境变量或密钥值。

## 容器隔离

以下展示了一个 **GitHub Actions** 工作流片段，用于构建一个高安全性的“沙箱”环境以运行代码审查任务。

```yaml
jobs:
  review:
    runs-on: ubuntu-latest
    container:
      image: node:20
      options: --read-only --tmpfs /tmp --network none
```

- `--read-only`：将文件系统设置为只读以防止写操作（配合 `--tmpfs /tmp` 为临时文件提供可写空间）。
- `--network none`：完全切断网络连接，适用于不需要外部访问的审查任务。在此模式下，Claude 只能读取仓库内的文件，无法与外界通信。

## 成本保护

以下展示了 GitHub Actions 中用于优化资源使用和降低运营成本的工作流配置片段。

```yaml
# 路径过滤：仅当 src 目录下的文件变更时触发
on:
  pull_request:
    paths:
      - 'src/**'
      - '!src/**/*.test.*'  # 排除测试文件

# 并发限制：仅保留同一 PR 的最新运行，自动取消中间版本
concurrency:
  group: claude-${{ github.event.pull_request.number }}
  cancel-in-progress: true
```

路径过滤和并发控制是成本优化的第一道防线。与限制单次运行的资源消耗相比，从源头阻止不必要的触发是降低总成本更有效的方式。

## 审计日志

以下展示了执行 AI 任务并持久化保存包含完整审计信息的运行日志的配置片段。

```bash
# JSON 输出格式天然包含完整审计信息
claude -p "task" --output-format json | tee -a /var/log/claude-audit.jsonl
```

每次无头模式调用的 JSON 输出自动包含 `session_id`、`total_cost_usd`、`num_turns` 和 `usage` 等关键审计字段。在有严格合规要求的环境中，建议实时将这些日志推送到中心化日志系统（如 ELK、Datadog 等）进行长期归档和深度分析。

**Kage's notes**

这五层防御不是“选修题”，而是生产环境中的“必修题”：最小权限原则防止 Claude“做不该做的事”，密钥管理严防凭证泄露，容器隔离阻断逃逸风险，成本保护避免账单失控，审计日志保障全链路可追溯。缺失任何一层都意味着安全防线上的致命缺口。

# 从 CLI 到 Agent SDK：无头模式的编程接口

前面的示例均通过 CLI 调用 Claude Code，但在实际工程实践中，Claude Code 还提供了 Agent SDK（支持 TypeScript 和 Python），允许开发者直接使用编程语言控制无头执行流程，而非依赖命令行参数。

以下是 TypeScript Agent SDK 和 Python Agent SDK 的示例。

以下代码演示了如何使用 Agent SDK 构建一个进程内 MCP 服务器，将两个特定业务功能（数据库查询和发送通知）封装为安全工具供 AI Agent 调用。

```typescript
// TypeScript Agent SDK Example
import { query } from "@anthropic-ai/claude-agent-sdk";

async function reviewPR(changedFiles: string[]) {
    let sessionId: string | undefined;
    let result: string = "";

    for await (const message of query({
        prompt: `Review the code changes in the following files:
            ${changedFiles.join(", ")}`,
        options: {
            allowedTools: ["Read", "Grep", "Glob"],
            maxTurns: 10,
            maxBudgetUsd: 0.50,
            model: "claude-sonnet-4-6",
            outputFormat: { type: "json_schema", schema: reviewSchema },
        },
    })) {
        if (message.type === "system" && message.subtype === "init") {
            sessionId = message.session_id;
        }
        if (message.type === "result" && message.structured_output) {
            result = message.structured_output;
        }
    }

    return { sessionId, result };
}
```

```python
# Python Agent SDK Example
from claude_agent_sdk import query, ClaudeAgentOptions, ResultMessage

async def review_pr(changed_files: list[str]):
    options = ClaudeAgentOptions(
        allowed_tools=["Read", "Grep", "Glob"],
        max_turns=10,
        max_budget_usd=0.50,
        model="claude-sonnet-4-6",
    )

    async for message in query(
        prompt=f"Review the following files: {', '.join(changed_files)}",
        options=options,
    ):
        if isinstance(message, ResultMessage):
            return message.result
```

CLI 和 Agent SDK 的关系类似于 curl 与 HTTP 客户端库的区别：CLI 以轻量便捷著称，适用于简单自动化脚本和 CI/CD 流水线；而 Agent SDK 专为复杂逻辑场景设计。无论是根据上一轮评审结果动态调整后续提示词、跨越多次 Claude 调用编排复杂工作流，还是将 Claude 深度集成到大型应用系统中，Agent SDK 都能提供所需的编程灵活性。在 Chapter 8 中，我们将深入探讨 Agent SDK 的具体用法。

# 渐进式实施策略

> “After all these technical details,” Xiaobing asked, “if our team wants to introduce Headless mode for PR reviews, how should we proceed step by step?”

Kage 建议了四阶段渐进式实施路径。

## Phase One: Observer Mode（观察者模式）

Claude 仅执行只读分析，输出评审报告但不阻塞任何流程。评审报告以评论形式发布在 PR 上，供团队成员参考或忽略。该阶段核心目标是建立信任，让团队直观体验 Claude 评审质量，同时识别其局限性（如易误报场景），并据此迭代优化提示词与参数配置。

## Phase Two: Advisor Mode（顾问模式）

积累足够正向反馈后，将 Claude 评审结果纳入 CI 状态监控。检测到关键问题时，CI 状态标记为 `warning`（黄色），但不阻塞合并。人工评审者需结合 Claude 报告做最终裁决。该阶段核心目标是人机协作。

## Phase Three: Gatekeeper Mode（守门人模式）

Claude 评审成为代码合并的必要条件。一旦检测到关键安全问题，CI 状态即刻转红并阻断合并。但此阶段必须提供 `escape hatch`（逃生通道），允许特定权限人员手动覆盖 Claude 的阻断决策（类似跳过不稳定测试用例）。

## Phase Four: Proactive Repair Mode（主动修复模式）

Claude 不仅发现问题，还能自动修复格式错误、补充缺失异常处理逻辑、生成单元测试。该阶段需开启 **Write tool permissions**，但所有修复必须作为新提交提交，**严禁直接修改 PR author's original code（PR 作者原始代码）**。

分阶段推进的原因非关耐心箴言，而是每个阶段都会产出下一阶段所需的证据。在拥有数周评审输出前，你无法区分真实发现与误报，也无依据决定 Agent 应被允许阻断什么。请让每个阶段运行足够久以积累证据——通常需数周——并让 dismissal rate（驳回率）而非日历决定何时推进。

# Summary

**Headless mode（无头模式）** 推动了 Claude Code 角色的转型：从仅限终端交互的智能助手，进化为可无缝嵌入各类工程工作流的可编程组件。这一转型由四个维度的参数定义。

- `-p` (Entry Point)：将 Claude Code 运行模式从交互式会话切换为非交互式执行，奠定自动化基础
- `--output-format` (Data Interface)：标准化输出结构，保障下游系统能高效解析、消费评审结果
- `--allowedTools` (Security Boundary)：严格限制工具权限，确保自动化环境下的操作合规可控
- `--max-turns` 与 `--max-budget-usd` (Cost Guardrails)：设定执行轮次与预算上限，防止无人值守场景下的资源失控

在工程架构层面，Headless mode 最大价值在于赋予 Claude Code **“first-class citizen in the CI/CD ecosystem”（CI/CD 生态系统中的一等公民）** 地位。正如单元测试、静态分析、容器镜像构建，AI 代码评审正式成为流水线标准步骤：它需要明确的输入定义、确定性的输出格式、可量化的成本预算、以及可纳入版本控制的配置文件。

实施策略上，建议从 `Observer Mode` 起步，逐步演进至 `Gatekeeper Mode`。这一渐进过程旨在给团队留出充足时间建立信任、积累实战数据、优化配置。切记勿急躁：AI 辅助评审的核心价值不在于追求 `perfect replacement of humans（完美替代人类）`，而在于以智能筛选让人工评审者将宝贵精力聚焦于最关键的决策环节。

在 Chapter 8 中，我们将从命令行参数迈入编程接口（Agent SDK）的世界。你将学习如何用 TypeScript 或 Python 代码替代繁琐的 **CLI parameters**，灵活控制 Claude 行为逻辑，从而构建真正定制化、高度集成的 AI 应用系统。

# Discussion Questions

1. Your team wants to add Claude Code PR review to a pipeline that already has required status checks. Should the review be advisory or blocking, and how would you decide when to promote it? What should Claude own that a linter cannot, and what do you do about false positives?

2. Under what circumstances would `--max-turns 5` and `--max-budget-usd 0.50` be triggered first? When designing Headless tasks for production environments, which metric should you prioritize? Please explain your reasoning.

3. Take the CI you already run — and if you have not chosen one yet, take the one your code is already hosted next to; a review job is not worth a platform migration. Write the workflow file: the trigger, the permissions, the tool whitelist, and the two ceilings (`--max-turns` and `--max-budget-usd`). Then say which of those two you expect to trip first, and why.
