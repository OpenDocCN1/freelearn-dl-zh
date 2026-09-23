# Codex CLI 入门

第 1 章 解释了 Codex CLI 是什么以及智能体模式为何重要。本章回答实际问题：如何安装、认证并运行真正的第一次会话？最后，你将拥有一个可用的安装环境，并有信心跟进后续所有内容。

### 学习目标

你将能够：

1. 安装 Codex CLI 并验证安装
2. 完成适合你情况的认证流程
3. 运行真正的第一次会话并理解你看到的内容

### 首次会话的样子

在安装步骤之前，先看看你安装的是什么。以下是在一个真实代码库上的真实首次会话：一个包含失败测试和待查找 Bug 的微型项目。你暂时还无法运行它，那在后几页，但先读一遍能让你知道输出开始滚动时该期待什么、该忽略什么。

任务只有一句话，输入提示词中：

```
Find the bug in main.js and fix it. Run the tests before and after to confirm the fix.
```

Codex CLI 不会直接回答。它开始观察，并边走边叙述：

```
> Reading main.js, package.json, test/main.test.js
> Running npm test
  O calculateCartTotal multiplies each item price by its quantity (1 failing)
```

第一步是获取证据，而非猜测。智能体在做任何更改前先读取相关文件并运行测试套件，因此它是从同一个你会从中开始的失败测试起步。然后它大声提出假设：

```
> The cart total adds item.price but ignores item.quantity entirely. Bringing
  quantity into the sum: total + item.price + item.quantity.
```

这是一个合理的初步判断，但它是错的，这才是值得关注的部分。智能体进行编辑，重运行测试，测试套件告诉它错了：

```
> Editing calculateCartTotal: total + item.price  ->  total + item.price + item.quantity
> Running npm test
  O calculateCartTotal multiplies each item price by its quantity (1 failing)
```

对话式工具会把第一次编辑交给你就停下了。Codex CLI 运行了测试，看到了红色（失败），并继续前进。它再次读取失败信息，这次测试名称告诉它遗漏了什么：

```
> Still failing: got 10.5, expected 12.5. The test name says "multiplies": the
  quantity should multiply the price, not add to it. Changing the + to *.
```

它将加法改为乘法，并重运行直到测试套件全绿：

```
> Editing calculateCartTotal: + item.quantity  ->  * item.quantity
> Running npm test
  P 2 passing
```

然后它停下并总结所做之事：一个忽略数量的购物车总计，一次错误的猜测（加而非乘）被撤回，两个测试均通过。整个过程耗时 20 到 40 秒，每次工具调用都在会话日志中实时可见。

你刚才目睹的是 第 1 章 中的智能体循环：规划、执行、观察、调整。错路并非演示中的缺陷，它*就是*演示。一个能自己运行测试的智能体，首次尝试错了也没关系，因为捕捉错误的是测试运行，而非你的代码审查。

#### 快速入门不会告诉你的事

官方快速入门只展示克隆和运行。它没告诉你：上述可见的推理过程才是重点，而非噪音。文档遗漏了三点：

* **第一次编辑往往是探针，而非修复。** 智能体被允许犯错，因为它会验证。把每个中间编辑都当成最终答案看会让你抓狂。看测试结果，别盯着按键。
* **它在改动任何东西前先运行测试。** 如果你的项目没有测试，或者测试慢、易变，智能体就失去了赖以生存的反馈，其第一次猜测极有可能成为最后一次猜测。快速的测试命令是你代码库中杠杆率最高的单一因素。
* **结尾的总结才是交付物，而非滚动日志。** 滚动的工具调用是实时追踪，不是报告。运行时瞥一眼；仔细阅读最终总结和差异。

### 更大的图景：四个界面，同一个智能体

Codex CLI 是共享同一智能体的四个界面之一：CLI、桌面应用、IDE 扩展和云端^[22]。配置、技能和记忆可在四者间迁移。CLI 是基础界面：开源、本地优先，也是本书的重点。

