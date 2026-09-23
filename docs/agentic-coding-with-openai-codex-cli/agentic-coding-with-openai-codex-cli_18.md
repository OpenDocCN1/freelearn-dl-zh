# Testing and Evaluation Strategy for Agentic Workflows (智能体工作流的测试与评估策略)

Every chapter in this book has referred to running the test suite as the primary feedback mechanism for Codex CLI. That assumption is only valid if your test suite is a reliable signal. Tests that are slow, flaky, or network-dependent mislead the agent: a spurious failure sends Codex CLI chasing the wrong problem. The quality of Codex CLI's output is bounded by the quality of your test suite.

本书的每一章都提到运行测试套件作为 Codex CLI 的主要反馈机制。这一假设仅在你的测试套件是可靠信号时才成立。缓慢、不稳定或依赖网络的测试会误导智能体：虚假的失败会让 Codex CLI 去追查错误的问题。Codex CLI 输出的质量受限于你的测试套件的质量。

This chapter shows you how to build a test suite that works as a reliable feedback signal, how to audit the one you have, and how to evaluate agentic workflows beyond unit tests.

本章将展示如何构建一个作为可靠反馈信号的测试套件，如何审计现有的测试套件，以及如何在单元测试之外评估智能体工作流。

The stakes are higher than they appear. When a single developer can prompt an agent to generate thousands of lines in a session, 10 to 100 times what they would write manually, bugs in the output are statistically guaranteed.^[175] The industry's instinct to solve this by stacking more agents (agent-written tests, agent reviewers, agent evaluators) does not eliminate the problem; it distributes it. Imperfect systems evaluating imperfect systems do not converge on perfection. The test suite is the one artefact that provides a mechanically verifiable signal, independent of whether a human or an agent wrote the code under test.

风险比表面看起来更高。当一名开发者可以在一个会话中提示智能体生成数千行代码，这是手动编写量的 10 到 100 倍时，输出中存在 Bug 在统计学上是必然的。^[175] 行业通过堆叠更多智能体（智能体编写测试、智能体评审、智能体评估）来解决此问题的本能并不能消除问题；它只是分散了问题。不完美的系统评估不完美的系统不会收敛于完美。测试套件是唯一能提供机械可验证信号的工件，独立于被测代码是由人类还是智能体编写的。

### Learning Objectives (学习目标)

You will be able to:

您将能够：

1.  Identify and remediate common test suite problems that cause false positives in agentic execution
    识别并修复导致智能体执行中出现误报的常见测试套件问题
2.  Design a single gate command that combines lint, types, and tests into a reliable quality signal
    设计一个单一的门禁命令，将 lint、类型检查和测试结合为一个可靠的质量信号
3.  Build a minimal evaluation harness for grading agent output against a task specification
    构建一个最小化的评估工具，根据任务规范对智能体输出进行评分
4.  Integrate agent evaluation into a CI pipeline
    将智能体评估集成到 CI 流水线中

### What Makes a Test Suite Agent-Friendly (什么使测试套件适合智能体)

On top of the qualities any good test suite has, an agent-friendly one needs to be hermetic, deterministic, and single-command invocable.

除了优秀测试套件应有的素质外，适合智能体的测试套件还需要是**密封的（hermetic）**、**确定性的（deterministic）** 和 **可单命令调用的（single-command invocable）**。

#### Hermetic (密封性)

A hermetic test has no external dependencies: no live API calls, no real database connections. An agent running in a sandbox may lack network access, so tests that make HTTP calls fail because the network is blocked, not because the code is wrong. Mock external calls at the test boundary: use `msw` (Node.js), `responses` (Python), or `httptest` (Go) for HTTP; use in-memory implementations or transaction rollbacks for databases.

密封测试没有外部依赖：没有实时 API 调用，没有真实的数据库连接。在沙箱中运行的智能体可能缺乏网络访问权限，因此进行 HTTP 调用的测试会因为网络被阻塞而失败，而不是因为代码错误。在测试边界模拟外部调用：HTTP 使用 `msw` (Node.js)、`responses` (Python) 或 `httptest` (Go)；数据库使用内存实现或事务回滚。

#### Deterministic (确定性)

Flaky tests cause agents to chase false failures, or worse, to produce 'fixes' that silence flakiness without addressing the underlying problem. Common sources are time-dependent assertions (use a fixed test date), ordering assumptions on database rows, shared global state without teardown, `sleep()` calls for async timing, and unfixed random seeds. Every test must produce the same result regardless of when or in what order it runs.

