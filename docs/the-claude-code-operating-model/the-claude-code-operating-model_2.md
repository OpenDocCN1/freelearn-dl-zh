# 第二章：以史为鉴：记忆系统的工程实践

"那些不能记住过去的人，注定要重蹈覆辙。"

—— 乔治·桑塔亚那

上次技术讨论结束后，小冰迫不及待地在她负责的订单服务项目中尝试 Claude Code。她兴致勃勃地输入了第一个请求："*帮我写一个用户登录 API。*"几秒钟后，Claude 生成了一段看起来十分完美的代码：基于 Express 框架，使用 JavaScript 语法，甚至还附带了 `npm install` 命令，一气呵成。小冰盯着屏幕看了三秒，才慢慢打出下一行字："*我们项目用的是 Fastify 和 TypeScript……*"她随后耐心地纠正了模型的约束条件，重新陈述了需求，最终才得到了符合预期的代码。

第二天，小冰关掉终端，开启了一个全新的会话，让 Claude 编写一个订单创建 API。这一次，Claude 更加热情，事无巨细地生成了一整套路由、控制器和数据模型。然而，框架依然是 Express，包管理器依然是 npm，甚至目录结构都与团队规范大相径庭。盯着屏幕上那行熟悉的 `const express = require('express')`，小冰感到一种深深的无力感在心底涌起。

这种"AI 失忆症"并非源于 Claude 能力的欠缺。恰恰相反，它在每一个独立会话中都展现出卓越的编程能力。然而，它所做的每一个看似"合理"的选择——选择 Express 而非 Fastify，选择 npm 而非 pnpm，选择 JavaScript 而非 TypeScript——都无一例外地触碰了团队的标准。

## 为什么 AI 需要"记忆"

沮丧的小冰去找 Kage。听完她的经历后，Kage 并没有急于给出解决方案。

"*小冰，你还记得第一天上班时的感受吗？*"Kage 问道。

小冰想了想。"*当然记得。我什么都不懂*——*代码仓库在哪、分支策略是什么、CI 流程怎么走……什么都得问同事。*"

"*没错。现在想象一下，如果你每天早上醒来，都忘记了前一天在公司学到的一切，每一天都像是* *'* *入职第一天'。* *你会怎么办？*"

小冰打了个寒颤。"*这不就是现在使用 Claude Code 的感觉吗？*"

"*正是如此*，"Kage 解释道。"*对 Claude 来说，***每一次对话都是它的'入职第一天'***（见图 2.1）*。它就像一个技术过硬的新员工，第一天上班时对公司的技术栈一无所知，只能根据个人经验和行业惯例来猜测。猜测或许合乎逻辑，但未必准确。*"

![图 2.1：每一次对话都是 Claude 的"入职第一天"。](img/B39394_2_1.png)

图 2.1：每一次对话都是 Claude 的"入职第一天"。

这是大模型的一个根本特征：会话之间缺乏持久状态。每次你关闭 Claude Code 会话并开启一个新的会话，Claude 模型面对的都是完全空白的白板。它不记得你的项目使用什么框架、团队偏好的代码风格，甚至不记得你五分钟前纠正过的错误。这不是某个特定模型的缺陷，而是当前所有大模型的共同特征——每一次对话都必须从零开始构建上下文。

从软件工程的角度来看，这与"约定优于配置"原则恰好相反。Ruby on Rails 之所以能赋予开发者高效能，正是因为其一套合理的默认约定：数据库表名用复数、模型名用单数、控制器放在 `app/controllers` 目录下，开发者只需关注约定之外的例外情况。

然而，Claude 默认的"约定"并非来自项目规范，而是来自它的训练数据——互联网上所有公开代码的统计规律。在这个分布中，Express 的出现频率高于 Fastify，npm 比 pnpm 更常见，JavaScript 样本数量远超 TypeScript。因此，在缺乏具体项目上下文时，Claude 会自然地倾向于最"主流"的方案。这些选择在通用场景下或许合理，但未必契合你团队的具体规范。

换句话说，Claude 缺的不是能力，而是上下文。





鉴于此，解决方案显而易见：既然每次对话都是 Claude 的“入职第一天”，何不为它准备一份专属的“入职手册”，让它在每次启动时自动研读？这份手册就是 `CLAUDE.md` 文件。作为一个纯 Markdown 文本文件，你只需将其放入指定的项目目录，`Claude Code` 便会在启动时自动扫描并加载其内容，将其作为高优先级的用户消息注入到系统提示词之后（而非嵌入系统提示词内部）。这也解释了为什么 `CLAUDE.md` 表现为一种指导性建议而非强制性配置。从此，Claude 无需再从零开始猜测你的项目长什么样——每一个规范和细节都明明白白地写在了手册里。

