# 第七章：6

# 使用 ChatGPT API 和 Microsoft Word 的桌面语言翻译应用

在当今全球化的世界中，语言翻译已成为企业和个人有效跨越国界沟通的必要工具。 幸运的是，随着 **自然语言处理** (**NLP**)和机器学习技术的进步，语言翻译比以往任何时候都更加准确和易于访问。 在本章中，我们将探讨如何使用 OpenAI ChatGPT API 和 Microsoft Word 构建语言翻译桌面应用程序。

在本章中，您将学习如何创建一个桌面应用程序，该程序可以使用强大的 ChatGPT API 实时翻译文本。 我们将逐步介绍将 API 与 Microsoft Word 集成的过程，使用户能够上传 Word 文档并将其翻译成多种语言。 我们还将介绍如何使用 Python tkinter 库构建简单的用户界面，使用户能够选择目标语言并查看翻译后的文本。 掌握这些技能后，您将能够开发自己的语言翻译应用，并提高您的 NLP 和机器学习能力。 学习

在本章中，我们将学习如何 执行以下操作：

+   将 ChatGPT API 与 Microsoft Office集成

+   使用 Tkinter构建用户界面 的

+   将 Microsoft Word 文本与 ChatGPT API集成

到本章结束时，您将掌握开发一个简单但功能齐全的桌面应用程序的基本知识，通过无缝集成 ChatGPT API 与 Tkinter 和 Microsoft Word。 掌握这些技能后，您将能够利用 AI 驱动的语言翻译功能，并将其无缝集成到您选择的任何应用程序中，为您提供一种有效的工具，与来自不同语言背景的个人进行有效沟通。 的

# 技术要求

为了完成语言翻译桌面应用程序项目，以下是需要满足的 技术要求：

+   在您的机器上安装了Python 3.7 或更高版本 的

+   代码编辑器，例如 VSCode（推荐）

+   Python 虚拟环境

+   OpenAI API 密钥

+   您的设备上可用的 Microsoft Word

在下一节中，您将了解如何有效地使用 `docx` Python 库来访问和提取 Word 文档中的信息。 这将使您能够无缝地将数据传递到 ChatGPT API 并利用其功能进行翻译。

您可以通过此 链接 在 GitHub 平台上找到本章中演示的代码片段：[https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API](https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API)。

# 将 ChatGPT API 与 Microsoft Office 集成

在本节中，我们将探讨如何设置我们的项目和安装 `docx` Python 库 以从 `docx` 库是一个 Python 包，它允许我们读取和写入 Microsoft Word (`.docx`) 文件，并提供一个方便的接口来访问存储在这些文件中的信息。

第一步是通过创建一个名为 `Translation App` 的新目录并使用 VSCode 加载它来启动您的作品。 这将使您拥有一个专门区域来构建和系统化您的翻译应用代码。 按照 *第一章*中概述的步骤，从终端窗口激活您的虚拟环境，*《st c="3256">使用 ChatGPT API 开始 NLP 任务*。

要运行语言翻译桌面应用，您需要安装以下库：

+   `openai`：该 `openai` 库允许您与 OpenAI API 交互并执行各种 NLP 任务

+   `docx`：该 `docx` 库允许您使用 Python 读取和写入 Microsoft Word `.docx` 文件 。

+   `tkinter`：该 `tkinter` 库是一个内置的 Python 库，允许您为您的 **图形用户界面** (**GUIs**) 创建 桌面应用

由于`tkinter`是一个内置库，因此无需安装，因为它已经存在于您的 Python 环境中。要安装`openai`和`docx`库，请访问 VSCode 终端，然后执行以下命令：

```py
 pip install openai
pip install python-docx
```

要访问和读取 Word 文档的内容，您需要在项目中创建一个示例 Word 文件。以下是创建新 Word 文件的步骤：

1.  在您的项目中，右键单击项目目录，选择`files`。

1.  右键单击`files`文件夹并选择**New File**。

1.  在出现的编辑字段中，输入一个带有`.docx`扩展名的文件名——例如，`info.docx`。

1.  按下*Enter*键来创建文件。

