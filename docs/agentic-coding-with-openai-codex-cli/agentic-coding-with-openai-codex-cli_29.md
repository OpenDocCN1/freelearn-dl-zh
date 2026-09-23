# 第二十二章：后端工程

前几章处理的是已有代码：审查、迁移、现代化。接下来三章从零构建并一直延伸到生产环境。本章编写后端服务及其 API，*第 23 章* 为其添加前端，*第 24 章* 使用基础设施即代码部署整个系统。一个贯穿始终的示例，一个核心问题：当后端、前端和基础设施各自发挥代理不同的优势和劣势时，如何让 Codex CLI 提供帮助？每章针对各自层面给出答案；合在一起，展示了同一个代理如何适应三种截然不同的工作。

示例刻意保持适度，并带点自娱自乐的意味。它是一块从 Audley End 到 Liverpool Street 的发车牌，即本书开篇提到的 07:02 列车路线。询问我是可以悠闲出门还是必须冲出门，它会给出答案：给定当前时间和时刻表，返回未来几班发车信息及裁决，`RUN` 或 `relax`。它足够小以装进脑海，又足够真实以锻炼让后端工作对代理而言棘手的一切：数据模型、带棘手边界的业务逻辑（何时算太晚出发？）、测试，以及 *第 23 章* 前端将依赖、*第 24 章* 部署将托管的 HTTP 契约。

我们使用 Python 和 FastAPI 构建后端，主线贯穿全书：问题不是“如何编写 FastAPI 服务”，而是“如何配置 Codex CLI 使其按团队方式编写服务”。Python 让这个问题更尖锐，因为它给代理提供的反馈循环比编译型静态类型语言更弱：构建时能捕获的错误迟至运行时或代码审查时才浮现。本章大部分配置旨在弥补这一差距，并在过程中构建 Run or Relax API。

### 学习目标

您将能够：

1.  编写完整的 Python `AGENTS.md`，消除最常见的“工具选型错误”

2.  配置钩子，在每次文件写入后强制执行代码检查，在会话结束前运行完整测试套件

3.  遵循本章构建端到端的 Run or Relax 服务：带有经过测试裁决规则的类型化 FastAPI 端点，准备好供前端消费

4.  将 `pyproject.toml` 配置为 uv、ruff 和 mypy 工具链的单一事实来源，使生成的代码在首次尝试时即通过代码检查和类型检查

5.  识别导致代理出错的 Python 特有失败模式，并为每种模式配置防御措施

### 我们要构建的服务

在进行任何配置之前，详细说明我们要构建的内容，因为本章每个配方都为此服务。目标塑造设计，在交给代理之前使其显性化很有价值。裁决是核心，因此位于一个小巧、纯净、可测试的函数中，而非埋在请求处理器里：它接收时间、计划发车时间和延误，返回两个词之一。规则本身有两个棘手边界。“太晚无法悠闲”不是零分钟，而是从家门走到车站所需时间；延误会推迟发车，从而为您争取时间。“太晚无法奔跑”是第二个更严格的阈值：即使冲刺也赶不上的车次根本不应出现在发车牌上。时刻表和延误是数据而非代码，因此存放在测试可控的种子 JSON 文件中。服务仅暴露一个只读端点，因为发车牌只读。

契约即这一个端点：

```
GET /departures?station=audley-end&now=<ISO 8601, optional, defaults to now>
200 OK
[
  {"service": "07:02", "platform": "2", "delayMinutes": 0,
   "minutesUntil": 10, "verdict": "RUN"},
  {"service": "07:32", "platform": "2", "delayMinutes": 4,
   "minutesUntil": 44, "verdict": "relax"}
]
```





### 合同里的两个决策

合同里隐藏着两个决策，文本将其全部明确，这样智能体就不用猜测了。延误会将实际发车时间往后推，因此一列计划时间已过的列车，只要其延误后的发车时间距离现在足够远以供奔赴，仍应保留在列表中：按延误后的实际发车时间而非计划时间进行筛选，并剔除距离小于奔赴阈值的车次。而 `minutesUntil` 也是从同一延误后的发车时间开始计算，这样读者看到的数字就与他们实际能赶上的那趟列车相符。

#### 计划项目布局

以下是计划的项目布局，下一节的 `AGENTS.md` 会向智能体描述该布局：

```
run-or-relax/
├── pyproject.toml
├── AGENTS.md
├── .codex/
│   ├── hooks.json          # PostToolUse, Stop, UserPromptSubmit 钩子
│   └── scripts/
│       └── block-legacy-pip.sh
├── data/
│   ├── timetable.json      # 每站的种子发车数据
│   └── delays.json         # 模拟延误数据源
├── src/departures/
│   ├── models.py           # Pydantic 响应模型
│   ├── timetable.py        # 加载时刻表 + 延误，计算下一班发车
│   ├── verdict.py          # RUN/relax 规则（纯函数）
│   └── app.py              # FastAPI 应用与 /departures 路由
└── tests/
    ├── conftest.py         # 时刻表固件
    ├── test_verdict.py
    ├── test_timetable.py   # 时刻表 + 延误源行为
    └── test_api.py
```

`data/` 下的两个文件是运行时种子数据，服务从检出目录运行时会从项目根目录读取它们，而非作为打包进 wheel 的应用资产。这对教学服务尚可接受，但若不加说明，这类决策部署后会悄无声息地导致故障，所以现在就给它定性：第 24 章 决定这些文件如何、或是否随部署服务一同分发。

#### 响应结构

响应结构是一个 Pydantic 模型，它是第 23 章 前端将要镜像的合同的单一事实来源：

```python
#### src/departures/models.py
from typing import Literal
from pydantic import BaseModel, ConfigDict
from pydantic.alias_generators import to_camel

class Departure(BaseModel):
    model_config = ConfigDict(alias_generator=to_camel, populate_by_name=True)
    service: str          # 计划时间，例如 "07:02"
    platform: str
    delay_minutes: int
    minutes_until: int
    verdict: Literal["RUN", "relax"]
```

