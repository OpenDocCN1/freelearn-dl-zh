

# 第十章：集成 ChatGPT 和 DALL-E API：构建端到端 PowerPoint 演示生成器

在这个令人兴奋的章节中，我们将深入 AI 艺术的世界，并探索由 OpenAI 开发的 AI 模型 **DALL-E** 的惊人能力。我们将首先向您介绍 DALL-E 及其从文本描述生成图像的开创性方法。 您将学习如何通过 DALL-E API 访问 DALL-E 的力量，使您能够将这项尖端技术集成到您自己的 Python 应用程序中。 随着您发现使用 DALL-E 生成 AI 艺术的迷人可能性，您将释放您的创造力。 您将能够使用 DALL-E 生成引人入胜的 AI 艺术作品。

在本章中，我们将展示如何使用 Python 框架和 DALL-E 图像生成自动化 PowerPoint 演示的开发。您将获得利用编程力量简化创建过程的实践经验。。

我们将探讨将 DALL-E 和 ChatGPT 这两个强大的 AI 模型集成起来，以构建一个端到端的 PowerPoint 演示生成器应用程序。这个项目将使您能够结合 DALL-E 在生成引人入胜的 AI 艺术方面的能力以及 ChatGPT 在生成类似人类文本方面的专业知识。 通过集成这些 API，您将能够创建一个应用程序，该应用程序可以接受用户指定的文本并生成带有图像和文本的 PowerPoint 幻灯片。 和文本。

在本章中，我们将涵盖以下主题：

+   使用 DALL-E 和 DALL-E API

+   使用 PPTX Python 框架构建 PowerPoint 应用程序

+   使用 DALL-E API 生成艺术作品

+   完成并测试 PowerPoint 演示生成器

到本章结束时，您将通过将 DALL-E 和 ChatGPT API 集成到构建端到端的 PowerPoint 演示生成器应用程序中来提高您的 AI 应用程序开发技能。通过结合 DALL-E 的图像生成能力和 ChatGPT 的文本生成专业知识，您将了解如何自动化包含引人入胜的 AI 生成的图像和类似人类文本的 PowerPoint 演示的开发。 。

# 技术要求

为了充分利用本章内容，您需要准备一些基本工具。 我们将提供详细说明和安装指南，这些内容在上一章中没有涉及。 以下是您需要的 内容：

+   在您的计算机上安装 Python 3.7 或更高版本 

+   一个 OpenAI API 密钥，您可以从您的 OpenAI 账户 获取

+   一个代码编辑器，例如 VS Code（推荐），用于编写和编辑 您的代码

+   您 Python 虚拟环境中安装的 Tkinter 框架 

+   在您的设备上安装的 PowerPoint 软件 

本章的代码示例可以在 GitHub 上找到 ，位于 [https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API](https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API)。

在下一节中，我们将探讨 DALL-E AI 艺术生成 AI 模型，并介绍 DALL-E API 的基础知识。 DALL-E API。

# 使用 DALL-E API

DALL-E 是一个 令人瞩目的 AI 模型，可以从文本描述中生成令人惊叹的图像。 在本节中，您将了解 DALL-E 背后的概念及其在图像合成方面的开创性方法。 您将了解 DALL-E 将文本提示转换为生动和富有想象力的视觉表示的独特能力。 此外，我们还将通过 DALL-E API 探索 DALL-E 的实际应用，使您能够无缝地将这项强大的技术集成到您的应用程序中，并解锁无数 创意可能性。

DALL-E 是由 OpenAI 开发的高级 AI 模型。 它于 2021 年 1 月向世界推出。 DALL-E 是一个基于神经网络的模型，结合了 ChatGPT 语言模型的元素和 **生成对抗网络** (**GAN**)架构。 DALL-E 这个名字是艺术家萨尔瓦多·达利和动画电影中的角色 WALL-E 的俏皮组合。 这个独特的模型旨在根据文本描述生成高度逼真和新颖的图像，展示了 AI 在视觉创造力领域的巨大潜力。 通过在庞大的图像和文本提示数据集上进行训练，DALL-E 可以创建复杂和富有想象力的视觉输出，为 AI 驱动 图像合成方面的突破性进展提供一瞥。