1.  文件创建后，使用 Microsoft Word 打开它。

您现在可以向该文件添加一些文本或内容，稍后我们将使用 Python 中的`docx`库来访问和读取这些内容。例如，我们创建了一篇关于纽约市的文章。您可以在以下链接中找到完整的文章：[纽约市](https://en.wikipedia.org/wiki/New_York_City)。然而，您可以选择任何包含您想要分析的文本的 Word 文档：

*`The United States’ most populous city, often referred to as New York City or NYC, is New York.` `In 2020, its population reached 8,804,190 people across 300.46 square miles, making it the most densely populated major city in the country and over two times more populous than the nation’s second-largest city, Los Angeles.` `The city’s population also exceeds that of 38 individual U.S.` `states.` `Situated at the southern end of New York State, New York City serves as the Northeast megalopolis and New York metropolitan area’s geographic and demographic center - the largest metropolitan area in the country by both urban area and population.` `Over 58 million people also live within 250 miles of the city.` `A significant influencer on commerce, health care and life sciences, research, technology, education, politics, tourism, dining, art, fashion, and sports, New York City is a global cultural, financial, entertainment, and media hub.` `It houses the headquarters of the United Nations, making it a significant center for international diplomacy, and is often referred to as the`* *`world’s capital.`*

`Now that you have created the Word file inside your project, you can move on to the next step, which is to create a new Python file called` `app.py` inside the` `Translation App` root directory.` `This file will contain the code to read and manipulate the contents of the Word file using the` `docx` library.` `With the Word file and the Python file in place, you are ready to start writing the code to extract data from the document and use it in` `your application.`

`To test whether` `we can read Word files with` `the` `docx-python` `library, we can implement the following code in our` `app.py` `file:`

```py
 import docx
doc = docx.Document("<full_path_to_docx_file>")
text = ""
for para in doc.paragraphs:
    text += para.text
print(text)
```

请确保将 `Make sure to replace` `<full_path_to_docx_file>` with the actual path to your Word document file.` Obtaining the file path is a simple task, achieved by right-clicking on your` `.docx` file in VSCode and selecting the` **Copy Relative Path` option from the` `drop-down menu.`

完成选择后，运行 `app.py` 文件并验证输出。 此代码将读取您的 Word 文档内容并将其打印到控制台。 如果文本提取正确工作，您应该在控制台中看到文档的文本（见 *图 6**.1*)。 现在 `text` 变量包含 `info.docx` 作为 Python 字符串。

![Figure 6.1 – Word text extraction console output](img/B22414_06_1.jpg)

图 6.1 – Word 文本提取控制台输出

本节 提供了一个逐步指南，说明如何设置项目和安装用于从 Word 文档中提取文本的 `docx` Python 库。 本节还包括有关如何创建新的 Word 文件以及如何使用 `docx` 库通过 Python 读取和操作其内容的说明。 使用 Python。

在下一节中，我们将深入探讨构建 Tkinter 应用程序框架的过程。 您将学习如何创建一个带有小部件的基本窗口，以及如何使用几何管理器将它们定位在 屏幕上。

# 使用 Tkinter 构建用户界面

在本节中，我们将学习如何使用 `Tkinter` 库为我们文本翻译应用程序创建一个 GUI。 Tkinter 是 Python 的标准库，用于创建 GUI，它提供了一个简单高效的方式来创建窗口、按钮、文本字段和其他 图形元素。

如图 *图 6**.2* 所示的文本翻译应用程序将被设计成一个简单且用户友好的界面。 当您运行应用程序时，将有一个标签为 **浏览** 的按钮和一个包含要翻译成语言的列表的下拉菜单：

1.  要将文本翻译，用户可以从 下拉菜单中选择他们想要翻译成的语言。

1.  一旦选择了语言，用户可以点击 **浏览** 按钮并选择他们想要翻译的 Word 文件。

1.  选择后，文件内容将使用 ChatGPT API 进行翻译，翻译后的文本将显示在窗口中央的大文本框中。 然后，用户可以复制并粘贴翻译后的文本，按需使用。

