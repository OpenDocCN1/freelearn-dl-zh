# 第八章：7

# 构建 Outlook 电子邮件回复生成器

电子邮件沟通是我们个人和职业生活中不可或缺的一部分。 然而，撰写完美的电子邮件回复可能是一项耗时且具有挑战性的任务。 这就是人工智能 **（AI） **（**AI**）**可以派上用场的地方。 使用 ChatGPT API **，你将学习如何生成既相关又个性化的自动电子邮件回复，这些回复与 发件人的消息相关。

许多公司已经开始使用人工智能来生成电子邮件回复以节省时间和提高生产力。 例如，谷歌的智能回复功能使用机器学习算法生成简短、简洁的电子邮件回复，这些回复与当前消息的内容上下文相关。 同样，我们 可以使用 OpenAI 最强大的 **自然语言处理** (**NLP**) 模型，GPT-4，与 GPT-3 相比生成更复杂的回复，帮助我们开发个性化的 电子邮件回复。

在本章中，你将学习如何 构建 **Outlook 电子邮件回复生成器** 使用 OpenAI 的 GPT-4 语言模型。 你将能够构建一个应用程序，该应用程序可以自动生成针对特定电子邮件的原创 回复 集成 **Outlook API** 和 OpenAI 的 GPT-4。 这将帮助你通过自动化撰写精心设计的电子邮件回复的过程来节省时间和提高你的生产力。 你还将学习如何使用 Outlook API 将数据发送到 ChatGPT API，以及如何自动化 ChatGPT API 提示以获取相关的 电子邮件回复。

对于这个项目，除了 `tkinter` 和 `openai` 库之外，我们还将使用 Python 中的 `win32com` 库来与 `win32com` 交互，我们可以轻松检索电子邮件消息的主题和正文，并将其用作 OpenAI 的 GPT-4 语言模型的输入。 与 `tkinter` 和 `openai` 一起， `win32com` 为我们提供了一套全面的工具，用于构建功能强大且用户友好的电子邮件 回复生成器。

通过利用 `win32com` 库与 Microsoft Outlook 交互，您可以有效地使用 Outlook API 将数据发送到 ChatGPT API。 这种集成允许创建自动电子邮件回复，可以通过设计有效的提示进行增强。 为了简化流程，可以使用 `tkinter`构建一个简单的 GUI 桌面应用程序，使电子邮件回复生成器用户友好且高效。

在本章中，我们将涵盖以下主题：

+   将 Outlook 数据传递到 ChatGPT API

+   生成自动 电子邮件回复

在本章结束时，您将能够使用 NLP 技术和用户友好的 图形界面生成和发送原始电子邮件回复。

# 技术要求

