# 第十一章：10

# 使用 Whisper API 进行语音识别和语音合成

欢迎来到 *第十章* 我们探索尖端人工智能技术世界的旅程。 在本章中，我们将开始探索令人瞩目的 **Whisper API**。借助先进的语音识别和翻译能力，Whisper API 为将音频转换为文本开辟了令人兴奋的可能性。 想象一下，能够轻松转录对话、访谈、播客或任何口语内容的能力。 无论你旨在从多语言音频文件中提取有价值的见解，还是为全球受众创建可访问的内容，Whisper API 都能为你提供解决方案。 Whisper API 涵盖了广泛的语音，确保与各种语音需求兼容。

在本章中，我们将通过使用 Python 开发语言转录项目来深入探讨 Whisper API 的核心功能。 我们将熟悉其基本端点，即转录和翻译，这些构成了其语音转文本能力的基础。 凭借其最先进的开源模型，Whisper 为开发者提供了强大的工具，可以无缝地将音频文件转录成多种语言，甚至翻译成英语。 Whisper API 处理广泛的语音，确保与各种语音需求兼容。 它可以用于智能扬声器、免提设备和其他语音启用技术，通过提供准确和响应迅速的语音合成能力来提升用户体验。

在全球化交流比以往任何时候都重要的世界里，Whisper API 的翻译端点发挥着至关重要的作用。 此端点不仅转录音频文件，还将它们翻译成英语，弥合语言之间的差距，促进 无缝理解。

在本章中，你将学习以下主题： 以下内容：

+   使用 Whisper API 实现文本翻译和转录

+   构建语音 转录应用程序

+   使用 PyDub 处理更长的音频输入

在本章结束时，你将全面了解 Whisper API 及其卓越功能，包括语音转文本转换和语言翻译。 你还将学习如何利用 API 准确转录音频文件，将其翻译成英语，处理更长的输入，并在你的应用程序中释放口语的潜力。

# 技术要求

为了成功进行开发桌面翻译应用程序的项目，您必须满足以下 技术先决条件：

+   确保您的机器已安装 Python 3.7 或更高版本 。

+   拥有代码编辑器，如 VSCode（推荐） 设置

+   创建一个 Python 虚拟环境

+   获取 OpenAI API 密钥

+   在您的项目中安装 PyDub

本章展示的代码片段可在 GitHub 平台上找到。 您可以通过以下 链接 访问它们：[https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API](https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API)。

# 使用 Whisper API 实现文本翻译和转录

在本节中，我们将 探讨 Whisper API 的功能 ，以使用 Python 无缝转录和翻译 音频文件。 随着语音识别和翻译技术的进步，我们现在可以轻松地将口语转换为文本，并有效地消除语言障碍。 通过遵循提供的逐步说明，您将获得将 Whisper API 集成到 Python 项目中并释放基于 音频数据 的潜力的知识和技能。

在本节中，我们将 探讨音频文件的转录和翻译 的不同方面。 从设置和安装 要求开始，我们将确保您拥有必要的工具，包括 Python、代码编辑器、Python 虚拟环境和 OpenAI API 密钥。

要使用 Python 中的 Whisper API 进行音频文件的转录和翻译，建议创建一个名为 `WhisperAPI` 的目录，并使用 VSCode IDE 加载它。 按照 *第一章*中概述的步骤，从终端窗口激活您的虚拟环境，*开始使用 ChatGPT API 进行 NLP 任务*。

一旦项目创建完成，您将看到 VSCode 编辑器窗口，在那里您可以开始编写您的 Python 代码。 您可以在 VSCode 项目视图中创建 Python 脚本、导入库和组织项目结构。 。

现在您已经设置了项目，下一步是安装必要的库，即 OpenAI 和 PyDub。 要这样做，请转到终端窗口打开终端。 从那里，输入以下命令：

```py
 $pip install openai
$pip install pydub
$brew install ffmpeg
```