这种设计哲学在软件工程中早已司空见惯。我们用 `.editorconfig` 统一编辑器行为，用 `.eslintrc` 规范代码风格，用 `tsconfig.json` 约束 TypeScript 编译选项，用 `.prettierrc` 固化格式规则。这些配置文件的本质，都是将隐性的“团队约定”从人的大脑中外化为机器可读的声明式规则。

`CLAUDE.md` 遵循的也是同样的逻辑，唯一的区别在于它的“读者”：不再是编译器或格式化工具，而是一个具备自然语言理解能力的大模型。这正是“配置驱动”思维在 AI 时代的自然延伸与进化。

Kage 的笔记

有人可能会问：“Claude Code 不是有读取项目文件的能力吗？让它看看 `package.json` 不就能推断出技术栈了？”话虽如此，但“主动探测”不仅会消耗宝贵的工具调用配额和上下文窗口空间，更重要的是，`package.json` 只记录了依赖列表，无法捕捉到架构决策和团队约定等更深层的信息。`CLAUDE.md` 的核心价值在于将零散的隐性知识固化为一份显性的指导文档，确保 Claude 在开始编码之前就能建立起对项目的准确心智模型。

# 五层记忆架构

“原理我懂了，但 `CLAUDE.md` 到底应该放在哪里？”小冰追问道。

Kage 微微一笑。“好问题。答案是：它不只存在于一个地方。”

Claude Code 的记忆系统采用了一套精妙的分层架构，从全局到局部共分为五个层级。这种设计让人联想到操作系统中的配置加载机制：正如 Linux 通过扫描 `/etc/` 中的全局设置、家目录中的用户专属设置以及当前目录中的项目专属设置来读取配置——逐层递进而愈发具体——Claude Code 的五层记忆系统遵循的也是同样的设计哲学。

本章使用的五层模型是本书自身对记忆工作机制的组织方式。Claude Code 的官方文档定义了四种记忆作用域——托管策略、用户、项目和本地。这里单独列出的“规则目录”层（`.claude/rules/`）在该分类法中并非独立作用域；它是项目作用域内的一种模块化、按路径限定范围的功能。我们将其单独列为一个层级，是为了突出条件规则机制。

我们可以用一个通俗的类比来说明这套体系：公司制度文件的层级。

- 第一层是总部下发的“集团行为准则”，是所有子公司都必须遵守且不可修改的铁律。它确立了公司的核心价值观和底线。
- 第二层是各部门的“部门操作手册”，适用于部门内所有项目，确保同一业务线内的规范性。
- 第三层是各项目团队的“项目开发规范”，将规则细化到单个产品，明确技术选型和编码风格等核心约定。
- 第四层是特定场景的“专项操作指南”。例如“数据库迁移操作规范”，仅在涉及特定任务时才被引用，具有高度的场景针对性。
- 第五层是每个员工工位上的个人便利贴——完全个性化的知识，只有自己可见，用于记录临时的调试技巧或个人偏好。

自上而下，适用范围逐层收窄，内容粒度逐层增加，管理灵活度也随之上升。

*图 2.2* 展示了 Claude Code 的五层记忆系统。

![图 2.2：Claude Code 的五层记忆系统](img/B39394_2_2.png)

图 2.2：Claude Code 的五层记忆系统

接下来，我们逐层分析每个层级的职责与典型用途。

**企业策略层**构成了整个体系的“天花板”，通常是开发者无法触及的。这一层由公司的 IT 管理员通过系统级配置文件（macOS `/Library/Application Support/ClaudeCode/CLAUDE.md`；Linux/WSL `/etc/claude‑code/CLAUDE.md`；Windows `C:\Program Files\ClaudeCode\CLAUDE.md`）或 Anthropic 企业管理控制台统一设定。典型策略包括：禁止代码发送至外部 API、要求所有数据库操作必须经过 ORM、严禁在代码中硬编码密钥等安全红线。这一层确立了企业合规的基线：无论开发者如何配置下面四层，这些强制性约束都无法被绕过。对于个人开发者和小团队而言，这一层通常为空，无需特别关注。

**用户层**本质上是用户的“个人偏好档案”。该配置文件位于用户家目录下的 `~/.claude/CLAUDE.md`，对本机打开的所有项目生效。它最适合存放与具体业务逻辑无关、但体现个人工作习惯的通用偏好，例如沟通语言的选择（中文还是英文）、Git 提交信息的格式约定，或将常用的个人快捷命令映射到具体的执行命令。这些偏好高度稳定，不随项目切换而变化。

```
<!-- ~/.claude/CLAUDE.md -->

# Personal Coding Preferences

- Communication language: Replies in Chinese, code comments in English

- Commit conventions: Git commit messages follow the format "type(scope): description in Chinese"

- Programming paradigm: Prefer functional programming style

- Shortcut command mappings:

- When I say "run tests," automatically execute pnpm test

- When I say "start service," automatically execute pnpm dev
```




