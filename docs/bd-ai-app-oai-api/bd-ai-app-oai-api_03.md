# 第四章：3

# 使用 Flask 创建和部署代码错误修复应用程序

在本章中，我们将深入探讨使用 Flask 创建和部署一个由人工智能驱动的代码错误修复 SaaS 应用程序。此 应用程序将利用 OpenAI 的 GPT-3.5 语言模型为用户提供代码错误解释和修复。随着软件开发速度的快速增长，对有效且高效的错误调试解决方案的需求比以往任何时候都更加重要。 通过构建此 SaaS 应用程序，我们的目标是向开发者提供一款强大的工具，以加快他们的调试过程并提高其代码的质量。 

为了开发此 SaaS 应用程序，我们将利用一个你可能已经非常熟悉的工具，即用于在 Python 中构建网络应用的 Flask 框架。我们的目标是熟练掌握这个框架，以构建一个有效且可扩展的 Web 应用程序，能够为用户提供高效的代码错误解决方案。 我们的目标是熟练掌握这个框架，以构建一个有效且可扩展的 Web 应用程序，能够为用户提供高效的代码错误解决方案。 此外，我们还将利用 OpenAI 的 GPT-3.5 语言模型提供自然语言解释和代码修复。

在本章中，我们将学习如何进行以下操作： 

+   执行多个 ChatGPT API 请求

+   设置代码错误 修复器项目

+   实现代码错误 修复器后端

+   使用文本区域 和容器

+   测试代码错误 修复器应用

+   将 ChatGPT 应用部署到 Azure 云 中

完成本章后，你将具备使你的 ChatGPT 应用程序对全球个人用户可访问所需的技能。

# 技术要求

本项目的技术要求如下 ： 

+   在 你的机器上安装了 Python 3.7 或更高版本

+   一个代码编辑器，例如 VSCode（推荐）

+   一个 Python 虚拟环境

+   在 虚拟环境 中安装的 Flask 网络框架

+   一个 OpenAI API 密钥

+   访问云托管服务，例如 Microsoft Azure

本章中的代码示例可以在 GitHub 上找到 ，位于 [https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API](https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API)。

# 执行多个 ChatGPT API 请求

让我们首先 深入了解 ChatGPT API 请求，并学习如何构建项目和 `app.py` 文件。 在这个项目中，我们将探讨如何进行多个 ChatGPT API 请求，以获取我们输入查询的解释和修复后的代码。 我们将逐步讲解整个过程，并提供清晰的示例，以便您可以轻松跟随。

为了修复用户的代码，ChatGPT 需要两个关键组件：一些有错误的代码和系统提供的错误信息。 代码错误修复器应用程序背后的理念是，你同时向 ChatGPT 提供两个单独的请求（参见 *图 3**.1*）：

+   **请求 1**：ChatGPT 使用有错误的代码和错误信息来修复 代码

+   **请求 2**：ChatGPT 使用有错误的代码和错误信息，用普通英语向用户解释错误

![图 3.1 – 代码错误修复器请求/响应映射](img/B22414_03_1.jpg)

图 3.1 – 代码错误修复器请求/响应映射

如图 所示 *图 3**.2*，代码错误修复器应用程序的设计是一个界面简洁、现代的 Web 应用程序。 它包含一个表单，允许用户输入代码和相关错误。 提交表单后，应用程序将代码和错误发送到服务器，并在两个单独的 文本区域中显示生成的解释和修复后的代码。

用户界面的设计简单直观，有两个文本区域用于代码和错误输入，以及两个只读文本区域用于显示生成的解释和修复后的代码。 应用程序的布局整洁，采用蓝色和白色的配色方案，并有一个居中的标题显示应用程序的名称。 应用程序旨在为识别和修复 代码错误提供无缝的用户体验。

![图 3.2 – 代码错误修复器应用程序用户界面](img/B22414_03_2.jpg)

图 3.2 – 代码错误修复器应用程序用户界面

我们的应用程序将接受有错误的代码和一个错误，并同时提供两个请求：一个 用于修复代码，另一个向用户解释错误。 应用程序将具有高效的用户界面和简单的布局，从而提供无缝的用户体验。 现在，我们可以回顾一下在 VSCode 中创建必要的文件和文件夹的过程，包括 `CodeBugFixer` 项目，包括 `templates` 文件夹，该文件夹将包含 Flask 应用程序的 HTML 模板，以及 `config.py` 和 `app.py` 文件，以在 Code Bug Fixer 应用程序中利用 ChatGPT API 奠定基础。

