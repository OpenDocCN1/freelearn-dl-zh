# 第九章：8

# 使用 PyQt 和 ChatGPT API 的论文生成工具

在本章中，我们将深入探讨将 ChatGPT API 与最流行的 Python 框架之一用于 应用程序开发，**PyQt**的激动人心的世界。您将构建一个用户友好的桌面论文生成工具，该工具由 PyQt 和 ChatGPT API 的集成提供动力，使用户能够轻松地生成关于任何主题的优秀文章。 在这里，您将获得有关构建 PyQt 桌面应用程序的设计阶段的有价值见解。

我们将引导您通过使用 PyQt 进行面向对象的方法设计桌面应用程序的过程，帮助您理解该框架中涉及的基本概念和组件。 在打下坚实基础之后，我们将转向利用 ChatGPT API 的能力。 您将学习如何将 API 集成到您的 PyQt 应用程序中，使您能够无缝生成文章。 。

通过结合 PyQt 和 ChatGPT API 的优势，您将能够创建一个交互式且智能的桌面应用程序，能够生成关于各种主题的优秀文章。 此外，我们将展示如何让用户能够直接从前端控制 API 令牌，赋予他们指定从 ChatGPT API 获得的文章响应长度的能力。 这种程度的定制化使用户能够根据特定要求调整生成的文章，无论是简洁的摘要还是 深入的分析。

在本章中，您将涵盖以下主题：

+   使用 PyQT 构建桌面应用程序

+   使用 ChatGPT API 创建论文生成方法

+   控制 ChatGPT API 令牌

在本章结束时，您将熟练使用 PyQt 设计和开发桌面应用程序，为应用程序开发打下坚实基础。 此外，您还将进一步发展将 ChatGPT API 集成到您应用程序中的技能，让您更好地控制 AI 响应参数。

# 技术要求

在本章中，我们将演示完整的 `PyQt6` 安装。 然而，为了成功完成论文生成工具项目，您还应确保满足以下 技术先决条件：

+   在您的机器上安装了 Python 3.7 或更高版本 。

+   一个代码编辑器，例如 VS Code (推荐)

+   一个 Python 虚拟环境

+   一个 OpenAI API 密钥

+   熟悉 Python **面向对象编程** (**OOP**) 概念

本章展示的代码片段可以在 GitHub 平台上找到。 通过访问以下 链接 https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API/tree/main/Chapter08%20ArticleGenerator 可以获取代码。

在接下来的章节中，你将开始使用 PyQT 构建桌面应用程序的旅程。 你将深入了解 GUI 设计的复杂性，并学习如何利用 PyQT 的强大功能来开发一个视觉上吸引人且交互式的 桌面应用程序。

# 使用 PyQT 构建桌面应用程序

在本节中，你 将获得在 创建 PyQT 应用程序用户界面的各种组件的实际经验，例如窗口、标签、按钮、文本字段和下拉菜单。 在这里，你将设置项目，安装 `PyQt6` 和 `docx` 库，并构建你的桌面 应用程序前端。

首先，让我们讨论一下 PyQt 库是什么，以及它与 Python 应用程序开发工具（如 Tkinter（见 *表 8.1*）有何不同。 PyQt 是一个强大的 Python 框架，广泛用于创建具有丰富 **图形用户界面** (**GUIs**)的桌面应用程序。 它为开发者提供了一套全面的工具、类和功能，用于设计和构建直观且视觉上吸引人的应用程序。 PyQt 是围绕 流行的 **Qt** 框架的一个包装器，将 Qt 的广泛库和控件 无缝集成到 Python 中。