不稳定的测试会导致智能体追查虚假失败，甚至产生“修复”来掩盖不稳定性而不解决根本问题。常见来源包括：时间相关的断言（使用固定测试日期）、数据库行顺序假设、无清理的共享全局状态、用于异步时序的 `sleep()` 调用、以及未固定的随机种子。每个测试必须无论何时运行、无论以何种顺序运行，都产生相同的结果。

#### Single-Command Invocable (单命令调用)

An agent task description that says 'run the tests' needs to map to a single command with a deterministic exit code. Zero means pass; non-zero means fail. No interactive prompts, no progress bars that confuse output parsing, no test runners that require pre-flight setup.

智能体任务描述中的“运行测试”需要映射到一个具有确定性退出码的单一命令。零表示通过；非零表示失败。无交互提示、无干扰输出解析的进度条、无需预设置的测试运行器。

```
##### Good: one command, clear exit code
# 好：单命令，清晰退出码
npm test
pytest
make test
go test ./...
##### Bad: requires setup before execution
# 差：执行前需要设置
npm install && npm test
source .env.test && pytest
```

If your test suite requires pre-flight setup, encode it in a Makefile target or a wrapper script. The agent's interface should be a single clean command.

如果你的测试套件需要预设置，请将其编码在 Makefile 目标或包装脚本中。智能体的接口应是一个单一、干净的命令。

Parallel test execution is generally fine for agents: faster is better. The issue is non-determinism, not parallelism. A parallel test run that always produces the same result is agent-friendly; a sequential test run that sometimes flips between pass and fail is not.

并行测试执行通常对智能体没问题：越快越好。问题在于非确定性，而非并行性。始终产生相同结果的并行测试运行是适合智能体的；有时在通过和失败之间翻转的顺序测试运行则不是。

### Designing for Agent Execution (为智能体执行设计)

Several patterns make test suites specifically easier for agents to interact with.

有几种模式使测试套件专门更易于智能体交互。

**See also:** Chapter 9 **(Hooks).** A `PostToolUse` hook that runs `pytest -q --tb=line` after every file write turns the test suite into an active feedback signal during the session. The two patterns are complementary: hooks provide the trigger, this chapter provides the test suite design that makes the trigger useful.

**另见：** 第 9 章 **(钩子)。** 在每次文件写入后运行 `pytest -q --tb=line` 的 `PostToolUse` 钩子，将测试套件变为会话期间的主动反馈信号。这两种模式是互补的：钩子提供触发器，本章提供使触发器有用的测试套件设计。

#### Seed Fixtures and Mock Services (种子固件与模拟服务)

Test suites for agent execution should replace live services with:

面向智能体执行的测试套件应使用以下方式替换实时服务：

**Seeded Fixtures. (种子固件。)** A deterministic dataset loaded at test suite startup and reset between runs:

在测试套件启动时加载并在运行之间重置的确定性数据集：

```python
##### pytest: a session-scoped fixture that seeds the database once
# pytest：一个会话级固件，为数据库播种一次
@pytest.fixture(scope="session", autouse=True)
def seed_database(test_db):
    test_db.execute_file("tests/fixtures/seed.sql")
    yield
    test_db.rollback()
```

**In-Memory Services. (内存服务。)** SQLite in-memory mode for SQL stores, dict-backed mocks for key-value stores, in-process queues. These are faster than real services, require no network, and fail loudly when the code does something unsupported, surfacing integration bugs early.

SQL 存储使用 SQLite 内存模式，键值存储使用字典支持的模拟，使用进程内队列。这些比真实服务更快，不需要网络，且当代码做不支持的操作时会大声报错，从而尽早暴露集成 Bug。

#### Test Isolation Patterns (测试隔离模式)

Each test must run in any order, start from a known state, and leave no side effects. Three patterns enforce this:

每个测试必须能按任何顺序运行、从已知状态开始、且不留副作用。三种模式强制执行此规则：

**Database Transaction Rollback. (数据库事务回滚。)** Wrap each test in a transaction rolled back at the end. Significantly faster than truncating and re-seeding tables, and completely isolation-safe.

将每个测试包裹在一个最后回滚的事务中。比截断并重新播种表快得多，且完全隔离安全。

**Environment Variable Reset. (环境变量重置。)** If tests modify environment variables (which they sometimes must), restore them in teardown:

如果测试修改环境变量（有时必须），请在拆卸阶段恢复它们：