# 设置 Code Bug Fixer 项目

任何 Python 开发过程中的第一步是创建一个名为 `CodeBugFixer`的 VSCode 项目。 你可以通过前往(venv) 指示器来在 VSCode 中打开终端，以确认你正在虚拟环境中工作。 这是防止项目间包安装冲突并确保你使用正确集合的依赖项的必要步骤。 的依赖项。

现在我们可以按照以下步骤创建必要的文件和文件夹，用于 `CodeBugFixer` 项目：

1.  首先，在项目的根目录中创建两个新的 Python 文件，分别命名为 `app.py` 和 `config.py`。 `app.py` 文件是编写`CodeBugFixer` 应用程序主要代码的地方，而`config.py` 文件将包含任何敏感信息，如 API 密钥 和密码。

1.  接下来，在项目的根目录中创建一个名为 `templates` 的新文件夹。 此文件夹将包含 Flask 应用程序将渲染的 HTML 模板。 在`templates` 文件夹内，创建一个名为 `index.html`的新文件。 此文件将包含`CodeBugFixer` 应用程序主页的 HTML 代码。

    项目结构应如下所示 ：

    ```py
    CodeBugFixer/
    ├── config.py
    ├── app.py
    ├── templates/
    CodeBugFixer project in your VSCode project. You can now start writing the code for your Flask app in the app.py file and the HTML code in the index.html file.
    ```

1.  在终端窗口中，你可以按照以下方式安装任何必要的库：

    ```py
     (venv)$ pip install flask
    (venv)$ pip install openai
    ```

1.  最后，为了在您的 `CodeBugFixer` 应用程序中建立使用 ChatGPT API 的基础，您需要将以下代码添加到 `config.py` 和 `app.py` :

config.py

```py
 API_KEY = <Your API Key>
```

app.py

```py
 from flask import Flask, request, render_template
from openai import OpenAI
import config
app = Flask(__name__)
# API Token
client = OpenAI(
  api_key=config.API_KEY,
)
@app.route("/")
def index():
    return render_template("index.html")
if __name__ == "__main__":
    app.run()
```

`config.py` 文件将安全地存储您的 OpenAI API 密钥。 请确保将 `<Your API Key>` 替换为您从 OpenAI 获得的实际 API 密钥。

The `app.py` 文件是一个 Python 脚本，它定义了一个 Flask 应用程序。 它导入了 `Flask` 模块以及 CodeBugFixer 项目所需的库。 它从 `config.py` 文件中获取 API 密钥。 该脚本定义了主页的路线，并使用 Flask 的 `render_template` 函数渲染 `index.html` 模板。

为了设置 `CodeBugFixer` 项目，您需要创建一个新的项目，配置项目设置，创建项目所需文件和文件夹，安装 Flask 和 OpenAI 库，并将代码添加到 `config.py` 和 `app.py` 文件中。 `config.py` 文件安全地存储 OpenAI API 密钥，而`app.py` 文件定义了一个 Flask 应用程序并渲染了`index.html` 模板。 此时，如果您运行`app.py` 文件，您将在浏览器上看到一个空白的（白色）页面。

在下一节中，我们将修改 Code Bug Fixer 应用程序后端的 `index()` 函数，以确保它可以区分 `GET` 和 `POST` 请求。 我们还将使用 OpenAI GPT-3.5 API 为用户的输入生成解释和修复后的代码，然后我们将使用 `render_template` 函数将这些内容传递给 `index.html` 模板。

# 实现 Code Bug Fixer 后端

代码错误修复应用程序的后端 `应确保我们正确地将有错误的代码发送到 ChatGPT，并在另一端接收正确的响应。 `因此，您需要确保 `index()` `函数能够清楚地区分 `GET` `和` `POST` `请求。 `您可以在 `index()` `函数中添加以下修改来实现这一点：

```py
 @app.route("/", methods=["GET", "POST"])
def index():
    if request.method == "POST":
        code = request.form["code"]
        error = request.form["error"]
        prompt = (f"Explain the error in this code without fixing it:"
                  f"\n\n{code}\n\nError:\n\n{error}")
        model_engine = "gpt-3.5-turbo"
```