您可以通过使用 DALL-E API 直接将 DALL-E 集成到您的应用程序中。 它提供了一个方便的接口与 DALL-E 交互，使用户能够根据自定义的文本 提示程序化地生成图像。

要使用 DALL-E API，您可以使用来自您的 OpenAI 账户的相同 API 密钥。 一旦您有了 API 密钥，您就可以向 API 端点发出请求，指定您想要生成图像的文本提示。 您可以在几行代码内创建一个单独的 Python 文件并生成您的第一个 DALL-E 图像。 API 将处理请求并返回生成的图像作为响应，如下面的 代码示例 所示：

```py
 response = client.images.generate(
        model="dall-e-3",
        prompt=dalle_prompt + " Style: digital art",
        n=1,
        size="1024x1024"
    )
    image_url = response.data[0].url
```

前面的 代码演示了如何使用 `openai.Image.create()` 函数通过 DALL-E API 生成图像并检索生成的图像的 URL。

`prompt` 参数作为所需图像的文本描述或指令。 此提示为 DALL-E 模型提供指导，说明生成的图像应描绘什么。 `size` 参数确定生成图像的尺寸，表示分辨率为 `1024` 像素乘以 `1024` 像素的正方形图像。

在发出 API 请求后，响应存储在 `response` 变量中。 然后代码从 API 响应中检索生成的图像的 URL。 URL 通过访问响应字典中的适当键来获取。 通过将 `response` 分配给 `image_url`，生成的图像的 URL 被存储以供在 任何应用程序中进一步使用。

这就是您如何使用 DALL-E API 将该 AI 艺术集成到您的应用程序中的方法。 在下一节中，我们将通过启动我们的 PowerPoint 演示 生成器应用程序来实现这一点。

# 使用 PPTX Python 框架构建 PowerPoint 应用程序

In this section, we will guide you through the process of setting up your very own PowerPoint Presentation Generator application using the `pptx` Python library and the ChatGPT API. We’ll explore how to leverage the power of the `pptx` library to automate the creation of PowerPoint presentations, allowing you to dynamically generate slides with customized content. Furthermore, we’ll demonstrate how to integrate this PowerPoint generation functionality into a Tkinter application, enabling you to build a user-friendly interface for your slide generator.

For this project, we will use the VS Code IDE, which is our preferred IDE when working with Python. Launch VS Code and create a new project. Choose the location where you want to create your project and name it `PowerPoint Generator`.

Once your project is open, find the **Terminal** tab. Once the **Terminal** tab is open, you can start using it just like a regular command-line interface to install the necessary libraries for our project:

```py
 $pip install python-pptx
$pip install openai
$pip install requests
```

Here’s how we’re going to use those libraries:

1.  `python-pptx`: The `python-pptx` package is used for working with PowerPoint files (`.pptx`) and provides functionalities to create, modify, and automate the development of PowerPoint presentations programmatically.

1.  `openai`: The `openai` package provides access to the OpenAI API, which allows you to interact with AI models such as DALL-E and ChatGPT, enabling you to utilize their capabilities in your Python applications.

1.  `requests`: The `requests` package is used for making HTTP requests and is used to communicate with the DALL-E API and retrieve the generated image URL after sending a request.

We can continue developing our project structure by creating two files, `app.py` and `config.py`:

app.py

```py
 import collections.abc
import config
assert collections
import tkinter as tk
from pptx import Presentation
from pptx.util import Inches, Pt
from openai import OpenAI
from io import BytesIO
import requests
# API Token
client = OpenAI(
  api_key=config.API_KEY,
)
```

config.py

```py
 API_KEY = "YOUR_API_KEY"
```

`在这里， `app.py` `文件是应用程序的核心，作为主入口点。 `它导入各种库和 `模块，以实现与 ChatGPT、DALL-E 和 PowerPoint 框架的连接。 `以下是导入及其 `目的的概述：`

+   `collections.abc` `被导入以确保与 Python `标准库` 的兼容性

