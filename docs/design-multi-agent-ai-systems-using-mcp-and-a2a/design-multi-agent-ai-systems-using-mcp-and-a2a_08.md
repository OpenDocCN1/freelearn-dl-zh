## 5 实现自定义工具

在上一章中，我们详细探讨了 AI-6 框架的后端架构，重点介绍了引擎如何编排智能体循环、管理记忆和会话状态，以及与多个 LLM 提供方和工具的集成。我们了解了工具和模型是如何被动态发现的，以及引擎如何通过统一抽象来协调它们。这一基础使得灵活、与提供方无关的 AI 工作流成为可能，具备持久记忆、可恢复会话以及安全、可扩展的工具使用能力。

在本章中，我们将把焦点从整体引擎架构转向对工具系统本身的更深入研究。虽然*第4章*在很大程度上将工具视为可插拔的黑盒，但本章我们将深入内部，考察通用工具如何被设计为可复用、一致，并可在不同 LLM 提供方之间移植。我们将探讨如何用结构化模式定义工具、如何验证参数，以及如何注册工具并将其转换为提供方特有的格式。

我们将首先讨论 AI-6 如何允许用户按照通用 AI-6 工具格式实现工具。这一个通用工具实现可以适用于任何 LLM 提供方，例如 OpenAI、Ollama，甚至某个尚不存在的未来 LLM 提供方。我们还将研究 OpenAI 和 Ollama 的具体 API，了解它们如何通过略有不同的规范提供类似功能，以及 AI-6 如何在不让用户接触其内部实现的情况下利用这两个提供方；接着会回顾模式定义和输入验证策略。在此基础上，我们将了解 AI-6 如何将其内部工具表示映射为提供方特有的格式（如 OpenAI 函数调用）。最后，我们将研究工具执行如何被管理，包括结果如何被捕获并通过会话回传给 LLM。本章将为构建功能强大、安全且可互操作的工具奠定基础，从而扩展智能体 AI 系统的能力。

在本章中，我们将涵盖以下主要内容：

- 定义通用工具
- 定义 AI-6 自定义工具
- 手动实现新工具

### 技术要求

本章没有特定的技术要求。如果你想构建自己的自定义工具，可以使用任意 Python 库。你只需要实现 AI-6 框架本身提供的 **Tool** 接口即可定制你的工具。本章代码可从这里获取：

`https://github.com/PacktPublishing/Design-Multi-Agent-AI-Systems-Using-MCP-and-A2A/tree/main/ch05/ai-six`

### 定义与提供方无关的工具

每个 LLM 提供方 API（如 OpenAI、Anthropic、Gemini 和 Ollama）都有自己指定工具的方式。由于 AI-6 需要与所有 LLM 提供方协作，因此它定义了自己与提供方无关的格式。AI-6 框架中直接与第三方 LLM 提供方 API 或 SDK 交互的 LLM 提供方，知道如何将通用工具定义转换为提供方特有的工具定义。

AI-6 的工具定义必须是所有 API 所需信息的超集。工具定义的重点是函数调用，不过也支持内置工具。让我们来看一下 OpenAI 和 Ollama 等 LLM 提供方 Python SDK 中的工具定义。我们之所以查看 Python SDK 而不是原始 REST API，是因为 AI-6 的 LLM 提供方是用 Python 实现的，并使用相应第三方 LLM 提供方的 Python SDK。

首先来看 OpenAI Python SDK，了解它如何指定工具和工具调用。

### OpenAI Python 工具规范

OpenAI Python SDK 可在 `https://github.com/openai/openai-python` 获取。它是根据 OpenAI REST API 的 OpenAPI 规范（`https://github.com/openai/openai-openapi`）自动生成的，因此与实际的 OpenAI API 保持 100% 一致。

我们关注的工具定义位于聊天补全（chat completions）端点，可通过 Chat Completions API 的 `Completions` 类的 `create()` 方法（`https://github.com/openai/openai-python/blob/main/src/openai/resources/chat/completions/completions.py#L76`）访问。请不要将其与旧版 Completions API（`https://platform.openai.com/docs/api-reference/completions`）混淆。

该方法会调用 LLM（可选地携带大量参数）并获取响应。该响应可以是最终响应，也可以是工具调用，AI-6 将对此进行处理（正如我们在上一章中讨论的那样）。

`create()` 方法有八个重载版本。这意味着有八个不同的方法，它们都名为 `create()`，只是参数略有不同。重载方法之所以有用，尤其是对于参数很多的方法，是因为不同的重载版本可以提供不同层次的控制，而非仅仅追求易用性：

```python
@overload
def create(
    self,
    *,
    messages: Iterable[ChatCompletionMessageParam],
    model: Union[str, ChatModel],
    audio: Optional[ChatCompletionAudioParam] | NotGiven = NOT_GIVEN,
    frequency_penalty: Optional[float] | NotGiven = NOT_GIVEN,
    function_call: completion_create_params.FunctionCall | NotGiven = NOT_GIVEN,
    functions: Iterable[completion_create_params.Function] | NotGiven = NOT_GIVEN,
    logit_bias: Optional[Dict[str, int]] | NotGiven = NOT_GIVEN,
    logprobs: Optional[bool] | NotGiven = NOT_GIVEN,
    max_completion_tokens: Optional[int] | NotGiven = NOT_GIVEN,
    max_tokens: Optional[int] | NotGiven = NOT_GIVEN,
    metadata: Optional[Metadata] | NotGiven = NOT_GIVEN,
    modalities: Optional[List[Literal["text", "audio"]]] | NotGiven = NOT_GIVEN,
    n: Optional[int] | NotGiven = NOT_GIVEN,
    parallel_tool_calls: bool | NotGiven = NOT_GIVEN,
    prediction: Optional[ChatCompletionPredictionContentParam] | NotGiven = NOT_GIVEN,
    presence_penalty: Optional[float] | NotGiven = NOT_GIVEN,
    reasoning_effort: Optional[ReasoningEffort] | NotGiven = NOT_GIVEN,
    response_format: completion_create_params.ResponseFormat | NotGiven = NOT_GIVEN,
    seed: Optional[int] | NotGiven = NOT_GIVEN,
    service_tier: Optional[Literal["auto", "default", "flex", "scale"]] |
```




```python
    NotGiven = NOT_GIVEN,
    stop: Union[Optional[str], List[str], None] | NotGiven = NOT_GIVEN,
    store: Optional[bool] | NotGiven = NOT_GIVEN,
    stream: Optional[Literal[False]] | NotGiven = NOT_GIVEN,
    stream_options: Optional[ChatCompletionStreamOptionsParam] | NotGiven = NOT_GIVEN,
    temperature: Optional[float] | NotGiven = NOT_GIVEN,
    tool_choice: ChatCompletionToolChoiceOptionParam | NotGiven = NOT_GIVEN,
    tools: Iterable[ChatCompletionToolParam] | NotGiven = NOT_GIVEN,  # 👈
    top_logprobs: Optional[int] | NotGiven = NOT_GIVEN,
    top_p: Optional[float] | NotGiven = NOT_GIVEN,
    user: str | NotGiven = NOT_GIVEN,
    web_search_options: completion_create_params.WebSearchOptions | NotGiven = NOT_GIVEN,
    # Use the following arguments if you need to pass additional parameters to the API
    # that aren't available via kwargs.
    # The extra values given here take precedence over values defined on the
    # client or passed to this method.
    extra_headers: Headers | None = None,
    extra_query: Query | None = None,
    extra_body: Body | None = None,
    timeout: float | httpx.Timeout | None | NotGiven = NOT_GIVEN,
) -> ChatCompletion:
    """
    ...
    """
```