此函数 `检查 HTTP 请求方法——是 `GET` `还是` `POST` `请求。 `如果请求方法是 `POST` `，这意味着用户已从 `index.html` `模板中提交了表单。 `这样，你可以确保只有在用户提交表单并点击了 **代码修复** `按钮后，ChatGPT API 才会被调用，我们将在下一节中构建该按钮。

函数随后从 `代码` `和` `错误` `变量中分别检索用户的输入。 `这些变量将保存用户想要修复的代码和接收到的错误代码。 `然后使用提取的 `代码` `和` `错误` `变量来创建一个传递给 OpenAI ChatGPT API 的提示。 `这些变量使用 Python 中的 **f-strings** `进行格式化。

例如，假设 `代码` `和` `错误` `变量具有以下值：

```py
 code = print(1
error = SyntaxError: '(' was never closed
```

在这种情况下，我们的提示将显示如下：

```py
 Explain the error in this code without fixing it:
print(1
Error:
SyntaxError: '(' was never closed
The API is instructed to generate an explanation for the error in the code.
```

`提示` `变量为 ChatGPT 模型提供了一个明确和具体的任务，并确保模型生成的输出与 `用户输入` `相关。

您可以将 `gpt-3.5-turbo` `字符串值设置为 `model_engine` `变量。 `此变量随后用于指定用于文本补全的 OpenAI 语言模型。 `在这种情况下， `gpt-3.5-turbo` `是 OpenAI 提供的功能最强大和最通用的模型之一。

`在` `if` `语句中，你现在可以调用 ChatGPT API 来为用户构建` `explanation_completion` `和` `fixed_code_completion` `：`

```py
 explanation_completions = client.chat.completions.create(
            model=model_engine,
            messages=[{"role": "user", "content": f"{prompt}"}],
            max_tokens=1024,
            n=1,
            stop=None,
            temperature=0.2,
        )
        explanation = explanation_completions.choices[0].message.content
        fixed_code_prompt = (f"Fix this code: \n\n{code}\n\nError:\n\n{error}." f" \n Respond only with the fixed code.")
        fixed_code_completions = client.chat.completions.create(
            model=model_engine,
            messages=[
                {"role": "user", "content": f"{fixed_code_prompt}"},
            ],
            max_tokens=1024,
            n=1,
            stop=None,
            temperature=0.2,
        )
        fixed_code = fixed_code_completions.choices[0].message.content
```

`前面的代码利用 ChatGPT API` `根据给定的提示生成补全内容。` `在这段代码中，` `client.chat.completions.create()` `函数被调用两次，以生成两个不同的补全内容：` `explanation_completions` `和` `fixed_code_completions` `。`

`在调用` `client.chat.completions.create()` `函数两次后，` `解释` `和` `fixed_code` `变量分别被分配给两个对象的 choices 属性中生成的文本。` `这些生成的文本将在代码的下一步中使用，以向用户显示解释和修复代码。`

最后，你可以使用`render_template` `函数来更新一个 HTML 模板，并使用 `ChatGPT 响应`：

```py
return render_template("index.html",explanation=explanation,fixed_code=fixed_code) return render_template("index.html")
if __name__ == "__main__":
    app.run()
```

`该函数接受两个参数：第一个是要渲染的模板名称，第二个是将在模板中使用的变量字典。 `传递给模板的变量是` `解释` `和` `fixed_code` `。` `解释` `是 ChatGPT API 生成的文本，用于解释用户代码中的错误。` `fixed_code` `是 ChatGPT API 生成的文本，表示用户代码的修复版本。`

`这些变量被传递到` `index.html` `模板中，以便它们可以显示给用户。` `模板将使用这些变量来动态更新页面内容，显示由 API 生成的解释和修复代码。`

`执行你的 Flask 应用程序后，不应显示任何错误。` `然而，你的浏览器会显示一个完全空白的屏幕，因为我们的` `index.html` `文件是空的。` `我们的下一个任务是创建该文件，这将在下一节中讨论。`

这就是如何修改 `index()` 函数以确保在用户提交表单并点击 `render_template` 函数之后才调用 ChatGPT API。

在下一节中，您将学习如何创建一个 `index.html` 文件并构建一个 **代码修复** 按钮，允许用户提交他们的代码进行调试。 您还将学习如何使用 JavaScript 和 AJAX 处理表单提交并在不重新加载 页面的情况下显示 Flask 应用程序的响应。

# 使用文本区域和容器

在本节中，您将学习如何使用 HTML 和 CSS 为您的 `CodeBugFixer` 应用程序创建一个简单的用户 界面。 我们将逐步讲解为用户设计一个简单表单的过程，以便他们输入代码和错误信息，并显示 ChatGPT API 调用的结果。 在本节结束时，您将拥有一个功能齐全的 Web 应用程序，可以帮助用户修复他们代码中的错误。

要修改 `index.html` 文件，首先，您需要打开位于您项目 `templates` 文件夹中的文件。 您可以在任何文本编辑器中打开此文件，或者您可以使用 VSCode 内置的 HTML 编辑器。 一旦文件打开，添加以下基本的 HTML 结构：

```py
 <!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Code Bug Fixer</title>