+   `config` `被导入以访问 `API_KEY` `变量，该变量包含用于与 OpenAI API `进行身份验证所需的 API 密钥`

+   `tkinter` `被导入以利用 Tkinter 库构建应用程序的用户界面 ``

+   `pptx` `和 `pptx.util` `被导入以处理 PowerPoint 文件，允许应用程序以编程方式创建和修改 PowerPoint `演示文稿`

+   `openai` `被导入以与 OpenAI API 交互并访问其服务，例如语言模型和 `图像生成`

+   `io` `和 `requests` `被导入以处理输入/输出操作和分别发出 HTTP `请求`

`另一方面， `config.py` `文件是项目的配置文件。 `它包含 API 密钥，由 `API_KEY` `变量表示，该变量是用于与 OpenAI API `进行身份验证所需的。 通过将 API 密钥分离到配置文件中，可以更容易地管理和更新密钥，而无需直接修改主应用程序代码。 `这种模块化方法有助于更好地组织和管理敏感信息。`

`接下来，在我们的 `app.py` `文件中，我们将专注于构建 Tkinter 应用程序窗口，该窗口将作为我们的 PowerPoint `演示文稿生成器应用程序` **`图形用户界面`** (`**GUI`**)：

```py
 app = tk.Tk()
app.title("Crate PPT Slides")
app.geometry("800x600")
# Create text field
text_field = tk.Text(app)
text_field.pack(fill="both", expand=True)
text_field.configure(wrap="word", font=("Arial", 12))
text_field.focus_set()
# Create the button to create slides
create_button = tk.Button(app, text="Create Slides", command=get_slides)
create_button.pack()
app.mainloop()
```

`在这里，我们创建了一个 Tkinter 应用程序框架，允许用户根据 `输入文本` 生成 PowerPoint 幻灯片。

首先，代码 使用 `tk.Tk()` 函数 初始化一个 Tkinter 应用程序窗口。 app 变量被分配为应用程序实例。 在这里，我们为应用程序窗口设置了一个标题，该标题将在窗口的标题栏中显示。 然后，我们将应用程序窗口的初始大小设置为 `800` 像素宽和 `600` 像素高。

我们也 使用 `tk.Text()` 函数 创建一个文本字段。 这个文本字段用于接受用户输入以生成演示文稿。 调用 `pack()` 方法将文本字段放置在应用程序窗口内。 使用 `configure()` 方法来自定义文本字段的显示外观。 在这种情况下， `wrap="word"` 被设置为在 `单词边界 处换行。

最后，我们使用 `tk.Button()` 函数 创建一个按钮。 按钮上标有 `创建幻灯片` 的标签，使用 `text` 参数，并且将 `command` 参数设置为 `get_slides`。这意味着当按钮被点击时， `get_slides` 函数将被 调用。

接下来，我们将 继续构建 `get_slides()` 函数，该函数将负责创建演示文稿和幻灯片。 需要注意的是， `get_slides()` 函数应该在创建 Tkinter 应用程序窗口 和相关的控件 的代码之上定义。 这确保了当应用程序与 用户 交互时，函数已被定义并可供使用：

```py
def get_slides():text = text_field.get("1.0", "end-1c")paragraphs = text.split("\n\n")prs = Presentation()width = Pt(1920)height = Pt(1080)prs.slide_width = widthprs.slide_height = heightfor paragraph in paragraphs:slide_generator(paragraph, prs)prs.save("my_presentation.pptx") app = tk.Tk()
app.title("Crate PPT Slides")
app.geometry("800x600")
# Create text field
text_field = tk.Text(app)
text_field.pack(fill="both", expand=True)
text_field.configure(wrap="word", font=("Arial", 12))
text_field.focus_set()
# Create the button to create slides
create_button = tk.Button(app, text="Create Slides", command=get_slides)
create_button.pack()
app.mainloop()
```

在这里，使用 `text_field.get()` 方法来检索文本框的内容。 参数 `1.0` 表示检索应从文本框的第一个字符开始，而 `end-1c` 表示检索应结束在最后一个字符之前，不包括换行符。 这使我们能够获取用户输入的整个文本。

接下来，将文本字符串分割成段落。 使用 `\n\n` 分隔符来识别段落，假设每个段落由两个连续的换行符分隔。 这个分割操作将在 `paragraphs` 变量中创建一个段落列表。

使用 `prs = Presentation()`创建一个新的 PowerPoint 演示对象。 然后，我们将幻灯片的尺寸设置为 `1920` 点宽和 `1080` 点高。

循环随后被启动，遍历 `paragraphs` 列表中的每个段落。 对于每个段落，都会调用 `slide_generator()` 函数，并将段落和演示对象作为参数传递。 这个函数将单独实现，其负责根据每个段落的 `内容创建单独的幻灯片。