### 先决条件

Codex CLI 需要 Node.js 22 或更高版本^[23]。在 macOS 和 Linux 上，请使用 `nvm` 或 `fnm` 管理 Node 版本：

```
fnm install 22 && fnm use 22
node --version # v22.x.x 或更高版本
```



Codex CLI 可在 macOS、Windows（原生支持 AppContainer 沙箱，或通过 WSL2）和 Linux 上运行。撰写本文时，原生 Windows 支持属于实验性但已可用；WSL2 仍推荐用于生产环境，因为它运行的内核级沙箱与原生 Linux 安装相同。WSL1 无法创建沙箱所需的用户命名空间，因此不受支持。

需要 macOS 13 Ventura 或更高版本；同时支持 Apple Silicon 和 Intel 芯片。在 Linux 上，沙箱基于 bubblewrap (`bwrap`) 构建。您无需自行安装：Codex 自带一份并自动使用，仅当您的 `PATH` 中存在更新版本的系统 `bwrap` 时才会优先使用后者。

### 安装

#### npm（推荐）

```
npm install -g @openai/codex
codex --version
```

如果遇到权限错误，请将 npm 配置为使用主目录下的文件夹：`npm config set prefix ~/.npm-global`，并将 `~/.npm-global/bin` 添加到您的 `PATH` 中。

Codex CLI 提供了内置的更新命令：

```
codex update # 通过安装时使用的包管理器进行自我更新
```

`update` 子命令会检测您的安装方式（npm、Homebrew 或二进制文件）并触发相应的更新机制。如需手动升级：

```
npm install -g @openai/codex@latest # npm 手动升级
```

#### Homebrew（macOS 和 Linux）

```
brew install --cask codex
```

Homebrew cask 直接安装预构建的二进制文件，因此不需要 Node.js。Homebrew 通过标准的 `brew upgrade` 机制处理更新。

#### 直接下载二进制文件

预构建的二进制文件可在 GitHub 发布页面获取，适用于 npm 和 Homebrew 均不适用的环境。下载适用于您平台的压缩包，解压，将二进制文件重命名为 `codex` 并将其放置在您的 `PATH` 中：

```
tar xzf codex-aarch64-apple-darwin.tar.gz
mv codex-aarch64-apple-darwin /usr/local/bin/codex
chmod +x /usr/local/bin/codex
```

#### 验证安装

确认一切正常工作：

```
codex --version
```

要确认可用的命令和标志，请运行：

```
codex --help
```

这将列出每个子命令和标志，并确认 CLI 响应正常。

#### Shell 补全

Tab 补全使标志和子命令更易于发现。Codex CLI 为 bash、zsh 和 fish（以及 PowerShell 和 Elvish）生成补全脚本。为您使用的 Shell 生成补全文件：

```
##### bash
mkdir -p ~/.local/share/bash-completion/completions
codex completion bash > ~/.local/share/bash-completion/completions/codex
##### zsh
mkdir -p ~/.zsh/completions
codex completion zsh > ~/.zsh/completions/_codex
##### fish
codex completion fish > ~/.config/fish/completions/codex.fish
```

Bash 和 fish 会自动从这些目录加载文件。Zsh 需要额外一步：在 `~/.zshrc` 中 `compinit` 之前将补全目录添加到 `fpath`：

```
fpath=(~/.zsh/completions $fpath)
autoload -Uz compinit
compinit
```

### 认证

根据您访问 OpenAI 的方式，共有三种途径。

#### 路径 1：ChatGPT 订阅（设备码流程）

```
codex login
```

CLI 会打印一个简短的代码并打开一个 URL。登录，输入代码，Codex 会将凭证令牌存储在 `~/.codex/auth.json` 中（仅所有者可读写）。

#### 路径 2：API 密钥

```
export OPENAI_API_KEY="sk-..."
```