</head>
<body>
<div class="header">
    <h1>Code Bug Fixer</h1>
</div>
</body>
</html>
```

The `<title>` 标签将 HTML 页面的标题设置为 `代码错误修复器`。这是当页面加载时将在浏览器标签上显示的文本。 它还被搜索引擎用于在 搜索结果中显示页面的标题。

然后，您可以使用 `<h1>` 标签在网页上创建一个包含文本 **代码错误修复器** 的标题，并使用大字体。

接下来，我们将向我们的 `index.html` 文件中添加一个输入表单。 此表单将允许用户输入他们的代码和相应的错误信息，然后点击 `<``div>` 标签：

```py
 <div class="header">
    <h1>Code Bug Fixer</h1>
</div>
<form action="/" method="post">
    <button class="submit-button" type="submit">Code Fix</button>
    <div class="container">
        <div class="left-column">
            <textarea name="code" placeholder="Enter Code"></textarea>
            <textarea name="error" placeholder="Enter Error"></textarea>
        </div>
        <div class="right-column">
            <textarea class="fixed-code" name="fixed-code" placeholder="Fixed Code" readonly>{{ fixed_code }}</textarea>
            <textarea class="explanation" name="explanation" placeholder="Explanation" readonly>{{ explanation }}</textarea>
        </div>
    </div>
</form>
```

表单被 HTML `<form>` 标签包围，其中 `action` 属性设置为 `"/"` ，而 `method` 属性设置为 `post`。这意味着当用户提交表单时，数据将通过 HTTP `POST` 方法发送到根 URL `("/")` 。 这指的是我们 `index()` 函数在 `app.py` 文件中的实现。

在表单内部，我们有两个文本区域，其 `name` 属性分别设置为 `code` 和 `error`。 这将允许用户输入他们的代码和 错误信息。

表单还包含一个 `submit` 按钮，带有 `submit-button` 类。 当用户点击此按钮时，表单将被提交，数据将被发送到 后端服务器。

表单 分为两列，使用 `<div>` 标签，并带有 `container` 类。 左侧列包含代码和错误信息文本区域，而右侧列包含固定的代码和解释 文本区域。

固定的代码和解释文本区域将 `name` 属性设置为 `fixed-code` 和 `explanation`，分别。 它们也被设置为 `readonly`，因此用户无法修改其内容。 这些文本区域将在用户提交 表单后，由 ChatGPT API 生成的固定代码和解释进行更新。

现在你已经添加了index 页面的必要 HTML 代码，让我们运行 Flask 应用程序并查看它在浏览器中的外观。 当你运行 Flask 应用程序时，你应该会看到一个带有标题的页面，标题为**Code Bug Fixer** ，以及一个包含两个文本区域以输入你的代码和错误信息的表单。 还有一个标记为**Code Fix**的提交按钮。 在表单的右侧，你将看到另外两个文本区域，它们将显示修复后的代码和说明。 然而，这些文本区域最初将是空的，直到用户输入代码和错误信息并点击**Code Fix** 按钮（见图 3**.3**）。

目前，由于我们没有将任何样式添加到 HTML 中，应用程序的外观可能不够吸引人。

![图 3.3 – 基于 HTML 的 Code Bug Fixer 应用前端](img/B22414_03_3.jpg)

图 3.3 – 基于 HTML 的 Code Bug Fixer 应用前端

最后，我们将向我们的应用程序添加一些 CSS 来样式化 HTML 元素。CSS 代码将定义页面中元素的字体、颜色、大小和位置。 我们将向我们的index.html 文件中添加 CSS 代码，以增强应用程序的外观和感觉。 通过这些样式改进，我们的应用程序将更具视觉吸引力，并且更容易为我们的用户使用。

你可以在`title` 标签内 `head` 的 `index.html` 文件中添加你的 CSS 代码：

```py
 <title>Code Bug Fixer</title>
    <style type="text/css">
        body {
            font-family: Arial sans-serif;
            background-color: #f6f7f9;
        }