```javascript
// jest: save and restore environment variables
// jest：保存和恢复环境变量
let originalEnv;
beforeEach(() => { originalEnv = { ...process.env }; });
afterEach(() => { process.env = originalEnv; });
```

**Module State Reset. (模块状态重置。)** For modules with side-effectful initialisation (connection pools, singletons), use your test framework's module reset mechanism to get fresh state for each test.

对于具有副作用初始化的模块（连接池、单例），使用测试框架的模块重置机制为每个测试获取全新状态。

#### Environment Variable Injection for Agents (面向智能体的环境变量注入)

Agents need to run tests against a controlled environment, not the production database or live API keys. Environment variable injection handles this, but it must be automatic.

智能体需要针对受控环境运行测试，而不是生产数据库或实时 API 密钥。环境变量注入可以处理这个问题，但必须是自动的。

In your `AGENTS.md`:

在你的 `AGENTS.md` 中：





#### 运行测试

始终使用 `npm test` 运行测试。不要针对生产数据库运行测试。
调用 `npm test` 时，测试配置会自动从 `.env.test` 加载。
除非任务特别要求，否则不要修改 `.env.test`。

在 `package.json` 中：

```json
{
 "scripts": {
 "test": "dotenv -e .env.test -- jest"
 }
}
```

代理运行 `npm test` 并获得正确的行为，而无需了解 `.env.test`。

### 反馈信号问题

“测试通过”是一种代理指标，而非证明。分支覆盖率比行覆盖率更好的代理指标：它能捕捉到代码虽执行但从未走过错误分支的代理 Bug。一组实用的 CI 阈值：

```json
// jest: 在 package.json 中
"jest": {
 "coverageThreshold": {
 "global": {
 "lines": 85,
 "branches": 75,
 "functions": 90
 }
 }
}
```

#### 门控命令模式

将 lint（代码检查）、类型检查和测试合并为一个具有明确通过/失败退出码的单一门控命令：

```makefile
##### Makefile
gate:
 @echo "Running lint..."
 @npm run lint || (echo "GATE FAILED: lint" && exit 1)
 @echo "Running type check..."
 @npm run typecheck || (echo "GATE FAILED: types" && exit 1)
 @echo "Running tests..."
 @npm test || (echo "GATE FAILED: tests" && exit 1)
 @echo "GATE PASSED"
```

代理运行 `make gate`。失败的步骤会产生非零退出码和带标签的错误消息。全部通过则产生“GATE PASSED”且退出码为 0。

如果您的门控命令耗时超过五分钟，请考虑慢速步骤是否可以并行运行。将 lint 和类型检查与测试套件并行运行的门控命令，可以在不改变退出码语义的情况下，将时钟墙时间缩减 40% 到 60%。请使用 `parallel` 或您的 CI 提供商的步骤并行性，而不是引入代理可见的复杂性。

### 使用 Codex CLI 审计您的测试套件

#### 要询问的内容

一个有用的审计提示词：

```
Review the test suite in the tests/ directory.
Identify any tests that:
- Make HTTP calls to external services (without mocking)
- Use sleep() or setTimeout to wait for async operations
- Access the filesystem outside of a designated test fixtures directory
- Modify module-level or global state without cleanup
- Depend on test execution order (e.g., assume a previous test created data)
- Use hardcoded absolute paths
For each issue found, provide: the file, the line, a one-sentence description of the problem, and a suggested fix.
Do not modify any files. Produce a report only.
```

在 `-a untrusted -s read-only` 模式下运行此命令。Codex CLI 可靠地发现：无 Mock 的直接 HTTP 调用、硬编码的绝对路径、`sleep()` 调用（导致测试不稳定的最常见原因）、未清理即修改的全局状态，以及测试间依赖（引用前一个测试创建的 ID 的测试）。

#### 使审计具有可操作性

审计生成问题列表后，按影响优先级排序：

1.  **网络调用。** 首先修复这些：它们是沙箱失败的最常见原因。

2.  **绝对路径。** 其次修复这些：即使本地通过，它们在 CI 上也会失败。

3.  `sleep()` **调用。** 有时间时修复这些：它们导致不稳定，但通常不会导致彻底失败。

4.  **全局状态。** 问题显现时修复这些：它们较难完全识别，且影响测试顺序，通常在 CI 中而非本地运行时暴露。

### 单元测试之外的评估