别名生成器会输出 camelCase JSON（`delayMinutes`、`minutesUntil`），使合同在第 23 章 的 TypeScript 端读起来自然，而 Python 端保持 snake_case。`Literal["RUN", "relax"]` 也在暗中发挥作用：它是前端将要切换的同一个双状态联合类型，因此第三种判定永远不可能意外泄露出 API。

你可以直通本章构建此服务。`AGENTS.md` 部分编写智能体遵循的规则；pytest 与类型检查部分产出判定逻辑及其测试；工具链部分锁定环境；最后一部分组装 FastAPI 路由并运行。最终你将获得一个在 `localhost` 上响应 `GET /departures` 的服务，已为第 23 章 的前端和第 24 章的部署做好准备。

### 为什么 Python 对智能体来说很难

在给出配置方案前，先说明为什么 Python 比静态类型语言需要更多方案。智能体真正的反馈回路是那组能快速、自动地告诉它“上一次修改是否正确”的信号。Python 给智能体的回路比 TypeScript 或 Rust 弱，本章大部分配置都是为了强化它。语言及其生态系统的五个特性几乎解释了所有麻烦。

#### 错误晚暴露

**错误晚暴露。** 在编译型静态类型语言中，智能体能获得快速、免费的正确性信号：构建失败。Python 将大多数此类信号推迟到运行时，或代码审查时，除非你接入了 `mypy` 或 `ty`。看不见类型错误的智能体会自信地把错误发布出去。对策是将类型检查纳入回路而非事后诸葛亮：在完成门禁中加入严格类型检查，让智能体像面对编译器一样感受到失败。

#### 环境是环境变量且会漂移

**环境是环境变量且会漂移。** Python 根据当前活动环境中碰巧安装的包来解析导入，而智能体的会话、CI 与各开发者的笔记本环境各不相同。在环境中运行 `pip install` 的智能体写出的代码在自己的会话能跑，在别人那里却会坏。对策是单一的、有锁文件支撑的环境，所有调用都经过它，这也是本章将一切通过 `uv run` 路由的根本原因。

#### 导入时副作用隐藏行为

**导入时副作用隐藏行为。** Python 在导入时执行代码：模块级常量、装饰器、注册表注册，有时甚至包括网络或文件系统访问。孤立推理某个函数的智能体看不见仅因导入模块就触发的效果，因而编写的测试和重构会悄悄改变启动行为。对策是智能体可遵循的约定，记录在 `AGENTS.md` 中：保持模块导入时无副作用，将副作用推到函数背后，把模块顶层视为新工作的禁区。

#### Mock 太容易

**Mock 太容易。** 动态类型让打补丁变得轻而易举，追求快速绿灯的智能体会 `unittest.mock.patch` 整个世界，而不是构建真实的固件。结果就是第 19 章 记载的过度模拟模式：测试通过却什么也没证明。Python 是这种失效模式最严重的地方，这正是为什么模拟策略专门要写在 Python 的 `AGENTS.md` 里。

#### Async 有锋利且无声的边缘

**Async 有锋利且无声的边缘。** `async` 函数里的阻塞调用不会报错；它会卡住事件循环，症状表现为负载下的延迟，远离病因。不了解你并发模型的智能体会以通过所有测试却在生产环境退化的方式混用阻塞与异步代码。对策是在 `AGENTS.md` 中制定显式的异步约定，并在同步与异步代码的边界上加强审查。

透过这个透镜阅读本章余下内容。每个方案并非随意的代码风格；它堵住了这五个缺口之一，把智能体看不见的特性变成它无法忽视的信号。

### Python 专用 AGENTS.md

搞对 `AGENTS.md` 是 Python 项目杠杆率最高的配置决策。没有它，Codex CLI 会做出看似合理实则错误的选择：用 `pip install` 而非 `uv add`，用 `python -m pytest` 而非 `uv run pytest`，用 `setup.cfg` 或 `.flake8` 而你的项目用的是 `pyproject.toml`。每个错误单独看都能恢复，但累积起来会侵蚀信任并造成环境漂移。

根目录的 `AGENTS.md` 应回答四个问题：用哪个包管理器、用哪条测试命令、用哪条 lint 与格式化命令、什么算作完成任务。





#### AGENTS.md

##### 环境

- Python 包管理器：`uv`，切勿直接使用 `pip`
- 所有工具调用必须通过 `uv run`（例如：`uv run pytest`、`uv run ruff`）
- 添加依赖：`uv add <package>`；开发依赖：`uv add --dev <package>`
- Python 版本：3.14（参见 `.python-version` 或 `pyproject.toml`）

##### 测试

- 运行测试：`uv run pytest tests/ -v`
- 运行单个测试文件：`uv run pytest tests/test_foo.py -v`
- 带覆盖率运行：`uv run pytest --cov=src --cov-report=term-missing`
- 所有新代码必须有 pytest 覆盖率，无例外

##### 代码检查与格式化

- 代码检查：`uv run ruff check . --fix`
- 格式化：`uv run ruff format .`
- 类型检查：`uv run mypy src/`（稳定的默认选择；`ty` 离开 beta 并加入开发依赖后可用 `uv run ty check`）
- 标记任务完成前运行所有检查：代码检查 → 格式化 → 类型检查 → 测试

##### 提交约定

- 规范化提交：`feat:`、`fix:`、`refactor:`、`test:`、`docs:`
- 除非依赖版本被有意更新，否则不要提交 `uv.lock` 变更

此处的特定工具——`uv`、`ruff`、`mypy`、`pytest`——是本书的示例技术栈，而非强制规定。其价值在于明确团队实际使用的工具，以便代理停止猜测：替换为 `poetry` 或 `hatch`、`black`、`pyright` 或 `unittest`，文件的结构完全相同。这正是 `AGENTS.md` 的意义：它适配你的环境和技术栈，而本章其余部分恰好使用上述技术栈。

“在标记任务完成前”这句话至关重要。没有它，代理满足字面提示后就会停止，而不会验证任何东西是否工作。有了它，Codex CLI 就将代码检查、格式化、类型检查和测试内化为完成的定义。