| **框架** | **基于 Qt (强大的** **GUI 工具包)** | **内置 Python GUI** **工具包 (Tkinter)** |
| --- | --- | --- |
| 跨平台 | 是 | 是 |
| 许可 | GPL 和商业 | 开源（Python 软件基金会） |
| 小部件库 | 广泛 | 有限 |
| 外观 和感觉 | 在所有平台上都有本地外观 | 仅在 一些平台上具有本地外观 |
| 文档 | 全面 且文档齐全 | 基本 但足够 |
| 流行度 | 在爱好 和工业界都受欢迎 | 标准库，在 Python 中广泛使用 |
| 学习曲线 | 适合初学者 | 适中 到陡峭 |
| 性能 | 通常由于 C++后端 | 通常由于 Python 后端 |
| 社区 支持 | 活跃的社区和 第三方模块 | 活跃的社区和 丰富的资源 |

表 8.1 – PyQt 与 Tkinter 的比较

重要注意事项

PyQt 有两种许可协议 – GNU **通用公共许可证** (**GPL**) 和商业许可。 GPL 允许你在遵守许可证条款和条件的情况下免费使用 PyQt，这些条件包括如果你分发应用程序，则必须提供应用程序的源代码。 如果你不想遵守 GPL 的要求或需要在专有、封闭源代码的项目中使用 PyQt，你可以从开发 PyQt 的 Riverbank Computing 公司购买商业许可。 商业许可允许你在不受 GPL 限制的情况下使用 PyQt。

PyQt 是一个 Python 框架，与 Tkinter 不同，Tkinter 是 Python 用于使用 Tk 工具包创建 GUI 的标准库。 PyQt 提供更广泛的控件和高度可定制的 外观。 它还提供了更 现代和美观的 GUI 外观。 现在你已经知道了 PyQt 是什么，我们可以设置你的项目并安装这个出色的 Python 桌面 应用程序框架。

## 设置文章生成工具项目

`让我们首先` `通过遵循我们已熟悉的步骤来配置我们的项目。` `你可以通过创建一个名为` `EssayGenerationTool` `的新目录并在 VS Code 中加载它来开始。` `一旦创建了项目目录，你就可以构建你的 Python 虚拟环境并创建一个名为` `app.py` `的 Python 文件，其中将包含邮件生成工具的主要代码。` `。`

`为了完成项目设置，你需要安装几个 Python 库。` `你可以使用` `pip` `包管理器来安装这些库。` `首先，在 VS Code 中打开一个终端并输入以下命令来安装所有必要的库：`

```py
 pip install PyQt6
pip install openai
pip install python-docx
```

`此外，让我们通过创建` `config.py` `模块来设置必要的 OpenAI API 密钥。` `正如我们在以前的项目中看到的那样，此文件将作为我们项目中所有 API 密钥的安全存储位置。` `确保` `config.py` `文件位于与代码相同的目录中。` `然而，你必须避免将此文件推送到 Git 仓库，以保持其机密性并防止暴露 API 密钥。` `在安全存储 API 令牌后，你可以将其包含在相关文件中，并附带必要的库，如下面的代码片段所示：` `以下` `代码片段` `:`

`config.py`

```py
 API_KEY = "<YOUR_CHATGPT_API_KEY>"
```

`app.py`

```py
 import sys
from PyQt6.QtWidgets import QApplication, QWidget, QLabel, QLineEdit, QPushButton, QTextEdit, QComboBox
from openai import OpenAI
import docx
import config
client = OpenAI(
  api_key=config.API_KEY,
)
```

在`app.py` `文件中，我们可以导入几个库来创建我们的桌面应用程序。` `从` `PyQt6` `导入的具体类代表不同的 GUI 组件，例如应用程序窗口、标签、文本字段、按钮和下拉菜单。` `此外，还导入了` `docx` `模块来处理将我们的 AI 生成文章导出为` `Microsoft` `Word 文档。`

## `使用 PyQt 构建应用程序 GUI`

`在本节中，我们将探讨创建 PyQt` `应用程序的过程。` `我们将使用` `PyQt6` `库来为文章生成应用程序设置基础。` `你将构建的应用程序用户界面由各种小部件组成，如标签、输入字段、文本区域和按钮。`

要开始创建用于生成论文的 PyQt 应用程序的旅程，我们首先将创建初始应用程序窗口并设置应用程序启动逻辑。 如前所述，我们的应用程序将使用 `EssayGenerator` 类。