单元测试不适合评估代理输出：它们仅涵盖功能正确性，而非输出质量，且无法判断“正确性”取决于判断的长程任务。

#### 代理工作流的集成测试策略

在代理循环期间运行快速单元测试套件。在生成的 PR 上运行单元和集成套件。集成测试验证代理更改端到端生效；使用带有真实但已播种服务的 Docker Compose 堆栈，将其与代理开发期间运行的快速单元测试分离开。

#### 面向代理的基于属性的测试

基于属性的测试验证对任何有效输入都应成立的不变量。对于代理输出，它们特别有价值，因为它们测试结构正确性，而无需测试作者预测每个具体输入：

```python
##### 一个属性：解析并序列化文档应具有幂等性
@given(st.text(alphabet=st.characters(whitelist_categories=('L', 'N', 'P'))))
def test_parse_serialise_roundtrip(content):
 parsed = parse_document(content)
 serialised = serialise_document(parsed)
 re_parsed = parse_document(serialised)
 assert parsed == re_parsed
```

如果代理修改了 `parse_document` 或 `serialise_document`，此属性测试会捕捉测试作者从未考虑过的输入的往返不变量违规。基于属性的测试比基于示例的测试更难编写，但作为结构性变更的反馈信号，其威力大得多。

#### 长程任务的基于采样的评估

对于“正确性”取决于判断的任务（代码质量、架构决策、文档清晰度），评估方法是采样：使用不同的随机种子多次运行任务并对输出进行评分。

评分可以是：

1.  **自动：** 对每个输出运行 lint/类型/测试门控。各样本的通过率即为得分。

2.  **LLM 作评委：** 使用单独的模型调用根据评分标准评估输出。这引入了自身的偏见，但扩展性好。

3.  **人工审查：** 抽取少量输出进行人工评分。这成本高但准确。

对于 CI 目的，在长程任务中预期存在一定输出方差的情况下，以 80% 到 90% 的门控通过率（M 个样本中有 N 个通过所有检查）作为实用阈值。

### 构建评估工具链

最小化的评估工具链运行代理执行任务，捕获输出并对其评分。这是 SWE-bench 风格评估背后的基础设施，为您自己的代码库构建它是切实可行的。

#### 最小工具链的结构

```
eval/
├── tasks/
│ ├── task-001/
│ │ ├── prompt.txt # 任务描述
│ │ ├── setup.sh # 设置脚本（克隆仓库、播种数据）
│ │ ├── teardown.sh # 清理脚本
│ │ └── grade.sh # 评分脚本（返回 0/1）
├── run_eval.sh # 编排设置、代理、评分
└── results/ # 评估运行的输出
```

`run_eval.sh` 脚本：

```bash
#!/bin/bash
TASK_DIR="eval/tasks/$1"
RESULTS_DIR="eval/results/$1/$(date +%Y%m%dT%H%M%S)"
mkdir -p "$RESULTS_DIR"
##### Setup
bash "$TASK_DIR/setup.sh" > "$RESULTS_DIR/setup.log" 2>&1
##### Run agent
codex exec "$(cat "$TASK_DIR/prompt.txt")" \
 --json > "$RESULTS_DIR/agent.json" 2>"$RESULTS_DIR/agent.log"
##### Grade
bash "$TASK_DIR/grade.sh" > "$RESULTS_DIR/grade.log" 2>&1
GRADE=$?
##### Teardown
bash "$TASK_DIR/teardown.sh" > "$RESULTS_DIR/teardown.log" 2>&1
echo "$GRADE" > "$RESULTS_DIR/score.txt"
echo "Task $1: $([ $GRADE -eq 0 ] && echo PASS || echo FAIL)"
exit $GRADE
```

代码任务的 `grade.sh` 脚本通常运行测试套件并返回其退出码。对于文档任务，它可能运行链接检查器、拼写检查器或 LLM 评估。

#### 连接到 SWE-bench 风格评估

SWE-bench 是针对真实 GitHub 问题评估编码代理的标准基准。^[¹⁷⁶] 上述结构刻意与 SWE-bench 的任务格式（提示词、设置、清理、评分）兼容，因此您可以将您的配置与已发布的基线进行比较。

对于大多数团队，自定义内部基准（任务取自您自己的积压工作）比公共基准更有信息量，后者衡量的是在公共仓库上的表现，而非您特定的代码库和约定。

#### CI 集成

**表 19-1. 测试策略对比**