表 22-1 总结了关键配置字段及其背后的理由。

##### 表 22-1. Python AGENTS.md 配置参考

| 字段 | 推荐值 | 重要性 |
| --- | --- | --- |
| 包管理器 | `uv` | 从 `uv.lock` 解析并安装；消除开发者与 CI 之间的环境漂移 |
| 测试命令 | `uv run pytest tests/ -v` | 通过 uv 管理的环境路由；`-v` 显示单个测试名称，便于诊断 |
| 覆盖率命令 | `uv run pytest --cov=src --cov-report=term-missing --cov-fail-under=90` | 硬性阈值防止覆盖率回归；`term-missing` 内联显示未覆盖的行 |
| 代码检查命令 | `uv run ruff check . --fix` | 自动修复消除机械性审查评论；`--fix` 仅应用安全转换 |
| 格式化命令 | `uv run ruff format .` | 替代 Black；相同的确定性输出，在大型代码库上亚秒级完成 |
| 类型检查 | `uv run mypy src/`（或 `uv run ty check`） | `mypy` 是稳定的默认选择，也是完成门控运行的工具；`ty` 是 Astral 基于 Rust 的检查器，写作时处于 beta 阶段 |
| 完成门控 | 代码检查、格式化、类型检查、测试 | 显式顺序防止部分完成 |
| 提交风格 | 规范化提交 | 启用自动变更日志生成和语义化版本控制 |

##### 随服务增长的嵌套覆盖

目前 Run or Relax 是单一服务。随着增长，假设出现一个单独的 worker 轮询真实延迟源而非种子源，根 `AGENTS.md` 保留共享约定，而嵌套文件承载差异。Codex CLI 按作用域顺序合并它们，深层文件优先。

```
#### src/delay_feed/AGENTS.md
##### 服务特定覆盖
- 这是一个后台 worker，而非 HTTP 服务：此处无路由处理器
- 它写入 `data/delays.json`；绝不可触及 `src/departures/`
- 集成测试轮询存根源：`uv run pytest tests/integration/ -m integration`
- 仅单元测试：`uv run pytest tests/unit/`
```

保持嵌套文件专注于差异。在每个文件中重复完整工具链会在根约定变更时产生漂移。

在 Codex CLI 中运行 `/init` 可根据实际项目结构生成初始 `AGENTS.md` 脚手架，再将其与你编写的文件对比。生成的初始文件在测试路径、工具配置或目录上与你的不同之处，即是 Codex 会以不同方式推断的约定，你在会话中期被咬之前就捕捉到了。

### Pytest 集成与测试生成

Pytest 是事实上的 Python 测试运行器，但其约定对未被告知 fixture 位置或标记工作方式的代理来说并不明显。在 `AGENTS.md` 中编码这些细节，区别在于产出语法有效但无用的测试的测试编写器，还是能与现有套件集成的测试编写器。

#### Fixture 发现

最常见的错误是 Codex CLI 创建了一个已存在于 `conftest.py` 中的 fixture。一条指令即可消除它：

```
#### pytest 约定
- 共享 fixture 在 `tests/conftest.py`：创建新 fixture 前请先检查那里
- `tests/conftest.py` 中的 `timetable` fixture 加载种子 JSON；请复用它，而不是在测试中读取文件或调用时钟
- 服务级 fixture 位于 `tests/<service>/conftest.py`
- 使用 pytest-asyncio 并设置 `asyncio_mode = "auto"`：无需手动添加 `@pytest.mark.asyncio`
- HTTP 客户端 fixture 使用带 ASGI 传输的 `httpx.AsyncClient`，而非 `requests`
```

使用 FastAPI 或任何异步框架的团队通常在 `pyproject.toml` 中设置此项：

```
[tool.pytest.ini_options]
testpaths = ["tests"]
asyncio_mode = "auto"
addopts = "--tb=short -q"
```

如果没有在 `AGENTS.md` 中注明这一点，Codex CLI 会给每个异步测试添加 `@pytest.mark.asyncio`，产生一个看似正确但违反项目约定的差异。

#### 覆盖率阈值与测试标记

指定覆盖率基线和测试类别：