一旦循环结束，生成的演示对象将被保存为 PowerPoint 文件。 生成的 PowerPoint 文件将被命名为 `my_presentation.pptx` ，并将包含从 `用户输入创建的幻灯片。

为了在早期阶段测试你的应用程序，你可以点击 `get_slides()` 函数，该函数根据 输入文本生成幻灯片：

![Figure 9.1 – PowerPoint Presentation Generator GUI](img/B22414_09_01.jpg)

Figure 9.1 – PowerPoint Presentation Generator GUI

然而， 在此阶段，如果你尝试生成 AI 幻灯片，你会遇到错误。 这是因为负责为我们的幻灯片创建 AI 艺术和文本的 `slide_generator()` 函数尚未 定义。

在下一节中，我们将进入构建 `slide_generator()` 函数，其中用户提供的文本将通过 DALL-E API 转换为 AI 艺术。 此函数将访问 DALL-E API 的功能，根据文本输入生成图像，从而创建带有 AI 生成的艺术品 的令人惊叹的幻灯片。

# 使用 DALL-E API 生成艺术作品

在本节中，我们将探索 ChatGPT API 和 DALL-E API 的激动人心的集成。 我们将展示这两个强大的 AI 工具如何结合以创建独特且视觉上令人惊叹的艺术作品。 通过利用 ChatGPT API，我们将根据应用程序中提供的用户段落生成 DALL-E 提示。 然后，我们将利用 DALL-E API 将该提示转换为 迷人的图像。

首先，我们将开始 构建 `slide_generator()` 函数，该函数在根据用户输入生成 PowerPoint 幻灯片方面将发挥关键作用。 需要注意的是，此函数应在 `app.py` 文件中 API 密钥定义之后以及 `get_slides()` 函数之上创建。 将其放置在此位置确保它在 `get_slides()` 函数中调用时已定义并可供使用。

这种方法允许对代码进行结构化组织，并确保 幻灯片生成过程的 顺利执行：

```py
 def slide_generator(text, prs):
    prompt = f"Summarize the following text to a DALL-E image generation " \
             f"prompt: \n {text}"
    model_engine = "gpt-4"
    dlp = client.chat.completions.create(
        model=model_engine,
        messages=[
            {"role": "user", "content": "I will ask you a question"},
            {"role": "assistant", "content": "Ok"},
            {"role": "user", "content": f"{prompt}"}
        ],
        max_tokens=250,
        n=1,
        stop=None,
        temperature=0.8
    )
    dalle_prompt = dlp.choices[0].message.content
    response = client.images.generate(
        model="dall-e-3",
        prompt=dalle_prompt + " Style: digital art",
        n=1,
        size="1024x1024"
    )
    image_url = response.data[0].url
