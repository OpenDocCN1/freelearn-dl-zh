# 18 介绍机器人流程自动化

正如你目前所见，Power Automate 在处理桌面应用程序方面是一个极其强大的平台。虽然 Power Automate 最初的重点是集成云服务，但微软意识到了一个差距并更新了平台以支持机器人流程自动化 (RPA)。RPA 流最初名为用户界面 (UI) 流，旨在捕获并重放人工干预执行的操作，例如键盘和鼠标点击。RPA 流帮助解决某些不存在脚本或 API 接口的挑战。常见示例包括：

- 通过复制和粘贴将数据复制到多个系统
- 读取手写订单单并将其转换为应用程序

你可以将 RPA 视为一种允许你自动化桌面应用程序的宏。Power Automate 支持两种类型的自动化：使用桌面应用程序创建的流和使用 Selenium IDE 的流。Selenium IDE 是一个可以用于记录浏览器会话的开源工具。

我们将直接处理桌面应用程序，因此我们将使用 Power Automate 桌面版（它也具有浏览器功能）。此外，Power Automate 支持两种 RPA 场景：交互式和无人值守。交互式 RPA 场景包括人类交互，用于启动流或在执行期间提供输入。无人值守流是无需用户干预即可运行的全自动化流程。

Power Automate Desktop 现在包含“使用 Copilot 录制”功能，允许你将录制过程与语音旁白相结合，帮助 Copilot 确定如何创建和更新操作。使用 Copilot 录制仅适用于美国地区的租户，且仅支持英语旁白。访问 `https://learn.microsoft.com/en-us/power-automate/desktop-flows/create-flow-using-ai-recorder`。

在本章中，我们将探索如何与 Copilot 和机器人流程自动化工作。我们在第 2 章“开始使用 Power Automate”中仅仅触及了皮毛。在这里，在深入探讨示例之前，我们将进一步地探索 RPA 的力量。

### 技术要求

除了 Power Automate 许可证外，本章将使用以下软件和应用程序：

* Power Automate Desktop
* Power Automate WebDriver
* Microsoft 数据网关

在本章中，我们将使用 Windows 11 自带的 Power Automate Desktop 版本。Power Automate Desktop 也可以为 Windows 10 提供免费下载，我们将在“配置先决条件”部分介绍这一点。

微软的数据网关与 Power Automate Desktop 一样，可以在 Power Automate 网页门户内部免费获取。我们稍后在“配置先决条件”部分也会介绍这一点。

提醒一下，你需要 Power Automate 用户计划（per-user plan）以及无人值守 RPA 附加组件。

### 学习操作和传递变量

在本章中，我们将介绍一些与使用 Power Automate Desktop 相关的新概念，特别是关于运行 Power Automate Desktop 流的操作，以及如何将变量从云端传递到桌面应用程序。

### 运行使用 Power Automate for desktop 操作构建的流

Power Automate Desktop 无法读取或使用云触发器。但是，通过数据网关建立的连接，Power Automate Desktop 可以接收执行其所负责的流所需的指令。

### 传递变量

变量是一种用于存储临时信息的编程结构。程序员将变量用作应用程序、脚本或进程运行时发生变化（或不同）的内容占位符。通过使用变量，应用程序可以在执行期间用实际值替换变量。

你已经在书中到处看到变量了，尽管 Power Automate 用户界面通常将它们称为**动态内容**（dynamic content）。当你向 Power Automate 流中添加操作时，动态内容标记或变量将根据上一个步骤的输出自动构建。

然而，Power Automate Desktop 无法自动从云流中导入这些变量。为了弥补这一差距，Power Automate 允许你定义变量以便在环境之间传输这些内容。

Power Automate 有两种类型的变量：输入变量和输出变量。输入变量是你根据 Power Automate 服务暴露出的信息创建的新变量。输出变量是 Power Automate Desktop 可以传回 Power Automate 服务的生成数据。当我们开始配置流时，你将学习如何创建新变量以便在环境之间传递。

### 与文件系统交互

使用 Power Automate Desktop 的另一个优势是能够与本地文件系统进行交互。例如，你可能有一个旧的业务流程，它依赖于主机向你数据中心内的另一个服务器发送文件。Power Automate Desktop 可以获取该文件并开始对其处理。由于 Power Automate Desktop 应用程序运行在网络内部的工作站上，它可以访问如映射驱动器或通用命名规范（UNC）路径等内容。

### 运行脚本

Power Automate Desktop 还可以启动各种脚本，包括 DOS、VBS、Java、PowerShell 等。

图 18.1 - Power Automate Desktop 脚本语言

# 18 

一旦激活了脚本操作，你就可以输入自己的代码，甚至可以使用 Copilot 进行引导指引，如图 18.2 所示：



# 18 使用 Copilot 生成脚本

图 18.2 – 使用 Copilot 生成脚本