通过执行这些命令，您将自动下载并安装 OpenAI 和 PyDub 库，确保它们可以随时用于您的项目。 FFmpeg 是一个功能强大且多才多艺的开源多媒体框架，用于处理音频和 视频文件。

安装这些库至关重要，因为 OpenAI 提供了与 Whisper API 交互所需的必要功能，而 PyDub 允许高效处理音频文件，包括分割和 导出它们。

现在，是时候在您的项目中创建一个名为 `config.py` 的新文件了。 此文件将安全存储您的 API 密钥，允许您的代码访问 Whisper API。

为了创建 `config.py` 文件并添加您的 API 密钥，请按照以下步骤操作： 这些步骤：

1.  在您的项目视图区域上右键点击。

1.  从 上下文菜单 中选择 **新建文件** 。

1.  将文件命名为 `config.py` 并 按 *Enter*。

现在，您 已经创建了 `config.py` 文件。 在编辑器中打开该文件，并添加以下代码行： 以下代码行：

```py
 API_KEY = "YOUR_API_KEY"
```

为了使 您的 音频文件在整章中组织有序且易于访问，建议在您的项目中创建一个名为 `audio_files` 的文件夹。 此文件夹将作为集中存储所有将在本章的各种示例和演示中使用的音频文件的地点。 This folder will serve as a centralized location to store all the audio files that will be used in the various examples and demonstrations covered in the chapter.

要创建 `audio_files` 文件夹，请按照以下步骤操作： 这些步骤：

1.  在您的项目视图区域上右键点击。

1.  从 上下文菜单 中选择 **新建文件夹** 。

1.  将目录命名为 `audio_files` 并 按 *Enter*。

现在您已经设置了必要的配置和文件夹结构，是时候创建一个名为 `transcribe.py`的新 Python 文件了。 在编辑器中打开该文件，这将是你编写与 Whisper API 交互并转录音频文件的代码的空间。

在编写任何代码之前，您可以将您选择的英语语音音频文件放入 `audio_files` 文件夹中。 在这个例子中，我将使用一个名为 `apple.mp3`的音频文件。该文件包含关于苹果电脑的几句话，您可以从我们的 Git 仓库下载它： [https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API/blob/main/Chapter10%20Whisper/audio_files/apple.mp3](https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API/blob/main/Chapter10%20Whisper/audio_files/apple.mp3)。

重要提示

在使用 Whisper API 进行音频处理和转录任务时，确保提供的输入音频文件格式是受支持的至关重要。 Whisper API 目前接受以下文件类型： `mp3`, `mp4`, `mpeg`, `mpga`, `m4a`, `wav`, 和 `webm`。

现在让我们按照以下步骤转录我们的音频文件：

1.  一旦您的音频 文件就绪，您可以通过向 `transcribe.py`添加以下代码来 转录它：

    ```py
     from openai import OpenAI
    import config
    # API Token
    client = OpenAI(api_key=config.API_KEY)
    file= open("audio_files/apple.mp3", "rb")
    result = client.audio.transcriptions.create(model="whisper-1", file=file)
    print(result)
    ```

    在这里， `openai` 库提供了访问 OpenAI API 的权限，这使得开发者能够与各种语言模型和 AI 功能进行交互。 API 密钥分配给 `OpenAI(api_key=config.API_KEY)` 变量，使用存储在 API 密钥中的值。

1.  然后，我们以读取二进制模式打开一个名为 `apple.mp3` 的文件，该文件位于 `audio_files/apple.mp3` 本地文件路径中， `rb`。这表明代码的意图是读取一个 音频文件。

1.  之后，调用了 `client.audio.transcriptions.create()` 函数，并传入两个参数： `whisper-1` 和打开的音频文件。 此函数是 OpenAI 库的一部分，用于转录音频文件。 其中， `whisper-1` 参数 是 OpenAI 系统中的特定 转录模型。