```

前面的代码展示了 我们如何定义 `slide_generator()` 函数，该函数负责根据用户提供的文本生成 DALL-E 图像生成提示，并将其与 ChatGPT API 集成。 此函数接受两个参数： `text`，代表用户的输入，以及 `prs`，指的是 PowerPoint 演示对象。

`prompt` 变量是通过将静态引入文本与用户提供的 `text` 参数连接起来定义的。 此提示将用于指示 ChatGPT 模型将给定文本总结为 DALL-E 图像 生成提示。

`The` `create()` 方法随后被调用，以向 ChatGPT API 发送文本补全请求。 ChatGPT API 的 JSON 响应存储在 `dlp` 变量中，其中包含补全结果。 从 ChatGPT 补全结果中提取的实际文本被分配给 `dalle_prompt` 变量。 这个 `dalle_prompt` 变量将作为 DALL-E 图像 生成 API 的输入提示。

`现在，我们可以开始使用 DALL-E API 生成图像，该图像基于在 `slide_generator` 函数中创建的 DALL-E 提示。 我们调用 `client.images.generate()` 方法，使用以下参数向 DALL-E API 发送图像生成请求：

1.  `prompt`: 图像生成的提示，该提示通过附加 `dalle_prompt` (由 ChatGPT API 生成) 和图像所需风格（指定为 `Style:` `digital art`) 构建而成。

    `这促使 DALL-E 根据文本输入生成具有数字艺术风格的图像。 您可以根据 您的喜好更改此风格。

1.  `n`: 这指定了要生成的图像数量。 在这种情况下，我们只请求 一张图像。

1.  `size`: 这设置了生成的图像所需的大小。 在这里，它被指定为 `1024x1024` 像素，这是 最高品质的风格。

`DALL-E API 的响应 存储在 `response` 变量中。 它 包含有关生成的图像的信息，包括其 URL。 生成的图像的 URL 从响应中提取并分配给 `image_url` 变量。 此 URL 可用于在我们的 `PowerPoint 幻灯片中检索和显示生成的图像。

`在这个部分，我们构建了 `slide_generator()` 函数，该函数使用 ChatGPT API 根据用户输入生成 DALL-E 提示，然后利用 DALL-E API 生成具有所需风格的图像。 这种集成使我们能够创建视觉上令人惊叹的艺术作品，并提高我们 PowerPoint 幻灯片的品质和影响力。

在下一节中，我们将介绍如何在我们的 PowerPoint 演示文稿中创建幻灯片标题和项目符号。 我们将演示如何将 AI 生成的图像和文本 整合到幻灯片中。

# 完成并测试 PowerPoint 演示文稿生成器

在本节的最后，您将学习使用 AI 生成的图像和文本创建 PowerPoint 演示文稿幻灯片内容的过程。 我们将探讨如何无缝地将 AI 生成的元素，包括 DALL-E 的图像和 ChatGPT 的文本，整合到幻灯片中。 通过完成本节，您将能够将 AI 生成的内容传递到您的 PowerPoint 幻灯片中，为您的演示文稿增添独特性和精致感。 完成本节后，您将能够将 AI 生成的图像 URL 获取并应用于幻灯片背景或将其插入到特定的幻灯片元素中。 您还将学习如何在幻灯片中将 AI 生成的文本作为项目符号整合。

您将学习 如何检索 AI 生成的图像 URL 并将其应用于幻灯片背景或插入到特定的幻灯片元素中。 此外，您还将把 AI 生成的文本作为幻灯片中的项目符号整合。 通过 结合 AI 图像生成的力量和 ChatGPT 的文本完成功能，用户将能够用视觉上吸引人的图像和 相关的文本丰富他们的幻灯片。

首先，我们将演示如何使用 AI 生成的图像和文本创建幻灯片标题和项目符号。 要生成该内容，您可以在您的 `slide_generator()` 函数中包含以下代码片段：

```py
 \
prompt = f"Create a bullet point text for a Powerpoint" \
             f"slide from the following text: \n {text}"
    ppt = client.chat.completions.create(
        model=model_engine,
        messages=[
            {"role": "user", "content": "I will ask you a question"},
            {"role": "assistant", "content": "Ok"},
            {"role": "user", "content": f"{prompt}"}
        ],
        max_tokens=1024,
        n=1,
        stop=None,
        temperature=0.8
    )
    ppt_text = ppt.choices[0].message.content
    prompt = f"Create a title for a Powerpoint" \
             f"slide from the following text: \n {text}"
    ppt = client.chat.completions.create(
        model=model_engine,
        messages=[
            {"role": "user", "content": "I will ask you a question"},
            {"role": "assistant", "content": "Ok"},
            {"role": "user", "content": f"{prompt}"}
        ],
        max_tokens=1024,
        n=1,
        stop=None,
        temperature=0.8
    )
    ppt_header = ppt.choices[0].message.content
```