将其添加到您的 Shell 配置文件以使其持久生效。使用 API 密钥时，您将按 Token 计费；建议在学习典型会话成本时，从较低的使用上限开始。

#### 路径 3：企业配置

直接编辑 `~/.codex/config.toml`：

```
api_base_url = https://your-org.openai.azure.com/...
api_key_source = "azure_ad"
```

对于管理员强制执行的设置，请使用 `requirements.toml`。对于命令行的一次性覆盖，请使用 `-c` / `--config` 标志并采用点号表示法（例如 `codex -c api_base_url=https://... exec "your task"`）。

*第 8 章* 详细介绍了企业部署。

### 首次会话

您已经看过演示；现在亲自尝试一下。克隆快速入门仓库并启动 Codex CLI：

```
git clone https://github.com/danielvaughan/codex-quickstart-example
cd codex-quickstart-example
codex
```

输入演练中使用的相同任务：

```
Find the bug in main.js and fix it. Run the tests before and after to confirm the fix.
```

预期会出现演练会话所示的情况：读取文件、运行测试、进行一次或多次编辑、可能出现智能体回退的错误尝试，最终测试套件全部通过。结果是文件系统上真实的差异，并由通过的测试确认，而不是需要您手动应用的建议。

如果智能体在读取文件或运行命令前暂停以请求许可，这是审批模式在起作用，而非故障。*第 5 章* 解释了各种模式及如何更改默认设置。

### 第一周常见的三个错误

几乎每个人在第一周都会犯同一类少数错误。这些错误都不严重，现在跳过它们比吃一堑长一智要快得多。

1.  **像聊天一样对待它，且任务描述不足。** “清理一下认证代码”没有给智能体提供任何验证标准，因此它会自行定义“清理”的标准并为此运行数分钟。解决方法是设定完成标准：指明文件、期望的更改、以及必须通过的测试。*第 3 章* 专门讲述这一点。

2.  **打断第一个您不同意的编辑。** 正如演练所示，第一次编辑往往是探测性的。智能体刚做出您不会做的举动就将其叫停，会移除使其有用的“测试-纠正”循环。让它完成一轮，再阅读差异。

3.  **在没有测试和 `AGENTS.md` 的情况下运行。** 没有测试，智能体无法判断是否成功；没有 `AGENTS.md`，它不知道运行哪个测试命令、遵循哪些约定，只能两者都猜。本章后面所述的最小化 `AGENTS.md` 只需十分钟，便能在第一次真实会话中收回成本。

### `codex` 与 `codex exec` 的区别

`codex`（不带参数）启动交互式 REPL：一个带有对话历史的持久会话。用于交互式工作：探索、多步骤任务和学习。

`codex exec` 非交互式运行并在完成时退出。用于脚本任务、CI/CD 流水线和定义明确的自动化：

```
codex exec "update all TypeScript interfaces in src/types/ to add the new userId field"
codex exec --json "analyse test coverage and report modules under 80 per cent"
```

一个实用的启发式判断：如果您会将其输入终端，请使用 REPL。如果您会将其写入 Makefile，请使用 `codex exec`。

### 配置文件位置

Codex CLI 从多个来源解析配置；后加载的条目会覆盖先加载的。

`~/.codex/config.toml`：用户级默认值（模型、审批模式、沙箱设置）：

```
model = "gpt-5.5"
approval_policy = "on-request"
sandbox_mode = "workspace-write"
```

Codex 启动时读取的**环境变量**：

| 变量 | 用途 |
| --- | --- |
| `CODEX_HOME` | Codex 状态根目录：配置、认证、日志、会话和技能 |
| `CODEX_API_KEY` | 单次非交互式 `codex exec` 运行的 API 密钥 |
| `CODEX_ACCESS_TOKEN` | 用于可信自动化的 ChatGPT 或 Codex 访问令牌 |
| `CODEX_CA_CERTIFICATE` | 具有企业 TLS 中间人拦截环境的 PEM CA 证书包 |
| `RUST_LOG` | CLI 和应用服务器的日志过滤和详细程度 |