文本翻译应用被设计成一个简单高效的工具，用于将一种语言中的文本翻译成另一种语言。

![图 6.2 – 文本翻译应用 UI](img/B22414_06_2.jpg)

图 6.2 – 文本翻译应用 UI

重要提示

我们不需要指定要翻译的文本，因为 ChatGPT 语言模型被设计成能够自动识别 提示语言。

现在我们已经明确了基本设计，我们可以用 Tkinter 将其实现。

为了开始，我们需要从我们的 `app.py` 文件中删除之前添加的示例代码。 只需删除包含示例代码的所有代码行，这样我们就可以从一个干净的状态开始。

创建 Tkinter 窗口是构建我们的文本翻译应用的下一步。 您可以通过在 `app.py`中编写以下代码来实现这一点，该代码初始化了 Tkinter 类的新实例并设置了 应用程序窗口：

```py
 from openai import OpenAI
import docx
import tkinter as tk
from tkinter import filedialog
root = tk.Tk()
root.title("Text Translator")
root.configure(bg="white")
header_font = ("Open Sans", 16, "bold")
header = tk.Label(root,
                  text="Text Translator",
                  bg="white",
                  font=header_font,
                  )
header.grid(row=0, column=0, columnspan=2, pady=20)
root.mainloop()
```

要开始构建 Tkinter 应用，我们首先需要导入必要的库。 我们将使用 `openai`, `docx`, 和 `tkinter` 库来完成这个项目。

接下来，我们需要创建我们应用程序的主窗口。 我们将使用 `Tk()` 方法来创建主窗口，这是 `tkinter` 库的一部分。 我们还将为我们的应用程序设置一个标题并将其背景颜色设置为白色。

我们还可以使用 `Label()` `方法来为我们的应用程序设置标题，该方法属于 `tkinter`。`我们可以将其文本设置为`文本翻译器`，背景颜色设置为`白色`，字体设置为`Open Sans`，大小为`16` ，并且为`粗体` 字体。 `然后我们将使用`grid()` 方法通过指定`行` 和`列` 值将标题放置在我们的应用程序窗口的特定位置，跨越两列，并具有`小的填充。

我们将使用 `mainloop()` 方法来运行我们的应用程序，该方法属于 `tkinter`。`mainloop()` 方法是一个无限循环，用于运行应用程序，监听事件，如按钮点击或窗口大小调整，并在需要时更新显示。`它将持续监听事件，直到用户关闭窗口或退出 应用程序。

在创建应用程序窗口之后，下一步是向其中添加元素。 这些元素之一将是 **浏览** 按钮：

```py
 browse_button = tk.Button(root, text="Browse",
                          bg="#4267B2", fg="black", relief="flat",
                          borderwidth=0, activebackground="#4267B2",
                          activeforeground="white")
browse_button.config(font=("Arial", 12, "bold"), width=10, height=2)
browse_button.grid(row=1, column=0, padx=20, pady=20)
```

重要提示

在调用`mainloop()` 方法之前添加所有元素是很重要的，以确保它们被正确地包含在应用程序窗口中。 `mainloop()` 应该始终是您的`app.py` 文件中的最后一行。

要创建一个按钮小部件，您可以在 `tk.Button()` 方法中，该方法属于 `tkinter` 库。 `按钮放置在根窗口中，其`bg` 参数将按钮的背景颜色设置为深蓝色，而`fg` 设置前景颜色为`黑色`。

同时， `relief` 被设置为 `flat` 以创建平面外观，并且 `borderwidth` 被设置为 `0` 以移除按钮的边框。 `然后，我们使用 `activebackground` 和 `activeforeground` 参数来设置按钮在被点击或 `悬停` 时的颜色。

在 **浏览** 按钮下方，我们可以创建一个包含语言列表的下拉菜单：

```py
 languages = ["Bulgarian", "Hindi", "Spanish", "French"]