1.  最后，我们在 控制台中看到了转录的结果：

    ```py
    {
     "text": "Macbook laptops are known for their modern design and high-quality build, offering seamless experience. They are favored by many professionals and creators for their powerful performance and integration with Apple's ecosystem."
    }
    ```

转录的结果，在控制台中打印出来，是一个包含单个键值对的 JSON 对象。 键是 `text`，对应的值是 转录的文本。

这表明转录过程已成功将音频文件转换为文本，捕捉了与 MacBook 笔记本的功能和声誉相关的 内容。

如果您的音频文件不是英语，Whisper 也会处理这种情况，通过在后台自动转录并翻译文件。 现在，我们将指导您如何使用 Whisper API 翻译转录的文本。 在这个例子中，我们将使用一个名为 `german.mp3`的德语语音文件： [https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-APIs/blob/main/Chapter10%20Whisper/audio_files/german.mp3](https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-APIs/blob/main/Chapter10%20Whisper/audio_files/german.mp3)。

您还可以使用任何语言的 `mp3` 文件。

要使用 Whisper API 翻译音频文件，您可以创建一个名为 `translate.py` 的新 Python 文件，并添加以下代码：

```py
 from openai import OpenAI
import config
# API Token
client = OpenAI(api_key=config.API_KEY)
whisper_file= open("audio_files/german.mp3", "rb")
result = client.audio.translations.create(model="whisper-1", file=whisper_file)
print(result)
```

一旦正确设置了 API 密钥和音频文件的路径，代码就会调用 `client.audio.translations.create()` 函数，提供模型名称或 `whisper-1` 配置 和打开的音频文件 `whisper_file`。此函数使用 Whisper API 进行翻译。

翻译结果 将存储在 `result` 变量中， 可以使用 `print(result)` 来打印，并在 控制台中显示翻译后的文本：

```py
 {
  "text": "The Germans are known for not liking to do small talk. Today I'm going to try here in Berlin to see if I can do a little small talk with the people here. Let's go!" }
```

正如你所见，文本已成功使用 Whisper API 和不到 10 行 Python 代码从德语翻译成英语。

在本节中，你了解了 Whisper API 使用 Python 无缝转录和翻译音频文件的能力。 逐步说明指导你完成了转录和翻译音频文件的设置过程，包括支持的文件格式。 本节展示了成功的转录和翻译示例，展示了 Whisper API 在将音频转换为文本和通过最小代码跨越语言障碍方面的有效性。

在下一节中，你将学习如何集成 Tkinter 和 Whisper API 来开发一个用户友好的语言转录应用程序，该应用程序可以将口语语言实时转换为文本。

# 构建语音转录应用程序

在本节中，我们将探讨通过集成 Tkinter（一个流行的 Python GUI 工具包）和强大的 Whisper API 来开发语言转录应用程序的开发。 这种集成将使我们能够创建一个用户友好的界面，实现口语语言的实时转录。 通过遵循逐步说明并利用 Tkinter 和 Whisper API 的功能，你将能够开发自己的 GUI 应用程序，为语音识别和语言处理打开无数可能性。 通过遵循逐步说明并利用 Tkinter 和 Whisper API 的功能，你将能够开发自己的 GUI 应用程序，为语音识别和语言处理打开无数可能性。

无论你渴望创建用于转录访谈的工具、为视频生成字幕，还是仅仅探索语音到文本技术的潜力，本节都将为你提供将你的想法付诸实践的知识和技能。 因此，让我们深入其中，开始使用 Tkinter 和 Whisper API 构建语言转录应用程序的激动人心的旅程。 Whisper API。

为了继续进行语言转录应用程序项目，你可以在同一项目中创建一个名为 `app.py` 的新 Python 文件。 此文件将作为开发 应用程序的主要代码库。

通过创建 `app.py` 文件，你将拥有一个专门的空间来编写必要的代码以集成 Tkinter 和 Whisper API，使你的应用程序能够实现实时转录功能。