在代码的第一部分，一个 `prompt` 字符串被 构建，使用户输入的单个段落。 此提示请求 AI 模型为 PowerPoint 幻灯片生成项目符号文本。 然后将提示传递给 ChatGPT 的 `create()` 方法。 完成的结果存储在 `ppt` 变量中，生成的项目符号文本从 `ppt.choices[0].message.content` 中提取，并分配给 `ppt_text` 变量。

同样，在代码的第二部分，另一个 `prompt` 字符串被构建，以请求 AI 模型根据相同的用户提供的文本生成一个 PowerPoint 幻灯片的标题。 完成文本的结果从 `ppt.choices[0].message.content` 中提取，并分配给 `ppt_header` 变量。

此生成的项目符号文本和标题将用于创建实际的 PowerPoint 幻灯片。 您可以将以下代码包含在内，向 PowerPoint 演示文稿添加新幻灯片，并用图像、项目符号 文本和 标题填充它：

```py
 # Add a new slide to the presentation
slide = prs.slides.add_slide(prs.slide_layouts[1])
response = requests.get(image_url)
img_bytes = BytesIO(response.content)
slide.shapes.add_picture(img_bytes, Inches(1), Inches(1))
# Add text box
txBox = slide.shapes.add_textbox(Inches(3), Inches(1),
                                 Inches(4), Inches(1.5))
tf = txBox.text_frame
tf.text = ppt_text
title_shape = slide.shapes.title
title_shape.text = ppt_header
```

以下是实现新 PowerPoint 幻灯片的步骤：

1.  首先，使用 `add_slide()` 方法将新幻灯片添加到演示文稿中。 该方法接受所需的幻灯片布局作为参数，在这种情况下表示内容幻灯片的布局。

1.  接下来，通过 HTTP 请求从图像 URL 检索图像。 获取响应，并将其内容读取到名为 `BytesIO` 的对象 `img_bytes`中。这允许访问和处理图像数据。 要将图像添加到幻灯片中，使用 `add_picture()` 方法。 它接受 `img_bytes` 对象以及所需的定位参数作为参数。 在这种情况下，图像定位在幻灯片左侧 `1` 英寸和顶部 `1` 英寸处。

1.  使用 `slide.shapes.add_textbox()` 方法将文本框添加到幻灯片中。 该方法接受文本框的位置参数作为参数。 使用 `txBox.text_frame` 属性访问文本框的文本框架，并将包含生成的项目符号文本的 `ppt_text` 变量分配给 `tf.text`。这设置了文本框的内容为 AI 生成的文本。

1.  最后，访问幻灯片的 标题形状。 包含生成的标题文本的 `ppt_header` 变量被分配给 `title_shape.text`。这设置了幻灯片的标题为 AI 生成的标题。

这样，我们的演示文稿将包含所需的内容，包括图像、项目符号文本和标题。

你的 PowerPoint 演示文稿生成器应用程序现在已完成，并准备好进行测试。 为了避免任何意外错误，你可以通过使用 GitHub 上可用的完整 `app.py` 文件来验证你的代码行是否正确： GitHub: [https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API/blob/main/Chapter09%20PowerPoint%20Generator/app.py](https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API/blob/main/Chapter09%20PowerPoint%20Generator/app.py)。

要运行应用程序，只需执行代码或运行 `app.py` 文件。 这将启动 Tkinter 应用程序窗口，该窗口作为你的 幻灯片生成器。

一旦应用程序运行，你将看到一个包含文本框的应用程序窗口，你可以在其中输入幻灯片的内 容。 只需将所需文本输入或粘贴到文本框中。 在这个例子中，我们使用了一篇关于水母的文章（见 *图 9**.2*）。