| **策略** | **速度** | **覆盖范围** | **代理友好度** | **用例** |
| :--- | :--- | :--- | :--- | :--- |
| 单元测试 | 快 (<2min) | 函数级 | 高 | 代理反馈循环 |





| 基于属性的测试 || 中等 (2-5 分钟) || 不变量级 || 高 || 结构正确性 |
| 集成测试 || 慢 (5-30 分钟) || 系统级 || 中等 || 合并前检查 |
| 评估套件 || 可变 || 任务级 || 低 || 基准测试、回归测试 |
| LLM-as-judge (LLM 作为评判者) || 快 (API) || 质量级 || 中等 || 长时程任务 |

推荐的 CI 集成策略：

1. **每次 Agent 提交时：** 运行单元测试。快速检查点；Agent 循环依赖于此。
2. **每次 PR 时：** 运行单元测试 + 集成测试 + 类型检查 + 代码规范检查。人工审查前的全面检查点。
3. **每晚：** 在具有代表性的任务集上运行评估套件。随时间跟踪通过率。
4. **模型变更前：** 运行完整的评估套件。通过率下降表明 Agent 智能能力出现回归。

不要将评估套件纳入 Agent 的反馈循环。评估套件用于从外部衡量 Agent 的能力。如果你给 Agent 访问评估套件的权限，它可能会优化输出以通过套件，而不是解决底层任务，这是古德哈特定律在 Agent 系统中的一种体现。

### TDD 作为 Agent 反馈循环

当你在实现前编写测试时，`Codex CLI` 获得了一个二进制的、可执行的规范：一个明确无误的“完成”定义，它可以在每一步验证。没有预先编写的测试，`Codex CLI` 只能从文本说明中猜测“完成”意味着什么。

#### 为 Codex 构建失败测试

失败的测试必须满足相同的标准（封闭性、确定性、单命令可调用），外加一个：它们必须在断言层面失败，而不是在导入层面。一个因 `ImportError` 而失败的测试会让 `Codex CLI` 追查错误的问题。在移交前确保导入成功：

```
##### 好：在断言处干净地失败，带有清晰信息
def test_renewal_creates_subscription_record():
    result = renew_subscription(user_id=42, plan="pro")
    assert result.status == "active"
    assert result.user_id == 42
    # NameError 或 ImportError 会让 Codex 走错方向

##### 有问题：在到达测试逻辑前的导入处失败
from src.billing.renewal import renew_subscription # raises ImportError
def test_renewal_creates_subscription_record():
    ...
```

使用条件导入或存根，使 `Codex CLI` 看到 `NotImplementedError` 或失败的断言，而不是导入链失败：

```
try:
    from src.billing.renewal import renew_subscription
except ImportError:
    def renew_subscription(*args, **kwargs):
        raise NotImplementedError("not yet implemented")
```

#### AGENTS.md TDD 约定

要使 `Codex CLI` 自动遵循 TDD，将其编码在 `AGENTS.md` 中：

```
#### 开发约定：TDD
实现任何功能时：
1. 在编写任何代码前，阅读 `tests/` 中该模块的测试
2. 运行 `make test-unit MODULE=<module>` 确认当前有哪些测试失败
3. 在 `src/` 中编写实现代码以使这些测试通过
4. **不要**修改测试以使其通过；只修改源代码
5. 当所有目标测试通过时，运行完整测试套件以检查回归
```

关键约束是第 4 步：`Codex CLI` 不得通过削弱断言来使测试通过。通过代码审查强制执行此项，并将其设为 `AGENTS.md` 的显式规则。“不要修改测试”是你能编码的最有价值的约束之一。

#### 面向 Bug 修复的测试优先

对于 Bug 修复：编写一个复现 Bug 的测试，验证其失败，然后移交：“修复此 Bug。`tests/test_billing.py::test_duplicate_charge` 中的测试可复现它。在不修改测试的情况下使测试通过。”这比文本描述有效得多，并自动创建了回归测试。

### 面向长时程评估的 4 文件持久化记忆模式

长时程任务跨越许多文件、许多决策和多个会话，需要可设置检查点的状态。4 文件模式使用 `.codex/` 中的四个文件，从 `AGENTS.md` 引用，以便每个会话自动加载它们：

| **文件** || **用途** |
| --- | --- | --- |
| `PLAN.md` || 分解的任务规范：需要做什么以及顺序 |
| `PROGRESS.md` || 已完成步骤、当前状态、阻碍项 |
| `DISCOVERIES.md` || 意外发现、API 怪癖、代码库惊喜 |
| `DECISIONS.md` || 选择背后的理由（语言、库、算法） |