显然，你将对 Copilot（或任何其他 AI 助手）生成的任何脚本进行测试，但它确实可以节省时间！

### Power Automate 录制器

`Power Automate` 录制器是一个强大的工具，允许你捕捉作为用户执行的步骤，它是了解 `Power Automate Desktop` 环境某些功能的绝佳方法。

如果你以前在 `Microsoft Excel` 中录制过宏，你就会对 `Power Automate` 录制过程的工作原理有一个大致了解。`Power Automate` 会记录诸如启动应用程序、选择对话框或界面、输入文本等行为。录制过程完成后，你将得到一个操作列表，你可以编辑这些操作以完善你的最终输出。

现在你已经了解了我们将使用的内容，让我们进入配置先决条件的阶段。

### 配置先决条件

如前所述，此流将需要一些软件先决条件——即 `Power Automate Desktop` 应用程序、支持的浏览器组件以及数据网关应用程序。

虽然可以在主计算机上完成这些步骤，但我们建议使用第二台计算机（或虚拟机）来配置先决条件并运行流。`Power Automate Desktop` 会自动化鼠标和键盘输入，如果你尝试从执行操作的同一台计算机上触发流，可能会遇到困难和意外错误。

如果你没有第二台计算机，可以启动一个免费的 `Azure` 试用订阅（`https://portal.azure.com`）并配置虚拟机。或者，你可以从你有权的另一个设备（如手机或平板电脑）触发流。

让我们开始吧！

### 配置 Power Automate Desktop

`Windows 11` 已经包含了 `Power Automate Desktop`，这就是我们在练习中将使用的版本。不过，你可以从 `https://go.microsoft.com/fwlink/?linkid=2137322` 或 `Power Automate` 制作网页门户免费下载并在 `Windows 10` 上安装 `Power Automate Desktop`，如图 18.3 所示。

图 18.3 - 下载 Power Automate Desktop

在配置时，可能会出现 `Power Automate` 网页门户无法识别 `Power Automate Desktop` 的情况；安装原生 `Windows` 版网关。

### 配置微软数据网关

网关的功能允许内部网络设备与 `Power Platform` 服务（如 `Power Apps` 和 `Power Automate`）通信。

### 机器人流程自动化

如果你在现有的 `Microsoft 365` 环境中工作，可能需要权限才能配置网关。如果你使用的是自己的设备，可以按照以下步骤操作：

- 在要安装的设备上，导航到 `Power Automate` 网页门户（`https://make.powerautomate.com`）。
- 滚动到页面底部，选择下载下的“网关”。如果链接未出现，也可以访问 `https://go.microsoft.com/fwlink/?2116849`。
- 同意使用条款，然后点击“安装”。

图 18.4 - 下载本地网关

- 软件安装后，输入你的 `Microsoft 365` 凭据以开始向租户注册网关。点击“登录”开始过程。
- 完成现代身份验证对话框。
- 选择“在此计算机上注册新网关”选项并点击“下一步”。

图 18.5 - 安装网关

- 为“新的本地数据网关名称”输入值。你可以使用任何喜欢的名称，不必是计算机名称。这是你在 `Power Automate` 网页门户中配置访问本地服务的流时显示的名称。
- 为“恢复密钥”输入值。当你需要恢复或重新配置网关时，需要该恢复密钥。在“确认恢复密钥”框中输入相同的值并点击“配置”。

图 18.6 – 配置网关设置

图 18.7 – 注册新网关

图 18.8 – 配置网关设置

9. 确认设置。

图 18.9 – 确认数据网关设置

10. 安装完成后，点击关闭。

数据网关现在已配置完成，并准备好路由到任何本地应用程序或数据库。

接下来，我们将为基于浏览器的自动化准备好组件。

### 配置浏览器扩展

最后一步是确保安装了 `Selenium` 浏览器扩展。此软件可用于多种自动化平台，帮助进行基于浏览器的测试和操作。按照以下步骤操作：

1. 打开您的浏览器（`Chrome` 或 `Edge`）。
2. 选择您想要使用的浏览器配置。
3. 点击浏览器扩展图标进入浏览器的扩展商店。

图 18.10 - 启动浏览器扩展商店

4. 搜索 `Selenium`。

图 18.11 – 搜索 Selenium 扩展

5. 选择一个可用的扩展。
6. 根据您的浏览器，点击“获取”或“添加至 Chrome”。

图 18.12 – 安装扩展

7. 点击**添加扩展程序**以确认安装。

您可能需要为每个想要自动化的浏览器重复安装扩展程序（尽管在接下来的章节中，我们将重点关注 `Microsoft Edge`）。

### 总结

RPA 有助于简化以前不适用于自动化的旧应用程序的操作。这对于依赖于不具备网络感知能力或使用无法在互联网上路由的旧通信协议的老应用程序的企业来说，非常有益。

在下一章中，我们将开始使用 RPA 和 `Power Automate Desktop` 应用来处理本地数据库应用程序。

---