language_var = tk.StringVar(root)
language_var.set(languages[0])
language_menu = tk.OptionMenu(root, language_var, *languages)
language_menu.config(font=("Arial", 12), width=10)
language_menu.grid(row=1, column=1, padx=20, pady=20)
```

语言列表包含用户可以选择的语言列表。 您可以将任何语言添加到该列表中，并且它将在 下拉菜单中显示给用户。

`language_var` 变量被创建为一个 `StringVar` 对象，并将其设置为列表中的第一种语言。 `language_var` 对象的 `set()` 方法随后被用来将其初始值设置为语言列表的第一个元素。 这样做是为了使语言下拉菜单的默认值为列表中的第一种语言，在本例中是 `Bulgarian` 。

然后，使用 `OptionMenu` 小部件和 `language_var` 变量以及 `*languages` 语法创建小部件，该语法将 `languages` 列表解包为单独的参数。 这将为下拉菜单设置可用选项为语言列表中的语言。 然后，我们在 应用程序窗口的第一行的第二列中配置和定位一个选项菜单小部件。

下一步是将文本字段添加到我们的 GUI 中。 `为此，您可以在 `app.py` 文件下方添加以下代码。 `此代码创建了一个具有特定尺寸、颜色和属性的文本字段，然后使用 `grid positioning` 将其放置在窗口中：

```py
 text_field = tk.Text(root, height=20, width=50, bg="white", fg="black",
                     relief="flat", borderwidth=0, wrap="word")
text_field.grid(row=2, column=0, columnspan=2, padx=20, pady=20)
text_field.grid_rowconfigure(0, weight=1)
text_field.grid_columnconfigure(0, weight=1)
root.grid_rowconfigure(2, weight=1)
root.grid_columnconfigure(0, weight=1)
root.grid_columnconfigure(1, weight=1)
```

此代码将在 GUI 窗口中添加一个文本字段，该字段将用于显示 Word 文档的 ChatGPT 翻译。 在此， `Text` 小部件对象名为 `text_field` ，放置在根窗口内部。 此 `Text` 小部件用于显示和编辑多行文本。 然后，我们使用 `grid()` 方法将 `Text` 小部件定位在 GUI 上。 此 `padx` 和 `pady` 参数在窗口中为小部件添加填充，以在它与其他小部件之间创建一些空间。

您可以通过使用 `text_field` 的配置来使主窗口在水平和垂直方向上扩展，使用 `grid_rowconfigure()` 和 `grid_columnconfigure()` 方法。 这允许文本字段填充窗口中的可用空间。 然后，您还可以配置主窗口， `root`，以扩展它。 这些设置确保文本字段保持在窗口中心，并填充可用空间。

一旦您的 GUI 完成，您就可以启动应用程序，并确认您的 GUI 与 *图 6**.2*中显示的样式和属性完全一致。 通过单击**语言** 菜单，您可以访问并选择任何语言 。

这就是如何使用 Tkinter 库创建文本翻译应用的 GUI。 我们已经创建了主窗口、标题、 **浏览** 按钮，以及包含语言列表的下拉菜单。

尽管您已经完成了 GUI，但单击 **浏览** 按钮不会触发任何操作，因为它尚未连接到任何活动的 Python 函数。 我们将在下一节中修复这个问题，我们将创建两个核心函数，分别负责打开 Microsoft Word 文件和执行 翻译。

# 将 Microsoft Word 文本与 ChatGPT API 集成

在本节中，我们将逐步指导你如何使用 Python 创建两个核心函数，这些函数对于构建文本翻译应用程序至关重要。 第一个函数，`translate_text()`，使用 `browse_file()`，允许用户从他们的本地系统中浏览并选择一个 Word 文件，并触发文本翻译过程。 这两个函数将通过代码示例进行详细解释，以帮助你理解和在自己的项目中实现它们。

## `使用 gpt-3.5-turbo 翻译 Word 文本`

在本节中，你将学习如何构建 `translate_text()` 函数。 此函数负责将 Microsoft Word 文件的文本翻译成用户通过 GUI 选择的语言。 我们将使用 OpenAI API，特别是 gpt-3.5-turbomodel，来翻译 文本。

在构建 `translate_text()` 函数之前，你需要在你的项目根目录下创建一个名为 `config.py` 的文件。 此文件将包含你的 OpenAI API 令牌，这是使用 OpenAI API 所必需的。 API 令牌应分配给名为 `API_KEY` 的变量，在 `config.py` 文件中：

`config.py`

```py
 API_KEY = "<YOUR_CHATGPT_API_KEY>"