```
**项目层**是日常开发中最常用的层，也是本章的重点。它由项目根目录下的 `CLAUDE.md` 文件表示，必须提交到 Git 仓库，成为项目代码资产的一部分。
```

这意味着当新同事克隆项目时，Claude Code 会自动读取该文件。无需额外配置——Claude 即刻获得完整的项目上下文。这就像在新工作入职第一天，欢迎礼包中就收到一份专属的“项目开发手册”——打开一看，技术栈选型、目录结构、编码规范、常用命令一目了然。

**规则目录层**是一项精妙的高级设计，旨在解决“单个 `CLAUDE.md` 文件无法容纳所有复杂规则”的痛点。你可以在 `.claude/rules/` 目录下部署多个独立的 Markdown 文件，每个文件聚焦于一个特定主题（如数据库约定、API 设计风格或测试策略）。更重要的是，该层支持条件触发机制——只有当操作特定文件或进入特定上下文时，规则文件才会被动态加载，从而避免无关信息干扰上下文。我们将在*条件规则系统*一节中深入探讨这一机制及其实际用法。

**本地层**是开发者的“个人备忘录”，对应 `CLAUDE.local.md` 文件。该文件不提交到 Git 仓库（Claude Code 仅在你运行 `/init` 并选择个人选项时才将 `CLAUDE.local.md` 添加到 `.gitignore`；如果你是手动创建该文件，请自行将其加入 `.gitignore`），用于记录对个人极具价值但不适合共享的信息，例如测试服务器的地址和端口、个人调试过程中积累的独特见解，或临时工作笔记。

例如，如果你知道开发环境的数据库会在特定时间进行维护，这类信息只需记录在本地层，无需通知整个团队，从而避免信息噪音。

```
<!-- ./CLAUDE.local.md (automatically added to .gitignore) -->

# Local Development Notes

- Database connection: postgresql://dev:dev123@10.0.1.50:5432/orders_dev

- Maintenance window: Database maintenance every Wednesday from 22:00 to 23:00; strictly forbidden to perform migration operations during this period

- Troubleshooting: If a CORS error occurs, first check the Nginx configuration (/usr/local/etc/nginx/nginx.conf)

- Integration configuration: The frontend service runs on port 5173; manual proxy forwarding configuration is required
```




至此，小冰敏锐地提出了一个关键问题：“*如果这五层之间出现了冲突怎么办？比如我在用户层设置了‘4 空格缩进’，但项目层规定的是‘2 空格缩进’？*”

“*问得好，*”Kage 赞许地回应，“*这些文件遵循的是‘累加而非覆盖’的原则。Claude Code 会读取所有层的内容，将它们合并成一个完整的上下文。当出现冲突时，Claude 模型会根据它的判断来做决策，核心逻辑是* *具体性优先。在缩进的例子里，项目层的‘2 空格’规则对当前项目更具体，因此会优先于用户层通用的‘4 空格’偏好。但企业策略层是个例外，它拥有绝对权威——无论下层如何配置，它的约束都无法被绕过。这就像公司的法律或安全红线，任何项目经理或个人偏好都无法凌驾其上。*”

值得一提的是，`CLAUDE.md` 还拥有强大的**文件引用机制**。在任意层的记忆文件中，你都可以通过 `@path/to/file` 语法来引入其他文件的内容，实现配置的模块化组织。

例如，在项目层的 `CLAUDE.md` 中写入 `@docs/api‑conventions.md`，Claude 就会自动加载并解析 API 约定文档的内容。该机制最多支持五层嵌套引用，对于大多数复杂项目的组织需求来说已经足够。

这为大型项目提供了一种优雅的文档治理方案：`CLAUDE.md` 不再需要独自承担全部细节，而是作为一个“总索引”，灵活地指向各类专项文档。这种设计成功规避了海量信息堆积在单一巨型文件中的弊端，使配置结构更清晰、更易维护。

# CLAUDE.md 的写作范式

理解了记忆文件的存放位置与层级职责后，一个更根本的问题浮出水面：它们究竟应该怎么写？

Kage 在白板上郑重写下三个词：**少即是多**。

“*`CLAUDE.md` 的内容具有全局持久性，*”Kage 解释道，“它会在每次对话开始时被完整注入上下文窗口。这意味着，你写的每一行，都在消耗每一次交互中宝贵的 token 配额。”

他打了个形象的比方：“*你可以把上下文窗口想象成一张面积有限的办公桌。桌面大小是固定的。如果在 Claude Code 开始实际工作之前，你就把厚厚的参考资料（也就是冗长的规则文件）堆满了桌面，那么留给真正需要处理的代码、文件和临时草稿的空间就所剩无几了。一旦桌面被塞满，Claude Code 就不得不‘扔掉’一些早期的关键信息来腾出空间，这往往会导致它‘忘记’重要指令或上下文细节。*”