```
<!-- AGENTS.md -->
#### 长时程任务管理
- 阅读 PLAN.md 了解任务规范
- 每完成一步后更新 PROGRESS.md
- 在 DISCOVERIES.md 中记录意外发现
- 在 DECISIONS.md 中记录架构选择
绝不跳过 PROGRESS.md 更新。它是下一会话的恢复点。
```

`PROGRESS.md` 提供可设置检查点的状态：如果会话意外结束，新会话将精确地从上一个会话停止的地方继续。`DECISIONS.md` 提供可审计的决策轨迹。当长时程任务出错时，这四个文件共同构成了取证记录。

长时程任务的评分应同时考虑功能结果（测试通过）和过程质量（`PROGRESS.md` 已更新、`DISCOVERIES.md` 已填充）。一个通过测试但未遵循计划的 Agent 并非可靠有能力；过程评分能捕捉到这一点。

### Agent 测试质量：过度模拟问题与审查基准

两项研究发现提供了关于 Agent 生成测试质量和 Agent 驱动代码审查的硬性数据，两者对 `Codex CLI` 配置都有直接影响。

#### 过度模拟问题 (MSR 2026)

一项被 MSR 2026 接收的实证研究分析了跨 2,168 个仓库的 120 多万次提交（包括 48,563 次 Agent 提交），发现编码 Agent 在 36% 的提交中添加了模拟对象，而人类为 26%。^[177] Agent 修改测试文件的频率也高得多，在 23% 的提交中修改测试，而人类为 13%，因此模拟差距在整个代码库中累积，而不是孤立在少数文件中。

更令人担忧的是类型集中度。人类开发者会使用一系列测试替身——模拟对象、伪对象、间谍和虚设对象——而 Agent 严重依赖模拟对象，极少使用替代方案。当 Agent 为所有情况默认使用 `jest.fn()` 或 `unittest.mock.patch` 时，会创建出验证交互模式（“你调用这个函数了吗？”）而非行为正确性的测试。

**为什么这很重要：** 一个由模拟对象主导的测试套件倾向于确认函数按预期顺序被调用，而不是它们表现正确，因此它可能保持绿色（通过），而真实的回归却溜走了。测试文件越依赖模拟对象而非伪对象或真实边界，其信号就越弱。

#### 为现实测试配置 Codex CLI

三个配置层使 Agent 生成的测试保持现实：

**AGENTS.md 模拟策略：**

```
#### 测试标准 — 模拟策略
- **模拟**：外部 HTTP API、第三方服务、系统时钟、随机生成器
- **伪造**：数据库（使用 testcontainers 或 SQLite 内存数据库）、消息队列、缓存
- **间谍**：事件发射器、日志记录、指标。验证调用而不替换行为
- **绝不模拟**：纯函数、值对象、你自己的领域逻辑
- 每个文件的模拟比例必须保持在 50% 以下。如果测试文件的模拟设置多于断言，重构以使用伪对象或集成边界。
```

**用于强制执行模拟比例的 PostToolUse 钩子：**

```
#!/bin/bash
##### ~/.codex/hooks/check-mock-ratio.sh
TOOL=$(cat | jq -r '.tool_name')
FILE=$(cat | jq -r '.args.file_path // empty')
if [[ "$TOOL" == "write_file" && "$FILE" == *test* ]]; then
    MOCK_COUNT=$(grep -c "mock\|jest\.fn\|patch\|MagicMock" "$FILE" 2>/dev/null || echo 0)
    ASSERT_COUNT=$(grep -c "assert\|expect\|should" "$FILE" 2>/dev/null || echo 0)
    if [ "$MOCK_COUNT" -gt "$ASSERT_COUNT" ]; then
        echo "WARNING: Mock count ($MOCK_COUNT) exceeds assertion count ($ASSERT_COUNT) in $FILE"
    fi
fi
```

**门控命令中的集成测试强制执行：**





```gate:
 @npm run test:unit
 @npm run test:integration # Ensures at least some tests hit real boundaries
 @echo "GATE PASSED"
```

#### 代码审查智能体基准测试

代码审查智能体基准测试于 2026 年发布，是一项面向代码审查智能体的可执行基准测试。^(¹⁷⁸) 其核心发现令人警醒：参与测试的审查智能体（包括开源的 PR-agent 以及 Devin、Claude Code 和 Codex 的商业智能体）合计仅能解决约 40% 的任务，留下大约 60% 的问题未被检出。智能体间的重叠率很低：每个智能体倾向于发现不同的问题。