```

`app.py`

```py
 import openai
import docx
import tkinter as tk
from tkinter import filedialog import config
client = OpenAI(
 api_key=config.API_KEY,
)
```

此步骤是必要的，以保持 API 令牌的安全，并防止其意外上传到 公共仓库。

在设置好 API 密钥后，你现在可以继续在 `app.py`中实现 `translate_text()` 函数，如下面的 代码片段 所示：

```py
 def translate_text(file_location, target_language):
    doc = docx.Document(file_location)
    text = ""
    for para in doc.paragraphs:
        text += para.text
    model_engine = "gpt-3.5-turbo"
    response = client.chat.completions.create(        model=model_engine,
        messages=[
            {"role": "user", "content": "You are a professional language translator. "
                                        "Below I will ask you to translate text. "
                                        "I expect from you to give me the correct translation"
                                        "Can you help me with that?"},
            {"role": "assistant", "content": "Yes I can help you with that."},
            {"role": "user", "content": f"Translate the following text in {target_language} : {text}"}
        ]
    )
    translated_text = response.choices[0].message.content
    return translated_text
```

`The` `translate_text()` `函数接受两个参数 –` `file_location` `，这是要翻译的 Microsoft Word 文件的位置，以及` `target_language` `，这是要将文本翻译成的语言。` `函数的第一行使用` `docx` `模块打开位于` `at` `file_location` `的 Word 文档。` `接下来的几行代码创建一个空字符串 text，然后遍历文档中的每个段落，将每个段落的文本连接到文本字符串中。` `换句话说，我们可以从 Word 文档中提取所有文本并存储在一个` `单独的字符串中。`

`然后，使用 GPT-3.5 API 模型来翻译文本。` `将` `model_engine` `变量设置为 GPT-3.5 模型。` `创建一个` `response` `变量，通过调用` `client.chat.completions.create()` `方法，向 API 发送提示信息，请求将给定的文本翻译成` `指定的` `target_language` `。`

`` `messages` `参数是一个字典列表，代表用户和语言翻译器之间的对话。`

`` `messages` `变量用于将对话历史传递给语言模型进行翻译。` `对话` `由用户和助手使用` `ChatGPT API` `交换的消息组成。`

`让我们分解一下` `messages` `变量的设计：`

1.  `该变量是一个字典列表，其中每个字典代表一个包含两个` `键值对的消息：`

    +   `role` `：这代表对话中参与者的角色。` `它可以是` `user` `或` `assistant` `。`

    +   `content` `：这代表消息的实际内容。`

1.  `对话遵循一个模式，前两条消息建立上下文，最后一条消息提供要` `翻译的文本：`

    +   `第一条消息来自用户的角色，解释了交互的` `上下文`

    +   第二条消息来自助手的角色，并确认其帮助

    +   第三条消息来自用户的角色，包含要翻译的文本，包括 目标语言

对话格式旨在为语言模型提供关于其需要执行的任务的上下文，使明确用户 希望助手将给定的文本翻译成指定的 目标语言。

The `role` 键指定消息是否来自用户或助手，而 `content` 键包含实际的消息文本。 最终的翻译文本是从 `response` 对象中获取并由函数返回的。

在将包含要翻译的文本和目标语言的文本发送到 GPT-3.5 API 模型后，响应将是一个包含模型生成的响应各种信息的 JSON 对象。 对于此模型，实际的翻译存储在响应的 `content` 字段中，该字段位于 `choices` 列表的第一个选择中。 因此，代码通过访问第一个选择的 `content` 字段来提取翻译文本，并将其分配给 `translated_text` 变量，然后该函数返回。

一旦我们将翻译文本处理成 Python 字符串，我们就可以将其显示给用户。 除此之外，我们还需要实现一个函数，使用 `browse_file()` 函数 建立我们的 Word 文件的路径：

```py
 def browse_file():
    file_location = filedialog.askopenfilename(initialdir="/",
                                               title="Select file",
                                               filetypes=(("Word files", "*.docx"), ("all files", "*.*")))
    if file_location:
        # Get the selected language from the dropdown menu
        target_language = language_var.get()
        translated_text = translate_text(file_location, target_language)
        text_field.delete("1.0", tk.END)
        text_field.insert(tk.END, translated_text)