```

首先，让我们打开 CSS 样式块。 它表示以下代码是 CSS，并将应用于网页中的 HTML 元素。 

CSS 代码针对 HTML 中的<body> 元素。 在 CSS 中，你可以使用它们的标签名、类名或 ID 来定位元素。 在这里，我们正在定位整个网页的 body。

在这里，我们还可以将font-family 设置为Arial sans-serif ，这意味着任何在body 元素内的文本都将使用 Arial 字体显示，或者如果 Arial 不可用，则使用类似的 sans-serif 字体。 我们还可以将 body 的背景色设置为浅灰色：

```py
 .header {
            background-color: #3b5998;
            color: #ffffff;
            padding: 15px;
            text-align: center;
        }
        .header h1 {
            margin: 0;
            font-size: 28px;
            font-weight: bold;
        }
        .container {
            display: flex;
            justify-content: space-between;
            padding: 15px;
        }
```

`.header` 类 指定了标题的背景颜色、文字颜色、填充和对齐方式。 在这里，文本是通过使用 `text-align` 属性来居中的。

`.header h1` 类 字体大小 属性设置为 28 像素， `字体粗细` 设置为 `粗体`，并且 `边距` 设置为 `0`，因此页面顶部和 标题之间没有空间。

`.container` 类 的`display` 属性设置为 `flex` ，以便我们可以使用 Flexbox 进行布局，并且 `justify-content` 设置为 `space-between` ，以便在水平轴上均匀分布容器的内容：

```py
 .left-column, .right-column {
            width: 49%;
            padding: 15px;
        }
        textarea {
            width: 100%;
            height: 200px;
            font-size: 16px;
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #cccccc;
            border-radius: 5px;
            resize: none;
        }
        .fixed-code, .explanation {
            background-color: #e9ebee;
            color: #3b5998;
            font-weight: bold;
        }
```

接下来，让我们设计一个 两列布局，其中左侧列包含用户输入代码和错误的输入字段，而右侧列显示 程序的输出。

`.left-column` 和 `.right-column` 类被赋予了 `49%` 的宽度，以便它们在页面上占据几乎相等的空间。 `textarea` 元素被设计为具有固定的宽度为 `100%` 和高度为 `200px` ，以创建一个清晰的输入字段。 `resize: none` 属性确保用户不能调整 输入字段的大小。

`.fixed-code` 和 `.explanation` 类被赋予了`bold` 的`font-weight` 属性，使它们从输入字段中脱颖而出。 `这有助于用户快速识别 输出字段：

```py
 .submit-button {
            background-color: #3b5998;
            color: #ffffff;
            font-size: 18px;
            font-weight: bold;
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            margin-top: 15px;
            cursor: pointer;
        }
        .submit-button:hover {
            background-color:  #4b76be;
        }
    </style>
```

我们的最终目标是 使 `提交` 按钮看起来更吸引人，并在用户悬停时向用户提供反馈。 我们将背景颜色设置为深蓝色（`#3b5998`）并将字体颜色设置为白色（`#ffffff`）以创建对比。 我们还添加了填充和边框半径，使按钮更引人注目 和吸引人。

`margin-top` 属性在按钮和文本区域之间添加了一些空间。 最后，我们添加了 `:hover` 伪类，当用户悬停时改变按钮的背景颜色，这为用户提供视觉反馈，表明按钮 是交互式的。

您现在已经完成了 `index.html` 文件，这是 `CodeBugFixer` 应用程序的。 现在您可以运行 `app.py` 文件并测试应用程序，以确保一切运行正常。 在下一节中，我们将提供一些如何使用应用程序修复代码错误的示例。 或者，您也可以尝试自己的代码示例来查看应用程序的表现。

这就是我们如何使用 HTML 和 CSS 为 `CodeBugFixer` 应用程序创建用户界面。 我们介绍了为用户设计表单的过程，以便他们可以输入代码和错误信息，并显示 ChatGPT API 调用的结果。

在下一节中，您将通过编写测试用例并验证它是否正确处理不同的场景来学习如何测试 `CodeBugFixer` 应用程序。 您还将看到 Code Bug Fixer 应用程序如何修复用 Java 和 Python 两种不同的编程语言编写的代码的示例，并学习如何测试修复后的代码以确认 其准确性。

# 测试代码错误修复应用程序