“*水母，也称为海蜇或海葵，是居住在世界上海洋中的迷人生物。 它们属于腔肠动物门，以其凝胶状的体形和触手为特征。 水母的形状、大小和颜色多种多样，从小而透明到大而鲜艳。 它们具有独特的生命周期，从附着在海底的珊瑚虫开始，然后转变为* *自由游动的海蜇。*

*水母最引人入胜的方面之一是它们的刺痛能力。 它们拥有称为刺细胞的专业细胞，其中含有能释放毒液的刺丝囊。 这些刺细胞使水母能够捕捉猎物并防御潜在的捕食者。 虽然大多数水母的刺痛对人类无害，但有些物种可以引起疼痛甚至危险的反应。 在已知有水母群的水域游泳时，建议谨慎行事。* *水母。*

*水母在海洋生态系统中扮演着重要的角色。 它们是机会主义捕食者，以小型浮游生物、鱼卵甚至其他水母为食。 作为捕食者，它们帮助维持海洋食物网的平衡。 此外，水母具有适应不断变化的* *环境条件。*。”

您可以选择使用提供的文本，或者根据您的偏好进行实验性的文本输入。

![图 9.2 – 使用文本输入填充 PowerPoint 演示文稿生成器应用](img/B22414_09_02.jpg)

图 9.2 – 使用文本输入填充 PowerPoint 演示文稿生成器应用

重要提示

确保在每段之间包含一个双行空格，以正确地表示段落之间的分隔，使您的应用能够准确地识别和区分它们。

现在，您可以通过点击**创建幻灯片**按钮来根据输入的文本生成 PowerPoint 幻灯片。请耐心等待几秒钟；您的演示文稿将迅速生成并保存在您项目的**主**目录中。

重要提示

OpenAI 有一个速率限制，指定了每分钟允许使用的令牌数量。如果您广泛使用该应用，可能会达到免费试用版的速率限制，目前为每分钟 40,000 个令牌。

打开演示文稿后，您将看到三张幻灯片，每张幻灯片对应输入文本的一个段落。这些幻灯片将展示由 DALL-E 创建的引人入胜的 AI 艺术图像，配有简洁的项目符号和每张幻灯片的独特标题（见*图 9*.3*）：

![图 9.3 – PowerPoint 演示文稿生成器应用生成的幻灯片](img/B22414_09_03.jpg)

图 9.3 – PowerPoint 演示文稿生成器应用生成的幻灯片

为了增强幻灯片上元素的可视布局，您可以使用 PowerPoint 内置的**设计器**选项，它方便地位于右上角。

这是完成和测试 DALL-E AI 演示文稿生成器的完整过程。您学习了如何生成幻灯片标题、项目符号和 AI 生成的图像。我们介绍了如何构建 AI 提示并检索生成的内容。本节还概述了如何使用生成的图像、项目符号和标题填充 PowerPoint 演示文稿。

# 摘要

本章探讨了 DALL-E，一个高级 AI 模型，根据文本描述生成逼真、新颖图像的能力。 您学习了如何使用 DALL-E API 将 DALL-E 集成到您的应用程序中。 此外，我们指导您使用 `pptx` Python 库和 ChatGPT API 开发 PowerPoint 演示生成器应用程序。 我们使用 Tkinter 构建了 GUI，允许用户使用 `get_slides()` 函数生成幻灯片，该函数检索用户输入并创建一个 PowerPoint 演示文稿。

然后，我们构建了 `slide_generator()` 函数，它在将用户输入转换为 AI 生成的幻灯片方面发挥了关键作用。 本章还提供了将 AI 生成的图像和文本，如项目符号和幻灯片标题，无缝集成到 PowerPoint 幻灯片中的说明。 我们测试了我们的 AI 演示生成器应用程序，让您能够运行并评估您增强的 PowerPoint 演示文稿。

在第 *第十章*中， *使用 Whisper API 进行语音识别和文本到语音转换*，您将学习如何利用 Whisper API 进行音频转录。 我们将专注于一个实际项目，该项目涉及将各种文件类型转录成文本，以创建使用英语人类水平鲁棒性和准确性的字幕。 语音识别。