参测工具在**稳健性和边界情况**（不安全的索引、空值检查、边界条件）上得分最高，但在**可维护性和设计关注点**（API 设计、抽象边界、命名一致性）上最弱。

**提升审查质量的配置：** 向 `AGENTS.md` 添加显式的审查清单：

```
#### 代码审查清单
审查代码时（通过 /review 或 @codex review），请检查：
1. 逻辑错误：差一错误、条件反转、缺少返回值
2. 错误处理：未处理的异常、被吞没的错误、缺少重试机制
3. 安全性：注入攻击、认证绕过、代码中的密钉、不安全的反序列化
4. 可维护性：命名不清、嵌套过深、缺少抽象
5. 测试覆盖：边界情况是否测试？新路径是否覆盖？
6. 文档：公共 API 是否有文档注释？破坏性变更是否已标注？
```

添加显式的审查清单正是 c-CRAB 指出的关键杠杆：清单指导型审查在不降低智能体原本擅长类别表现的前提下，缩小了可维护性方面的差距。

### 评估框架：CocoaBench、HiL-Bench 和 AAR

三项基准测试提供了企业团队可直接采用的评估框架：不是作为学术练习，而是作为智能体工作流中测试覆盖率缺口的具体清单（详见*第 26 章*的完整基准分析）。

#### 将故障分类法用作测试覆盖率清单

最具可操作性的收获是一份源自故障分类法的测试覆盖率清单。大多数团队广泛测试工具使用，但基准数据表明工具使用很少是瓶颈：它仅占智能体故障的一小部分。主要的故障模式却鲜有测试：

**导航测试（占故障的 27% 至 52%）。** 你的评估套件是否包含要求智能体在代码库陌生区域查找文件的任务？大多数内部基准测试的是智能体见过的文件。增加要求智能体导航至其常规工作区之外的任务。如果智能体在这些任务上失败，说明你的 `AGENTS.md` 文件映射（*第 4 章*）需要扩充。

**求助判断测试。** 你的评估套件是否包含正确行动是请求澄清而非继续执行的任务？大多数测试套件奖励完成、惩罚不完整，导致团队在本应使用 Suggest（建议）模式的任务上使用 Full Auto（全自动）模式。增加模棱两可的任务，并对智能体识别并上报不确定性的行为给予正向评分。

**多模态组合测试（约 55% 的任务失败）。** 你的评估套件是否包含跨越多种模态的任务：代码、文档、测试和配置？大多数评估任务是单模态的。增加要求跨所有模态进行协调变更的任务，并按跨模态一致性评分。

#### CocoaBench 作为评估框架

CocoaBench 的任务结构（基于跨模态一致性评分的多模态组合任务）可直接适配于内部评估。如果你的团队维护着如前所述的评估框架，请增加一类 CocoaBench 风格的任务：智能体必须在单一连贯操作中修改代码、更新对应测试并修订相关文档。按三个输出间的一致性评分，而非仅按任一输出的功能正确性评分。

#### HiL-Bench 用于审批模式校准

HiL-Bench 提供了一种校准审批模式的原则性方法。在 Full Auto 和 Suggest 模式下运行模棱两可的任务。如果 Suggest 模式因智能体能适当升级而产生显著更好的结果，说明你的 Full Auto 配置正在损害质量。采用 HiL-Bench 的评分方法论——衡量求助与非求助表现之间的差距——作为审批模式决策的量化输入。

#### AAR 用于评估 AGENTS.md 有效性

AAR 研究的导航准确率指标（基线 37.2%）^(¹⁷⁹) 为你的 `AGENTS.md` 文件映射效果提供了基准。运行导航密集型任务（查找特定函数、定位模块的测试、识别服务的配置文件），分别在有和没有文件映射章节的情况下进行。准确率的提升直接衡量了你的文件映射的价值。

### 测试 MCP 服务器

MCP 服务器是任何使用它们的 Codex CLI 工作流的关键依赖，它们需要独立的测试策略。*第 8 章* 涵盖了隔离测试 MCP 服务器的内容。本章的关键原则：将 MCP 服务器故障视同数据库故障。它们会悄无声息地降低输出质量，因为智能体失去了对其预期拥有的数据的访问权，转而给出貌似合理但错误的答案，而不是抛出可见的错误。