现在，我们将学习如何测试我们的代码错误修复应用程序。 我们 将编写一些测试用例以确保应用程序能够正确运行并适当地处理不同的场景。 测试是软件开发的一个关键部分，因为它确保应用程序按预期工作并满足 用户的需求。

代码错误修复器应用程序可以修复任何编程语言，使其成为一个多功能的工具。因此，我们将通过使用两种不同编程语言编写的示例来展示其有效性：Java 和 Python。

**我们的 Python 示例有两个属性。 **首先，我们有以下 **有错误的代码：**

```py
 def factorial(n):
    if n == 0:
        return 1
    else:
        return n * factorial(n-1)
print("The factorial of 5 is: " + factorial(5))
```

然后，您可以输入以下错误：`类型错误：`

**此代码返回错误，因为它试图在`print` 语句中将字符串和整数连接起来。 **`factorial` 函数返回一个整数，但它被连接到字符串`"The factorial of 5` is: "`.**

**为了在代码错误修复器应用程序中修复此代码，请按照以下步骤操作： **这些步骤：**

1.  将代码粘贴到**输入** **代码** 字段中。

1.  将错误粘贴到**输入** **错误** 字段中。

1.  左键单击**代码** **修复** 按钮。

完成列出的步骤后，固定代码和 ChatGPT API 生成的解释将显示在相应的字段中。如图**图 3**.4*所示，ChatGPT 的响应以粗体显示且不可编辑。 您还可以看到解释是准确的。

![**图 3.4 – 代码错误修复器处理一些 Python 代码**](img/B22414_03_4.jpg)

**图 3.4 – 代码错误修复器处理一些 Python 代码**

为了确保固定代码正确运行，您可以通过在 Python IDE（如 VSCode 或任何其他在线 Python 编译器）中运行它来测试它。通过检查与固定代码一起提供的说明，您可以确认它提供了对错误的准确和详细的解释。 错误。

**最后，为了验证代码错误修复器应用程序可以与任何编程语言一起工作，让我们尝试修复以下 **Java 代码：**

```py
 public class ComplexError {
    public static void main(String[] args) {
        String str = "Hello, World!";
        int length = str.length();
        for (int i = 0; i <= length; i++) {
            System.out.println(str.charAt(i));
        }
    }
}
```

**以下是 Java 编译器产生的错误代码：**