使用面向对象设计（OOD）与 PyQt 相结合是有益的，因为 PyQt 建立在 Qt 框架之上，而 Qt 框架本身是按照面向对象原则设计的。 通过将我们的代码与底层框架对齐，我们可以充分利用 PyQt 的强大功能和灵活性。 通过将应用程序功能封装在类中，我们可以轻松管理和维护我们的应用程序。 这 促进了代码的可重用性和可扩展性，因为类可以根据需要继承、修改和扩展。 `EssayGenerator` `类将继承自`QWidget` `类，这是 PyQt 中所有用户界面对象的基类：

```py
 class EssayGenerator(QWidget):
    def __init__(self):
        super().__init__()
        self.initUI()
    def initUI(self):
        self.setWindowTitle("Essay Generator")
        self.setGeometry (300, 300, 1200, 800)
if __name__ == '__main__':
    app = QApplication(sys.argv)
    ex = EssayGenerator()
    ex.show()
    sys.exit(app.exec())
```

以下是我们的 PyQt 应用程序 的实现方式：

1.  首先，我们在`QWidget` `类中将其作为`EssayGenerator` `类的基类，这使我们能够采用应用程序 GUI 中的所有`QWidget` `功能。 `QWidget` 是 PyQt 中的一个基本类，它提供了创建窗口、处理事件和管理布局的功能。 虽然 PyQt``中还有其他可用的替代基类，例如`QMainWindow` `用于更复杂的应用程序或`QDialog` `用于自定义对话框，但`QWidget` `在这里被选择，因为它作为大多数用户界面元素的通用基础，并为我们的论文生成应用程序中创建基本窗口提供了必要的功能。 `

1.  `The` `EssayGenerator` `类是一种特殊类型的控件，可用于创建 GUI 元素。 `在类内部，我们有` `__init__` `方法，它是 Python 中称为构造函数的 `特殊方法。` `当创建类的对象时，它会执行。 `在` `__init__` `方法中，我们使用 `EssayGenerator` `类的 `super()` `关键字来调用父类 `__init__` `方法， `QWidget`，并对其进行初始化。 `这确保了在执行任何针对 `EssayGenerator` `类的特定初始化之前，执行了父类中必要的设置。

1.  `然后，` `initUI()` `方法负责设置应用程序的用户界面和其他元素，例如窗口标题、大小和位置。 `它使用内置的` `Essay Generator` `` `setWindowTitle` `` 方法将应用程序的窗口标题设置为，并指定窗口在屏幕上的位置和大小，其中数字代表窗口左上角的 `x` `` 和 `y` `` 坐标，然后是其宽度和 `高度。

