# 第二章：1

# 开始使用 ChatGPT API 进行 NLP 任务

**自然语言处理** (**NLP****)是人工智能（**AI****）领域的一个分支，专注于通过自然语言进行人机交互。多年来，自然语言处理在语言理解领域取得了显著的进步；**ChatGPT**是近年来在 NLP 领域获得显著流行的一种革命性 NLP 工具。

ChatGPT 是由**OpenAI**开发的高级 AI 语言模型，并在包括书籍、文章和网页在内的各种文本的庞大数据集上进行了训练。凭借其理解和生成类似人类文本的能力，ChatGPT 已成为许多 NLP 应用（包括聊天机器人、语言翻译和内容生成）的首选工具。

本章中，我们将探讨 ChatGPT 的基础知识以及如何使用它来执行 NLP 任务。我们将从介绍 ChatGPT 及其对 NLP 领域的影响开始。 然后我们将探讨如何从网络使用 ChatGPT 及其优势。 接着，我们将学习如何开始使用 ChatGPT API，包括创建账户和生成 API 密钥。 之后，我们将了解如何设置开发环境以与 ChatGPT API 一起工作。 最后，我们将通过一个简单的 ChatGPT API 响应示例来了解该工具的基本功能。

本章中，我们将涵盖以下主题： 以下内容：

+   ChatGPT 革命 

+   从网络使用 ChatGPT 

+   开始使用 ChatGPT API 

+   设置您的 Python开发环境 

+   简单的 ChatGPT API 响应 

到本章结束时，您将对该章内容有扎实的理解，了解 ChatGPT 及其如何高效地执行 NLP任务。 

# 技术要求

为了充分利用本章内容，您需要一些基本工具来处理 Python 代码和 ChatGPT API。本章将指导您完成所有软件安装和注册。  

您需要以下内容： 以下：

+   在您的计算机上安装了Python 3.7 或更高版本 

+   一个 OpenAI API 密钥，可以通过注册一个 OpenAI 账户来获取

+   一个代码编辑器，例如 **VSCode** (推荐)，用于编写和运行 Python 代码

+   基本的 Python 编程经验 使用 Python

本章的代码示例可以在 GitHub 上找到 在 [https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API](https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API)。

# ChatGPT 革命

ChatGPT 是由 OpenAI 开发的高级 AI 语言模型，并在 NLP 领域产生了重大影响。 该模型基于 transformer 架构，并在包括书籍、文章和 网页在内的各种文本的大量数据集上进行了训练。

ChatGPT 的一个关键特性是它能够生成连贯且上下文适当的文本。 与之前的 NLP 模型不同，ChatGPT 对语言有更广泛的理解，并且可以生成与人类生成文本在风格和结构上相似的文本。 这一特性使 ChatGPT 成为各种应用的宝贵工具，包括对话 AI 和 内容创作。

ChatGPT 在对话 AI 领域也取得了显著进展，其中它被用于开发能够与人类自然互动的聊天机器人。 凭借其理解上下文并生成与人类生成文本风格相似文本的能力，ChatGPT 已成为开发 对话 AI的必备工具。

**大型语言模型** (**LLMs**)如 GPT-3 的出现，彻底改变了聊天机器人的格局。 在 LLMs 之前，聊天机器人的功能有限，依赖于基于规则的系统，并具有预定义的响应。 这些聊天机器人缺乏上下文理解，难以进行有意义的对话。 然而，基于 LLMs 的聊天机器人经历了显著的变化。 这些模型能够理解复杂的查询，生成连贯且细微的响应，并拥有更广泛的知识库。 它们表现出改进的上下文理解能力，从用户交互中学习，并不断改进其性能。 基于 LLMs 的聊天机器人通过提供更自然和个性化的交互，提升了用户体验，展示了聊天机器人技术的显著进步。

ChatGPT 在 NLP 领域有着悠久而成功的历史。 该模型在多年中经历了多次改进，包括以下内容：

+   **GPT-1 (2018)**：拥有 1.17 亿个参数，并在多样化的网页集上进行训练。 它在各种 NLP 任务中表现出色，包括问答、情感分析和 语言翻译。

+   **GPT-2 (2019)**：拥有 15 亿个参数，并在超过 800 万个网页上进行训练。 它在语言理解和生成方面取得了显著的进步，并成为各种 NLP 应用中的广泛使用工具。