```py
for loop is iterating from 0 to length, which is the length of the string. However, the index of the last character in the string is length -1, so when i equals length, the program will try to access an index that is out of bounds and throw an IndexOutOfBoundsException message.
			Once you run the code through the Code Bug Fixer app, you will get a response similar to the one depicted in *Figure 3**.5*. You can verify that the fixed code and the description are correct. ChatGPT fixed the error because it changed the condition in the `for` loop from `i <= length` to `i < length`. By changing the condition to `i < length`, the loop only iterates up to the last index of the string and avoids accessing an index that does not exist.
			This confirms that your Code Bug Fixer has the ability to differentiate between two distinct programming languages and was able to repair the code within a matter of seconds.
			![Figure 3.5 – Code Bug Fixer processing some Java code](img/B22414_03_5.jpg)

			Figure 3.5 – Code Bug Fixer processing some Java code
			This section explained how to test the Code Bug Fixer application by providing examples in the Python and Java programming languages. The application can fix any programming language, and the fixed code can be verified by running it in a Python or Java IDE. We saw how the application can differentiate between two programming languages and fix the code within seconds.
			In the next section, you will learn how to set up an Azure account, install the Azure CLI, and deploy its application to the cloud using the CLI. You will also learn how to create a `requirements.txt` file in the root directory of your project, listing all the Python packages and their specific versions that your app needs to run.
			Deploying the ChatGPT app to the Azure cloud
			Now let’s upload our Code Bug Fixer application to the **Azure** cloud platform. Azure is a cloud computing service provided by Microsoft that allows developers to host, manage, and scale their applications in the cloud. By hosting our application on Azure, we can make our application accessible to a wider audience. We will go through the steps required to create an Azure account, set up our application for deployment, and deploy it to Azure using the Azure CLI.
			You can create your Azure account by following these steps:

				1.  Go to the Azure website at [https://azure.microsoft.com/free/](https://azure.microsoft.com/free/) and click on **Start free**.
				2.  Sign in with your Microsoft account or create one if you don’t have one.
				3.  Enter your personal information and payment details. You won’t be charged unless you upgrade to a paid plan.
				4.  Choose your subscription type and agree to the terms and conditions.
				5.  Verify your account by entering a phone number and entering the code you receive.
				6.  You can log in to your account at [https://signup.azure.com/](https://signup.azure.com/).
				7.  Once you log in, on the `portal.azure.com/#home`, where you can start using Azure services (see *Figure 3**.6*).

			![Figure 3.6 – Azure home page](img/B22414_03_6.jpg)

			Figure 3.6 – Azure home page
			Important note
			When deploying the Code Bug Fixer app to the Azure cloud, it is important to name the file containing the Flask app `app.py`. This is because Azure expects the main file to be named `app.py` for deployment to work properly.
			To deploy our application on Azure, we need to install the Azure **Command-Line Interface** (**CLI**). The CLI provides us with an efficient way to automate and manage our Azure resources. Next, you can find the steps to install the Azure CLI.
			These are the steps for macOS users ([https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-macos](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-macos)):

				1.  Open your macOS Terminal.
				2.  Install `brew`. Homebrew is a popular package manager for macOS:

    ```

    **$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"**

    ```py

    				3.  Once Homebrew is installed, run the following command to install Azure CLI:

    ```

    **$ brew update && brew install azure-cli**

    ```py

    				4.  Verify that the installation was successful by running the following command:

    ```

    **$ az --version**

    ```py

    				5.  This should display the version number of the Azure CLI that you just installed:

    ```

    **azure-cli** 2.47.0

    **core** 2.47.0

    **telemetry** 1.0.8

    依赖项:

    msal                              1.20.0

    azure-mgmt-resource               22.0.0

    Python 位置 '/opt/homebrew/Cellar/azure-cli/2.47.0/libexec/bin/python'

    扩展目录 '/Users/martinyanev/.azure/cliextensions'

    Python (Darwin) 3.10.11 (main, Apr  7 2023, 07:24:47) [Clang 14.0.0 (clang-1400.0.29.202)]

    法律文件和信息：aka.ms/AzureCliLegal

    您的 CLI 已更新。

    ```py

			These are the steps for Windows users ([https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows?tabs=azure-cli](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows?tabs=azure-cli)):

				1.  Open a web browser and navigate to [https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows).
				2.  Scroll down to the section titled **Install or update** and click on the **Microsoft Installer** (**MSI**) tab.
				3.  Click on the **Latest release of the Azure CLI** button to start the MSI package download.
				4.  Once the MSI package has downloaded, double-click on the file to begin the installation process.
				5.  Follow the prompts in the installation wizard to complete the installation process.
				6.  Once the installation is complete, open a Command Prompt or PowerShell window and type the following:

    ```

    $ az –version

    ```py

    For Linux users ([https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=script](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=script)), you can type the following in the Terminal:

    ```

    curl -L https://aka.ms/InstallAzureCli | bash

    ```py

			This line will automatically download the installation script and you can follow all steps for setting up your Azure CLI.
			Important note
			After installing the Azure CLI, you may need to close and re-open your Terminal window to ensure that the CLI is fully recognized and operational.
			You should now be able to use the Azure CLI on your Linux machine.
			Before deploying your app to Azure using the CLI, you’ll need to log in to your Azure account from the CLI. This will enable you to manage your resources and perform other tasks related to your account.
			To log in to your Azure account using the Azure CLI, you can type in the Terminal window:

```

$ az login

```py

			A pop-up window will appear asking you to enter your Azure account credentials. Enter your account email and password and click on the **Sign in** button. Once you are successfully logged in, you will see a message in the Command Prompt indicating that you are authenticated.
			To prepare our app for deployment, we need to create a file called `requirements.txt` in the root directory of our `CodeBugFixer` project. This file will list all the Python packages and their specific versions that our app needs to run. The `requirements.txt` file is a common practice in Python projects to document and manage the project’s dependencies. It lists all the external packages and their versions that are necessary for the application to function properly. Each line in the `requirements.txt` file typically represents a separate package and may include a specific version number or a version range. Azure will use this file to install the dependencies in the server where our app will be hosted.
			The project structure should look like the following:

```

CodeBugFixer/

├── config.py

├── app.py

├── requirements.txt

├── templates/

│   └── index.html

```py

			To create a `requirements.txt` file in your project, follow these steps:

				1.  Right-click on your `CodeBugFixer` project folder in the **Explorer panel** on the left-hand side of VSCode.
				2.  Select `requirements.txt` as the filename.
				3.  Once the file is created, you can add the necessary libraries to it. To include Flask and OpenAI libraries, add the following lines to your `requirements.txt` file:

    ```

    Flask==3.0.2

    openai==1.13.3

    ```py

			Now that we have everything set up, it’s time to deploy the app to Azure. We’ll be using the Azure CLI to accomplish this: 

				1.  First, navigate to the root directory of your project, where the `app.py` and `requirements.txt` files are located.
				2.  Next, run the following command in the CLI:

    ```

    $ az webapp up --name <app_name>

    ```py

    In this command, `webapp up` deploys the web app to Azure. Replace `<app-name>` with a unique name of your choice.

    				3.  After deploying the app, if everything is set up correctly, you should see a JSON output in the Azure CLI terminal. This output will contain information about the deployed app, such as the app name, URL, location, resource group, and other details. It will also include the status of the deployment, such as whether it was successful or whether there were any errors:

    ```

    {

    "URL": "http://<app_name>.azurewebsites.net",

    "appserviceplan": "martinyanev94_asp_7277",

    "location": "location",

    "name": "您的应用名称",

    "os": "Linux",

    "resourcegroup": "resourcegroup",

    "runtime_version": "python|3.10",

    "runtime_version_detected": "-",

    "sku": "PREMIUMV2",

    "src_path": "//Local/app/path"

    }

    ```py

			After successfully deploying your app on Azure, you can access it from anywhere around the world using the URL provided in the JSON output. Simply copy the URL and paste it into your web browser’s address bar to open the app. You can also share this URL with others to let them use your Code Bug Fixer app.
			If you want to upload the Code Bug Fixer app to your *WordPress* website, you can easily do so by using an `iframe`. Simply copy and paste the following code into the text editor, replacing `<your_app_url>` with the URL of your deployed app:

```

<iframe src="img/<your_app_url> " width="100%" height="1000"></iframe>

```py

			This will embed the app into your website and allow your visitors to access it directly from your page.
			Finally, you can access your deployed application from the Azure web portal. To access your app from your Azure web account, follow these steps:

				1.  Sign in to the Azure portal at [https://portal.azure.com/#home](https://portal.azure.com/#home).
				2.  In the left-hand menu, click on **App Services**.
				3.  From there, you will be able to manage the app that you deployed.

			Here, you can modify or delete the deployment from your Azure web account (see *Figure 3**.7*):

				*   To modify the deployment, you can upload a new version of your code using the Azure CLI or Azure portal
				*   To delete the deployment, simply click on the **Delete** button on the **Overview** page of your app in the Azure portal

			![Figure 3.7 – Azure cloud app management](img/B22414_03_7.jpg)

			Figure 3.7 – Azure cloud app management
			This is how you can deploy your app on the Azure cloud using the Azure CLI. We listed all the steps involved in creating an Azure account, installing the Azure CLI, and preparing the app for deployment. The section also emphasized that the main file of the app should be named `app.py` for proper deployment, and a `requirements.txt` file is needed to list all the Python packages required by the app to run. The steps to install the Azure CLI for macOS and Windows users were also listed.
			Summary
			This chapter described the creation and deployment of an AI-powered SaaS application, Code Bug Fixer, which uses OpenAI’s GPT-3.5 language model to provide code error explanations and fixes to users. It covered building the application using Flask, creating a web form that accepts user input for code and error messages, and designing a web interface to display the generated explanations and solutions. The chapter also provided instructions on how to test and deploy the application to the Azure cloud platform, offering security and scalability features to the application.
			Furthermore, you learned how to create a user interface for Code Bug Fixer using HTML and CSS, adding a basic HTML structure, a header, an input form, and two columns containing text areas for the user to enter their code and error message. The testing process involved running test cases for the application in two different programming languages, Python and Java. By following the given steps, users can check whether the application is repairing the codes accurately or not. This chapter provided a comprehensive guide to building, testing, and deploying an AI-powered SaaS application that can help users fix errors in their code.
			In *Chapter 4*, *Integrating the Code Bug-Fixing Application with a Payment Service*, you will learn how to integrate the popular Stripe payment service into your web application using the Stripe API. The chapter will cover the step-by-step process of setting up a Stripe account, configuring the API keys, creating a form to collect user payment information, and processing payments securely through Stripe. Additionally, the chapter will explain how to track and store user data securely for future transactions and analysis.

```