在我们追求简洁的过程中，我们的应用程序将采用具有文本字段和按钮的最简设计。 文本字段将作为显示转录文本的专用空间，而按钮将提供轻松浏览文件系统并定位所需音频文件以进行转录的功能 。

要将我们的语言翻译代码转换为实际的应用程序，我们需要创建 `transcribe_audio()` 函数，并从 `Tkinter 图形界面` 中使用它：

```py
 import tkinter as tk
from tkinter import filedialog
from openai import OpenAI
import config
# API Token
client = OpenAI(api_key=config.API_KEY)
def transcribe_audio():
    file_path = filedialog.askopenfilename(filetypes=[("Audio Files", "*.mp3")])
    if file_path:
        try:
            audio_file = open(file_path, "rb")
            transcript = client.audio.transcriptions.create(model="whisper-1", file=audio_file)
            text_window.insert(tk.END, transcript.text)
        except Exception as e:
            text_window.insert(tk.END, f"Error: {str(e)}")
    else:
        text_window.insert(tk.END, "No file selected.")
# Create the Tkinter window
window = tk.Tk()
window.title("Chapter 10 Whisper Transcription App")
# Create a text window
text_window = tk.Text(window, height=30, width=60)
text_window.pack()
# Create a button to select the audio file
button = tk.Button(window, text="Select Audio File", command=transcribe_audio)
button.pack()
# Start the Tkinter event loop
window.mainloop()
```

在执行函数时，它首先使用 `filedialog.askopenfilename()` 方法打开文件对话框，允许用户选择用于转录的音频文件。 文件对话框仅显示具有 `.mp3` 扩展名的文件，这是由 `filetypes` 参数指定的。

如果从文件对话框中获取了有效的文件路径，表示用户已选择了一个音频文件，则代码将进入转录音频的步骤。 在 `try` 块中，使用 `open()` 以读取二进制模式打开选定的音频文件，并将其分配给 `audio_file` 变量。

使用 `client.audio.transcriptions.create()` 函数，将音频文件与特定的转录模型一起传递，通过 Whisper API 启动转录过程。 生成的转录存储在 `transcript` 变量中。

最后，将转录的文本插入到文本窗口中。 `tk.END` 参数确保文本被插入到文本窗口的末尾。 如果在过程中出现任何异常或错误，将在 `文本窗口` 中显示错误消息。

如果没有从文件对话框中获取文件路径，表示用户未选择音频文件，则将在 `未选择文件` 消息插入到 `文本窗口` 中。

在 `transcribe_audio()` 函数下，我们创建了使用它的 Tkinter 应用程序。 首先，使用 `tk.Tk()`创建了一个 Tkinter 窗口。窗口的标题设置为 `Whisper` `转录应用程序`。

接下来，使用 `tk.Text()`创建了一个文本窗口，其中将显示 Whisper API 的转录结果。 还使用 `tk.Button()`在 Tkinter 窗口中添加了一个按钮。按钮的标签设置为 `选择音频文件`，并将 `command` 参数设置为 `transcribe_audio`。这意味着当按钮被点击时，将执行之前定义的 `transcribe_audio()` 函数。

要运行 Whisper 转录应用程序，请按照 以下步骤操作：

1.  在 VSCode 中，找到 `app.py` 文件并 运行它。

1.  一旦应用程序运行，将出现一个标题为 **Whisper 转录应用程序** 的 Tkinter 窗口。

1.  窗口将显示一个文本区域，其中将显示转录的文本。

1.  点击 **选择音频文件** 按钮。 将打开一个文件对话框窗口，允许您浏览您的 文件系统。

1.  找到并选择您想要 转录的 MP3 音频文件。

选择文件后，代码将尝试转录音频。 如果成功，转录的文本将在 Tkinter 窗口内的文本区域中显示（见 *图 10**.1*）。 如果在转录过程中出现任何错误，将显示错误消息。