**解析顺序**（后者优先）：内置默认值、`~/.codex/config.toml`、项目 `.codex/config.toml`、命令行标志。

### 一分钟了解 AGENTS.md

`AGENTS.md` 是位于您仓库根目录的普通 Markdown 文件，Codex CLI 在每次会话开始时会自动读取。您可在其中记录智能体无法从源代码推断的项目特定知识：测试命令、包管理器、遵循的约定、不应修改的文件。

一个最小化的 `AGENTS.md` 如下所示：





### 项目：支付服务

#### 构建与测试

- 安装：`npm install`
- 测试：`npm test`

##### 约定

- 全程使用 async/await；不使用 `.then()` 链
- 金额以整数（分）表示，绝不使用浮点数

#### 请勿修改

- `src/legacy/` — 已弃用，只读
- 任何匹配 `*.generated.ts` 的文件

文件可组合：`~/.codex/AGENTS.md` 设置个人默认值，仓库根目录设置项目默认值，子目录可覆盖两者。路径越深，指令越具体。

`AGENTS.md` 在全书中被反复引用。*第 4 章* 专门介绍它：应包含什么、不应包含什么，以及如何在项目演进过程中保持其实用性。

### 现状

你现在已拥有一个可用的 Codex CLI 安装、一个已认证的会话、一次在真实仓库上的首次运行，并对配置和项目上下文的位置有了了解。这是基础。本书后续的所有内容都建立在此基础之上。

下一个投入产出比极高的技能是提示词工程。Codex CLI 对具体性的奖励程度是对话式工具无法比拟的，因为模糊的提示词不会产生模糊的答案；它会产生一次可能持续数分钟、并可能触及你意想不到的文件的运行。*第 3 章* 介绍了高效提示词的四要素、在运行中引导会话的斜杠命令，以及在常见任务中行之有效的模式。请在下一次非简单会话前阅读该章。

### 摘要

1.  **CLI 是 Codex 四大界面之一。** CLI、桌面应用、IDE 扩展和云端共享同一智能层、配置和记忆。

2.  **系统要求。** Codex CLI 需要 Node.js 22 或更高版本，可在 macOS、Windows（原生或 WSL2）和 Linux 上运行。

3.  **安装并验证。** 使用 `npm install -g @openai/codex`、`brew install --cask codex` 或直接下载二进制文件。运行 `codex --version` 进行确认。

4.  **Shell 补全加速日常使用。** 在开始编写提示词前，为 bash、zsh 或 fish 设置好补全。

5.  **三种认证路径。** 订阅制使用设备码流程，API 访问使用 `OPENAI_API_KEY`，托管部署使用企业配置。

6.  **交互式 REPL 与脚本化执行。** `codex` 是日常工作的交互式 REPL；`codex exec` 是用于脚本和 CI/CD 的无头模式。

7.  **配置分层解析。** 三个层级，具有明确的解析顺序；更近的设置覆盖更远的设置。

8.  **AGENTS.md 是项目约定文件。** 一个 Codex 在会话开始时自动读取的 Markdown 文件。用于项目约定、测试命令和禁区文件。*第 4 章* 深入介绍。

### 练习

**练习 2-1。** 安装 Codex CLI，运行 `codex --help`，并找出三个你之前不知道的标志。为每个标志写一句话，解释何时使用它。

**练习 2-2。** 使用“AGENTS.md 一分钟上手”部分的最小示例，在当前项目中创建一个包含测试命令、一条编码约定和一个禁区目录的最小 `AGENTS.md`。运行一次会话，观察指令是否生效。*第 4 章* 介绍完整模式。

**练习 2-3。** 在一个有界任务上运行 `codex exec`（“查找所有 TODO 注释并生成列出它们的 TODOS.md”）。然后以交互方式运行同一任务。对比交互式会话向你展示了什么而 `codex exec` 没有展示。