一些重载方法是同步的，另一些是异步的，但它们都以相同的方式定义工具：

```python
tools: Iterable[ChatCompletionToolParam] | NotGiven = NOT_GIVEN,
```

这一行是什么意思？

`Iterable[ChatCompletionToolParam]` 表示提供的工具应当是一个可迭代序列，其中每个元素都是 `ChatCompletionToolParam` 类型。

第二部分 `| NotGiven = NOT_GIVEN` 表示 `tools` 参数也可以属于 `NotGiven` 类型，并且当未提供任何工具时使用该默认值。`NOT_GIVEN` 值是 `NotGiven` 类型的一个常量。

换句话说，`tools` 参数可以是两种类型之一（Python 允许这样做）。第一种类型是 `Iterable[ChatCompletionToolParam]`，你可以将其视为 `ChatCompletionToolParam` 类型的条目列表。第二种类型是 `NotGiven`。`tools` 参数的默认值是 `NOT_GIVEN`。如果调用者未提供任何工具，那么 `tools` 参数的值将为默认值 `NOT_GIVEN`。

如果你想知道 `NotGiven` 是什么，它是一个简单的哨兵类，用于标记未指定的参数，并为它们提供默认值 `NOT_GIVEN`。

好了，让我们来理解 `ChatCompletionToolParam` 是什么。它是一个具有两个字段的类：`function` 和 `type`。这两个字段都是必填的。`function` 字段的类型是 `FunctionDefinition`，而 `type` 字段必须是字面量字符串 `"function"`。

下面是你定义这个类的方式：

```python
class ChatCompletionToolParam(TypedDict, total=False):
    function: Required[FunctionDefinition]
    type: Required[Literal["function"]]
    """The type of the tool. Currently, only `function` is supported."""
```

这里可能会有点令人困惑，因为有两种不同类型的类型。`Required[FunctionDefinition]` 中的函数类型是对必填 Python 类型 `FunctionDefinition` 的类型提示。OpenAI Chat Completions API 的有趣之处在于，它只支持一种工具，即函数工具，但该 API 的定义方式更为通用，似乎可以支持其他工具种类。