![图 10.1 – Whisper 转录应用程序](img/B22414_10_01.jpg)

图 10.1 – Whisper 转录应用程序

重要提示

请注意，在某些屏幕分辨率下， **选择音频文件** 按钮可能会被隐藏。 为了确保其可见性，请最大化应用程序窗口。

通过运行应用程序并选择一个 MP3 文件，您将能够见证 Whisper API 的转录功能在行动中。应用程序将利用所选的音频文件，使用 Whisper API 启动转录过程，并在 Tkinter 窗口中显示转录的文本。 这允许您将口语转换为书面文字，为各种应用和用例打开可能性。 使用 Whisper API，您只需花上`20 min`就能深入理解知识点，而且记忆深刻，*难以遗忘*。

在本节中，我们开始了语言转录应用程序的开发，集成了 Tkinter 和 Whisper API。该应用程序采用极简设计，具有文本字段和按钮，允许用户选择用于转录的 MP3 音频文件。 选择文件后，代码将使用 Whisper API 启动转录过程，并在 Tkinter 窗口中显示转录的文本。

在下一节中，您将学习如何将 PyDub 与 Whisper API 集成以克服文件大小限制，并有效地分割大型音频文件以实现无缝转录。

# 使用 PyDub 处理较长的音频输入

在本节中，我们将探讨**PyDub**，一个强大的 Python 音频处理库，与 Whisper API 的集成，以克服 API 强加的 25 MB 文件大小限制。使用 PyDub，我们可以有效地将大型音频文件分割成更小的片段，从而实现长时间录音的无缝转录。 通过遵循说明并利用 PyDub 的功能，您将能够充分发挥 Whisper API 的潜力，转录任何大小的音频文件。

利用 PyDub 的强大功能来增强您的语言转录工作流程是一个简单的过程。通过使用这个库，您可以轻松地将长音频文件分割成更小的片段。 例如，如果您有一个 10 分钟的音频文件，您可以轻松地将其分割成两个单独的文件，每个文件持续 5 分钟。 然后，您可以将这些较小的文件提交给 Whisper API 进行转录，确保您的文件不会因为其大小限制而被拒绝。 使用 PyDub，您可以无缝克服文件大小限制并简化您的转录过程。

在这个练习中，我们使用了一个大约十一分钟的 iPhone 评测音频文件。现在，你可以在你的项目中创建一个新的 Python 文件，具体命名为 `longer_files.py`。这个文件将作为一个专门的空间来实施处理更大音频文件的语言 转录工作流程：

```py
 from openai import OpenAI
from pydub import AudioSegment
import config
# API Token
client = OpenAI(api_key=config.API_KEY)
song = AudioSegment.from_mp3("audio_files/phone.mp3")
# 5 minute portion
five_minutes = 5 * 60 * 1000
first_min_5 = song[:five_minutes]
first_min_5.export("audio_files/phone_first_5.mp3", format="mp3")
last_min_5 = song[five_minutes:]
last_min_5.export("audio_files/phone_last_5.mp3", format="mp3")
file= open("audio_files/phone_first_5.mp3", "rb")
result = client.audio.transcriptions.create(model="whisper-1", file=file)
print(result)
file= open("audio_files/phone_last_5.mp3", "rb")
result = client.audio.transcriptions.create(model="whisper-1", file=file)
print(result)
```

提供的代码演示了在语言 转录工作流程中处理更大音频文件时使用 `PyDub` 库和 Whisper API 的方法。

您可以从 `phone.mp3` 文件中下载，链接如下： [https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API/blob/main/Chapter10%20Whisper/audio_files/phone.mp3](https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API/blob/main/Chapter10%20Whisper/audio_files/phone.mp3)

首先，执行必要的导入，包括从相应库中的 `openai` 和 `AudioSegment` 模块。 我们使用 PyDub 的 `phone.mp3` 文件，通过 `AudioSegment.from_mp3()` 方法加载，并将其分配给 `song` 变量。