根据 Anthropic 官方指导，用户应尽量将每个 `CLAUDE.md` 文件控制在 200 行以内；更长的文件会消耗更多上下文并降低遵循度。其底层逻辑是：指令密度与遵循概率成反比。指令越冗长复杂，每条规则被模型“深度关注”并“严格执行”的可能性就越低。

这与管理学中的“**信息过载**”原理高度吻合：当一本员工手册厚到没人能读完时，它的实际效力往往还不如一页提炼了核心要点的 A4 摘要。在 AI 协作中，简洁不仅是节省 token 的方式，更是确保指令被有效执行的关键策略。

那么，哪些内容值得写入 `CLAUDE.md`？Kage 给出了一套高效的“三问框架”作为筛选标准。

## 第一问：WHY（为什么这样做？）

**核心价值**：赋予 Claude 模型“举一反三”的能力。

这类信息旨在揭示决策背后的底层逻辑，帮助 Claude 理解项目的核心关切。一旦掌握了“为什么”，即使遇到未明确规定的边缘情况，模型也能基于原则做出正确推断。

**示例**：“*我们选择 Fastify 而非 Express，是因为 Fastify 基于 schema 的验证方式与本项目‘类型安全优先’的策略相契合。*”

**效果**：当 Claude 理解“类型安全”是最高优先级后，即便你在某处未指定库，它也会主动倾向类型定义更完善的方案，而非机械匹配关键词。

## 第二问：WHAT（做什么，不做什么？）

**核心价值**：界定不可逾越的“行为边界”。

这是最直接、最刚性的行为指令，明确划定了允许与禁止的界限。这类规则通常无需解释，要求绝对遵循与执行。绝大多数项目规范都属于此类。

**示例**：“*必须使用 pnpm 作为包管理器；禁止使用 npm 或 yarn。*”

**效果**：建立清晰“红线”，消除工具选择的不确定性，确保技术栈的一致性。

## 第三问：HOW（如何按步骤做？）

**核心价值**：规范“标准操作流程”。

这类内容适用于常用命令、固定流程或特定操作范式。模型无需理解命令背后的复杂原理，只需准确执行既定步骤。

**示例**：“*执行数据库迁移，运行* `pnpm db:migrate`*；填充测试数据，运行* `pnpm db:seed`*。*”

**效果**：将团队最佳实践转化为模型的肌肉记忆，减少因命令拼写错误或参数遗漏导致的低级错误，提升执行效率。

并非所有信息都需要同时回答这三个问题。大多数规则只需在 WHAT 层面用一句话清晰界定即可。只有那些容易被误解，或模型容易“好心办坏事”的关键决策，才值得在 WHY 层面补充深入解释。

如果你发现 `CLAUDE.md` 中每条规则后面都跟着冗长的背景解释或情感表达，那通常意味着内容已经过载。简洁，才是高效协作的关键。

接下来，让我们通过一个反面例子与随后的正面例子对比，快速建立直觉。

先看一个反面例子：含糊其辞的“正确废话”。

```
# Project Description

This is a backend service for an e-commerce platform. Our team has 5 members, and the project started development in March 2024.

It is written in Node.js and is a RESTful API service.

The code should be written well, with attention to performance.

Testing should be comprehensive.

Please follow best practices.
```





`CLAUDE.md`中充满了人类社交语境中常见的礼貌用语和模糊指令，这些对 AI 来说几乎等同于无用信息。

* **Written well**：好到什么程度？是指符合`Prettier`格式化，还是使用设计模式？
* “**Comprehensive**”：应该覆盖哪些场景？是单元测试、集成测试还是 E2E 测试？覆盖率要求是 80% 还是 100%？
* “Best practices”：谁的最佳实践？

读完这些，Claude 几乎获取不到任何可执行的信息。

最致命的问题是“follow best practices”这条指令。它看起来正确，实际上却是空洞的，因为 Claude 默认就会尝试遵循它所认为的“最佳实践”（基于海量互联网数据的统计模式）。核心问题在于，模型通用的“最佳实践”往往与你们团队特定的“技术约定”不同。

此外，像“团队有 5 个人”或“开发始于 2024 年 3 月”这样的元数据，对代码生成逻辑没有任何指导意义，却占用了宝贵的上下文空间。

现在，让我们看一个正面例子：一份精确的“操作手册”。