```
##### 覆盖率
- 最低覆盖率：总体 90%，`src/departures/verdict.py` 为 100%（规则绝不能回归）
- 不要在无注释解释的情况下添加 `# pragma: no cover`
##### 测试标记
- @pytest.mark.unit：无 I/O，每次推送在 CI 运行
- @pytest.mark.integration：需要 Docker 服务，仅在 PR 时在 CI 运行
- @pytest.mark.slow：长时间运行；本地用 `-m "not slow"` 跳过
- 默认本地命令：`uv run pytest -m "unit" -q`
```

有了这些指令，Codex CLI 在生成新测试文件时会应用正确的标记。被意外标记为 `unit` 的集成测试会在每次推送时引入数据库调用，造成那种侵蚀 CI 信心的缓慢、易变的失败。

##### 自动化测试扩展

Codex CLI 最可靠的 pytest 贡献是为现有代码扩展覆盖率。当 `AGENTS.md` 精确时，像“为 `src/departures/timetable.py` 中尚无覆盖的每个公共函数添加 pytest 测试，使用项目的 `timetable` fixture”这样的提示能产生良好结果。代理读取 `tests/conftest.py` 获取 fixture，检查 `pyproject.toml` 获取标记定义，并生成能融入现有结构的测试。

大规模测试生成可能产生立即通过但断言错误的测试。它们验证当前行为可复现，而非行为正确。审查生成的测试要像审查生成的源代码一样仔细。一个在 `None` 是 Bug 而非 Feature 时断言 `result == None` 的测试，比没有测试更糟。

### 类型提示与文档字符串自动化

Python 的类型提示生态系统在 3.10 和 3.12 版本中成熟。语法更简洁（`str | None` 而非 `Optional[str]`）[¹⁹⁶]，工具更快（`ty` 和 `mypy`）[¹⁹⁷] 严格模式下），交付类型化代码的团队报告生产环境运行时错误更少。Codex CLI 可加速类型提示采用，但前提是有一致的风格指南。

#### AGENTS.md 中的类型风格





#### 类型提示

- 使用 Python 3.10+ 的联合类型语法：`str | None`，而非 `Optional[str]`
- 在每个模块顶部使用 `from __future__ import annotations` 以支持前向引用
- 所有公开函数必须具有完整的类型注解（参数和返回类型）
- 对于领域概念，优先使用 `TypeAlias` 和 `NewType` 而非裸类型别名
- 在函数间传递的结构化字典使用 `TypedDict`
- 除非有注释解释原因，否则不要使用 `Any`

严格执行禁止裸 `Any` 的规定。Codex CLI 在不确定类型时默认使用 `Any`，而这正是显式声明能产生最大价值的场景。

#### 文档字符串生成

针对文档字符串的 `AGENTS.md` 指令应指定格式、必需章节和范围：

```
#### 文档字符串
- 格式：Google 风格（Args、Returns、Raises 章节）
- 所有公开函数、方法和类都需要文档字符串
- 每个公开模块也需要单行模块文档字符串：Ruff 的 `D` 规则集包含 D100
- 包含单行摘要，然后空一行，如有需要再添加详细说明
- `Args` 章节：每个参数一条，包含类型和描述
- `Raises` 章节：仅针对调用者预期处理的异常
```

模块文档字符串行很容易被遗漏。在 Ruff 中启用 `D` 规则集后，首次对 `app.py`、`models.py`、`timetable.py` 和 `verdict.py` 运行 lint 时，会因 `D100 Missing docstring in public module` 报错，直到每个文件都添加了简短的模块文档字符串，因此最好预先声明，而不是一次次通过 `--fix` 发现。

有了这个设置，像“为 `src/departures/timetable.py` 中缺少文档字符串的所有公开函数添加 Google 风格文档字符串”这样的提示词，会生成一致的输出，且无需手动编辑即可通过文档字符串风格检查（`pydocstyle` 或 Ruff 的 `D` 规则集）。

### uv、ruff 与现代 Python 工具链

现代 Python 工具链将历史上分离的工具合并为两个：

1.  **uv** 替代了 pip、venv、pyenv、pipx 和 poetry。它由 Rust 编写，能在毫秒级解析依赖，并生成固定完整依赖图的 `uv.lock` 文件。^[198]
2.  **Ruff** 替代了 flake8、Black、isort、pyupgrade 以及 900 多条 lint 规则背后的插件。同样基于 Rust，在大型代码库上能在亚秒级完成运行。^[199]

一条原则贯穿整个工具链：每次工具调用都通过 `uv run`。这保证了在任何工具运行前，项目的虚拟环境处于激活且最新状态。

#### pyproject.toml 作为单一事实来源

所有工具配置都位于 `pyproject.toml` 中。没有 `.flake8`，没有 `setup.cfg`，没有 `mypy.ini`。一个完整的基线配置如下：

```
[project]
name = "run-or-relax"
version = "0.1.0"
requires-python = ">=3.14,<3.15"
dependencies = [
 "fastapi>=0.115",
 "pydantic>=2.9",
 "uvicorn[standard]>=0.30",
]
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
[tool.hatch.build.targets.wheel]
packages = ["src/departures"]
[dependency-groups]
dev = [
 "httpx>=0.27",
 "mypy>=1.13",
 "pytest>=8.3",
 "pytest-asyncio>=0.24",
 "pytest-cov>=6.0",
 "ruff>=0.9",
]
[tool.pytest.ini_options]
testpaths = ["tests"]
asyncio_mode = "auto"
addopts = "--tb=short -q"
pythonpath = ["src"]
[tool.ruff]
target-version = "py314"
line-length = 100
exclude = [".uv-cache"]
[tool.ruff.lint]
select = [
 "D",
 "E",
 "F",
 "I",
 "UP",
 "B",
 "SIM",
]
ignore = ["E501"]
[tool.ruff.lint.per-file-ignores]
"tests/**/*.py" = ["D"]
[tool.ruff.lint.isort]
known-first-party = ["departures"]
[tool.ruff.lint.pydocstyle]
convention = "google"
[tool.ruff.format]
quote-style = "double"
indent-style = "space"
[tool.mypy]
strict = true
ignore_missing_imports = true
mypy_path = "src"
```

有了这个文件，Codex CLI 就能从单一位置发现你的项目约定。有几个条目虽不起眼但至关重要：

1.  `[tool.ruff.lint]` 中的 `select` 列出了激活的 lint 规则，因此生成的代码能在首次尝试时就通过 lint 检查。
2.  `exclude = [".uv-cache"]` 让 Ruff 忽略项目本地的 uv 缓存。下面的钩子将 `UV_CACHE_DIR=.uv-cache` 设置为将缓存写入项目内部，但如果不排除它，`uv run ruff check .` 会扫描该目录并报出数千个来自第三方包的 lint 错误。排除它才使得整个树 `.` 目标变得安全；否则你必须将每个 lint 命令限定为 `src tests`。
3.  `[build-system]` **加上** `uvicorn[standard]` 使得 `uv run uvicorn departures.app:app` 能正常解析。声明了构建后端后，`uv run` 会以可编辑模式安装项目，这样 `src/` 布局下的 `departures` 模块就能通过名称导入，而不仅限于路径导入。
4.  `[tool.hatch.build.targets.wheel]` 告诉 Hatchling 包的位置。当项目名为 `run-or-relax` 时，它无法推断出 `departures` 位于 `src/` 下，如果没有 `packages = ["src/departures"]`，首次构建会失败并提示 `Unable to determine which files to ship inside the wheel`。
5.  `pythonpath = ["src"]` 让 pytest 能在无需安装步骤的情况下导入同一个包。
6.  完成门控运行的是 **mypy**，**而非** `ty`。`ty` 是备选的 beta 版本；可运行的基线仍使用稳定的检查器。

`requires-python = ">=3.14,<3.15"` 的上限是刻意设定的：固定次版本号能保持工具链的可复现性，避免 `uv` 在新解释器一安装就跳转过去。在项目根目录下的 `.python-version` 文件中记录相同的固定版本，这样 `uv` 无需询问即可选择 3.14：

```
3.14
```

跳出个别条目来看，模式才是本章的重点：`pyproject.toml` 和 `.python-version` 中的每一行，都是 Codex CLI 不再需要猜测的一项决策。包管理器、lint 规则、类型检查器、包布局、解释器版本，每一项都是代理本会从不完整信号中推断（有时错误推断）的决策。将它们一次性写下来，将一系列猜测变为查阅得到的事实，这也是生成的代码能在第一次尝试而非第三次尝试时就正确落地的原因。

#### 用于自动执行的钩子

Codex CLI 的钩子系统（见 第 9 章）在会话层面强制执行工具链纪律，而不依赖代理的记忆。三个钩子覆盖了关键的 Python 护栏：

```json
{
 "hooks": {
 "PostToolUse": [
 {
 "hooks": [
 {
 "type": "command",
 "command": "uv run ruff check . --fix --quiet && uv run ruff format . --quiet",
 "statusMessage": "正在进行 Lint 和格式化…",
 "timeout": 30
 }
 ]
 }
 ],
 "Stop": [
 {
 "hooks": [
 {
 "type": "command",
 "command": "if UV_CACHE_DIR=.uv-cache uv run --no-sync pytest tests/ -q --tb=short >&2; then printf '{}\\n'; else hook_status=$?; printf '{}\\n'; exit \"$hook_status\"; fi",
 "statusMessage": "正在运行测试套件…",
 "timeout": 120
 }
 ]
 }
 ],
 "UserPromptSubmit": [
 {
 "hooks": [
 {
 "type": "command",
 "command": "bash .codex/scripts/block-legacy-pip.sh",
 "statusMessage": "正在检查提示词…",
 "timeout": 5
 }
 ]
 }
 ]
 }
}
```

脚本 `block-legacy-pip.sh` 会检查传统工具调用，若发现则以代码 `2` 退出并显示消息：

```bash
#!/usr/bin/env bash
##### .codex/scripts/block-legacy-pip.sh
##### 阻止包含传统 Python 工具调用的提示词
set -euo pipefail
##### UserPromptSubmit 通过标准输入传递 JSON 对象；文本在 `.prompt` 中。
prompt="$(jq -r '.prompt // empty')"
if printf '%s' "$prompt" | grep -qE 'pip install|pip add|python -m pytest|python -m ruff'; then
 echo "请对所有 Python 操作使用 uv：uv add <pkg>、uv run pytest、uv run ruff" >&2
 exit 2