为了将音频文件分解成更小的片段，代码使用 `five_minutes` 变量定义了五分钟的部分，该变量代表所需的持续时间（以毫秒为单位）。 然后， `song` 变量使用指定的持续时间进行切片，以获取音频的前五分钟，并将其存储在 `first_min_5` 变量中。 这个片段随后被导出为一个名为 `phone_first_5.mp3` 的单独 MP3 文件，使用的是 `export()` 方法。

同样，代码使用切片操作获取音频文件的剩余部分，从五分钟标记开始直到结束，并将其分配给 `last_min_5` 变量。 这个片段也被导出为一个名为 `phone_last_5.mp3`的单独 MP3 文件。

代码继续使用 `phone_first_5.mp3` 文件，并通过 `client.audio.transcriptions.create()` 方法调用打开。 生成的转录结果存储在 `result` 变量中，然后将其打印到控制台。 按照相同的程序，代码重复对 `phone_last_5.mp3` 文件进行转录过程，打开它，转录它，并打印 结果。

你可以在控制台看到每个文件的独特转录结果，如下所示：

```py
 {
  "text": "It's finally here, the most wanted phone this year, the most amazing camera set ever built in a phone. Here is the iPhone 13 Pro.…….reduced the front camera module, now the phone will look good." }
{
  "text": "weights about 10 grams more which is something you can't really feel. However, while everybody is trying to reduce the weight on the phones, Apple actually increased it……... Now thanks for watching and I will see you in the next one." }
```

通过使用 PyDub 将较大的音频文件分解成更小的片段，代码使得这些片段可以在 Whisper API 的 `大小限制` 内进行处理。 这种方法允许我们有效地处理较大的音频文件，同时利用 Whisper API 提供的转录能力。

这就是我们如何将 PyDub 与 Whisper API 集成以克服文件大小限制，从而实现更长音频文件的流畅转录。

# 总结

在本章中，我们探讨了 Whisper API，这是一个通过高级语音识别和翻译将音频转换为文本的强大工具。 本章提供了使用 Python 开发语言转录项目的逐步指导，涵盖了处理音频文件、安装必要的库和设置 API 密钥等基本方面。 你学习了如何使用 Whisper API 转录和翻译音频文件。 本章还介绍了一个语音转录应用程序，它集成了 Tkinter 和 Whisper API 以实现 实时转录。

你还学习了如何使用 PyDub，一个强大的 Python 音频处理库，结合 Whisper API 来克服 25 MB 的文件大小限制。 通过利用 PyDub 的功能，我们可以有效地将大型音频文件分割成更小的片段，从而实现长时间录音的无缝转录。 你看到了如何使用 PyDub 和 Whisper API 在语言转录工作流程中处理更大的音频文件。 通过将音频文件分解成更小的片段并逐个转录每个片段，我们可以在利用 Whisper API 的转录能力的同时处理更大的音频文件。

在第 *第十一章*中，你将了解 ChatGPT API 中可用的不同 API 模型，并获得如何为你的特定项目选择最合适的模型的见解。 我们将探讨在 API 请求中可以使用的各种参数，以实现更高效、改进的提示完成。 你还将了解与不同 AI 模型相关的限制。

# 第四部分：为 ChatGPT 应用提供高级概念

在本最后一部分，你将探索 AI 模型及其独特的属性，从而让你全面了解它们的成本、质量和提示长度考虑因素。 掌握这些知识后，你将能够选择与你的特定应用需求相匹配的最佳模型。 此外，你将解锁构建自己的 ChatGPT 模型的能力，并通过微调和嵌入来增强其功能，从而使其完美地满足你的 个人需求。

本部分包含以下章节：

+   *第十一章*, *选择正确的 ChatGPT API 模型*

+   *第十二章*, *微调 ChatGPT 以创建独特的 API 模型*