```
# Order Service API

## Technology Stack

- Node.js 20 + TypeScript 5.3 (strict mode)

- Fastify 4 framework (do not use Express)

- Prisma ORM + PostgreSQL 15

- pnpm 8 package management (do not use npm/yarn)

## Project Structure

- src/routes/ — Route definitions, only for parameter parsing and response construction

- src/services/ — Business logic layer, all core logic resides here

- src/repositories/ — Data access layer, encapsulates Prisma calls

- src/schemas/ — Zod validation schemas, one-to-one correspondence with routes

## Key Conventions

- Unified API response format: { success: boolean, data?: T, error?: { code: string, message: string } }

- Error codes use UPPER_SNAKE_CASE, e.g., ORDER_NOT_FOUND

- Database table names use snake_case plural form, primary key is UUID, must include created_at and updated_at

## Common Commands

pnpm dev           # Start development server, port 3000

pnpm test          # Run all tests (vitest)

pnpm build         # TypeScript compilation + type checking

pnpm db:migrate    # Run Prisma database migration
```





两份文档的差距一目了然。优秀的版本简洁、具体、可执行——每一条规则都直接锚定 Claude 的行为逻辑。

* 技术栈锁定：看到“Fastify 4 框架（不要用 Express）”，Claude 会立即排除 Express 相关的方案，基于 Fastify 生成代码
* 架构规范化：读到目录结构后，Claude 能准确判断新增路由文件应放在 `src/routes/` 目录下，而不是随意散落
* 零错误执行：有了常用命令及其注释，Claude 能准确执行构建和测试任务，省去了在 `npm run build` 和 `yarn build` 之间的猜测

除了手动编辑，Claude Code 还提供了两个便捷工具来帮助管理记忆文件。

首先是 `/init` 命令。在项目根目录下执行后，Claude Code 会自动扫描项目的目录结构、依赖配置文件（如 `package.json`、`pyproject.toml`、`Cargo.toml` 等）以及其他关键配置，然后生成一份 `CLAUDE.md` 草稿。这份草稿通常涵盖技术栈识别、目录结构概览和常用脚本命令，是一个很好的起点。

然而，对于 `/init` 无法自动推断的内容，仍需要人工审查和补充，例如分层架构的具体约束、统一的 API 响应格式，以及团队特有的命名约定。

这些“隐性知识”只存在于团队成员的大脑中，无法仅从配置文件中提取。必须通过人工干预将其显式化，以确保 AI 的行为完全符合团队预期。

其次是 `/memory` 命令，用于在对话过程中动态更新记忆。执行该命令后，Claude Code 会弹出一个文件选择器，列出当前所有可用的记忆文件供你选择和编辑。这些文件覆盖了不同的作用范围，如*表 2.1*所示。

| 作用范围 | 项目记忆 | `./CLAUDE.md` 或 `./.claude/CLAUDE.md` |
| --- | --- | --- |
| 团队共享：可纳入版本控制，适合统一团队规范 | 用户记忆 | 个人全局：个人偏好配置，对当前用户的所有项目生效 |
| 项目本地记忆 | `~/.claude/CLAUDE.md` | 个人本地：仅针对当前项目的个人偏好，通常被 `.gitignore` 自动排除，避免提交到代码仓库 |
| 系统文件 | `./CLAUDE.local.md` | 承载内容 |

表 2.1：项目记忆的作用范围

选择文件后，系统会直接在编辑器中打开该文件，供你立即修改。

假设你在交互过程中发现 Claude Code 又使用了 `moment.js`，而项目规范明确要求使用 `date-fns`。此时，你无需退出对话或手动查找文件，只需按以下步骤操作。

1.  输入 `/memory` 命令
2.  选择项目级的 `CLAUDE.md`
3.  添加规则：日期处理一律使用 `date-fns`，禁止使用 `moment.js`
4.  保存并关闭

下一次对话启动时，这条新规则会立即生效。如果这个偏好只是个人习惯，不希望影响团队，可以选择写入 `CLAUDE.local.md`——该文件默认通过 `.gitignore` 排除，不会被提交到代码仓库。

这种机制形成了一个“犯错 → 纠正 → 更新记忆 → 避免再犯”的正反馈循环。久而久之，你的记忆文件会越来越精准，Claude Code 在项目中的表现也会持续提升。

# 条件规则系统

随着项目不断演进，单一的 `CLAUDE.md` 文件终将面临容量瓶颈。

当一个项目涵盖前端组件规范、后端 API 设计、数据库迁移流程、测试策略（覆盖率/模拟）以及基础设施（Docker 和 CI/CD）的所有规则时，将所有内容堆砌到一个文件中会导致两个核心问题。

* **维护噩梦**：文件变得极其冗长，难以查找和更新特定规则。
* **注意力稀释**：例如，当 Claude 编写一个简单的前端组件时，其上下文被迫加载数据库迁移规范和 CI 配置要求等无关信息。这些噪音稀释了模型的注意力，降低了其处理当前任务的效率，甚至可能导致幻觉或关键前端约束被忽略。

这正是 `.claude/rules/` 目录的核心价值所在。它将记忆系统从“一本厚重、包罗万象的单一手册”进化为“一个可按需访问的模块化知识库”。