+   **GPT-3 (2020)**：拥有创纪录的 1750 亿个参数，为语言理解和生成设定了新的基准。 它被用于各种应用，包括聊天机器人、语言翻译和 内容创作。

+   **GPT-3.5**：在 OpenAI 的持续优化和改进后发布。 在成本和性能比较中是目前最佳价值模型。

+   **GPT-4**：能够以更高的精度解决难题，这得益于其更广泛的一般知识和 问题解决能力。

开发者可以利用 **生成式预训练变换器** (**GPT**) 模型的力量，而无需从头开始训练自己的 模型。 这可以节省大量时间和资源，尤其是对于小型团队或 个人开发者。

在下一节中，你将学习如何从网页上使用 ChatGPT。你将学习如何创建一个 OpenAI 账户并探索 ChatGPT 的 网页界面。

# 从网页上使用 ChatGPT

通过 OpenAI 网站与 ChatGPT 交互 非常简单。 OpenAI 提供 一个基于网页的界面，可以在 [https://chat.openai.com](https://chat.openai.com)找到，使用户能够无需 任何先前的编码知识或设置即可与模型互动。 一旦访问网站，您就可以开始输入您的问题或提示，模型将产生其最佳可能的答案或生成的文本。 值得注意的是，网页上的 ChatGPT 还向用户提供各种设置和选项，使他们能够跟踪对话的上下文并保存与 AI 的所有互动历史。 这种功能丰富的基于网页的 AI 交互方法使用户能够轻松地尝试模型的功能，并深入了解其广泛的应用潜力。 要开始使用基于网页的界面，您需要通过 OpenAI 注册一个账户，我们将在下一节中详细介绍。 一旦创建了账户，您就可以访问网页界面，开始探索模型的功能，包括各种设置和选项来增强您的 AI 交互。

## 创建 OpenAI 账户

在使用 ChatGPT 或 ChatGPT API 之前，您必须在 OpenAI 网站上创建一个账户，这将使您能够访问公司开发的所有工具。 为此，您可以访问 [https://chat.openai.com](https://chat.openai.com)，在那里您将被要求登录或注册一个新账户，如图 *图 1**.1*所示：

![图 1.1 – OpenAI 欢迎窗口](img/B22414_01_1.jpg)

图 1.1 – OpenAI 欢迎窗口

只需点击 **注册** 按钮，按照提示进入注册窗口（见 *图 1**.2*）。 从那里，您可以选择输入您的电子邮件地址并点击 **继续**，或者您可以选择使用 Google 或 Microsoft 账户注册。 完成此步骤后，您可以选择一个密码并验证您的电子邮件，就像在其他任何网站的注册过程中一样 注册过程。

完成注册 流程后，您就可以开始探索 ChatGPT 的全部功能。 只需点击 **登录** 按钮，如图 图 1**.1* 所示，并在登录窗口中输入您的凭证。 登录成功后，您将获得对 ChatGPT 以及所有其他 OpenAI 产品的完全访问权限。 通过这种简单易用的访问方式，您可以无缝探索 ChatGPT 的全部功能，并亲眼见证它为何成为 NLP 任务如此强大的工具 。

![图 1.2 – OpenAI 注册窗口](img/B22414_01_2.jpg)

图 1.2 – OpenAI 注册窗口

现在我们可以更详细地探索 ChatGPT 网页界面的功能和特性。 我们将向您展示如何导航界面，并充分利用其多样化的选项，以从 AI 模型中获得最佳结果 。

## ChatGPT 网页界面

ChatGPT 网页界面 允许用户与 AI 模型交互。 一旦用户注册服务并登录，他们可以将文本提示或问题输入到聊天窗口中，并从模型那里获得回应。 您可以使用 **发送消息** 文本字段向 ChatGPT 提问。 聊天窗口还会显示之前的消息和提示，使用户能够跟踪对话的上下文，如图 F图 1**.3* 所示：

![图 1.3 – ChatGPT 跟随对话上下文](img/B22414_01_3.jpg)

图 1.3 – ChatGPT 跟随对话上下文

除了这些，ChatGPT 允许用户轻松记录与模型交互的历史。用户的聊天记录会自动保存，稍后可以从左侧侧边栏访问以供参考或分析。 此功能对于研究人员或希望跟踪与模型对话并评估其性能随时间变化的人来说特别有用。 聊天记录还可以用于训练其他模型或比较不同模型的性能。 你现在能够区分并使用不同 ChatGPT 模型的进步。 您还可以通过网页使用 ChatGPT，包括创建账户和生成 API 密钥。 ChatGPT API 灵活且可定制，可以节省开发者的时间和资源，使其成为聊天机器人、虚拟助手和自动内容生成的理想选择。 在下一节中，您将学习如何使用 Python 轻松访问 ChatGPT API。 。

# 开始使用 ChatGPT API

ChatGPT API **是由 OpenAI 开发的，允许开发者与 GPT 模型进行 NLP 任务交互。 此 API 提供了一个易于使用的界面，用于生成文本、完成提示、回答问题和执行其他 NLP 任务，使用的是最先进的机器学习模型。**

ChatGPT API 用于聊天机器人、虚拟助手和自动内容生成。 它还可以用于语言翻译、情感分析和内容分类。 API 灵活且可定制，允许开发者针对特定用例微调模型性能。 现在让我们了解获取 API 密钥的过程。 这是从您自己的应用程序访问 ChatGPT API 的第一步。

## 获取 API 密钥

要使用 ChatGPT API，您需要获取 一个 API 密钥。 这可以从 OpenAI 获取。 此密钥将允许您验证对 API 的请求，并确保只有授权用户才能访问 您的账户。

要获取 API 密钥，您必须 使用 ChatGPT 凭据访问 [https://platform.openai.com](https://platform.openai.com) 的 OpenAI 平台。 OpenAI 平台页面提供了一个管理您的 OpenAI 资源的中心枢纽。 一旦 您注册成功，您就可以导航到 API 访问页面： [https://platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys)。在 API 访问页面上，您可以管理 ChatGPT API 和其他 OpenAI 服务的 API 密钥。 您可以生成新的 API 密钥，查看和编辑每个密钥的权限，并监控 API 的使用情况。 该页面提供了您 API 密钥的清晰概述，包括它们的名称、类型和创建日期，并允许您根据需要轻松撤销或重新生成密钥 。

点击 **+ 创建新的密钥** 按钮，您的 API 密钥将被创建：

![图 1.4 – 创建 API 密钥](img/B22414_01_4.jpg)

图 1.4 – 创建 API 密钥

创建您的 API 密钥后，您将只有一次机会复制它（见 *图 1**.5*）。 保护您的 API 密钥的安全和机密性非常重要，因为任何能够访问您的密钥的人都有可能访问您的账户并使用您的资源。 您还应该小心不要将密钥与未经授权的用户共享，并避免将密钥提交到公共存储库或通过不安全的渠道以明文形式共享。

![图 1.5 – 保存 API 密钥](img/B22414_01_5.jpg)

图 1.5 – 保存 API 密钥

在我们的应用程序和脚本中复制和粘贴 API 密钥 使我们能够使用 ChatGPT API。 现在，让我们来探讨 ChatGPT 的 token 及其在 OpenAI 定价模型中的作用。

## API token 和定价

在使用 ChatGPT API 时，理解 token 的概念非常重要 至关重要 。 Token 是模型用于处理和理解的文本的基本单元。

标记可以是单词或字符块，通过将文本分解成更小的部分来创建。 例如，单词 *汉堡* 可以被分解为 *ham*, *bur*, 和 *ger*，而像 *梨* 这样的较短的单词是一个单独的标记。 标记也可以以空白字符开头，例如 *hello* 或 *bye*。

在 API 请求中使用的标记数量取决于输入和输出文本的长度。 一般来说，一个标记大约对应于 4 个字符或 0.75 个英文单词。 需要注意的是，文本提示和生成的响应的总长度不能超过模型的最大上下文长度。 *表 1.1* 显示了某些流行 ChatGPT 模型的标记限制。

| **模型** | **最大标记数** |
| --- | --- |
| gpt-4 | 8,192 标记 |
| gpt-4-32k | 32,768 标记 |
| gpt-3.5-turbo | 4,096 标记 |
| gpt-3.5-turbo-0125 | 16,385 标记 |

表 1.1 – API 模型标记限制

要了解更多关于文本如何转换为标记的信息 ，您可以查看 OpenAI 的**标记化工具** 。 标记化工具是 OpenAI 提供的一个有用的资源，用于了解文本如何转换为标记。 该工具将文本分解成单个标记，并显示它们对应的字节偏移量，这对于分析和理解 您的文本结构非常有用。

您可以在 [https://platform.openai.com/tokenizer](https://platform.openai.com/tokenizer)] 找到标记化工具。要使用标记化工具，只需 输入您想要分析的文本并选择合适的模型和设置。 然后，该工具将生成一个标记列表，以及它们对应的字节偏移量（见 *图 1**.6*）。

![图 1.6 – 标记化工具](img/B22414_01_6.jpg)

图 1.6 – 标记化工具

ChatGPT API 的定价 是按照每处理 1,000 个令牌计费 ，并且每个 API 请求有最低费用。 这意味着你的输入和输出文本越长，处理的令牌就越多，费用也就越高。 *表 1.2* 显示了截至 2024 年 5 月，处理 1,000 个令牌的当前成本，适用于几个常用的 ChatGPT 模型。

| **模型** | **提示** | **完成** |
| --- | --- | --- |
| gpt-4 | $0.03 / 1K 令牌 | $0.06 / 1K 令牌 |
| gpt-4-32k | $0.06 / 1K 令牌 | $0.12 / 1K 令牌 |
| gpt-3.5-turbo | $0.0005 / 1K 令牌 | $0.0015 / 1K 令牌 |
| gpt-3.5-turbo-0125 | $0.0005 / 1K 令牌 | $0.0015 / 1K 令牌 |

表 1.2 – ChatGPT API 模型定价

重要提示

注意你的令牌使用情况，以避免意外收费。 你可以通过 **使用情况** 仪表板 在 [https://platform.openai.com/account/usage](https://platform.openai.com/account/usage)上跟踪你的使用情况和监控 账单信息。

正如你所见，ChatGPT 是一个易于使用的界面，允许开发者与 GPT 模型进行 NLP 任务交互。 令牌是模型用于处理和理解输入和输出文本的基本文本单元。 ChatGPT API 的定价结构基于处理的令牌数量，每个 API 请求有最低费用。

在下一节中，我们将介绍 如何设置 Python 开发环境 以使用 ChatGPT API。 这包括安装 Python 和 VSCode 的 `venv` 模块以及如何在 VSCode 中访问 **终端** 选项卡。

# 设置你的 Python 开发环境

在我们开始编写 我们的第一个代码之前，创建一个工作环境并安装任何必要的依赖项是很重要的。 幸运的是，Python 有一个出色的工具系统来管理虚拟环境。 Python 中的虚拟环境是一个复杂的话题，但就本书的目的而言，只需知道它们是独立的 Python 环境，与您的全局 Python 安装分开即可。 这种隔离允许开发者使用不同的 Python 版本，在环境中安装包，并管理项目依赖项，而不会干扰 Python 的 全局安装。

为了在您的 NLP 项目中使用 ChatGPT API，您需要设置您的 Python 开发环境。 本节将指导您完成启动所需的步骤，包括以下内容：

+   安装 Python

+   安装 VSCode IDE

+   安装 `pip`

+   设置 虚拟环境

+   安装所需的 Python 包

一个正确配置的开发环境 将允许您向 ChatGPT 发送 API 请求，并在您的 Python 代码中处理生成的响应。

## 安装 Python 和 VSCode IDE

Python 是一种流行的 编程语言 ，被广泛用于各种目的，包括机器学习和数据分析。 您可以从官方网站下载并安装 Python 的最新 版本， [https://www.python.org/downloads/](https://www.python.org/downloads/) (见 *图 1.7**.7*). 为了验证 Python 是否已安装，您可以在设备上的终端中输入以下内容：

```py
 $ python --version
```

一旦您下载了 Python 安装程序，只需按照说明在您的计算机上安装 Python 即可。 下一步是选择一个 IDE 来 与之配合。

![Figure 1.7 – Python installation](img/B22414_01_7.jpg)

Figure 1.7 – Python installation

Python 开发者中一个流行的选择是 VSCode，这是一个强大且用户友好的 IDE。 VSCode 提供了一系列功能，使开发 Python 应用程序变得容易，包括代码补全、调试工具和项目 管理功能。

要安装 VSCode，您可以从 VSCode 网站免费 下载它，[https://code.visualstudio.com/download](https://code.visualstudio.com/download)。下载安装程序后，只需 按照说明在您的计算机上安装 VSCode。

## 设置 Python 虚拟环境

设置 Python 虚拟环境 是创建项目独立开发环境的关键步骤。 通过创建虚拟环境，您可以在不干扰系统上其他项目的情况下安装特定版本的 Python 包和依赖项。 您的系统。

为您的 ChatGPT 应用程序项目创建特定的 Python 虚拟环境是一种推荐的最佳实践。 这样做可以确保所有包和依赖项都保存在您的项目文件夹中，而不是弄乱您计算机的全局 Python 安装。 这种方法为项目开发和执行提供了一个更组织化和隔离的环境。

VSCode IDE 允许您使用其内置终端设置 Python 虚拟环境。 您可以通过以下步骤设置您的 VSCode 项目：

1.  在您的计算机上启动 VSCode。

1.  使用文件资源管理器或 `ChatGPTResponse`。

1.  创建文件夹后，通过选择**文件** | **打开文件夹** 并导航到您刚刚创建的文件夹来在 VSCode 中打开它。

1.  在打开的文件夹内，创建一个新的 Python 文件，您将在其中编写代码。 您可以通过在 `.py` 扩展名上右键单击，例如 `app.py`。

1.  通过选择**视图** | **终端** 从菜单中打开 VSCode 的集成终端。

1.  进入您的项目文件夹后，运行以下命令创建虚拟环境。 第二个 `venv` 参数指的是您的虚拟环境文件夹名称：

    ```py
    venv that contains the virtual environment (see *Figure 1**.8*). You can replace venv with any name you want.
    ```

1.  要激活虚拟环境，请运行以下 终端命令：

    +   在 Windows 上，运行以下终端命令以激活 虚拟环境：

        ```py
        $ venv\Scripts\activate.bat
        ```

    +   在 macOS 或 Linux 上，运行以下终端命令来激活虚拟环境：

        ```py
        $ source venv/bin/activate
        ```

![图 1.8 – VSCode 项目层次结构](img/B22414_01_8.jpg)

图 1.8 – VSCode 项目层次结构

一旦激活，您应该在命令提示符或终端中看到虚拟环境的名称，如图所示。 从这里，您可以安装您项目所需的任何包或依赖项，而不会干扰系统上的其他 Python 安装： 。

```py
 ChatGPTResponse % source venv/bin/activate (venv) ChatGPTResponse % (venv) ChatGPTResponse %
```

重要提示

为了保持一致性，您需要为每个新创建的 Python 项目激活虚拟环境。

安装任何 Python 包的关键组件是 pip 。让我们看看如何检查 pip 是否已安装在您的系统上，以及如果需要的话如何安装它 。

## pip 包安装器

pip 是 Python 的包安装器。 它允许您轻松安装和管理第三方 Python 库和包，例如 `openai 。如果您使用的是 Python 的较新版本， `pip 应该已经安装。 您可以通过打开命令提示符或终端并键入 `pip 然后按下 *Enter 键来检查您的系统上是否已安装 pip。 如果 `pip 已安装，您应该会看到一些描述其用法和命令的输出。

如果您的系统上没有安装 pip，您可以通过以下步骤安装它： 。

1.  首先，从 get-pip.py 脚本从官方 Python 网站下载： [https://bootstrap.pypa.io/get-pip.py](https://bootstrap.pypa.io/get-pip.py) 。

1.  将文件保存到您容易访问的位置，例如桌面或 下载文件夹。

1.  打开命令提示符或终端，导航到您保存 get-pip.py 文件的目录。

1.  运行以下命令来安装 pip： python get-pip.py 。

1.  安装完成后，您可以通过在命令提示符或终端中键入 pip 并按下 *Enter 键来验证 pip 是否已安装。

你现在应该已经在你的系统上安装了 pip，并且可以使用它来安装 Python 的包和库 。

这是一份关于如何在 NLP 项目中使用 ChatGPT API 设置 Python 开发环境的完整指南。 设置虚拟环境是在创建一个独立开发环境的过程中一个关键步骤 为你的项目。 你现在可以准备使用 Python 与 ChatGPT API 进行交互，完成你的第一个实践练习，以与 OpenAI 库进行交互。

# 一个简单的 ChatGPT API 响应

使用 Python 与 ChatGPT API 结合是一个相对简单的过程。首先，你需要确保你已经创建了上一节中描述的名为 `CHATGPTRESPONSE` 的目录（参见 *图 1**.8*）。 一旦设置好，你就可以使用 OpenAI Python 库与 ChatGPT API 进行交互。 在 VSCode 中打开一个新的终端，确保你处于你的项目文件夹和虚拟环境中，并安装 `openai` 包：

```py
 $ pip install openai
```

要开始，你需要将 `openai` 库导入到你的 `app.py` 文件中。 此外，你还需要提供 `你的 OpenAI API 密钥。` 你可以通过遵循本书前几节中概述的步骤从 OpenAI 网站获得 API 密钥。 然后，你需要在 Python 代码中将它设置为参数。 一旦你的 API 密钥设置好，你就可以开始与 `ChatGPT API`进行交互：

```py
 from openai import OpenAI
client = OpenAI(api_key="YOUR_API_KEY")
```

将 `YOUR_API_KEY` 替换为你从 OpenAI 平台页面获得的 API 密钥。 通常，将 API 密钥保存在与你的主要代码相同的文件中并不是一个好的做法。 在下一章中，我们将讨论如何安全地保存这些密钥。 现在，你可以使用 `input()` 函数向用户提问：

```py
input() function is used to prompt the user to input a question they would like to ask the ChatGPT API. The function takes a string as an argument, which is displayed to the user when the program is run. In this case, the question string is "What would you like to ask ChatGPT?". When the user types their question and presses *Enter*, the input() function will return the string that the user typed. This string is then assigned to the question variable.
			To pass the user question from your Python script to ChatGPT, you will need to use the ChatGPT API `create` function:

```

response = client.chat.completions.create(

    model="gpt-3.5-turbo",

    messages=[

        {"role": "user", "content": f"{question}"},

    ]

    max_tokens=512,

    n=1,

    stop=None,

    temperature=0.8,

)

```py

			The `client.chat.completions.create()` function in the code is used to send a request to the ChatGPT API to generate the completion of the user’s input prompt. The `model` parameter allows us to specify the specific variant or version of the GPT model we want to utilize for the request, and in this case, it is set to `"gpt-3.5-turbo"`. `messages` is a list of dictionaries that specifies the text prompts for the API to complete, which is the user’s input question in this case.
			The `max_tokens` parameter specifies the maximum number of tokens the request and the response should contain together. The `n` parameter specifies the number of completions to generate for the prompt. The `stop` parameter specifies the sequence where the API should stop generating the response.
			The `temperature` parameter controls the creativity of the generated response. It ranges from `0` to `1`. Higher values will result in more creative but potentially less coherent responses, while lower values will result in more predictable but potentially less interesting responses. Later in the book, we will delve into how these parameters impact the responses received from ChatGPT.
			The function returns a `ChatCompletion` object containing the generated response from the ChatGPT API, which then can be accessed and printed to the console in the next line of code:

```

打印(response)

```py

			From the terminal, you can run the project through the `app.py` file using the following command:

```

$ python app.py

```py

			You should receive a message in the **TERMINAL** window that asks you to write a question for ChatGPT (see *Figure 1**.9*).
			![Figure 1.9 – Asking ChatGPT a question](img/B22414_01_9.jpg)

			Figure 1.9 – Asking ChatGPT a question
			Once you have entered your question, press the *Enter* key to submit your request to the ChatGPT API. The response generated by the ChatGPT API model will be displayed in the TERMINAL window as a complete `ChatCompletion` object:

```

ChatCompletion(

    id='chatcmpl-8xoDCiDPy0ZUzjLtJMTTHZ0keGeEh',

    choices=[

        Choice(

            finish_reason='length',

            index=0,

            logprobs=None,

            message=ChatCompletionMessage(

                content='游泳是一种极佳的锻炼方式，也是一项宝贵的生存技能。以下是一些基本步骤来…',

                role='assistant',

                function_call=None,

                tool_calls=None

            )

        )

    ],

    created=1709265058,

    model='gpt-3.5-turbo-0125',

    object='chat.completion',

    system_fingerprint='fp_86156a94a0',

    usage=CompletionUsage(

        completion_tokens=20,

        prompt_tokens=11,

        total_tokens=31

    )

)

```py

			This response produced by the OpenAI API contains information about the response generated by the GPT-3.5 model. This response consists of the following fields:

				*   The `id` field is a unique identifier for the API request that generated this response.
				*   The `choices` field contains an array of objects with the generated responses, which in this case only contains one response object as the parameter `n=1`.
				*   The `finish_reason` field indicates the reason why the response was generated; in this case, it was because the model reached the `stop` condition provided in the request. Since in our case `stop=None`, the full response from the ChatGPT API was returned.
				*   The `index` variable of this particular choice in the array of choices, we can see only `index=0` as `n=0`.
				*   The `created` field specifies the Unix timestamp of when the response was created.
				*   The `model` field specifies the GPT-3.5 model that was used to generate the response.
				*   The `object` field specifies the type of object that was returned, which in this case is `chat.completion`.
				*   `system_fingerprint` is an identifier for the current combination of model weights and infrastructure.

			The `ChatCompletionMessage` object includes the information related to the specific ChatGPT response. It includes the following:

				*   `content`: The completed answer generated by the model
				*   `role`: Indicates the role of the message; in this case, it’s from the assistant
				*   `function_call`: Any function calls associated with the completion
				*   `tool_calls`: Any tool calls associated with the completion

			Finally, the `usage` field provides information about the resource usage of the API request. It contains information about the number of tokens used for the completion, the number of tokens in the prompt, and the total number of tokens used.
			The most important parameter from the response is the `message` field, which contains the answer to the question asked to the ChatGPT API. This is why most API users would like to access only that parameter from the `ChatCompletion` object. You can easily separate the text from the main body as follows:

```

answer = response.choices[0].message.content

print("OpenAI:" + answer)

```py

			By following this approach, you can guarantee that the `answer` variable will hold the complete ChatGPT API text response, which you can then print to verify. Keep in mind that ChatGPT responses can significantly differ depending on the input, making each response unique:

```

OpenAI:

游泳是一种极佳的锻炼方式，也是一项宝贵的生存技能。以下是一些基本步骤：

1\. 首先进入水中。如果你在游泳池游泳，你可以从侧面、梯子或跳板进入水中。如果你在海边或湖中游泳，你可以从岸边或码头进入水中。2\. 深吸一口气，然后慢慢呼出。这有助于你放松并准备游泳。

```py

			ChatGPT can be employed for a multitude of NLP tasks across a wide array of topics accessible in the vast expanse of information. We can utilize our script to inquire about diverse areas of expertise and receive advanced responses from the ChatGPT API, as demonstrated here:

				*   Solving mathematical problems:
    *   **User**: What is the square root of 256?
    *   **ChatGPT**: The square root of 256 is 16.
				*   Performing sentiment analysis:
    *   **User**: Analyze the sentiment of the sentence “I had a great day today!”
    *   **ChatGPT**: The sentiment of the sentence “I had a great day today!” is positive.
				*   Helping with programming tasks:
    *   `variable_name =` `value`.
				*   Improving scientific knowledge:
    *   **User**: What is the process of photosynthesis in plants?
    *   **ChatGPT**: The process of photosynthesis in plants is the fundamental mechanism by which they convert light energy into chemical energy.
				*   Answering questions related to literature:
    *   **User**: What are the major themes in Shakespeare’s play “Hamlet”?
    *   **ChatGPT**: “Hamlet,” one of Shakespeare’s most renowned tragedies, explores several major themes that continue to captivate audiences and provoke thought.

			In this section, you learned how to use the OpenAI Python library to interact with the ChatGPT API by sending a request to generate the completion of a user’s input prompt/question. You also learned how to set up your API key and how to prompt the user to input a question, and finally, how to access the generated response from ChatGPT in the form of an object containing information about the response. You are now ready to build more complex projects and integrate the ChatGPT API with other frameworks.
			Summary
			In this chapter, you learned the basics of getting started with the ChatGPT API. We covered the concept of NLP and how ChatGPT has revolutionized the field. You also learned how to access the ChatGPT API through the web interface and how to create an OpenAI account.
			We dived into the technical details of using the ChatGPT API, including obtaining an API key, API tokens, and pricing. We covered how to set up a Python development environment, specifically using the VSCode IDE, and creating a virtual environment. To help you get started with using the ChatGPT API, we walked through a simple example of obtaining a ChatGPT API response.
			*Chapter 2*, *Building a ChatGPT Clone*, builds upon the foundational knowledge gained in this chapter by guiding you through the process of creating your own ChatGPT clone using the Flask framework. This chapter will provide you with a comprehensive overview of how to seamlessly integrate the ChatGPT API with Flask to develop your first end-to-end AI application.

```