1.  通过调用 `super().__init__()` 和 `self.initUI()`，我们确保应用程序得到适当的初始化，并且一旦运行它，主应用程序窗口就会启动。这种方法遵循继承原则，其中 `EssayGenerator` `子类继承并扩展了父类的功能， `QWidget`，从而为我们的应用程序创建了一个功能齐全且定制的控件。

1.  `在 Python 中，通常使用` `__name__ == '__main__'` `条件来确保后续代码仅在脚本作为主模块运行时执行。 `如果是这样，代码将继续创建` `QApplication` `类的实例，该实例管理应用程序的 `控制流。`

1.  `ex` 对象是从 `EssayGenerator` 类创建的，用于显示应用程序窗口，最后， `sys.exit(app.exec())` 启动了应用程序的事件循环，确保程序在用户关闭窗口或退出应用程序之前保持活跃。 这允许我们在直接运行脚本时执行应用程序。 您可以通过运行项目来验证这一点，以显示主 应用程序 窗口，如图 *图 8**.1**所示。

![图 8.1 – 文章生成器工具窗口](img/B22414_08_02.jpg)

图 8.1 – 文章生成器工具窗口

现在我们已经构建了应用程序窗口，我们可以开始添加文章生成器工具的核心元素。 这些元素如下：

+   **主题输入**: 文章生成器需要一种机制，让用户输入文章的主题或内容。 这可以通过一个文本 输入框来实现。

+   **文章输出**: 一旦生成算法生成了文章内容，就需要将其显示给用户。 这可以通过一个文本区域来实现，其中展示生成的文章。

+   **保存功能**: 提供用户保存生成文章的选项通常很有用。 这可能包括将文章保存到文件中，例如 Word 文档，或者提供将文本复制到 剪贴板的能力。

这些核心 元素 可以轻松地包含在 `initUI()` 函数中，该函数包含我们应用程序中显示的所有元素：

```py
 def initUI(self):
        self.setWindowTitle("Essay Generator")
        self.setGeometry(300, 300, 1200, 800)
        topic_label = QLabel('Enter the topic:', self)
        topic_label.move(20, 40)
        self.topic_input = QLineEdit(self)
        self.topic_input.move(20, 100)
        self.topic_input.resize(1000, 30)
        self.essay_output = QTextEdit(self)
        self.essay_output.move(20, 150)
        self.essay_output.resize(1100, 500)
        generate_button = QPushButton("Generate Essay", self)
        generate_button.move(1050, 100)
        generate_button.clicked.connect(self.generate_essay)
        save_button = QPushButton("Save", self)
        save_button.move(20, 665)
        save_button.clicked.connect(self.save_essay)
```

在创建文章主题输入框之前，我们将实例化一个 `QLabel` 对象，命名为 `topic_label`。这个标签将被放置在 `topic_input` 输入框的上方。 标签的目的是指导用户了解下方文本框的目的。 然后我们使用 `move()` 方法将标签定位到应用程序窗口内的坐标 `(20, 40)` 处。

接下来，我们将创建一个 `QLineEdit` 对象，命名为 `topic_input` 作为主题的输入字段。 在这里，我们可以再次使用 `move()` 方法将输入字段定位在坐标 `(20, 100)`下方的标签下方。 此外，我们使用 `resize()` 方法设置输入字段的尺寸。 这确保了输入字段有适当的大小以供 用户输入。

然后，我们可以定义 `essay_output` 属性为 `QTextEdit` 类的实例，它表示一个多行文本编辑小部件。 我们再次使用 `move()` 和 `resize()` 内置方法将文章输出字段放置在主题输入文本字段下方。 `essay_output` 文本 区域用于显示生成的文章 文本。 现在，用户可以轻松阅读和与文章输出交互，增强了应用程序的可用性和功能性。

重要提示

在 Python 中， `self` 关键字是一个在类内部引用该类实例的约定。 它不是一个保留关键字，而是类中实例方法的第一个参数的常用名称。 在类内部定义方法，包括将 `self` 参数作为第一个参数，允许你访问和修改该类的实例变量和方法。 that class.

最后，我们将为文章生成器应用程序创建两个按钮 – 一个 `QPushButton` 类，它提供了一个可点击的按钮元素，用户可以通过点击或按下它与之交互。 在这里，使用 `clicked.connect()` 方法至关重要，它将按钮的点击信号连接到方法，允许在按钮 被点击时调用该方法。

一旦用户点击 `generate_essay()` 函数。 请耐心等待，因为 AI 在生成响应之前需要一些时间来处理 `essay_output` 文本字段。 同样，一旦文章生成，用户将能够点击 `save_essay()` 函数，在那里我们将稍后使用 `docx` 库将文章保存为 Word 文档。

尽管 `save_essay()` 和 `generate_essay()` 尚未实现，但我们可以将它们初始化以测试我们的应用程序。 这两种方法应放置在 `EssayGeneration` 类内部，但不在 `initUI()` 方法内部。 将函数定义放置在主 Python 函数之上也是一种良好的实践，以避免任何 意外错误：

```py
 def initUI(self):
    ...
    ..
    . def generate_essay(self):
 pass