fi
```





##### Hooks

正如第 9 章所述，`UserPromptSubmit` hook 在标准输入 (stdin) 上接收一个 JSON 对象形式的提交提示词，文本位于 `prompt` 字段中，因此脚本使用 `jq -r '.prompt'` 读取它，然后再进行 grep 操作。如果你针对的是较旧的 Codex 版本，请在依赖此机制前确认负载确实通过 stdin 到达了 hook，但 stdin JSON 是文档规定的通道。

`PostToolUse` hook 在每次工具调用后运行 ruff，因此每个写入的文件都会立即被检查和格式化。`Stop` hook 使测试门禁显性化：在完成前保持测试套件通过（绿色）。`UserPromptSubmit` hook 在 Codex CLI 处理遗留调用前将其拦截，这是主动强制执行，而非被动响应。

`Stop` 命令看起来比 'run pytest' 复杂得多，主要有两个原因。第一是 JSON 包装器：`Stop` hook 会在标准输出 (stdout) 上解析一个 JSON 控制对象，而原始的 pytest 输出不是有效的 JSON，因此该命令将 pytest 的报告发送到标准错误输出 (stderr) (`>&2`)，它仍能到达你这里，在 stdout 上打印一个空的 `{}` 对象以满足解析器，并以 pytest 自身的状态码重新退出，这样失败的测试套件（红色）仍会阻止停止操作。裸露的 `uv run pytest tests/ -q --tb=short` 即使在所有测试通过时也会导致 hook 失败，因为其测试输出的第一行会被解析为格式错误的 JSON。

第二个原因是 `uv run` 本身的两个标志。默认情况下，`uv run` 在运行前会同步项目环境，从锁文件解析并安装依赖，这可能会访问网络并写入 uv 的全局缓存。在 hook 中进行这种同步既缓慢，又容易因 hook 无权写入的目录而失败，从而报告一个与你的测试无关的失败。`--no-sync` 跳过此步骤，因为环境已在会话期间就绪，而 `UV_CACHE_DIR=.uv-cache` 将任何缓存写入保留在项目内部，而不是 hook 可能不拥有的全局位置。它们共同使门禁变得快速且无副作用；遗漏它们正是导致此次更改的失败原因。

撰写本文时，`PostToolUse` 匹配器过滤支持 Bash、`apply_patch`（也匹配 Edit 和 Write）以及 MCP 工具名称。它尚不拦截 WebSearch 或其他非 shell、非 MCP 工具调用。上述 `PostToolUse` hook 因未指定匹配器而在每次工具调用后触发；无条件运行 ruff 足够快，在实践中这并非显著开销。

### 服务与异步约定

Run or Relax API 是一项单一服务，但使其对智能体友好的约定同样适用于多服务扩展。FastAPI 和异步 Python 各自带有 Codex CLI 无法自行推断的规则，两者都应置于 `AGENTS.md` 中，以便智能体默认应用它们，而非逐个路由即兴发挥。

#### FastAPI 服务约定

FastAPI 是新建 Python 服务的常见选择。关键约定：

```
#### FastAPI 约定
- 路由处理器必须为异步；出站 HTTP 调用使用 `httpx.AsyncClient`
- Pydantic 响应模型位于 `src/departures/models.py`（每个服务一个模块；增长时拆分为包）
- 通过 FastAPI 的 `Depends()` 进行依赖注入：禁止全局状态
- 错误处理：在路由处理器中抛出 `HTTPException`；绝不让领域异常向上传播
- OpenAPI 模式：在每个路由上保持 `response_model` 显性化
- 后台任务：即发即弃型使用 `BackgroundTasks`；持久化工作使用任务队列（Celery 或 ARQ）
```

#### 异步模式

异步 Python 有 Codex CLI 无指示无法推断的约定：

```
#### 异步模式
- 对于并发独立操作，使用 `asyncio.gather()`
- 数据库会话：使用异步上下文管理器（`async with session_factory() as session`）
- 切勿在异步函数中调用阻塞式 I/O；如必需，请使用 `asyncio.run_in_executor`
- HTTP 客户端：在启动时创建 `httpx.AsyncClient` 实例并通过依赖注入传递
- 避免将 `asyncio.sleep(0)` 用作让出点；仅在真正需要时使用 `asyncio.sleep(0.001)`
```

#### 构建 Run or Relax API

有了 `AGENTS.md` 中的这些约定和章节开头的 `Departure` 模型，服务的其余部分只需一个简短提示词，因为契约已敲定：

```
使用 FastAPI 构建 Run or Relax API，遵循 AGENTS.md。
- GET /departures?station=audley-end&now=<ISO 8601，可选，默认为当前时间>
- 从 data/timetable.json（已植入）加载时刻表；应用来自 data/delays.json 的模拟延误源
- 对每个即将到来的发车返回：服务时间、站台、延误分钟数、距离发车分钟数，以及 RUN 或 relax 的判定
- 如果距离发车分钟数（含任何延误）至少为 WALK_MINUTES（14 分钟，步行至车站所需时间），判定为 relax；如果至少为 RUN_MINUTES（8 分钟，跑步所需时间），判定为 RUN；看板从不显示比 RUN_MINUTES 更近的列车，因为即使跑也赶不上
- 添加 CORSMiddleware，允许来自 RUN_OR_RELAX_CORS_ORIGIN 环境变量的看板源；未设置时，默认允许两个本地看板源 http://localhost:3000 和 http://127.0.0.1:3000，以便第 23 章的前端无论在 localhost 还是 127.0.0.1 上打开，都能从浏览器调用此服务
- Pydantic 响应模型；pytest 测试以时刻表作为 fixture 注入，无真实时钟且无网络
```

CORS 这一条是在浏览器介入前不可见的要点：所有后端测试不加它也能通过，随后第 23 章的看板因从不同源提供服务，其跨域请求被阻塞。将其写入提示词而非事后发现，能保持两章之间的接缝诚实。

默认值还藏着第二个陷阱，仅在浏览器中显现：根据浏览器的同源策略，`http://localhost:3000` 和 `http://127.0.0.1:3000` 是*不同源*，尽管它们解析到同一台机器。仅允许 `localhost` 的默认值会在读者用 `127.0.0.1` 打开看板的那一刻将其阻塞，UI 只会显示 `Could not reach the departure service.` 默认允许两者可移除陷阱；在生产环境设置 `RUN_OR_RELAX_CORS_ORIGIN` 则将其收窄为唯一交付的源：