在 CI 流水线中加入 MCP 服务器健康检查。一个简单的集成测试即可满足大多数团队需求：启动每个配置的 MCP 服务器，验证工具注册，用已知输入调用每个工具，并检查关闭流程。在任何智能体级评估之前运行这些测试，以确保基础设施可靠。

### 总结

1.  **三个属性定义了智能体友好的测试套件。** 封闭性（无外部依赖）、确定性（每次运行结果相同）和单命令可调用。不稳定测试对智能体的危害尤大，因为智能体无法区分真实故障与噪音。

2.  **隔离每个测试。** 使用种子固定装置、内存服务、数据库事务回滚和环境变量注入，使测试具有封闭性且独立于运行顺序。

3.  **TDD 映射到智能体反馈循环。** 先编写失败的测试，再交给 Codex CLI 并指示“使这些测试通过且不修改测试文件”。预先编写的失败测试给 Codex CLI 一个明确的、可执行的“完成定义”。

4.  **在 AGENTS.md 中编码 TDD。** 将其作为团队约定，并加上“严禁修改测试”的显式约束；这是你能添加的最有价值的护栏之一。

5.  **“测试通过”只是代理指标，而非证明。** 将 lint、类型检查和测试合并为单一门禁命令。使用分支覆盖率阈值作为比行覆盖率更有意义的代理指标。

6.  **信任前先审计测试套件。** 部署为智能体反馈信号前，用 Codex CLI 本身检查套件。它能可靠地发现测试中的网络调用、硬编码绝对路径、`sleep()` 调用和全局状态变更。

7.  **4 文件持久化记忆模式承载长时段状态。** `PLAN.md`、`PROGRESS.md`、`DISCOVERIES.md` 和 `DECISIONS.md` 提供可检查点的进度、可审计的决策轨迹，以及跨会话的评估任务可外部恢复的状态。

8.  **超越单元测试以保障质量。** 集成测试覆盖系统级验证，基于属性的测试覆盖结构不变量，基于采样的评估覆盖具有定性输出的长时段任务。

9.  **最小评估框架随时间追踪能力。** 提示词、设置、评分和拆卸提供了衡量智能体能力并在模型或配置变更时捕获回归的基础设施。

### 练习




#### 1. 审核你的测试套件

从“使用 Codex CLI 审核你的测试套件”部分运行 Codex CLI 审核提示词，针对你自己项目的测试套件，在 `untrusted` / `read-only`（不受信/只读）模式下运行。收集生成的问题列表。按对智能体可靠性的预估影响程度，优先处理前三个问题并修复它们。重新运行审核，验证这些问题不再出现。

#### 2. 构建一个最小化的评估框架

从你项目的近期待办事项中挑选至少三个任务来构建一个框架。为每个任务编写一个 `grade.sh`，运行测试套件并返回退出代码。通过 `codex exec` 在框架中运行每个任务并记录通过/失败结果。然后更换活动模型（例如，从 `gpt-5.5` 切换到 `gpt-5.4-mini`）并重新运行。记录哪些任务在不同模型间显示出不同的结果，并利用这些数据来指导你的模型路由决策（参见第 6 章）。

#### 3. 将 TDD 应用于 Bug 修复

选择你当前项目中的一个真实 Bug，一个目前在生产环境或功能分支上失败的 Bug。在编写任何修复代码之前，先编写一个能复现该 Bug 的测试。验证该测试在当前代码下会失败。然后移交给 Codex CLI：“修复这个 Bug。`<file>::<test_name>` 中的测试能复现它。在不修改测试文件的情况下让测试通过。”审查修复方案，并确认测试通过。接着运行完整的测试套件以检查是否有回归。记录该 Bug 是否在后续的任何提交中复发，以及该测试是否捕获到了它。

#### 4. 实现 4 文件持久化记忆模式

为你的项目选择一个中等复杂度的功能，该功能至少需要更改三个文件，且如果手动实现实际上需要两小时或更长时间。创建 `.codex/` 目录并编写 `PLAN.md`，将该功能分解为至少五个具体步骤，包含明确的文件路径和验证标准。添加 `AGENTS.md` 约定，指示 Codex CLI 在每个步骤后更新 `PROGRESS.md`。运行 Codex CLI 会话完成前两步，然后中断它。确认 `PROGRESS.md` 准确反映了已完成的工作。然后用一个新会话恢复，并验证它能从上一会话停止的地方继续进行。

# 第 5 部分

# 实践指南