其他 OpenAI API 确实支持多种工具种类。已弃用的 Assistants API（beta）(https://github.com/openai/openai-python/blob/main/src/openai/types/beta/assistant.py) 支持两种内置工具：`file_search` 和 `code_interpreter`。

嗯，OpenAI 走过的 API 设计之路相当漫长。最初只有 Chat Completion API，我们使用的正是它，并且它只有 `"function"` 这一种工具类型。

然后，Assistants API 始终未走出 beta 阶段，最终被弃用。后来，Responses API 发布，并带有更多工具种类。

**注意**

以下是内置 `CodeInterpreter` 和 `FileSearch` 工具的定义：

```python
class ToolResourcesCodeInterpreter(BaseModel):
    file_ids: Optional[List[str]] = None
    """
    A list of [file](https://platform.openai.com/docs/api-reference/files) IDs made
    available to the `code_interpreter` tool. There can be a maximum of 20 files
    associated with the tool.
    """

class ToolResourcesFileSearch(BaseModel):
    vector_store_ids: Optional[List[str]] = None
    """
    The ID of the
    [vector store](https://platform.openai.com/docs/api-reference/vector-stores/object)
    attached to this assistant. There can be a maximum of 1 vector store attached to
    the assistant.
    """

class ToolResources(BaseModel):
    code_interpreter: Optional[ToolResourcesCodeInterpreter] = None
    file_search: Optional[ToolResourcesFileSearch] = None
```

新的 Responses API 支持以下在 `types/responses/tool_param.py` (https://github.com/openai/openai-python/blob/main/src/openai/types/responses/tool_param.py#L170) 中定义的工具：

```python
ToolParam: TypeAlias = Union[
    FunctionToolParam,
    FileSearchToolParam,
    WebSearchToolParam,
    ComputerToolParam,
    Mcp,
    CodeInterpreter,
    ImageGeneration,
    LocalShell,
]
```

目前，AI-6 的 OpenAI LLM 提供方使用 Chat Completions API，因此让我们回到 `FunctionDefinition` 类 (https://github.com/openai/openai-python/blob/main/src/openai/types/shared/function_definition.py#L11)。它表示一个函数工具，并具有名称、描述、可选参数和一个可选的 strict 标志（除非显式设置为 `True`，否则为 `False`）：

```python
class FunctionDefinition(BaseModel):
    name: str
    """The name of the function to be called.
    Must be a-z, A-Z, 0-9, or contain underscores and dashes, with a maximum length
    of 64.
    """

    description: Optional[str] = None
    """
    A description of what the function does, used by the model to choose when and
    how to call the function.
    """

    parameters: Optional[FunctionParameters] = None
    """The parameters the functions accepts, described as a JSON Schema object.
    See the [guide](https://platform.openai.com/docs/guides/function-calling) for
    examples, and the
    [JSON Schema reference](https://json-schema.org/understanding-json-schema/) for
    documentation about the format.

    Omitting `parameters` defines a function with an empty parameter list.
    """

    strict: Optional[bool] = None
    """Whether to enable strict schema adherence when generating the function call.
    If set to true, the model will follow the exact schema defined in the
    `parameters` field. Only a subset of JSON Schema is supported when `strict` is
    `true`. Learn more about Structured Outputs in the
    [function calling guide](docs/guides/function-calling).
    """
```

让我们继续深入，看看 `FunctionDefinition` 类中的 `FunctionParameters` (https://github.com/openai/openai-python/blob/main/src/openai/types/shared/function_parameters.py) 到底是什么。事实证明，它是一个简单的 string -> any object 字典。

```python
# File generated from our OpenAPI spec by Stainless.
from typing import Dict
from typing_extensions import TypeAlias

__all__ = ["FunctionParameters"]

FunctionParameters: TypeAlias = Dict[str, object]
```

让我们回顾一下 OpenAI 规定的 Chat Completions API 工具定义要求：

- 每个工具都必须是 `"function"` 类型
- 每个工具都有名称和描述
- 一个工具可以有零个或多个参数，每个参数都有名称和任意值类型
- 一个工具可以指定其输出是否为严格模式

OpenAI 工具规范还有一个重要元素。除了 `create()` 方法的 `tools` 参数之外，还有一个 `tool_choice` 参数：

```python
tool_choice: ChatCompletionToolChoiceOptionParam | NotGiven = NOT_GIVEN,
```

该工具选择参数可以取以下值之一：`"none"`（不调用任何工具）、`"auto"`（由你决定是否要调用一个或多个工具以及调用哪些工具）、`"required"`（你必须至少调用一个工具；由你决定调用哪个或哪些工具），或特定的工具名称（你必须调用该工具）：

```python
ChatCompletionToolChoiceOptionParam: TypeAlias = Union[
    Literal["none", "auto", "required"],
```



```python
ChatCompletionNamedToolChoiceParam
]
```

如果这听起来令人困惑，那是因为它确实如此。我写了一整篇博文来讨论工具选择的问题，以及如何大幅简化它：*修复 OpenAI 工具调用 API*。你可以在这里找到它：`https://medium.com/@the.gigi/fixing-the-openai-tool-calling-api-ede67419f9f5`。

好的。让我们记住这一点，然后看看 Ollama Python SDK 是如何指定工具调用的。

### Ollama Python 工具规范

Ollama Python SDK（`https://github.com/ollama/ollama-python`）已经是许多 API 的抽象层，因为它通过统一接口支持许多提供商。

让我们看看在 Ollama Python SDK 中如何指定工具。

Ollama 使用 `Client` 类（`https://github.com/ollama/ollama-python/blob/main/ollama/_client.py#L114`）的 `chat()` 方法（与 OpenAI 类似的多个重载）与 LLM 交互并指定工具：

```python
@overload
def chat(
    self,
    model: str = '',
    messages: Optional[Sequence[Union[Mapping[str, Any], Message]]] = None,
    *,
    tools: Optional[Sequence[Union[Mapping[str, Any], Tool, Callable]]] = None, # 👈
    stream: Literal[False] = False,
    think: Optional[bool] = None,
    format: Optional[Union[Literal['', 'json'], JsonSchemaValue]] = None,
    options: Optional[Union[Mapping[str, Any], Options]] = None,
    keep_alive: Optional[Union[float, str]] = None,
) -> ChatResponse: ...
```

工具定义非常冗长。让我们来分析一下。名为 `tools` 的参数是可选的，默认值为 `None`。如果提供了该参数，它必须是一个序列（例如列表或元组），其中每一项是三种允许类型之一：

- 一个 `Mapping[str, Any]` JSON 模式，即一个键为字符串、值可以是任意类型的字典。
- 一个 Ollama `Tool` 实例（由 Ollama SDK 提供的类，封装工具元数据和行为）。
- 一个遵循 Google 风格文档字符串的 `Callable` 对象，它会被转换为 Ollama `Tool` 实例：

```python
tools: Optional[
    Sequence[Union[Mapping[str, Any], Tool, Callable]]
] = None
```

无论采用哪种表示形式，它们最终都会变成 Ollama `Tool` 实例的序列（`https://github.com/ollama/ollama-python/blob/main/ollama/_types.py#L312`）：

```python
class Tool(SubscriptableBaseModel):
    type: Optional[Literal['function']] = 'function'

    class Function(SubscriptableBaseModel):
        name: Optional[str] = None
        description: Optional[str] = None

    class Parameters(SubscriptableBaseModel):
        model_config = ConfigDict(populate_by_name=True)
        type: Optional[Literal['object']] = 'object'
        defs: Optional[Any] = Field(None, alias='$defs')
        items: Optional[Any] = None
        required: Optional[Sequence[str]] = None

    class Property(SubscriptableBaseModel):
        model_config = ConfigDict(arbitrary_types_allowed=True)
        type: Optional[Union[str, Sequence[str]]] = None
        items: Optional[Any] = None
        description: Optional[str] = None
        enum: Optional[Sequence[Any]] = None
        properties: Optional[Mapping[str, Property]] = None

    parameters: Optional[Parameters] = None
    function: Optional[Function] = None
```

我们不打算逐行解析这段 Python 代码，只需注意 Ollama 会在工具定义上附加一些模型信息，这些信息可能在将 `Tool` 转换为特定模型的工具定义时使用。

需要注意的重要一点是，与 OpenAI Completion API 类似，唯一支持的工具类型是 `"function"`。

总而言之，OpenAI 和 Ollama 以自己的格式定义工具，但从概念上讲，它们为每个工具包含等价的信息，这对 AI-6 来说非常有利。

我们现在可以看看 AI-6 的通用工具规范模式了。

### 定义 AI-6 工具规范模式

我们在前一章已经介绍了 AI-6 的 `Parameter` 和 `Tool` 类。这里快速回顾一下。它的目标是简单，没有太多花哨的东西。AI-6 中有一个特殊功能，而 OpenAI 和 Ollama 的工具规范中都没有，那就是 `Tool` 类的 `required` 字段。该字段是一组参数名称，当 LLM 调用工具时必须提供这些参数。它是 OpenAI REST API 中工具规范的一部分，但不知何故没有进入 Python SDK：

```python
class Parameter(NamedTuple):
    name: str
    type: str
    description: str

@dataclass(slots=True)
class Tool(ABC):
    name: str
    description: str
    parameters: list[Parameter]
    required: set[str]

    def configure(self, config: dict) -> None:
        """Optional: configure the tool with given parameters."""

    @abstractmethod
    def run(self, **kwargs) -> str:
        """Execute the tool using the given arguments and return the result as a string."""
```

这段直白的代码就是 AI-6 的整个工具规范模式。它非常简单，易于理解。

好了，你已经看到了 AI-6 如何通过 `LLMProvider` 接口抽象提供商及其各自的实现来定义工具。在下一节中，我们将看到如何定义自定义工具，并将其转换为当前 LLM 提供商的工具格式。

### 定义 AI-6 自定义工具

AI-6 自带许多开箱即用的工具示例，而且实现新工具很容易。在本节中，我们将回顾一些现有工具，然后从头实现一个全新的工具。

定义 AI-6 自定义工具的过程包括以下步骤：

1. 创建一个新的 Python 类。
2. 直接或间接继承 `Tool` 基类。
3. 实现 `run()` 方法。
4. 可选地实现 `configure()` 方法。

让我们从通用命令行工具开始，它们非常强大。

#### 通用命令行工具

AI-6 开箱即用的许多有用工具都是命令行工具。这些工具可以在 AI-6 运行的任何地方运行（在你的机器上、容器中、云端等）。AI-6 为命令行工具提供了一个基类，它处理了运行带任意参数的命令行程序所需的所有样板代码，并提供了以专用用户（不同于当前用户）身份运行命令行程序的选项。让我们先看看 `CommandTool` 类是如何工作的，然后再研究从它派生的工具。

**注意：** LLM 并不知道它们调用了命令行工具。你可能还记得，LLM 会收到工具定义，然后通过代理循环请求调用这些工具。实际的工具执行发生在 AI 系统/框架端，在那里它们通常会运行命令行工具。

它导入了 `sh` 包（`https://sh.readthedocs.io/en/latest/index.html`）用于运行 shell 命令，`shlex` 内置包（`https://docs.python.org/3/library/shlex.html`）用于解析 POSIX 命令行参数，以及来自 AI-6 对象模型的 `Parameter` 和 `Tool`：

```python
import sh
import shlex
from backend.object_model import Tool, Parameter
```

下面是 `CommandTool` 类的组织方式，以帮助你实现命令行工具：

1. 该类继承自 `Tool` 基类，因此它可以作为 AI-6 工具的基类。
2. 构造函数接收一个命令名（路径上可用的命令行程序的名称）、一个可选用户，以及一个可选的命令文档链接。
3. 然后它构建一个参数列表，其中只包含一个名为 `args` 的字符串参数，该参数应包含目标命令的所有命令行参数。
4. `args` 参数是必需的，但它可以是空字符串。如果提供了文档链接，它将被添加到命令的描述中，这将帮助 LLM 理解如何调用该命令。

让我们看一下类的定义：

```python
class CommandTool(Tool):
    def __init__(
        self, command_name: str, user: str | None = None,
        doc_link: str = ""
    ):
        self.command_name = command_name
        self.user = user

        description = f'{command_name} tool. ' + f'See {doc_link}' if doc_link else ''
        parameters = [Parameter(name='args', type='string',
                                description=f'command-line arguments for {command_name}')]
        required = {'args'}

        super().__init__(
            name=command_name,
            description=description,



parameters=parameters,
required=required
)
```

`run()` 方法有两个任务。首先，使用 `shlex.split()` 方法解析 `args` 参数中的命令行参数，然后以指定用户（如果提供了）或当前用户的身份执行命令。请注意，当前用户必须能够切换到指定用户（通过 `sudo`）：

```python
def run(self, **kwargs):
    args = shlex.split(kwargs['args'])
    if self.user is not None:
        return sh.sudo('-u', self.user, self.command_name, *args)
    else:
        return getattr(sh, self.command_name)(*args)
```

`CommandTool` 类没有提供特殊的 `configure()` 方法来覆盖可选的 `Tool.configure()` 方法，但从 `CommandTool` 派生的工具类如有必要可以这样做。

现在，让我们看看实现命令工具并为我们的 AI 代理提供强大能力是多么容易。

### 文件系统工具

一些最有用的命令涉及文件和目录。AI-6 提供了基本的目录和文件命令，如 `pwd`、`ls`、`cat` 和 `echo`。

它还提供了一些更复杂的文件编辑命令，如 `sed`、`awk` 和 `patch`。

让我们从**打印工作目录**（`pwd`）开始。我们所要做的就是定义一个名为 `Pwd` 的类，其构造函数只需提供 POSIX `pwd` 命令的文档链接：

```python
from backend.tools.base.command_tool import CommandTool

class Pwd(CommandTool):
    def __init__(self, user: str | None = None):
        doc_link = 'https://www.gnu.org/software/coreutils/manual/html_node/pwd-invocation.html'
        super().__init__('pwd', user=user, doc_link=doc_link)
```

我们可以直接测试它，而无需 AI-6 的其余部分，只需实例化它并检查 `parameters` 和 `description` 等属性：

```text
❯ source venv/bin/activate
❯ python
Python 3.13.3 (main, Apr 8 2025, 13:54:08) [Clang 17.0.0 (clang-1700.0.13.3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> from backend.tools.file_system.pwd import Pwd
>>> pwd = Pwd()
>>> pwd.parameters
[Parameter(name='args', type='string', description='command-line arguments for pwd')]
>>> pwd.description
'pwd tool. See https://www.gnu.org/software/coreutils/manual/html_node/pwd-invocation.html'
```

我们还可以运行它，验证它是否真的返回当前工作目录：

```text
>>> pwd.run(args='')
'/Users/gigi/git/ai-six/py\n'
```

其他文件系统命令也同样简单。以下是 `ls` 命令：

```python
from ..base.command_tool import CommandTool

class Ls(CommandTool):
    def __init__(self, user: str | None = None):
        super().__init__(command_name='ls', user=user,
                         doc_link='https://www.gnu.org/software/coreutils/manual/html_node/ls-invocation.html')
```

以下是 `awk` 命令：

```python
from ..base.command_tool import CommandTool

class Awk(CommandTool):
    def __init__(self, user: str | None = None):
        super().__init__(command_name='awk', user=user,
                         doc_link='https://www.gnu.org/software/gawk/manual/gawk.html')
```

其他大多数文件系统工具都遵循相同的模式，但有一个例外，即 `echo` 工具。我们接下来将讨论它。

#### 实现 echo 工具

`echo` 工具有所不同。`echo` 工具的目的是让 LLM 能够创建新文件或替换现有文件。当然，这是一个超级有用的能力。但是，要在 shell 中做到这一点，必须使用如下所示的 shell 重定向：

```text
> echo "123" > ~/ttt/some-file
> cat ~/ttt/some-file
123
```

请记住要管理工具的安全性和权限，正如我们在*第4章*中讨论的那样。

我们的 `CommandTool` 基类没有提供将命令输出重定向到文件的方法。一种选择是扩展 `CommandTool` 以支持输出重定向，但我们也可以直接实现该功能。让我们看看它是如何工作的。

导入语句类似，不同之处在于它导入了内置的 `os` 包、`Tool` 模块，并且没有导入 `shlex` 包或 `CommandTool` 模块。我们很快就会明白为什么：

```python
import os
import sh
from backend.object_model import Tool, Parameter
```

`Echo` 类直接继承自 `Tool` 模块，而不是 `CommandTool`，因为它需要实现自己的 `run()` 方法：

```python
class Echo(Tool):
```

构造函数稍微复杂一些。由于它不调用内置的 `echo` 命令，因此描述中不包含外部文档链接。它还有两个必需的参数 `content` 和 `file_path`，而不是从 `CommandTool` 派生的工具所具有的通用 `args` 参数。它还存储了 `user`，默认值为 `None`：

```python
def __init__(self, user: str | None = None):
    self.user = user
    desc = 'Write content to a file, creating any necessary directories.'
    super().__init__(
        name='echo',
        description=desc,
        parameters=[
            Parameter(
                name='file_path', type='string',
                description='The path of the file to write to.'),
            Parameter(
                name='content', type='string',
                description='The content to write to the file.')
        ],
        required={'file_path', 'content'}
    )
```

`run()` 方法从参数中提取要写入的文件名和内容（请记住，这些将由 LLM 在调用该工具以创建或替换文件时提供）。然后，它获取目标文件的目录：

```python
def run(self, **kwargs):
    filename = kwargs['file_path']
    content = kwargs['content']
    dir_path = os.path.dirname(filename)
```

接下来的逻辑根据用户进行分支。如果提供了用户，则使用 `sudo` 和 shell 命令 `mkdir` 和 `tee` 来创建目标目录（如果需要）并写入/替换文件：

```python
if self.user is not None:
    sh.sudo('-u', self.user, 'mkdir', '-p', dir_path)
    sh.sudo('-u', self.user, 'tee', filename,
            _in=content, _out=os.devnull)
    return f"Content written to {filename} as user {self.user}"
```

如果没有提供用户，则直接使用 Python 创建目录并写入文件：

```python
os.makedirs(dir_path, exist_ok=True)
with open(filename, 'w') as file:
    file.write(content)
return f"Content written to {filename}"
```

现在，我们将把注意力转向另一个实用工具——测试运行器工具。

### 测试运行器工具

文件系统工具允许 AI-6 修改自己的代码！没错。想一想。它可以使用 `ls` 工具列出自己的文件和目录。它可以使用 `cat` 工具读取自己源代码文件的内容。它可以使用 `echo` 工具创建新文件（或替换现有文件）。最后，它还可以使用 `sed`、`awk` 和 `patch` 工具修改现有文件。但是，智能体式 AI 框架和 LLM 还不能（尚未）在第一次尝试时就做到完全正确。在提交代码更改之前，反复迭代并验证更改是否有效仍然是最佳实践。这就是 `test_runner` 工具的用武之地。该工具可以在任何目录中运行使用标准 Python `unittest` 框架编写的 Python 单元测试。让我们看看它是如何工作的。

像往常一样，它从 AI-6 对象模型中导入 `sh` 模块以及 `Tool` 和 `Parameter`：

```python
import sh
from backend.object_model import Tool, Parameter
```

`TestRunner` 类直接继承自 `Tool` 基类。构造函数调用 `Tool` 基类，向其传递一个描述和一个必需的参数 `test_directory`：

```python
class TestRunner(Tool):
    def __init__(self):
        desc = 'A tool to run Python unit tests using the unittest framework.'
        super().__init__(
            name='python_test_runner',
            description=desc,
            parameters=[Parameter(name='test_directory', 
                                  type='string', 
                                  description='The directory containing tests to run')],
            required={'test_directory'}
        )
```




`run()` 方法通过调用带有 `unittest` 模块的 Python 解释器并指示它发现目标目录中的所有单元测试，来运行目标目录中的全部 Python 单元测试。它返回一个以 `"stdout"` 和 `"stderr"` 为键的字典。它会妥善处理 `sh` 执行异常，但会将其他异常传播给调用方处理：

```python
def run(self, **kwargs):
    test_directory = kwargs['test_directory']
    try:
        result = sh.python('-m', 'unittest', 'discover', '-s',
                           test_directory)
        return {"stdout": str(result), "stderr": ""}
    except sh.ErrorReturnCode as e:
        return {"stdout": e.stdout.decode('utf-8'),
                "stderr": e.stderr.decode('utf-8')}
```

让我们看看它的实际操作。AI-6 后端单元测试位于 Python 根目录下的以下相对目录 `backend/tests` 中：

```python
>>> from backend import tests
>>> tests_dir = tests.__path__[0]
>>> tests_dir
'/Users/gigi/git/ai-six/py/backend/tests'
```

现在我们已经有了测试目录，可以实例化并调用测试运行器工具，并验证结果是成功的：

```python
>>> result = tr.run(test_directory=tests_dir)
>>> assert(result['stdout'] != '')
>>> assert(result['stderr'] == '')
```

很多时候，你需要启动或重启应用程序才能使用你的工具。`bootstrap` 工具可以帮助你做到这一点。让我们看看它是如何实现的。

### Bootstrap 工具

正如我们所见，AI-6 的内置工具支持修改其自身代码并测试这些更改（因为 AI-6 单元测试是使用 `unittest` 框架实现的）。剩下的最后一块拼图是在做出更改并测试这些更改之后，重启控制 AI-6 的程序。这就是 `bootstrap` 工具的用武之地。在对 AI-6 代码进行更改后，LLM 可能会决定重启控制 AI 系统，该系统将以修改后的代码启动：

```python
import importlib
from backend.object_model import Tool
import os
import sys
```

`Bootstrap` 类直接派生自 `Tool`。构造函数非常简单，因为它没有参数。描述解释了将使用 (https://linux.die.net/man/3/execv) 来重启程序：

```python
class Bootstrap(Tool):
    def __init__(self):
        desc = 'Tool to restart the program using execv.'
        super().__init__(
            name='bootstrap',
            description=desc,
            parameters=[],  # No parameters needed for execv
            required=set()
        )
```

`run()` 方法比我们之前看过的工具更复杂。让我们来分解一下。首先，它定位程序 `main` 模块的文件名。如果找不到，就会引发异常：

```python
def run(self, **kwargs):
    main_module = sys.modules['__main__']
    module_path = getattr(main_module, '__file__', None)
    if not module_path:
        raise RuntimeError("Cannot determine __main__.__file__; are you in a REPL or notebook?")
```

如果 `main` 模块具有 `spec` 属性，那么它是作为 `python -m module` 运行的。让我们正确准备参数：

```python
spec = getattr(main_module, '__spec__', None)
if spec and spec.name:
    # Executed as a module: python -m package.module
    module_name = spec.name
    args = [sys.executable, '-m', module_name, *sys.argv[1:]]
```

否则，它是作为文件以 `python <main file>` 方式运行的。让我们为这种模式准备参数：

```python
else:
    # Executed as a script: python script.py
    args = [sys.executable, module_path, *sys.argv[1:]]
```

最后，让我们使用 `os.execv()` 和适当的命令行参数重启程序。

```python
os.execv(sys.executable, args)
```

`bootstrap` 工具是一个强大的工具，它允许 LLM 在对 AI-6 代码进行更改后重启控制 AI 系统。这个工具虽然简单，但功能非常强大，可以用来实现涉及自我修改代码的复杂工作流。但我们还可以更进一步，让 LLM 直接操作当前对话的记忆。

### 记忆工具

你可能还记得，AI-6 拥有一个强大的记忆系统，该系统围绕“会话”这一概念展开，会话是一个持久的消息集合，包含多个智能体循环。会话存储在文件中，我们可以让 LLM 和/或用户直接操作会话文件并将会话文件加载进来。这意味着 AI-6 允许 LLM 按照它认为合适的方式重写当前会话的记忆。这是一个强大的能力，使用时必须格外小心。通常情况下，LLM 不会尝试直接操作会话记忆，但用户可能会指示它列出以前的会话并加载以前的会话，以便从中断的地方继续。

重要的是要记住，会话结束后，会话文件仍会保留在磁盘上，使用 AI-6 的 AI 系统必须确保维护用户的权利和隐私，并为用户提供删除会话的选项。

让我们看看记忆工具是如何实现的。有四个不同的工具负责处理记忆：

- `list_sessions`：列出记忆目录中的所有会话
- `load_session`：按 ID 加载会话
- `get_session_id`：获取当前会话的 ID
- `delete_session`：按 ID 删除会话

请注意，没有工具可以保存当前会话。这是因为 AI-6 会定期自动保存会话（请参阅 *第四章* 中讨论的已配置的检查点间隔）。

设计选择使用多个记忆工具而不是具有多个操作的单一记忆工具是经过深思熟虑的。它显著简化了每个工具的实现和测试。

#### 列出会话

`ListSessions` 工具只需要导入 `json`，这是我们返回会话所使用的格式，当然还有 `Tool` 基类。不需要命令行参数：

```python
import json
from backend.object_model import Tool

class ListSessions(Tool):
    """Tool to list all available sessions in memory."""
```

构造函数使用对 `Engine` 实例的引用来初始化工具，稍后它将使用该引用来实际列出会话。这个工具没有参数，所以 `parameters` 列表为空，`required` 集合也为空：

```python
def __init__(self, engine=None):
    """Initialize the tool.

    Args:
        engine: Reference to the Engine instance
    """
    self.engine = engine
    super().__init__(
        name='list_sessions',
        description='List all available sessions in memory.',
        parameters=[],
        required=set()
    )
```

`run()` 方法是实际列出会话的地方。它检查 `engine` 引用是否已设置，如果未设置，则返回错误消息。如果 engine 已设置，它会调用 engine 的 `list_sessions()` 方法，该方法返回会话 ID 列表。如果未找到会话，则返回一条指示未找到会话的消息。否则，它返回一个格式化字符串，其中包含序列化为 JSON 的会话 ID 列表：

```python
def run(self, **kwargs):
    """List all available sessions.

    Returns:
        String with the details of all stored sessions (id, title and filename)
    """
    if not self.engine:
        return "Error: Engine reference not set."
    sessions = self.engine.list_sessions()
    if not sessions:
        return "No sessions found in memory."
    return "Available sessions:\n" + json.dumps(sessions)
```

将实际工作委托给 `Engine` 实例的模式在所有记忆工具中都很常见。毕竟，`Engine` 实例是 AI-6 记忆系统的事实来源。它负责管理会话、消息以及其他与记忆相关的任务。无需在工具中重复这些逻辑，这只会使代码更复杂、更难维护。



### 加载会话

当加载会话时，我们需要指定会话 ID。然后，工具将使用 `Engine` 实例将会话加载到其内存中。让我们看看这是如何实现的。

我们只需要从 AI-6 对象模型中导入 `Tool` 基类和 `Parameter` 类：

```python
from backend.object_model import Tool, Parameter
```

`LoadSession` 类派生自 `Tool` 基类。构造函数使用对 `Engine` 实例的引用来初始化工具。它只需要一个必需的参数 `session_id`，即要加载的会话的 ID：

```python
class LoadSession(Tool):
    """Tool to load a specific session by ID."""
    def __init__(self, engine=None):
        """Initialize the tool.

        Args:
            engine: Reference to the Engine instance
        """
        self.engine = engine
        super().__init__(
            name='load_session',
            description='Load a specific session by ID.',
            parameters=[
                Parameter(
                    name='session_id',
                    type='string',
                    description='ID of the session to load'
                )
            ],
            required={'session_id'}
        )
```

`run()` 方法是实际加载会话的地方。它检查是否设置了引擎引用以及是否提供了 `session_id` 参数。如果没有，则返回一条指示问题的错误消息。然后，它使用提供的会话 ID 调用引擎的 `load_session()` 方法。如果会话加载成功，它返回一条包含会话 ID 的成功消息。如果会话不存在或加载失败，则返回一条失败消息：

```python
def run(self, session_id, **kwargs) -> str:
    """Load a specific session.

    Args:
        session_id: ID of the session to load

    Returns:
        String indicating success or failure
    """
    if not self.engine:
        return "Error: Engine reference not set."
    if not session_id:
        return "Error: Session ID is required."
    success = self.engine.load_session(session_id)
    if success:
        return f"Successfully loaded session: {session_id}"
    else:
        return f"Failed to load session: {session_id}. Session may not exist."
```

获取当前会话 ID 通常很有用，这样以后可以加载它。下面是具体做法。

### 获取当前会话 ID

获取当前会话 ID 是一项简单的操作。工具只需返回引擎存储的当前会话 ID。

我们只需要从 AI-6 对象模型中导入 `Tool` 基类。此工具不需要任何参数：

```python
from backend.object_model import Tool
```

`GetSessionId` 类以熟悉的模式派生自 `Tool` 基类。构造函数期望一个引擎引用，该引擎将完成获取会话 ID 的实际工作。`parameters` 列表为空，`required` 集合也为空，因为获取当前会话 ID 不需要任何参数：

```python
class GetSessionId(Tool):
    """Tool to get the current session ID."""
    def __init__(self, engine=None):
        """Initialize the tool.

        Args:
            engine: Reference to the Engine instance
        """
        self.engine = engine
        super().__init__(
            name='get_session_id',
            description='Get the ID of the current session.',
            parameters=[],
            required=set()
        )
```

`run()` 方法是实际获取会话 ID 的地方。和往常一样，它先执行健全性检查，然后通过调用其 `engine.get_session_id()` 方法将工作委托给 `Engine` 实例：

```python
def run(self, **kwargs):
    """Get the current session ID.

    Returns:
        String with the current session ID
    """
    if not self.engine:
        return "Error: Engine reference not set."
    session_id = self.engine.get_session_id()
    return f"Current session ID: {session_id}"
```

有时清理旧会话以避免混乱是很有用的。下面是具体做法。

#### 删除会话

删除会话是一项强大的操作，应谨慎使用。该工具将删除持久化会话文件，因此在删除之前务必确保不再需要该会话。和往常一样，该工具将使用 `Engine` 实例来删除会话。此时，你应该已经熟悉导入、类声明和构造函数：

```python
from backend.object_model import Tool, Parameter

class DeleteSession(Tool):
    """Tool to delete a specific session by ID."""
    def __init__(self, engine=None):
        """Initialize the tool.

        Args:
            engine: Reference to the Engine instance
        """
        self.engine = engine
        super().__init__(
            name='delete_session',
            description='Delete a specific session by ID.',
            parameters=[
                Parameter(
                    name='session_id',
                    type='string',
                    description='ID of the session to delete'
                )
            ],
            required={'session_id'}
        )
```

`run()` 方法是实际删除会话的地方。与其他内存工具一样，它检查是否设置了 `engine` 引用以及是否提供了 `session_id` 参数。但它增加了一项额外检查，以确保我们不会尝试删除当前会话。这是一项安全措施，可防止意外删除活动会话，从而使引擎处于无效状态：

```python
def run(self, session_id, **kwargs):
    """Delete a specific session.

    Args:
        session_id: ID of the session to delete

    Returns:
        String indicating success or failure
    """
    if not self.engine:
        return "Error: Engine reference not set."
    if not session_id:
        return "Error: Session ID is required."
    # Don't allow deleting the current session
    if session_id == self.engine.get_session_id():
        return "Error: Cannot delete the current active session."
    success = self.engine.delete_session(session_id)
    if success:
        return f"Successfully deleted session: {session_id}"
    else:
        return f"Failed to delete session: {session_id}. Session may not exist."
```

我们在本节中涵盖了很多内容。我们考察了 AI-6 中多个自定义工具的实现，这些工具基于我们在*第 4 章*中介绍的工具系统设计。我们还看到了如何使用这些工具来修改 AI-6 代码、测试更改、重启程序以及操作会话内存。这是一组强大的工具，理论上可以让 AI-6 在良性循环中自我改进。

在下一节中，我们将看到如何将这些工具付诸实践，让 AI-6 从零开始实现一个新工具。

### 使用 AI-6 从零开始实现一个新工具

AI-6 内置了许多工具，理论上可以改进自己的代码。它可以读写自己的源代码、编写新代码和测试、运行测试以及重启程序。它还有一个我们没有详细研究的 Git 工具，但它允许 AI-6 执行任何 Git 操作，例如克隆仓库、创建分支、添加文件、提交更改以及将其推送到远程仓库。

但 AI-6 没有与 GitHub 交互的工具。GitHub 等集中式源代码控制系统是现代软件的核心。让我们试试 AI-6，看看它能否在我们的一些有用帮助下构建自己的工具。

#### 如何与 AI-6 交互

AI-6 开箱即用地支持多种接口：聊天机器人 CLI、基于 Slack 的接口和 Web UI 接口。

对于这个小项目，我们将通过其聊天机器人 CLI 使用 AI-6，这与我们在*第 3 章*中看到的 `k8s-ai` 接口非常相似。我们将在下一章详细介绍 Slack 和 Web 接口。

当我们使用聊天机器人 CLI 时，它就是使用 AI-6 的 AI 系统。下面是我们启动聊天机器人 CLI 的方式：

```text
❯ python -m frontend.cli.ai6
Using .env file from /Users/gigi/git/ai-six/py/frontend/cli/.env
```

接下来是聊天界面中的交互：

👤 [你]: 你好
🤖 [AI-6]: 你好！今天我能帮你做些什么？ ———-
👤 [你]:

完成所有这些准备工作后，我们就可以开始与 AI-6 交互，来构建一个全新的 GitHub 工具。

#### 使用 AI-6 构建新的 GitHub 工具

我们的目标是让 AI-6 根据我们想要实现工具的高级描述来完成所有工作。这将是一个高度互动的过程：我们提供指导，AI-6 执行工作，并且在每一步，我们都验证结果并提供反馈。我们想要实现的工具是一个 GitHub 工具，它将允许 AI-6 与 GitHub 仓库、问题、拉取请求等进行交互。让我们从描述我们想要实现的工具开始：

👤 [你]: 我想创建一个用于与 GitHub 交互的命令行工具，使用 `gh` CLI（已安装并配置好），类似于 `/Users/gigi/git/ai-six/py/backend/tools` 中的工具。你建议怎么做？

🤖 [AI-6]: 要使用 GitHub CLI（gh）创建一个用于与 GitHub 交互的命令行工具，你可以按照以下步骤来设置一个类似于 Ollama 工具等现有工具的结构。

你可以这样实现：


- **创建一个基础命令工具类（如果尚不存在）：** 这个类（如 `CommandTool`）将处理通用的命令行执行，包括设置基本配置（如用户数据）。
- **创建 GitHub CLI 工具类：** 使用这个类定义 GitHub 特定的命令行交互。

好的，这是一个好的开始，但不需要创建基础命令工具类，因为我们已经有了。让我们把这一点告诉 AI-6。

👤 [用户]：既然 Ollama 工具存在，那么 `CommandTool` 基础类可能已经存在了。

🤖 [AI-6]：你说得对。如果 `Ollama` 工具使用了 `CommandTool` 基础类，那么它很可能已经存在于你的项目结构中。在这种情况下，你可以直接利用这个现有的基础类来实现 GitHub CLI 工具。具体步骤如下：

#### 创建 GitHub CLI 工具的分步指南

1. **定位现有的 `CommandTool` 基础类：**
   - 首先，确认项目中 `CommandTool` 类的位置和可用方法。
2. **创建 GitHub CLI 工具类：**
   有了基础类，你可以创建一个扩展 `CommandTool` 的新工具类来进行 GitHub 交互。

```python
# gh_cli_tool.py
# Import the CommandTool class from its existing location
from backend.tools.base.command_tool import CommandTool

class GhCLI(CommandTool):
    def __init__(self, user: str | None = None):
        super().__init__(
            command_name='gh', user=user,
            doc_link='https://cli.github.com/manual/')

    def list_repositories(self) -> str:
        return self.run_command('repo list')

    def create_repository(self, name: str) -> str:
        return self.run_command(f'repo create {name}')

    def clone_repository(self, repo: str) -> str:
        return self.run_command(f'repo clone {repo}')

# Add more methods to interact with other GitHub functionalities using the 'gh' CLI
```

嗯，看起来 AI-6 没有花时间去研究 `CommandTool` 基础类，该类已经提供了一个 `run()` 方法，可以用任意参数执行命令行程序。它建议添加 `list_repositories()`、`create_repository(name: str)` 和 `clone_repository(repo: str)` 等方法。这些方法不是必需的，因为我们可以直接使用 `CommandTool` 基础类的 `run()` 方法来运行任何命令行程序，并传入任意参数。

让我们把这一点告诉 AI-6：

👤 [用户]：等等，我们一步一步来。找到并给我看看 `CommandTool` 基础类的代码。

🤖 [AI-6]：这是 `CommandTool` 基础类的代码：

```python
import sh
import shlex
from backend.object_model import Tool, Parameter

class CommandTool(Tool):
    def __init__(
        self, command_name: str, user: str | None = None,
        doc_link: str = ""
    ):
        self.command_name = command_name
        self.user = user
        description = f'{command_name} tool. ' + f'See {doc_link}' if doc_link else ''
        parameters = [Parameter(
            name='args', type='string',
            description=f'command-line arguments for {command_name}')]
        required = {'args'}
        super().__init__(
            name=command_name,
            description=description,
            parameters=parameters,
            required=required
        )

    def run(self, **kwargs):
        args = shlex.split(kwargs['args'])
        if self.user is not None:
            return sh.sudo('-u', self.user, self.command_name, *args)
        else:
            return getattr(sh, self.command_name)(*args)
```

#### 解释：

- **类继承：** `CommandTool` 继承自一个名为 `Tool` 的基础类。
- **初始化：** 构造函数使用命令名、可选用户和文档链接来初始化命令工具。
- **参数：** 它为工具设置了必需的参数，包括命令行参数。
- **命令执行：** `run` 方法使用 `sh` 库执行 shell 命令，可以选择为指定用户使用 `sudo`。

很好，现在我们离目标更近了。让我们继续小心地引导 AI-6 更好地理解它自己的工具架构。

👤 [用户]：好的。既然你现在了解了 CommandTool 基础类的工作原理，那么检查一下它的基类 Tool。

🤖 [AI-6]：`CommandTool` 继承自的 `Tool` 类是一个抽象基类（ABC）。以下是 `Tool` 类的详细说明：

- **属性：**
  - `name`：一个字符串，表示工具的名称。
  - `description`：一个字符串，提供对工具的描述。
  - `parameters`：一个 `Parameter` 对象列表，每个对象代表工具的一个命令行或功能参数。
  - `required`：一个字符串集合，指示哪些参数是必需的。

- **方法：**
  - `configure`：该方法允许使用提供的字典对工具进行可选配置。如有需要，可以在子类中重写。
  - `run`：这是一个抽象方法，必须由任何子类实现。它定义了如何使用给定的参数执行工具，并将结果作为字符串返回。

#### 下一步

有了对 `CommandTool` 基础类及其父类 `Tool` 的理解，你可以自信地通过扩展 `CommandTool` 来为 GitHub CLI 创建一个新工具。如果你准备好了，我们可以继续创建 `GhCLI` 工具类，它利用 GitHub CLI（`gh`）进行 GitHub 交互。如果你希望继续这一步，请告诉我！

太棒了。但我不喜欢 `GhCLI` 这个类名。它暴露了实现细节，并且不符合其他工具的命名约定：

👤 [用户]：我希望 GitHub 工具类就叫 Github。它使用 gh cli 这一事实是实现细节。需要创建哪些文件或目录？

🤖 [AI-6]：要创建一个名为 `Github` 的新命令行工具类，你应该按照以下步骤将其集成到你现有的项目结构中：

#### 第 1 步：确定文件位置

由于现有工具（如 `Ollama`）位于项目的特定目录中，为了保持一致性和组织性，你应该将新工具放在类似的位置。

假设你遵循以下结构（类似于 `Ollama`）：

```
backend/
    tools/
        base/
        ollama/  # Contains the Ollama tool definition
        github/  # This is where we will place the new Github tool
```

#### 第 2 步：创建必要的目录和文件

1. **为 Github 工具创建一个目录：**
   - 路径：`backend/tools/github/`
   - 该目录将包含 `Github` 工具的实现。
2. **创建 `Github` 工具类文件：**
   - 路径：`backend/tools/github/github.py`
   - 该文件将包含 `Github` 类的定义。

#### 第 3 步：实现 `Github` 工具类

...

这正是我们想听到的。AI-6 走在正确的轨道上。让我们继续：

👤 [用户]：好的。展示一下 Github 类。

🤖 [AI-6]：这是名为 `Github` 的新命令行工具类

```python
# backend/tools/github/github.py
from backend.tools.base.command_tool import CommandTool

class Github(CommandTool):
    def __init__(self, user: str | None = None):
        # Initialize with the command name 'gh' and provide a documentation link
        super().__init__(
            command_name='gh', user=user,
            doc_link='https://cli.github.com/manual/')

    def execute(self, args: str) -> str:
        """
        Execute any GitHub CLI command by providing the appropriate arguments.

        Args:
            args: Command-line arguments to pass to the `gh` CLI.

        Returns:
        """
```




```
The output from the command execution.
"""
return self.run(args=args)
```

非常好！甚至文档链接也是正确的。但它添加了一个无用的 `execute()` 方法，该方法只是调用 `run()` 方法。我本可以自己删除它，但为了保持这次会话完全交互式，我请 AI-6 来删除它：

👤 \[你\]：难道 run() 方法不足以执行任何 gh CLI 命令吗？

🤖 \[AI-6\]：你观察得完全正确。如果你想直接访问 `CommandTool` 的 `run()` 方法，你并不严格需要在 `Github` 类中额外添加一个 `execute()` 包装器。`execute()` 方法的主要作用是为执行 GitHub CLI 命令这一行为提供语义化的表示或别名，但它的确没有在 `run()` 之外增加任何额外功能。

因此，在实践中，你可以直接从 `Github` 类的实例调用 `run()` 方法来执行命令，因为它本来就是为使用指定参数运行 CLI 而设计的。以下是不使用 `execute()` 方法的修订版本：

#### 不使用 `execute()` 的实现

```python
# backend/tools/github/github.py

from backend.tools.base.command_tool import CommandTool

class Github(CommandTool):
    def __init__(self, user: str | None = None):
        # Initialize with the command name 'gh' and provide a documentation link
        super().__init__(
            command_name='gh', user=user,
            doc_link='https://cli.github.com/manual/')
```

#### 使用示例

```python
if __name__ == '__main__':
    gh_tool = Github()
    # List repositories
    print(gh_tool.run(args='repo list'))
    # Create a new repository
    # print(gh_tool.run(args='repo create new-repo'))
    # Clone a repository
    # print(gh_tool.run(args='repo clone owner/repo'))
```

#### 结论

通过直接使用 `run()` 方法，你保持了简洁高效的实现，并依赖于 `CommandTool` 基类中已经定义的能力。这使 `Github` 类保持简单，同时充分利用了执行 `gh` CLI 所支持的任何命令的灵活性。

在稍加引导之后，AI-6 得出了正确的解决方案。我继续这次会话，让它编写测试并执行这些测试，它也成功地做到了。测试是用 `unittest` 框架编写的，并使用我们之前见过的 `python_test_runner` 工具执行的。

测试代码如下：https://github.com/PacktPublishing/Design-Multi-Agent-AI-Systems-Using-MCP-and-A2A/blob/main/ch05/ai-six/py/backend/tests/tools/github/test_github.py。

这次会话很好地展示了 AI-6 如何通过学习理解自身的代码和架构来为自己添加新能力，同时也说明了人在回路（human-in-the-loop）以及在整个过程中引导 AI 系统的重要性。虽然 GitHub 工具的早期建议并非明显错误，但它们与现有的工具架构并不十分契合。

让我们看看如何在不借助 AI-6 的情况下手动实现一个新工具。

### 手动实现一个新工具

下一个工具会稍微复杂一些。我们将构建一个 Claude 工具。该工具将允许 AI-6 向 Anthropic Claude LLM（`https://claude.ai/`）发送 LLM 推理请求。

当 AI-6 与 OpenAI 等 LLM 提供商协作时，它也可以利用另一个不同的 LLM 提供商，例如 Anthropic Claude。Claude 工具实现在 `backend/tools/claude_tool.py` 中。它是一个自定义工具，扩展了 `Tool` 基类，并使用 Anthropic Python SDK（`https://github.com/anthropics/anthropic-sdk-python`）向 Claude LLM 发送请求。让我们看看它是如何实现的。

该工具导入了 `anthropic` 模块（即 Anthropic Claude LLM 的 Python SDK），并从 AI-6 对象模型中导入了 `Tool` 和 `Parameter`：

```python
import anthropic
from backend.object_model import Tool, Parameter
```

`ClaudeTool` 类派生自 `Tool` 基类。它的构造函数比我们之前看到的工具稍微复杂一些。它使用描述、参数和一组必填参数来初始化工具。共有四个参数：

- `prompt`：要发送给 Claude 的提示或问题。
- `model`：用于推理的模型。默认值为 `claude-sonnet-4-20250514`。
- `max_tokens`：要生成的最大 token 数量。默认值为 `1000`。
- `temperature`：采样温度（`0.0`–`1.0`）。默认值为 `0.7`。

`max_tokens` 和 `temperature` 的默认值取自 Anthropic 文档（`https://docs.anthropic.com/en/docs/about-claude/models/overview`）。当 LLM 调用该工具时，它可以通过在请求中提供不同的参数来覆盖这些默认值。只有 `prompt` 参数是必填的。

请注意，我们有一个初始化为 `None` 的 `self.client` 属性。稍后将用它来存储 Anthropic 客户端：

```python
class Claude(Tool):
    def __init__(self):
        desc = """
Send inference requests to Anthropic Claude LLM. Useful for getting a
second opinion or
different perspective from another AI model.
"""
        super().__init__(
            name='claude',
            description=desc,
            parameters=[
                Parameter(
                    name='prompt',
                    type='string',
                    description='The prompt or question to send to Claude'),
                Parameter(
                    name='model',
                    type='string',
                    description="""
See https://docs.anthropic.com/en/docs/about-claude/models/overview.
Defaults to claude-sonnet-4-20250514
"""),
                Parameter(
                    name='max_tokens',
                    type='integer',
                    description='Maximum number of tokens to generate. Defaults to 1000'),
                Parameter(
                    name='temperature',
                    type='number',
                    description='Temperature for sampling (0.0-1.0). Defaults to 0.7')
            ],
            required={'prompt'}
        )
        self.client = None
```

终于，我们有了一个需要配置的工具。`configure()` 方法用于从配置字典中获取 Anthropic API 密钥，并在存在密钥时初始化 Anthropic 客户端：

```python
def configure(self, config: dict) -> None:
    """Configure the Claude tool with API key."""
    if 'api_key' in config:
        self.client = anthropic.Anthropic(api_key=config['api_key'])
```

`run()` 方法是实际将推理请求发送到 Claude LLM 的地方。它会检查客户端是否已初始化，如果没有，则返回错误消息。然后，它从 `kwargs` 字典中获取参数。这些参数包括 `prompt`、`model`、`max_tokens` 和 `temperature`。它们将由 AI-6 当前正在使用的 LLM（例如 OpenAI GPT-4o）在调用该工具时提供。你可以看到，只有 `prompt` 参数是必填的。其他参数都有默认值，因此是可选的。获取参数后，它会调用客户端的 `messages.create()` 方法，将请求发送到所选的 Claude 模型。然后，响应会以字符串形式返回：

```python
def run(self, **kwargs) -> str:
    if self.client is None:
        return "Error: Claude API key not configured. Set 'api_key' in tool_config for claude tool"
    prompt = kwargs['prompt']
    model = kwargs.get('model', 'claude-sonnet-4-20250514')
    max_tokens = kwargs.get('max_tokens', 1000)
    temperature = kwargs.get('temperature', 0.7)
    response = self.client.messages.create(
        model=model,
        max_tokens=max_tokens,
        temperature=temperature,
        messages=[
            {"role": "user", "content": prompt}
        ]
    )
    return response.content[0].text
```

我们可以看到，AI-6 简单的工具接口使我们能够实现各种各样的工具，包括命令行工具（如文件系统工具）、AI-6 内部工具（如记忆工具），以及调用外部 API 的工具（如 Claude 工具）。该工具接口足够灵活，可以容纳不同类型的工具及其特定需求。





#### 摘要

让我们回顾一下。我们对 AI-6 工具系统进行了全面的探索，将重点从高层编排引擎转移到工具自身的内部结构和行为上。我们考察了 AI-6 如何通过定义统一的通用工具规范来实现与提供商无关的工具互操作性，该规范能够无缝映射到特定于提供商的格式，例如 OpenAI 的函数调用或 Ollama 的工具架构。这些工具被设计为可复用、一致且易于在不同 LLM API 之间转换，其核心是强大的 schema 和输入验证机制。

我们继续详细介绍了内置命令行工具，例如 `pwd`、`ls`、`awk`，以及用于文件写入的特殊用途 `echo` 工具。这些工具使用 `CommandTool` 基类实现，能够直接在 AI 框架内执行文件操作和 shell 交互。其他工具，例如用于执行 Python 单元测试的 `test_runner` 和用于重启 AI-6 程序的 `bootstrap`，展示了系统如何支持迭代式自我修改。诸如 `list_sessions`、`load_session` 和 `delete_session` 之类的记忆工具扩展了智能体以受控方式管理和操作持久会话状态的能力。

然后，我们探讨了如何使用 AI-6 本身以交互方式开发新工具，并通过创建一个包装 `gh` CLI 的 GitHub 工具进行了演示。这一练习展示了如何将人工指导与智能体推理相结合，通过将 AI-6 指向其自身源代码来扩展其功能。最后，我们探讨了构建一个基于 API 的 Claude 工具来与 Anthropic 的 LLM 交互，强调了 AI-6 如何同时集成和利用多个 LLM 提供商。总的来说，这些示例凸显了 AI-6 工具系统的强大功能和灵活性。

在下一章中，我们将转向 AI-6 项目的前端部分，介绍通过 Slack 和 Chainlit（Web UI）实现的面向用户的界面，这些界面支持对系统进行交互式和对话式控制。

#### 获取本书 PDF 版本和独家附加内容

扫描二维码（或访问 `packtpub.com/unlock`）。按书名搜索本书，确认版本，然后按照页面上的步骤操作。

*注意：请备好发票。直接从 Packt 购买无需发票。*

![](img/840072a5db7cd7dead6a4daec17c5a79_195_0.png)

![](img/840072a5db7cd7dead6a4daec17c5a79_195_1.png)