```
DEFAULT_CORS_ORIGINS = ["http://localhost:3000", "http://127.0.0.1:3000"]
def cors_origins() -> list[str]:
    configured = os.getenv("RUN_OR_RELAX_CORS_ORIGIN")
    return [configured] if configured else DEFAULT_CORS_ORIGINS
app.add_middleware(
    CORSMiddleware,
    allow_origins=cors_origins(),
    allow_credentials=False,
    allow_methods=["GET"],
    allow_headers=["*"],
)
```

CORS 正是那种单元测试全过却在浏览器中崩坏的接缝，所以用一个像浏览器一样发送 `Origin` 头并断言响应将其回显的测试将其钉死：

```
import httpx
from departures.app import app
async def test_cors_allows_127_0_0_1() -> None:
    transport = httpx.ASGITransport(app=app)
    async with httpx.AsyncClient(transport=transport, base_url="http://test") as client:
        resp = await client.get("/departures", headers={"Origin": "http://127.0.0.1:3000"})
    assert resp.headers["access-control-allow-origin"] == "http://127.0.0.1:3000"
```

当智能体编写路径操作时，FastAPI 惯用的 `station: str = Query(default="audley-end")` 会触发 Ruff 的 `B008`（函数调用在默认参数中），因为 `B` (flake8-bugbear) 在选定的规则集中。请改用 FastAPI 现在推荐的 `Annotated` 形式，即 `station: Annotated[str, Query()] = "audley-end"`，它将默认值保持为普通字面量，从而干净地通过 `B008`。在 `AGENTS.md` 中命名此约定，以便生成的路由首次尝试即通过 lint 检查，而非第二次。




根据约定，以及前文给出的 `Departure` 模型，Agent 填充了时刻表加载器、异步路由和判决规则，后者作为一个小型纯函数位于 `src/departures/verdict.py`。该函数值得重点审查，因为它包含了目标所警告的棘手边界情况：

```python
from datetime import datetime

WALK_MINUTES = 14  # Audley End: 前门到车站步行所需时间
RUN_MINUTES = 8    # 前门到车站跑步所需时间

def verdict(now: datetime, departs_at: datetime, delay_min: int) -> str:
    minutes_until = (departs_at - now).total_seconds() / 60 + delay_min
    return "relax" if minutes_until >= WALK_MINUTES else "RUN"
```

`verdict` 函数掌握第一个边界：`“太晚无法从容”` 不是零分钟，而是 `WALK_MINUTES`；延误会让发车时间推迟，反而为你争取时间，而非消耗时间。如果 Agent 未被告知此规则，很可能会拿零分钟做比较，建议你从容走出家门，去赶一班实际上需要你冲刺才能赶上的列车。第二个边界 `RUN_MINUTES` 是加载器的职责（连跑步都赶不上的列车永远不会出现在发车板上），我们稍后再回到那里。通过命名边界的测试来固定 `verdict` 的行为，沿用前文的 fixture 约定，避免依赖真实时钟：