为了完成本章，我们假设您已在您的设备上安装了 Microsoft Office 和 Outlook。 您可以在以下链接中学习如何安装 Microsoft Office： [https://learn.microsoft.com/en-us/microsoft-365/admin/setup/install-applications?view=o365-worldwide](https://learn.microsoft.com/en-us/microsoft-365/admin/setup/install-applications?view=o365-worldwide)。

然而，我们将逐步讲解 Outlook API 的完整安装过程。 您的设备上安装了最新的 Microsoft Office 365 应用程序（Outlook）。

您需要以下内容：

+   您的计算机上安装了 Python 3.11 或更高版本 。

+   一个 OpenAI API 密钥

+   一个代码编辑器，例如 VS Code（推荐）

+   一个 Windows 操作系统

+   您的设备上安装的 最新 Microsoft Office 365 应用程序（Outlook）

在下一节中，您将学习如何安装 Microsoft Outlook 和 `win32com` 库。 您还将学习如何将电子邮件数据从 Outlook 传递到 ChatGPT API 以实现自动电子邮件回复生成。

*《Outlook 电子邮件回复生成器》* 项目的代码可以在 GitHub 上找到 ，地址为 [https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API](https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API)。

# 将 Outlook 数据传递到 ChatGPT API

要开始，我们将首先介绍设置开发环境的基本步骤 以构建 Outlook 邮件回复生成器。 在这里，你将开始安装 Microsoft Outlook 到你的电脑上，并 设置一个电子邮件账户。 一旦安装了所有库，我们将向你展示如何使用 `win32com` 库从 Outlook 提取电子邮件数据并将其传递给 ChatGPT API 以生成自动 电子邮件回复。

让我们设置你的项目以 构建 Outlook 邮件回复生成器。 为此，创建一个名为 `EmailReplyGenerator` 的新目录，并在 VS Code 中打开它。 一旦项目创建，你可以激活你的虚拟环境，创建一个名为 `app.py`的新 Python 文件，并开始编写从 Outlook 提取电子邮件数据并将其传递给 ChatGPT API 的代码。

为了完成项目设置，你还需要安装两个 Python 库。 你可以使用 `pip` 包管理器来安装这些库。 要开始，打开命令提示符或你的机器上的终端，并输入以下命令来安装 这两个库：

```py
 $pip install openai
$pip install pywin32
```

安装这些库后，我们现在可以继续安全地处理 ChatGPT API 密钥，这将使我们能够在连接到 AI 时进行身份验证。 为此，让我们在我们的项目中创建一个名为 `config.py`的新文件。正如前一个项目所示，此文件将安全地存储项目中使用的所有 API 密钥。 现在，你可以在适当的文件中包含 API 令牌和必要的库。

使用此代码用于 API 令牌：

config.py

```py
 API_KEY = "<YOUR_CHATGPT_API_KEY>"
```

使用此代码用于 库：

app.py

```py
 from openai import OpenAI
import win32com.client
import tkinter as tk
import config
client = OpenAI(
  api_key=config.API_KEY,
)
```

`config.py` 文件安全地存储了 ChatGPT API 密钥，该密钥被导入到 `app.py` 文件中。 然后，API 密钥通过 `openai` 库与 ChatGPT API 建立安全连接。 此外，还导入了 `win32com.client` 和 `tkinter` 库，分别用于处理 Outlook 电子邮件数据和创建图形用户 界面。

通过这些步骤，您可以确保项目得到正确设置，从而可以顺利地在您的设备上启动已安装的 Outlook 应用程序。 设备上启动已安装的 Outlook 应用程序。

## 设置 Outlook 应用程序

在本节中，我们将 指导您完成设置 Microsoft Outlook 应用程序的过程。 Microsoft Outlook 是由 Microsoft 开发的一款广泛使用的电子邮件客户端和个人信息管理器。 它是 Microsoft Office 应用程序套件的一部分，其中包括 Word、Excel 和 PowerPoint 等流行的生产力工具。 虽然 Microsoft Outlook 通常与 Microsoft 电子邮件服务相关联，但重要的是要注意，Outlook 可以与任何电子邮件账户一起使用，无论电子邮件提供商是谁。 这意味着即使用户来自 Gmail、Yahoo 或其他任何电子邮件服务提供商，他们也可以利用 Outlook 的功能和优势。 电子邮件服务。

要使用安装在您计算机上的 Outlook 打开并登录到您的电子邮件账户，请按照 以下步骤操作： 这些步骤：

1.  **启动 Microsoft Outlook**：在您的计算机上找到 Outlook 应用程序图标，双击它以打开 程序。

1.  **设置新账户**：首次打开 Outlook 时，您将能够登录到您的电子邮件地址。 只需输入您的电子邮件地址，然后点击 **连接** 按钮开始设置过程（见 图 7**.1**）。

![Figure 7.1 – The Outlook email setup window](img/B22414_07_1.jpg)

Figure 7.1 – The Outlook email setup window

1.  **遵循屏幕提示**：Outlook 将尝试根据您的电子邮件地址自动配置账户设置。 您将看到确认消息，并且您的电子邮件账户将被添加到 Outlook。

1.  **访问您的电子邮件**：设置过程完成后，您可以在 Outlook 导航面板中访问您的电子邮件。 您将能够在 Outlook 应用程序内查看、发送、接收和管理您的电子邮件消息，如图 图 7**.2** 所示。

![Figure 7.2 – The Outlook desktop interface](img/B22414_07_2.jpg)

Figure 7.2 – The Outlook desktop interface

到达 Outlook 应用程序界面后，您现在可以使用 Outlook API 来访问和使用 win32com.client 来管理您的 电子邮件数据。

## 使用 win32com 客户端访问电子邮件数据

在本节中，我们将指导您通过检索 Outlook 账户中最后 10 封电子邮件的主题，并在用户友好的下拉菜单中显示它们的过程。 您将学习如何使用 `win32com` 库与 Outlook 交互，访问电子邮件数据，并在 `tkinter`中填充下拉 菜单。这将使用户能够方便地选择他们想要生成回复的电子邮件。

为了实现这一点，我们将使用 `last_10_emails()` 函数。 在这个函数内部，我们将利用 `win32com.client` 库。 我们将连接到 Outlook 应用程序并访问默认的电子邮件收件箱文件夹。 从那里，我们将检索收件箱中的电子邮件消息集合：

```py
 def last_10_emails():
    outlook = win32com.client.Dispatch("Outlook.Application").GetNamespace("MAPI")
    inbox = outlook.GetDefaultFolder(6)
    messages = inbox.Items
    emails = [messages.GetLast().Subject]
    email_number = 10
    for i in range(email_number):
        emails.append(messages.GetPrevious().Subject)
    return emails
```

前面的代码演示了如何使用 `win32com.client` 模块与 Outlook 应用程序交互，访问和操作电子邮件消息，具体步骤如下：

1.  首先，我们将使用 `win32com.client.Dispatch()` 函数来创建与 Outlook 应用程序的连接。 此函数接受一个字符串参数， `Outlook.Application`，以指定我们想要访问的应用程序。

1.  然后，我们将使用 `GetNamespace()` 方法，并使用 `MAPI` 参数来检索 Outlook 中的消息和协作服务命名空间。 此命名空间提供了访问各种 Outlook 对象 和文件夹的权限。

1.  通过调用 `GetDefaultFolder(6)` ，我们检索默认的电子邮件 `收件箱` 文件夹。 参数 `6` 对应于 Outlook 命名空间中 `收件箱` 文件夹的索引。 这确保了我们访问正确的文件夹。 通过将结果分配给 `inbox` 变量，我们现在有了对 `收件箱` 文件夹的引用，这允许我们检索电子邮件消息并对它们进行操作。 `inbox.Items` 检索收件箱文件夹中的项目或电子邮件消息集合。 此集合被分配给 `messages` 变量。

1.  要检索最新电子邮件的主题， `messages.GetLast().Subject` 将被使用。 `GetLast()` 返回集合中的最后一个电子邮件项，而 `.Subject` 检索该电子邮件的主题。 `我们现在可以将电子邮件主题包裹在一个列表中，以确保它是一个包含单个主题的列表。 `我们还可以定义一个名为 `email_number`的变量，它代表我们想要 检索主题的前置电子邮件数量。

除了 `GetList` 方法之外，您还可以使用以下 `win32com` 方法获取特定的电子邮件项：

+   `FindNext`: 此方法用于检索与指定 搜索条件 匹配的电子邮件集合中的下一个电子邮件项

+   `GetFirst`: 使用此方法，您可以从电子邮件集合中检索第一个电子邮件项，从而能够访问其内容 或属性

+   `GetNext`: 与 `GetFirst`结合使用，此方法检索集合中的下一个电子邮件项，便于对集合中所有电子邮件进行顺序访问 。

+   `GetPrevious`: 此方法补充 `GetFirst` ，并允许您检索电子邮件集合中当前访问项之前的电子邮件项，从而实现电子邮件的顺序向后访问 。

我们现在可以遍历前 10 个先前电子邮件的主题范围。 `在循环中，我们使用 `GetPrevious().Subject` 来访问迭代中当前电子邮件之前的电子邮件的主题。 `此方法将指针移动到集合中的上一个电子邮件，并且 `.Subject` 检索电子邮件的 `主题。

对于每次迭代，我们使用 `append()` 方法将检索到的主题追加到电子邮件列表中。 `这将从最新开始构建一个电子邮件主题列表，并按时间顺序回溯。

循环完成后，我们就有了一个包含 10 个电子邮件主题的列表存储在电子邮件列表中。 `然后我们可以从 `last_10_emails()` 函数中返回此电子邮件主题列表，使其可用于我们的 Outlook 电子邮件 `回复生成器。`

现在，我们可以专注于创建用户界面来显示电子邮件主题，并允许用户选择一个来生成 回复：

```py
 root = tk.Tk()
root.title("Outlook Emails")
root.geometry("300x300")
email_subjects = last_10_emails()
selected_subject = tk.StringVar()
dropdown = tk.OptionMenu(root, selected_subject, *email_subjects)
dropdown.pack()
label = tk.Label(root, text="")
label.pack()
root.mainloop()
```

让我们分析一下前面 代码的每个部分：

1.  首先，我们将使用 `tkinter` 库来初始化和配置应用程序的主要图形窗口。

    `root = tk.Tk()` 创建了一个主窗口对象，通常被称为根或顶级窗口。 此对象代表包含我们应用程序用户界面元素的 主窗口。

1.  然后，我们可以将窗口的标题 设置为 `Outlook Emails`。这个标题将在窗口的标题栏中显示。 我们还将设置窗口的尺寸为 `300` 像素宽和 `300` 像素高。 这决定了窗口首次在 屏幕上显示时的初始大小。

1.  在完成窗口模板后，我们可以初始化用于在用户界面中管理电子邮件主题的变量。 这里使用的 `last_10_emails()` 函数用于检索电子邮件主题列表。 该函数将 Outlook 应用程序中的最后 10 个电子邮件主题返回到 `email_subjects` 变量中。

1.  `selected_subject` 变量从 `StringVar` 对象创建了一个 `tkinter` 库。 此对象是一种特殊的变量类型，可以与各种 GUI 小部件相关联。 通过创建一个 `StringVar` 对象，我们有一个可以存储字符串值并轻松与 GUI 元素（如按钮或下拉菜单）相关联的变量。 这使我们能够在 我们的应用程序中跟踪和操作所选电子邮件主题。

1.  然后，我们在用户界面中创建一个下拉菜单来显示邮件主题列表。 `OpenMenu` 小部件代表一个下拉菜单，用户可以从列表中选择一个选项。 `root` 参数指定下拉菜单应放置在我们的应用程序的主窗口中，而`selected_subject` 参数指定了将与下拉菜单关联的变量。 它表示将持有从下拉菜单中当前选中的邮件主题的变量。

    `*email_subjects` 参数使用`*` 运算符将`email_subjects` 列表解包成单独的项目。 这允许每个邮件主题都被视为`OptionMenu` 小部件的一个单独的参数，为下拉菜单提供选项列表。

1.  然后，我们可以构建一个标签小部件并启动应用程序的主事件循环。 `label` 变量创建了一个标签小部件，用于在用户界面中显示文本或其他信息。 `text` 参数指定了标签中要显示的初始文本，在这种情况下是一个空字符串。 `label.pack()` 将小部件打包到主窗口中。

1.  最后，`root.mainloop()` 启动主事件循环。 此方法负责处理用户事件，如鼠标点击或按钮按下，并相应地更新 GUI。 程序将保持在这个循环中，直到用户关闭应用程序窗口，确保应用程序保持响应性和交互性。

为了测试应用程序，让我们想象我们是一名软件工程师，收到了来自 我们同事的 10 封工作邮件。

现在，您可以执行您当前版本的 Outlook 邮件回复生成器。 一旦代码开始运行，将出现一个标题为 **Outlook 邮件** 的窗口。 此窗口包含一个下拉菜单，列出了来自 *Outlook* 应用程序的最后 10 封邮件主题，如图 7*.3*.3所示。

![图 7.3 – 邮件回复生成器的最后 10 封邮件下拉菜单](img/B22414_07_3.jpg)

图 7.3 – 电子邮件回复生成器的最后 10 封电子邮件下拉菜单

你现在可以从下拉菜单中选择一个电子邮件主题。 选择后，所选电子邮件主题的文本将被存储在 `selected_subject` 变量中，并在下拉菜单按钮内显示（见 *图 7**.4*）。

![图 7.4 – 下拉菜单按钮内的所选电子邮件主题](img/B22414_07_4.jpg)

图 7.4 – 下拉菜单按钮内的所选电子邮件主题

这些 是设置构建 Outlook 电子邮件回复生成器开发环境的必要步骤。 我们通过使用`win32com` 库从 Outlook 提取电子邮件数据并将其传递给 ChatGPT API 来演示了这一过程。 你还学习了如何使用`win32com` `和`tkinter` 库来访问和显示电子邮件数据，以创建 `GUI`。

在下一节中，你将使用 ChatGPT API 生成用户选择的特定电子邮件的 AI 回复，从而完成你的 Outlook 电子邮件回复生成器。 我们还将构建相应的按钮，让用户可以从桌面应用程序访问该功能。

# 生成自动电子邮件回复

现在，让我们 深入了解我们应用程序中自动回复 Outlook 电子邮件的关键功能实现。 我们将逐步讲解构建应用程序最后部分的过程，该部分利用 Outlook API 检索电子邮件数据，并利用上一节获取的数据，通过 ChatGPT API 生成原创回复。 在这里，你将学习如何无缝地将 ChatGPT API 集成到你的 Outlook 应用程序中。

我们将构建 `reply()` 函数，该函数是生成电子邮件回复的核心组件。 在这个函数中，你将从用户界面检索所选电子邮件主题，并使用它从 Outlook 获取相应的电子邮件内容。 我们将演示如何利用 ChatGPT API 根据电子邮件内容生成 AI 驱动的响应。 通过这个过程，你将获得在 ChatGPT 和 Outlook 之间传递数据的实践经验。

您可以在 `reply()` 函数上方 `root.mainloop()` 内部 `app.py 文件 中包含该函数。 请注意， `root.mainloop()` 函数调用应该是 `tkinter 应用程序 中的最后一行代码。 这是因为 `root.mainloop() 启动了主事件循环，该循环负责处理用户事件并更新 GUI：

```py
 def reply():
    email = win32com.client.Dispatch("Outlook.Application").GetNamespace("MAPI").\
        GetDefaultFolder(6).Items.Item(selected_subject.get())
    response = client.chat.completions.create(
        model="gpt-4",
        max_tokens=1024,
        n=1,
        messages=[
            {"role": "user", "content": "You are a professional email writer"},
            {"role": "assistant", "content": "Ok"},
            {"role": "user", "content": f"Create a reply to this email:\n {email.Body}"}
        ]
    )
    reply = email.Reply()
    reply.Body = response.choices[0].message.content
    reply.Display()
    return
```

在这里，我们可以使用 `win32com 库 从 Outlook 应用程序中检索选定的电子邮件内容。 我们将通过 `Dispatch 方法 访问 Outlook 应用程序。 从那里，我们使用 `GetDefaultFolder(6) 检索 `Inbox 电子邮件文件夹，并根据从 `selected_subject `获得的主题检索特定的电子邮件项。生成的 `email 对象代表用户指定的主题的电子邮件，并且可以进一步用于生成回复或访问其他 电子邮件属性。

然后，我们可以利用 ChatGPT API 来生成对电子邮件的回复。 我们将调用 `client.chat.completions.create()` 方法，并提供几个参数来生成回复。 `max_tokens` 参数限制了生成的电子邮件回复的长度为 1,024 个标记。 通过调整此参数，您可以增加或减少标记的数量，以达到您所需的电子邮件回复长度。

正如您在这里所看到的，我们正在使用 最先进的 ChatGPT 模型，**GPT-4**。与 GPT-3 相比，该模型训练的数据量显著更多，因此在解决复杂任务方面更加准确、事实性强、可靠性高，包括个性化的 电子邮件文案。

`client.chat.completions.create()` 方法是实现的关键部分，因为它利用 ChatGPT API 来生成自动电子邮件回复。 `prompt` 参数尤其重要，因为它设置了语言模型接收的初始输入。 在这种情况下，提示包括从 Outlook 获得的电子邮件内容，这作为生成 回复的上下文。

为了提高回复的质量和多样性，开发者可以尝试不同的提示。 通过制定更具体或多样化的提示，可以引导模型输出满足特定标准或针对不同情境定制的回复。 以下是一些替代方案：

+   **上下文提示**： “你收到一位同事的邮件，请求你参加一个会议。 撰写一个 礼貌且简洁的回复，确认 你的出席。”

    在这个提示中，模型被明确指示回应同事的会议请求。 通过提供具体场景并要求“礼貌且简洁”的回复，生成的回复更有可能与期望的情境 和语气相符。

+   **情感提示**： “你刚刚收到一封来自亲密朋友的邮件，分享了一些令人兴奋的消息。 草拟一个 温暖且热情的回复，向他们表示祝贺并分享 你的想法。”

    通过在提示中融入情感关键词，如“令人兴奋的消息”、“温暖”和“热情”，模型被鼓励产生传达快乐和支持感的回复，使其听起来更真实、更像人类。

+   **正式商务提示**： “作为客户服务代表，你收到一封来自对近期产品问题不满的 客户的邮件。 撰写一个专业且富有同理心的回复，以解决他们的担忧并提供 解决方案。”

    在这里，提示设定了一个正式的商务背景，指示模型以 客户服务代表的身份回应处理不满的客户。 使用“同理心”一词强调了回复中表现出同情和帮助的重要性，这可能导致更真实和 体贴的回复。

+   **个性化提示**： “你的最佳朋友给你发了一封邮件，分享了一些关于他们最近度假的令人兴奋的更新。 回复 时，用你真诚的兴奋之情，提出后续问题，并分享一些你生活中的事情。”

    这个提示 鼓励更个性化的回复，因为它具体说明了回复应该表达真诚的兴奋之情，并涉及提出后续问题。 通过强调分享发件人生活信息的互惠性，生成的回复可能会感觉更自然，更像是真实的对话。

+   **教学提示**：“您收到一封来自同事的邮件，询问如何使用 一款新软件工具的详细说明。 如有必要，请提供带有清晰解释和视觉辅助的逐步指南。”

    在这种情况下，提示为模型生成包含详细说明的回复设置了明确的指令。 使用“逐步指南”和“清晰解释”等术语指导模型提供更信息丰富和 结构化的响应。

主题和邮件内容在生成回复中起着至关重要的作用。 选定的邮件主题作为用户想要回复的邮件的关键标识符。 一旦识别出邮件，其内容正文就被用作生成回复的上下文。 通过利用主题和邮件内容，回复函数确保 ChatGPT 生成的响应相关且具有上下文。 邮件内容提供了有关发件人查询或消息的必要信息，模型使用这些信息来构建连贯且 适当的回复。

与 ChatGPT 或任何外部 API 分享邮件内容，或任何此类事宜，都会引发有效的安全担忧。 这可以通过适当的隐私政策和用户同意机制来防止。 此外，使用安全的通信协议（例如 HTTPS）并与值得信赖的 API 提供商（如 OpenAI）合作，可以在传输 和处理过程中帮助保护数据。

在获取 ChatGPT API 响应后，我们可以通过使用 `email.Reply()`创建回复对象来准备邮件回复。然后，将回复的主体文本分配为从 ChatGPT `response` 参数中提取的响应文本。 这确保了 AI 生成的内容包含在回复中。

然后调用 `reply.Display()` 函数以在单独的窗口中显示回复，使用户能够在发送之前进行审查和可能的必要编辑。 `return` 语句表示 reply() 函数的结束，并将控制权返回到主程序，在那里我们将构建 **回复** 按钮：

```py
button = tk.Button(root, text="Generate Reply",
 command=reply)
button.pack() root.mainloop()
```

该按钮与根窗口相关联，并显示文本 `command` 参数指定当按钮被点击时，之前构建的 `reply()` 函数应该被执行。 通过调用 `button.pack()`，按钮被包装在窗口中，允许它向用户显示。 这就是我们如何设置一个用户界面元素，当点击时触发 `reply()` 函数，从而在用户交互时生成电子邮件回复的方法。

一旦您启动 Outlook 电子邮件回复生成应用程序，您的应用程序窗口将显示一个名为 **生成回复** 的新按钮（见 *图 7**.5*）。

![Figure 7.5 – The Generate Reply button](img/B22414_07_5.jpg)

图 7.5 – 生成回复按钮

现在，您可以从下拉菜单中选择一封电子邮件，并点击 `reply()` 函数，将电子邮件内容传递给 ChatGPT API，并要求它根据该内容生成回复。 此过程将在 后台进行，几秒钟后，您的应用程序将打开 Outlook 回复窗口。 正如您在 *图 7**.6*中看到的那样，回复窗口将填充生成的 ChatGPT 对所选电子邮件的回复。

![Figure 7.6 – An email reply generated by ChatGPT](img/B22414_07_6.jpg)

图 7.6 – 由 ChatGPT 生成的电子邮件回复

为了验证生成的回复是否相关，您可以 检查所选电子邮件的内容和 ChatGPT 的回复，例如 以下内容：

+   **所选** **电子邮件内容**：

    **嗨，马丁，**

    **我有一个令人兴奋的新功能请求要向您提出。 我们的用户一直在要求一个提供账户活动概述和统计信息的个性化仪表板。** **。**

    **如果您能开始这项功能的工作，并给出所需开发** **工作量估计，我将不胜感激。**

    **问候，**

    **比尔。**

+   **GPT-4** **API 回复**：

    **亲爱的比尔，**

    **感谢您的邮件和提出如此令人兴奋的新** **功能请求。**

    **个性化仪表板确实是一个极好的补充，将显著提升用户体验。 我理解一个能够为我们用户提供其账户活动全面概述的工具的重要性** **和统计数据。**

    **我将立即开始初步工作以评估可能性。 这包括深入了解用户需求，定义必要的功能，以及分析工作** **范围。**

    **考虑到这个功能的复杂性和范围，我预计初步估计将在一周内完成。 这将帮助我提供一个准确的开发** **工作量估计。**

    **我期待着这个新功能可能对我们用户体验带来的积极影响。 感谢您将这项任务** **交给我。**

    **最好的祝愿，**

    **马丁**

除了生成相关回复外，您的应用程序还赋予用户在发送之前编辑人工智能生成的回复的能力。 此功能还允许他们解决任何小瑕疵或对回复进行调整，以确保电子邮件的最终版本完美无瑕。 通过提供审查和修改人工智能生成的回复的选项，用户对内容有更大的控制权，可以在发送 电子邮件之前根据其特定需求进行定制。

这标志着我们使用 Outlook 和 ChatGPT API 生成自动电子邮件回复的实现工作告一段落。 现在，您可以从 Outlook 中检索电子邮件数据，使用 ChatGPT API 生成人工智能驱动的回复，并将此功能集成到 Outlook 应用程序中，使用户在 发送之前能够审查和修改生成的回复。

# 摘要

在本章中，我们介绍了使用 Outlook 和 ChatGPT API 以及最先进的 GPT-4 模型开发 Outlook 电子邮件回复生成器的完整过程。 我们学习了如何使用 `win32com` 库从 Outlook 中检索电子邮件数据，并使用 `tkinter` 库在下拉菜单中显示电子邮件主题。 我们编写了连接到 Outlook 应用程序、访问默认电子邮件收件箱文件夹以及检索最后 10 个电子邮件主题的代码。 我们还介绍了用户界面的创建，包括主窗口、下拉菜单和 标签。

我们也学习了构建 `reply()` 函数的过程，该函数从 Outlook 中检索电子邮件数据，使用 ChatGPT API 生成 AI 驱动的响应，并在单独的窗口中显示生成的回复以供审查和编辑。 本章还解释了如何将 `reply()` 函数集成到应用程序中，以及如何在用户界面中创建一个触发 `reply()` 函数的按钮。 我们还通过从下拉菜单中选择电子邮件主题并使用 ChatGPT API 生成 AI 回复电子邮件来测试了应用程序。

在第 *第八章*中，你将学习如何将 ChatGPT API 与最受欢迎的应用程序开发 Python 框架之一 **PyQT**集成。你将构建一个能够就任何主题创建论文的论文生成工具。 在这里，我们将更进一步，向你展示如何直接从应用程序前端控制 API 令牌。 这将允许你指定响应的长度，从而确定从 ChatGPT API 收到的生成的论文。