这种机制有两个核心优势。

* **领域分割**：允许将庞大的规则集拆分为多个主题特定的文件，每个文件聚焦于一个特定领域，极大地提升了可维护性。
* **智能激活**：通过在每个文件开头添加 YAML Frontmatter，并使用 `paths` 字段声明 Glob 模式，只有当 Claude Code 操作的文件路径匹配该模式时，对应的规则才会被激活并注入上下文。在无匹配的场景下，这些规则文件静静地存储在磁盘上，不消耗任何 token 或上下文空间。

来看一个示例。如果你需要规范测试文件的编写，可以创建如下规则文件。

```
<!-- .claude/rules/testing.md -->
---
paths:
- "**/*.test.ts"
- "**/*.spec.ts"
- "tests/**"
---
# Testing Guidelines

- Use vitest as the testing framework; jest is prohibited.
- Every test file must contain a describe block, and its name must match the module under test.
- Module mocking must be done uniformly with vi.mock(); manual mocking is strictly forbidden.
- All asynchronous tests must use async/await syntax; the use of done callbacks is prohibited.
- Test data must be generated through factory functions; hardcoding data in tests is strictly forbidden.
```

有了这个配置，当 Claude 编辑 `src/services/order-service.ts` 时，这些测试指南不会被加载；然而，一旦编写 `tests/order-service.test.ts`，这些规则就会自动生效。这种“按需加载”的设计理念与编程语言中的懒加载概念高度一致：无需在启动时预加载所有潜在资源——只在真正需要时动态调用。

同样的方法可以扩展到开发的其他领域。例如，数据库指南只在处理 `Prisma` 文件或数据访问层时加载，而 API 设计指南只在编辑路由或 schema 文件时生效。

```
<!-- .claude/rules/database.md -->
---
paths:
- "prisma/**"
- "src/repositories/**"
---
# Database Guidelines

- Once a migration file is committed to the main branch, it is strictly forbidden to modify it; only create new migrations as needed.
- All queries must go through the repository layer; the service layer is prohibited from directly invoking the Prisma client.
- Batch operations must be wrapped in a transaction. When writing more than 100 records at a time, batch processing is mandatory.
```

```
<!-- .claude/rules/api-design.md -->
---
paths:
- "src/routes/**"
- "src/schemas/**"
---
# API Design Guidelines

- Every route must be configured with a corresponding Zod schema for input parameter validation.
- List endpoints must uniformly support pagination parameters: page (starting from 1) and limit (default 20, maximum 100).
```





- 错误响应结构必须包含机器可读的 `code` 字段和人类可读的 `message` 字段。

小雪听后，精辟地总结道：*“这就好比为公司里不同岗位的员工量身定制专属操作手册——前台遵循前台流程，仓库员工遵守仓库规范，没人需要背下整个公司的规章制度。只有当你踏入仓库作业时，才需要翻开‘****仓库管理手册****’。”*

Kage 点头补充道：*“这种模块化的规则组织方式还有另一个重要的实践优势：在协作场景中，领域专家可以独立维护自己的规则文件，大大降低了 Git 合并冲突的可能性。前端工程师负责* `frontend.md`*，后端工程师维护* `api‑design.md`*，DBA 管理* `database.md`*——各管一摊，互不干扰。随着团队规模增长，这种‘分而治之’策略的价值会越来越明显。”*

**Kage 的笔记**

如果某个规则文件没有配置 `paths` 前置元数据，系统会将其视为全局无条件规则，在每个会话中强制加载——效果等同于直接写入主 `CLAUDE.md` 文件。因此，务必为每个规则文件设置精确的 `paths` 限制，才能真正发挥记忆系统“按需加载”的核心优势。

# 实践：三种典型项目配置

理论讲完了，让我们通过三个典型项目的 `CLAUDE.md` 实战示例来融会贯通。请特别留意它们各自不同的侧重点。

*   **前端项目**：聚焦组件约定与状态管理策略
*   **后端项目**：围绕分层架构设计与 API 规范
*   **数据项目**：强调数据处理流程的严谨性与实验可复现标准

这些差异绝非偶然，它们恰恰反映了不同技术领域中“最高频出错点”的分布。一份优秀的 `CLAUDE.md` 应当像精确制导武器一样，将火力集中在那些最容易出问题的关键区域。

## React 前端项目配置

在 **React** 前端项目中，组件粒度、状态归属和样式方案选型往往是分歧的高发地带，也是 Claude 最容易“自作主张”导致代码风格碎片化的地方。如果不明确指定状态管理策略，Claude 极有可能在同一个项目中混用 Redux、Context API 和局部状态，造成架构混乱。

以下是一个电商平台前端项目的 `CLAUDE.md` 实战示例。

````
# 电商平台前端项目规范

## 技术栈

- 核心：React 18 + TypeScript（严格模式）