```python
from datetime import datetime

def test_ten_minutes_out_means_run() -> None:
    now = datetime(2026, 6, 15, 6, 52)          # 07:02 发车的列车，无延误（10 分钟）
    assert verdict(now, datetime(2026, 6, 15, 7, 2), 0) == "RUN"

def test_a_delay_turns_a_run_into_a_relax() -> None:
    now = datetime(2026, 6, 15, 6, 52)          # 同样是 10 分钟，但晚点 6 分钟
    assert verdict(now, datetime(2026, 6, 15, 7, 2), 6) == "relax"
```

喂给 `verdict` 的加载器留给 Agent 实现，但它掌管第二个边界 `RUN_MINUTES`，以及前述已定的延误与过滤决策，因此了解你将要审查的代码结构很有必要。`next_departures` 很精简：读取种子数据，将各延误应用到计划时间，丢弃即使跑步也赶不上的车次，返回前几个作为 `Departure` 模型。

```python
##### src/departures/timetable.py (参考结构)
from datetime import datetime, timedelta
from pathlib import Path
from typing import List

##### 假设 Departure, load_timetable, load_delays, RUN_MINUTES, verdict 在别处定义/导入

def next_departures(
    station: str, now: datetime, limit: int = 5
) -> list[Departure]:
    scheduled = load_timetable()[station]      # data/timetable.json
    delays = load_delays()                      # data/delays.json
    board = []
    for train in scheduled:
        delay_min = delays.get(train.id, 0)
        departs_at = train.scheduled + timedelta(minutes=delay_min)
        minutes_until = round((departs_at - now).total_seconds() / 60)
        if minutes_until < RUN_MINUTES:         # 即使跑步也赶不上
            continue
        board.append(Departure(
            service=train.service, platform=train.platform,
            delay_minutes=delay_min, minutes_until=minutes_until,
            verdict=verdict(now, train.scheduled, delay_min),
        ))
    return sorted(board, key=lambda d: d.minutes_until)[:limit]
```

这只是一个参考结构，而非成品模块：确切的加载器签名和时刻表记录取决于 Agent 如何建模种子 JSON。它固定的是决策顺序：先应用延误，再过滤，最后限制数量，以便你检查 Agent 是否按此顺序决策，而非（例如）在应用延误前先按计划时间过滤。

##### 播种时刻表与延误数据

加载器的优劣取决于背后的数据，第一版种子数据通过惨痛教训证明了这一点：仅含上午班次的时刻表会导致发车板在下午显示为空，即使 API 运行完美。一个过滤“即将出发”班次的发车板，若没有全天候的班次数据，高峰过后就无车可显。**必须播种全天数据**。相对于包而非硬编码路径来解析数据目录，以便无论仓库签出到何处都能找到文件，并提供可选的环境变量覆盖以指向备用数据集：

```python
DEFAULT_DATA_DIR = Path(__file__).resolve().parents[2] / "data"
DATA_DIR = Path(os.getenv("RUN_OR_RELAX_DATA_DIR", DEFAULT_DATA_DIR))
```

`parents[2]` 从 `src/departures/timetable.py` 向上走两级到达项目根目录，因此 `DATA_DIR` 默认落在仓库的 `data/` 目录，且从不烘焙入特定机器的路径。若要针对不同数据集运行，将该变量指向任何包含同名两个文件 `timetable.json` 和 `delays.json` 的目录即可：

```bash
RUN_OR_RELAX_DATA_DIR=/path/to/alternate/data \
  uv run uvicorn departures.app:app --reload --host 127.0.0.1 --port 8000
```

创建 `data/timetable.json`，包含该车站全天的班次服务，每个服务拥有稳定的 `id`，供延误数据源关联：

```json
{
  "audley-end": [
    { "id": "ae-lst-0505", "service": "05:05", "platform": "2" },
    { "id": "ae-lst-0538", "service": "05:38", "platform": "2" },
    { "id": "ae-lst-0554", "service": "05:54", "platform": "2" },
    { "id": "ae-lst-0606", "service": "06:06", "platform": "2" },
    { "id": "ae-lst-0632", "service": "06:32", "platform": "2" },
    { "id": "ae-lst-0638", "service": "06:38", "platform": "2" },
    { "id": "ae-lst-0702", "service": "07:02", "platform": "2" },
    { "id": "ae-lst-0708", "service": "07:08", "platform": "2" },
    { "id": "ae-lst-0732", "service": "07:32", "platform": "2" },
    { "id": "ae-lst-0738", "service": "07:38", "platform": "2" },
    { "id": "ae-lst-0802", "service": "08:02", "platform": "2" },
    { "id": "ae-lst-0808", "service": "08:08", "platform": "2" },
    { "id": "ae-lst-0828", "service": "08:28", "platform": "2" },
    { "id": "ae-lst-0842", "service": "08:42", "platform": "2" },
    { "id": "ae-lst-0909", "service": "09:09", "platform": "2" },
    { "id": "ae-lst-0939", "service": "09:39", "platform": "2" },
    { "id": "ae-lst-1017", "service": "10:17", "platform": "2" },
    { "id": "ae-lst-1039", "service": "10:39", "platform": "2" },
    { "id": "ae-lst-1117", "service": "11:17", "platform": "2" },
    { "id": "ae-lst-1139", "service": "11:39", "platform": "2" },
    { "id": "ae-lst-1217", "service": "12:17", "platform": "2" },
    { "id": "ae-lst-1239", "service": "12:39", "platform": "2" },
    { "id": "ae-lst-1317", "service": "13:17", "platform": "2" },
    { "id": "ae-lst-1339", "service": "13:39", "platform": "2" },
    { "id": "ae-lst-1417", "service": "14:17", "platform": "2" },
    { "id": "ae-lst-1439", "service": "14:39", "platform": "2" },
    { "id": "ae-lst-1517", "service": "15:17", "platform": "2" },
    { "id": "ae-lst-1539", "service": "15:39", "platform": "2" },
    { "id": "ae-lst-1557", "service": "15:57", "platform": "2" },
    { "id": "ae-lst-1614", "service": "16:14", "platform": "2" },
    { "id": "ae-lst-1639", "service": "16:39", "platform": "2" },
    { "id": "ae-lst-1656", "service": "16:56", "platform": "2" },
    { "id": "ae-lst-1717", "service": "17:17", "platform": "2" },
    { "id": "ae-lst-1738", "service": "17:38", "platform": "2" },
    { "id": "ae-lst-1806", "service": "18:06", "platform": "2" },
    { "id": "ae-lst-1838", "service": "18:38", "platform": "2" },
    { "id": "ae-lst-1907", "service": "19:07", "platform": "2" },
    { "id": "ae-lst-1938", "service": "19:38", "platform": "2" },
    { "id": "ae-lst-2017", "service": "20:17", "platform": "2" },
    { "id": "ae-lst-2039", "service": "20:39", "platform": "2" },
    { "id": "ae-lst-2117", "service": "21:17", "platform": "2" },
    { "id": "ae-lst-2139", "service": "21:39", "platform": "2" },
    { "id": "ae-lst-2208", "service": "22:08", "platform": "2" },
    { "id": "ae-lst-2239", "service": "22:39", "platform": "2" },
    { "id": "ae-lst-2308", "service": "23:08", "platform": "2" }
  ]
}
```