```

The `browse_file()` 函数创建一个文件对话框窗口，允许用户选择要翻译的 Word 文件。 如果用户选择了一个文件，该函数会从下拉菜单中检索所选语言，然后调用 `translate_text()` 函数，并将文件位置和目标语言作为参数。 一旦文本被翻译，该函数会清除文本字段，并将翻译后的文本 插入其中。

该函数打开一个文件对话框，让用户选择要翻译的 Word 文件。 其中， `initialdir` 参数设置文件对话框打开时显示的初始目录，而 `title` 参数设置文件对话框窗口的标题。 `filetypes` 参数指定用户可以在文件对话框中选择的文件类型。 在这种情况下，用户可以选择具有 `.docx` 扩展名的文件。 所选文件的路径存储在 `file_location` 变量中。

然后，我们检查是否已选择了一个文件；如果是的话，我们可以使用 `language_var.get()` 函数从下拉菜单中检索所选语言。 接着调用 `translate_text()` 函数，并将所选文件位置和目标语言作为参数传递，以将文件中的文本翻译为目标语言。 

文本翻译完成后，我们可以删除文本字段中可能存在的任何文本，并将翻译后的文本插入到文本字段中。 文本字段是我们应用程序中显示翻译文本的 GUI 小部件。

最后，你可以向 `browse_button`添加另一个参数，确保在 `browse_file()` 函数被激活时，只需点击 `按钮：

```py
 browse_button = tk.Button(root, text="Browse",
                          bg="#4267B2", fg="black", relief="flat",
                          borderwidth=0, activebackground="#4267B2",
                          activeforeground="white", command=browse_file parameter associates the browse_file() function with the tk.Button widget. When the button is clicked, the browse_file() function will be executed.
			You can now run your application to start it and display its GUI window. From there, you can select the target language from the drop-down menu and click on the **Browse** button to select a Word file (see *Figure 6**.3*).
			![Figure 6.3 – Browsing a Word file with the Text Translator app](img/B22414_06_3.jpg)

			Figure 6.3 – Browsing a Word file with the Text Translator app
			Once the file is selected, the ChatGPT API will process your request, and the translated text will be displayed in the text field below the buttons, as shown in *Figure 6**.4*.
			![Figure 6.4 – Text translated using the Text Translator app](img/B22414_06_4.jpg)

			Figure 6.4 – Text translated using the Text Translator app
			In this section, you learned how to build the `translate_text()` function in Python using OpenAI’s GPT-3.5 Turbo model, translating text in a Microsoft Word file into a language selected by the user through a GUI. We also discussed how to build the `browse_file()` function to get the path to the Word file using the **Browse** button, displaying the translated text to the user.
			Summary
			In this chapter, you learned how to develop a text translation application that can translate text from a Microsoft Word file into a target language selected by the user. The chapter covered the integration of Microsoft Word with the ChatGPT API using Python.
			We learned how to use Tkinter to create a user interface for the text translation application. The user interface comprised a simple and user-friendly design that included a drop-down menu, with a list of languages to translate to, and a **Browse** button that allowed users to select a Word file. Once the user selected a file, the contents of the file were translated using the ChatGPT API, and the translated text was displayed in the large text field in the center of the window.
			We also saw how to set up a `docx` Python library to extract text from Word documents. The `docx` library provided an interface to access information stored in Word files.
			In the next chapter, *Chapter 7*, *Building an Outlook Email Reply Generator*, you will learn how to build an **Outlook** email reply generator application using the most advanced ChatGPT model – **GPT-4**. You will learn how to pass email data from Outlook to the ChatGPT API and use it to generate an original reply to a specific email. You will also learn how to automate the ChatGPT API prompt to get relevant email replies.

```