def save_essay(self):
pass statement, which is a placeholder statement in Python that does nothing. This is used to indicate that the method does not have any implementation yet.
			When you execute your application again, you will see that all the text fields and buttons are presented within the application window (see *Figure 8**.2*).
			![Figure 8.2 – The Essay Generator text fields and buttons](img/B22414_08_03.jpg)

			Figure 8.2 – The Essay Generator text fields and buttons
			This is how you can create a basic PyQt application. Now, you know how to build the application window, initialize the user interface elements, and create the core elements of the essay generator tool, such as the topic input field, essay output text area, and buttons for generating and saving essays.
			Since both the `save_essay()` and `generate_essay()` methods contain the placeholder `pass` keyword, clicking the buttons will not perform any substantial actions at this point. We will need to implement the necessary functionality within the respective methods to achieve the desired behavior when the buttons are clicked. You will learn how to build those methods in the upcoming section.
			Creating essay generation methods with the ChatGPT API
			In this section, we will dive into the implementation of the key functions within the essay generator application. These functions are responsible for generating the essay based on user input and saving it to a file. By understanding the code, you will be able to grasp the inner workings of the application and gain insight into how the essay generation and saving processes are accomplished.
			We will begin by exploring the `generate_essay()` function. This function will retrieve the topic entered by the user from the input field. It will then set the engine type for the OpenAI API, create a prompt using the topic, and make a request to the OpenAI API for essay generation. The response received from the API will contain the generated essay, which will be extracted and displayed in the essay output area of the application. To add that functionality, simply remove the `pass` placeholder and write this code:

```

def generate_essay(self):

        topic = self.topic_input.text()

        length = 500

        model = "gpt-4"

        prompt = f"Write an {length/1.5} words essay on the following topic: {topic} \n\n"

        response = client.chat.completions.create(

            model=model,

            messages=[

                {"role": "user", "content": "You are a professional essay writer."},

                {"role": "assistant", "content": "Ok"},

                {"role": "user", "content": f"{prompt}"}

            ],

            max_tokens=length

        )

        essay = response.choices[0].message.content

        self.essay_output.setText(essay)

```py

			Here, we retrieve the topic entered by the user from the `topic_input's` `QLineEdit` widget and assign it to the topic variable, using the `text()` method. This captures the user’s chosen topic for the essay. For now, we can define the `tokens` variable and set it to `500`. This indicates the desired length of the generated essay. We will modify this value later by adding a drop-down menu with different token sizes to generate essays of different lengths.
			We can also specify the engine used for the OpenAI API to be `gpt-4`, which will generate the essay. You can adjust this value to utilize different language models or versions, based on your requirements. We can also create the `prompt` variable, which is a string containing the prompt for essay generation.
			This is constructed by concatenating the `Write an {tokens/1.5} essay on the following topic:` text, where the `tokens/1.5` variable specifies how many words our essay should be. We need to divide the token number by 1.5, as 1 word in English represents about 1.5 tokens according to OpenAI. After specifying the instructions, we can pass the `topic` variable to the prompt. This prompt serves as the initial input for the essay generation process and provides context for the generated essay.
			Once all variables are defined, we will make a request to the ChatGPT API with the specified engine, prompt, and the maximum number of tokens (in this case, 500). The API processes the prompt and generates a response, which is stored in the `response` variable. From the response, we extract the generated essay by accessing the `text` attribute of the first choice. This represents the generated text of the essay. Finally, we can pass the AI response to the `essay_output()` function, displaying it in the user interface for the user to read and interact with.
			Next, we will examine the `save_essay()` function. This function will retrieve the topic and the generated essay. It will utilize the `docx` library to create a new Word document and add the final essay to the document. The document will then be saved, with the filename based on the provided topic, resulting in a Word document that contains the generated essay. After removing the `pass` keyword, you can implement the described functionality using the following code snippet:

```

def save_essay(self):

        topic = self.topic_input.text()

        final_text = self.essay_output.toPlainText()

        document = docx.Document()

        document.add_paragraph(final_text)

        document.save(topic + ".docx")

```py

			Here, we will retrieve the text entered in the `topic_input` widget and assign it to the `topic` variable, using the `text()` method. This captures the topic entered by the user, which will be used as the filename for the saved essay. Next, we use the `toPlainText()` method on the `essay_output` widget to retrieve the generated essay text and assign it to the `final_text` variable. This ensures that the user can edit the ChatGPT-generated essay before saving it. By capturing the topic and the final text, we are now equipped to proceed with the necessary steps to save the essay to a file.
			We can now use the `docx` library to create a new Word document by calling `docx.Document()`, which initializes an empty document. We then add a paragraph to the document by using the `add_paragraph()` method and passing in the `final_text` variable, which contains the generated essay text. This adds the generated essay as a paragraph to the document. We can now save the document by calling `document.save()` and providing a filename, constructed by concatenating the `topic` variable, which represents the topic entered by the user. This saves the document as a Word file with the specified filename.
			You can now test your essay generator by running the code and generating an essay following these steps (see *Figure 8**.3*):

				1.  `Ancient Egypt`.
				2.  **Generate an essay**: Click once on the **Generate Essay** button. The app will reach ChatGPT API, and within a few seconds, you will have your essay displayed inside the **essay** **output** field.
				3.  **Edit the essay**: You can edit the essay generated by artificial intelligence before saving it.
				4.  `save_essay()` method. The Word document will be saved in the `root` directory of your project.

			![Figure 8.3 – Essay Generator creating an Ancient Egypt essay](img/B22414_08_04.jpg)

			Figure 8.3 – Essay Generator creating an Ancient Egypt essay
			Once the essay has been saved as a Word document, you can reshare it with your peers, submit it as a school assignment, or use any Word styling options on it.
			This section discussed the implementation of key functions in our essay generator application using the ChatGPT API. We built the `generate_essay()` method, which retrieved the user’s topic input and sent a request to the ChatGPT API to generate an AI essay. We also developed the `save_essay()` method, which saved the generated essay in a Word document.
			In the next section, we will introduce additional functionality to the essay generator application. Specifically, we will allow the user to change the number of AI tokens used to generate an essay.
			Controlling the ChatGPT API tokens
			In this section, we will explore how to enhance the functionality of the essay generator application by allowing users to have control over the number of tokens used when communicating with ChatGPT. By enabling this feature, users will be able to generate essays of different lengths, tailored to their specific needs or preferences. Currently, our application has a fixed value of 500 tokens, but we will modify it to include a drop-down menu that provides different options for token sizes.
			To implement this functionality, we will make use of a drop-down menu that presents users with a selection of token length options. By selecting a specific value from the dropdown, users can indicate their desired length for the generated essay. We will integrate this feature seamlessly into the existing application, empowering users to customize their essay-generation experience.
			Let’s delve into the code snippet that will enable users to control the token length. You can add the following code snippet inside the `initUI()` methods, just under the `essay_output` resizing:

```

def initUI(self):

        self.setWindowTitle("Essay Generator")

        self.setGeometry(300, 300, 1200, 800)

        topic_label = QLabel('Enter the topic:', self)

        topic_label.move(20, 40)

        self.topic_input = QLineEdit(self)

        self.topic_input.move(20, 100)

        self.topic_input.resize(1000, 30)

        self.essay_output = QTextEdit(self)

        self.essay_output.move(20, 150)

        self.essay_output.resize(1100, 500) length_label = QLabel('Select Essay Length:', self)length_label.move(327, 40)self.length_dropdown = QComboBox(self)self.length_dropdown.move(320, 60)self.length_dropdown.addItems(["500", "1000", "2000", "3000", "4000"]) generate_button = QPushButton("Generate Essay", self)

        generate_button.move(1050, 100)

        generate_button.clicked.connect(self.generate_essay)

        save_button = QPushButton("Save", self)

        save_button.move(20, 665)

        save_button.clicked.connect(self.save_essay)

```py

			The preceding code introduces `QLabel`, `length_label`, which serves as a visual indication of the purpose of the drop-down menu. It displays the text `Select Essay Length` to inform users about the functionality.
			Next, we create a `length_dropdown` `QcomboBox`, which provides users with a drop-down menu to choose the desired token length. It is positioned below `length_label` using the `move()` method. The `addItems()` function is then used to populate the drop-down menu with a list of token length options, ranging from `500` to `4000` tokens. Users can select their preferred length from this list.
			The final step is to implement the functionality that allows users to control the number of tokens used when generating the essay. We need to modify the `generate_essay()` function. The modified code should be the following:

```

def generate_essay(self):

        topic = self.topic_input.text() length = int(self.length_dropdown.currentText()) model = "gpt-4"

        prompt = f"以{length/1.5}个单词的长度撰写一篇关于以下主题的论文：{topic} \n\n"

        response = client.chat.completions.create(

            model=model,

            messages=[

                {"role": "user", "content": "您是一位专业的论文作家。"}

                {"role": "assistant", "content": "好的"},

                {"role": "user", "content": f"{prompt}"}

            ], 最大令牌数=max_tokens )

        essay = response.choices[0].message.content

        self.essay_output.setText(essay)

```py

			In the modified code, the `length` variable is updated to retrieve the selected token length from the `length_dropdown` drop-down menu. The `currentText()` method is used to obtain the currently selected option as a string, which is then converted to an integer using the `int()` function. This allows the chosen token length to be assigned to the length variable dynamically.
			By making this modification, the `generate_essay()` function will utilize the user-selected token length when making the request to the ChatGPT API for essay generation. This ensures that the generated essay will have the desired length specified by the user through the drop-down menu.
			We can now click on the `addItems()` function.
			![Figure 8.4 – Controlling the essay length](img/B22414_08_05.jpg)

			Figure 8.4 – Controlling the essay length
			The user will be able to choose a token amount between `500` and `4000`. Now, you can select the `4000` tokens option, resulting in a longer length for the generated essay. We can follow the steps from our previous example and verify that the ChatGPT API generates a longer essay for `500` to `4000`.
			This is how you can enhance the functionality of an essay generator application by allowing users to control the number of tokens used when communicating with ChatGPT. By selecting a specific value from the drop-down menu, users can now indicate their desired length for the generated essay. We achieved that by using the `QComboBox` class to create the drop-down menu itself. The modified `generate_essay()` method retrieved the selected token length from the drop-down menu and dynamically assigned it to the `length` variable.
			Summary
			In this chapter, you learned how to build a desktop application with PyQt and enhance its functionality by integrating the ChatGPT API for essay generation. We discussed the basics of PyQt and its advantages over other Python GUI development tools. We used that framework to create the application’s GUI components, such as windows, labels, input fields, text areas, and buttons.
			The chapter also delved into the implementation of the essay generation functionality using the ChatGPT API. The main method discussed was `generate_essay()`, which took the user’s chosen topic, set the engine type, created a prompt using the topic, and sent a request to the API to generate the essay. The generated essay was then displayed in the application’s output area. You also learned how to build the `save_essay()` function, which used the `docx` library to save the generated essay as a Word document.
			Furthermore, the chapter explored how to enhance the application by allowing users to control the length of the generated essay. It introduced a drop-down menu implemented with the `QLabel` and `QComboBox` classes, allowing users to select different token sizes. The modified `generate_essay()` function retrieved the selected token length from the drop-down menu and adjusted the length of the generated essay accordingly.
			In *Chapter 9*, *Integrating the ChatGPT and DALL-E APIs: Building an End-to-End PowerPoint Presentation Generator*, you will learn how to integrate two AI APIs, **ChatGPT** and **DALL-E**, to build an end-to-end **PowerPoint** presentation generator. You will be introduced to the DALL-E API and learn about decoding JSON responses from the API. The chapter will also cover the PowerPoint Python framework and demonstrate how to generate AI art using the DALL-E API, enabling you to create PowerPoint slides and images.

```