- 构建/包管理：Vite 5 + pnpm

- 状态管理：

- 服务端状态：TanStack Query (React Query)

- 全局客户端状态：Zustand

- 样式方案：Tailwind CSS（严禁使用 CSS Modules 或 styled-components）

## 组件规范

- 范式：仅使用函数组件 + Hooks；禁止使用类组件。

- 命名：

- 文件使用 PascalCase（如 ProductCard.tsx）

- Props 类型命名为 ComponentName + Props（如 ProductCardProps）

- 目录结构：

- 通用 UI 原子组件 → src/components/ui/

- 业务领域组件 → src/components/features/

## 状态管理决策树

- 服务端数据（API 响应、缓存、同步）→ TanStack Query

- 全局客户端状态（主题切换、用户登录状态、临时购物车状态）→ Zustand

- 组件局部状态（表单输入值、下拉展开/收起、弹窗显示）→ useState

## 常用命令

```bash
pnpm dev          # 在 5173 端口启动开发服务器
pnpm build        # 生产环境构建
pnpm test         # 运行 vitest 单元测试
pnpm lint         # 运行 ESLint 代码检查
```
````

请注意“状态管理决策树”这一节。它不仅仅是一份技术栈名称清单，而是构建了一套“从场景到工具”的映射逻辑。这种“决策指引”远比简单的工具罗列有效得多——它直接告诉 Claude 在面对具体问题时“该选哪把钥匙”，从而从根本上杜绝了架构风格的漂移。

## Node.js 后端项目配置

**Node.js** 后端项目的命脉在于分层架构中清晰的职责边界和数据格式的统一。后端代码的“正确性”往往取决于每一层是否严格遵守依赖规则——如果路由层绕过服务层和仓储层直接调用 Prisma，业务逻辑就会泄漏到基础设施层，导致后期维护的泥潭。

以下是一个订单微服务的 `CLAUDE.md` 实战示例。

````
# 订单微服务后端规范

## 技术栈

- 运行时：Node.js 20 + TypeScript 5.3

- 框架：Fastify 4（严禁使用 Express）

- 数据库：Prisma ORM + PostgreSQL 15

- 工具链：pnpm（包管理）、Zod（数据校验）

## 分层架构（严格单向依赖）

- routes/ → 解析请求参数、校验输入、调用控制器、构造 HTTP 响应

- controllers/ → 编排服务调用、处理 HTTP 语义、捕获和转换异常

- services/ → 实现核心业务规则、管理事务、协调多个仓储

- repositories/ → 封装 Prisma 查询、数据映射（严禁其他层直接调用 Prisma）

## API 响应规范

所有端点必须返回统一格式：{ success: boolean, data?: T, error?: { code: string, message: string } }

分页规范：page（从 1 开始）、limit（默认 20，最大 100）

错误码：必须使用 UPPER_SNAKE_CASE 格式

## 常用命令

```bash
pnpm dev           # 在 3000 端口启动开发服务器
pnpm test          # 运行 vitest 测试套件
pnpm build         # 使用 tsc 编译
pnpm db:migrate    # 执行 Prisma 数据库迁移
pnpm db:studio     # 启动 Prisma Studio 可视化界面
```
````

在这份配置中，最关键的一条规则是“严禁其他层直接调用 Prisma”。这不仅仅是一个编码风格偏好问题，而是一条不可违背的架构原则，直接决定了项目的可维护性和生命周期。

## Python 数据项目配置

**Python** 数据项目呈现出与 Web 开发截然不同的面貌。如果 Claude 不熟悉数据科学领域的工程惯例，很容易生成违反最佳实践的代码——例如滥用 `inplace=True` 原地修改 DataFrame，破坏数据流的可追溯性，或混用 `None` 和 `pd.NA`，导致缺失值处理逻辑混乱。

以下是一个用户行为分析系统的 `CLAUDE.md` 规范实战示例。

````
# 用户行为分析系统规范

## 技术栈

- 环境：Python 3.11 + uv（依赖与虚拟环境管理）

- 核心库：

- 数据处理：pandas 2.x

- 机器学习：scikit-learn

- 可视化：matplotlib + seaborn

- 代码标准：

- 类型注解：严格使用 typing 模块

- 文档字符串：强制使用 Google Style

## 项目结构

- notebooks/：探索性数据分析（Jupyter），命名格式为“编号-描述.ipynb”，如 01-data-exploration.ipynb

- src/data/：数据加载、清洗与管道构建

- src/features/：特征工程逻辑
````




- `src/models/`：模型定义、训练工作流和评估指标

- `tests/`：`pytest` 单元测试套件

## 数据处理约定

- 缺失值：仅使用`pd.NA`；不要使用`None`或裸`np.nan`

- 日期列：始终转换为`datetime64`类型；输出/存储格式必须为`YYYY-MM-DD`