接着创建 `data/delays.json`，以每个服务的 `id` 为键，值为延误分钟数，模拟真实发车板会轮询的实时数据源。只有少数班次晚点；其余缺省即为准点：

```json
{
  "ae-lst-0702": 6,
  "ae-lst-0738": 3,
  "ae-lst-0808": 10,
  "ae-lst-1717": 5,
  "ae-lst-1806": 2
}
```




### 总结

1.  **Python 削弱了反馈回路。** 其正确性信号弱于静态类型语言：后期才浮现的类型错误、环境变量依赖、导入时的副作用、易于模拟以及静默的 async 边缘情况是这五大鸿沟，本章中的每个秘诀都是为了填补其中之一。

2.  **在 `AGENTS.md` 中命名工具。** 一份写明 `uv run pytest`、`uv run ruff check --fix` 和 `uv run ruff format` 的 Python `AGENTS.md` 能在错误发生前消除绝大多数工具误用。完整质量关卡（lint → format → type check → tests）将完整的质量周期变成了 Agent “完成”定义的一部分。

3.  **Pytest 需要命名 fixtures 和 markers。** 当 Codex CLI 知道 fixtures 位于何处、存在哪些 markers 以及覆盖率阈值是多少时，集成效果会显著提升。Fixture 发现和 marker 规范是配置不足的项目中最常缺失的两个约定。

4.  **类型生成需要风格规范。** 当风格被精确指定时，类型提示和文档字符串适合自动化生成。禁止裸露的 `Any` 以及选择 Google 风格文档字符串是产出最一致输出的两条指令。

5.  **一套工具链，一个配置文件。** `uv` 和 `ruff` 合并了历史上半打独立的工具，`mypy` 作为类型检查关卡。将 `pyproject.toml` 编码为单一事实来源，并通过 `uv run` 路由所有调用，可生成一个 Codex CLI、CI 和团队每位开发者完全共享的可复现环境。

6.  **FastAPI 和 async 需明确约定。** Async 路由处理器、`Depends()` 注入、显式的 `response_model` 以及 async 函数内无阻塞 I/O，所有这些都应写入 `AGENTS.md`，否则 Codex CLI 会在不同路由间不一致地即兴发挥。

7.  **“运行或放松”的回报。** 有了这些约定，服务只需一个简短提示，而值得审查的部分仅是裁决规则和钉住其棘手边缘情况的测试。其 `GET /departures` 契约即是 第 23 章 所渲染、第 24 章 所部署的那个贯穿三章的单一示例。

### 练习

1.  **基线审计。** 选取一个你正在从事的现有 Python 项目，使用本章约定编写一份根目录下的 `AGENTS.md`。在 Codex CLI 中运行 `/init` 并将其生成内容与你编写的内容对比。它们在哪些地方出现分歧？这说明了 Codex CLI 能推断出哪些约定，又有哪些必须显式声明？

2.  **覆盖率扩展。** 找出你项目中测试覆盖率低于 80% 的模块。编写一个 Codex CLI 提示词，利用 `AGENTS.md` 中的 fixture 约定将覆盖率扩展至 95%。审查生成的测试：它们是断言行为还是仅断言当前输出？修改那些断言附带行为而非预期契约的测试。

3.  **钩子管道。** 实现本章中的三钩子配置（PostToolUse lint、Stop 测试关卡、UserPromptSubmit 遗留命令拦截）到 `.codex/hooks.json` 文件中。运行一个向模块添加新函数的 Codex CLI 会话，观察哪些钩子触发以及触发时机。修改 Stop 钩子命令使其在测试失败时非零退出，观察 Codex CLI 如何报告错误。

4.  **类型注解扩展。** 找出代码库中至少 30% 函数缺少类型注解的 Python 模块。编写一个 Codex 提示词，为每个未注解的函数添加注解，遵循 `AGENTS.md` 中的约定（例如 Python 3.10+ 使用 `str | None` 而非 `Optional[str]`）。Codex CLI 完成注解后，在严格模式下运行 `mypy` 或 `ty` 并审查浮现的任何类型错误。注意这些错误源于新增注解，还是注解暴露出的预存类型问题。

5.  **构建 Run or Relax API。** 使用包含本章 FastAPI 和 async 约定的 Python `AGENTS.md`，提示 Codex CLI 构建上述 `GET /departures` 端点，对接种子时刻表，将裁决实现为纯函数，并编写注入时钟而非读取时钟的测试。然后重新生成，但刻意在提示词中省略“站台进站规则”：Agent 是否会与零比较并让你冲向一班你赶不上的列车？将该规则加入 `AGENTS.md`，确认有测试钉住它，并保留该契约；你将在 第 23 章 中针对它构建前端。

---