- 内存优化：低基数分类变量必须显式转换为`category`类型

- `DataFrame`：不要使用`inplace=True`；所有转换操作必须返回一个新的`DataFrame`副本

## 常用命令

```bash
uv sync                       # 安装/同步依赖环境
uv run pytest                 # 运行测试套件
uv run jupyter lab            # 启动 Jupyter Lab
uv run python -m src.train    # 执行模型训练脚本
```




Kage 指着那三个配置文件，给出了他的最终总结和点评：

“*你注意到了吗？这三个文件，每一个都只保持在二三十行，但每一行都是精华——每一条都是干货。它们放弃了 Claude 已经烂熟于心的通用知识——你不需要解释什么是 React，也不需要重申 Python 的缩进规则——而是精准聚焦于特定领域和这个项目独有的约定与决策。*”

这就是编写 `CLAUDE.md` 的核心法则：只记录 Claude 必须知道、但无法自行推断的关键信息。请始终记住这个黄金测试：如果删除某条规则后 Claude 依然能正确行事，那这条规则就不属于 `CLAUDE.md`。

# 小结

回到本章开头的场景：每次小冰开启新会话，Claude 都习惯性地使用 `Express` 而非 `Fastify`，使用 `npm` 而非 `pnpm`。现在我们明白了，这不是 Claude 模型能力的局限，而是因为它缺少一份至关重要的“入职手册”。

`CLAUDE.md` 正是解决这一痛点的关键机制。它将团队的技术选型、编码规范、项目结构和常用命令固化为一份机器可读的配置文档。每当会话启动，Claude Code 便自动加载这些信息，彻底免除反复纠正的繁琐，直接进入高效协作状态。

五层记忆架构的设计，深刻体现了“关注点分离”这一经典软件工程原则。

- **企业策略层**：严守安全底线，由组织统一管理
- **用户层**：捕捉个人偏好，尊重个体差异
- **项目层**：承载团队共识，确保协作一致
- **规则目录层**：实现模块化与条件化加载
- **本地层**：存放私人笔记，提供灵活的临时上下文

每一层都有清晰的职责边界和管理方式，在有力支撑团队协作的同时，充分尊重个人习惯。

其中，条件化规则系统是一项尤为精妙的工程设计。它利用 Glob 模式精确定义规则的生效范围，实现了“在恰当的时刻将恰当的信息送入上下文”的智能机制。这种设计彻底摒弃了将所有规则一股脑堆在 Claude 面前的粗暴做法，确保了上下文的清爽高效。

至于编写范式，“少即是多”绝非一句空洞的口号——它背后有着严肃的技术逻辑。上下文窗口是大模型最宝贵的战略资源，每一个 token 都承载着高昂的机会成本。强行将无关信息塞入 `CLAUDE.md`，就好比在早高峰时段往地铁车厢里硬塞非乘客——不仅占用了宝贵的空间，还将真正需要上车的“关键信息”挤在门外，无法进入模型的视野。

一份优秀的 `CLAUDE.md` 应当如同一份简洁的作战备忘录，严格遵循以下三条铁律。

- **必要性**：每一条都是 Claude 必须知道且无法自行推断的信息
- **可操作性**：每一条都是具体、明确、可执行的指令
- **脆弱性测试**：删除任何一条，都会直接导致 Claude 犯错或偏离预期

在*第 3 章*中，我们将正式踏入 Skills 的广阔世界。如果说 `CLAUDE.md` 是让 Claude 建立基础认知的“入职手册”，那么 Skills 就是为它量身定制的“专业培训教材”。当你的“入职手册”变得越来越臃肿，试图将所有领域知识都塞进去时，不仅浪费了上下文资源，还降低了核心规则的响应效率。此时，你需要一种更优雅、更智能的按需加载机制——这正是 Skills 系统的核心价值所在：在真正需要之时，将专业知识精准注入对话上下文。

# 思考题

1. 请识别你当前项目中 Claude 必须知道的核心信息。尝试用“三问框架”（WHY、WHAT、HOW）进行拆解，并起草一份不超过 30 行的 `CLAUDE.md`。起草完毕后，逐行审视：如果删除这一行，Claude 会犯什么错误？如果你无法回答这个问题，那一行或许就是多余的。
2. 在团队协作中，如果开发者对 `CLAUDE.md` 中的规则存在分歧（例如缩进使用 2 个还是 4 个空格），你如何利用“五层记忆架构”来化解？请具体说明：哪些规则必须放在项目层以强制执行统一标准，哪些偏好适合放在用户层或本地层以保留个人选择自由？
3. 在你的项目中运行 `/init` 命令自动生成 `CLAUDE.md`，然后将其与你手动编写的版本进行深度对比：哪些显性信息机器能够准确推断？而哪些“隐性知识”必须依赖人工补充？

# 3

