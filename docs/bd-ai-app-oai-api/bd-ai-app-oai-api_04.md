# 第五章：4

# 将代码错误修复应用程序与支付服务集成

在本章中，我们将探讨如何使用 **Stripe** API 将支付服务集成到您的 ChatGPT 代码错误修复应用程序中，以便您能够跟踪用户在您的 ChatGPT 应用程序上的访问次数，并实现高级功能或内容的支付机制。 您还将学习如何在您的 ChatGPT 项目中使用基本数据库，这对于跟踪用户数据和交易至关重要。

首先，我们将介绍 Stripe 并向您展示如何设置账户和 API 密钥。 然后，我们将介绍创建用于存储用户数据和交易历史的 SQL 用户数据库的步骤。 最后，我们将指导您使用 Stripe API 将支付功能添加到您的 ChatGPT 应用程序中，包括创建结账流程、处理支付 webhook 以及更新 数据库。

在本章中，我们将学习如何做 以下内容：

+   设置 SQL 用户数据库

+   向 ChatGPT 应用程序添加支付 功能

在本章结束时，您将具备添加支付服务到您的 ChatGPT 应用程序并构建成功的 在线业务所需的必要知识和工具。

# 技术要求

本项目的技术要求如下： 如下：

+   在您的 机器上安装了 Python 3.7 或更高版本

+   代码编辑器，例如 VSCode（推荐）

+   Python 虚拟环境

+   一个 OpenAI API 密钥

+   一个 Stripe 账户和 API 密钥

在下一节中，您将学习如何集成 Stripe 支付。 您将了解如何创建 Stripe 账户，获取必要的 API 密钥，并在 您的应用程序中设置 Stripe API。

本章中的代码示例可以在 GitHub 上找到 在 [https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-APIs](https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-APIs)。

# 介绍和集成 Stripe 支付

**Stripe** 是一个 流行的支付网关，提供了一种简单的方式来在线接受支付。 它允许企业和个人在网上接受支付、管理订阅和跟踪收入，一切都在一个地方完成。 Stripe 提供了一个易于使用的 API，允许 开发人员将支付功能集成到他们的网站和应用程序中。 我们将从创建一个 Stripe 账户 并配置我们将用于验证对 Stripe API 请求的 Stripe API 密钥开始。 在这里，您将使用 Stripe API 创建一个支付表单，并在您的 Python 代码中安全地处理支付。 与 Stripe 类似的服务还包括 PayPal、Square 和 Braintree。

接受支付的第一步是在您的 **《代码错误修复器》** 应用程序中创建一个 Stripe 账户。 Stripe 是一个流行的支付处理平台，它使企业能够在线接受支付。 通过设置 Stripe 账户，您可以将其与 ChatGPT 应用程序集成，并开始接受用户的支付。 设置 Stripe 账户既简单又直接，我们将引导您完成所有步骤。

要开始使用 Stripe，您需要创建一个账户。 按照以下步骤创建一个新的 Stripe 账户：

1.  访问 Stripe 网站： [https://stripe.com/](https://stripe.com/)。

1.  点击页面右上角的 **登录** 按钮。

1.  点击 **创建** **账户** 按钮。

1.  填写注册表单，提供您的个人或业务信息。 您需要提供您的姓名、电子邮件地址和密码（见 *图 4**.1*）。

1.  通过点击 Stripe 发送给您的验证电子邮件中的链接来确认您的电子邮件地址。

1.  提供您的 业务详细信息，例如您的 公司名称、地址和税务识别号 （可选）。

1.  设置您的支付设置。 您可以选择接受所有主要信用卡和借记卡的支付，以及像 Apple Pay 和 Google Pay 这样的数字钱包支付（可选）。

![图 4.1 – Stripe 注册表单](img/B22414_04_1.jpg)

图 4.1 – Stripe 注册表单

一旦你创建了 Stripe 账户，你就可以开始接受客户的支付。 你还可以使用 Stripe 的仪表板，如图 *图 4**.2**所示，来管理你的交易，处理退款，并查看你的支付活动报告。 Stripe 对其平台处理的每笔交易都会收取费用，但费用具有竞争力 且透明。

![图 4.2 – Stripe 仪表板](img/B22414_04_2.jpg)

图 4.2 – Stripe 仪表板

接下来，你需要 获取访问你的 Stripe API 密钥，这些密钥将在你的应用程序中用于后续的支付处理。 从 仪表板中，点击屏幕左侧的 **开发者** 标签页，然后点击 **API 密钥**。在这个页面上，你可以看到你的实时和测试 API 密钥，以及显示每个密钥秘密的选项。 请确保复制并安全存储你的密钥，因为你将需要它们从你的 ChatGPT 应用程序向 Stripe 发送 API 请求。

重要提示

你的实时 API 密钥应保持私密，不要公开分享，因为它们提供了访问你的 Stripe 账户敏感数据的权限，并允许用户进行更改。 相反，你可以使用你的 **测试 API 密钥** 进行开发和 测试目的。

Stripe 提供了三种类型的 API 密钥 – **发布密钥**，**秘密密钥**，和**测试密钥**：

+   **发布密钥** 用于客户端，以安全地向 Stripe 发送请求。 它没有任何访问你账户的权限，可以公开分享。 这个密钥通常放置在 应用程序前端。

+   **秘密密钥** 用于服务器端，以向 Stripe 发送请求，并且 具有完全访问你账户的权限，包括进行收费的能力。 它应该保密，并且永远不应该公开分享。 这个密钥通常放置在 应用程序后端。

+   **测试密钥** 用于测试环境，以执行测试交易而不收取任何费用。 它像秘密密钥一样工作，但只影响测试数据，并且不应该在生产中使用。

在**API keys** 页面，您将看到**publishable** 和**test** API keys。 我们将使用**publishable** key 来识别您的应用程序前端中的 Stripe 账户，而**test** key 用于与 Stripe 的后端通信。 您需要将**publishable** key 添加到您的 ChatGPT 应用程序的前端代码中，并将**test** key 添加到后端代码中。

您可以使用**Stripe**将支付集成到网站或应用程序中，Stripe 是一个流行的支付网关，它提供了一种轻松接受在线支付的方式。This is how you can integrate payments into a website or application using **Stripe**, a popular payment gateway that provides an effortless way to accept payments online. 在本节中，为了使用 Stripe，您创建了一个 Stripe 账户并获得了 API 密钥，包括可发布密钥和秘密密钥，它们具有不同的访问级别，应该**have different access levels and should be kept secure.

在下一节中，您将学习如何为 Code Bug Fixer 应用程序设置 SQL 用户数据库。In the next section, you will learn about setting up a SQL user database for the Code Bug Fixer application. We will explore the different payment models and understand how to design a subscription-based payment plan.

# **设置 SQL 用户数据库**

在将支付基础设施集成到您的应用程序之前，确定您的业务策略至关重要，这关系到您将如何向您的 Web 应用程序用户收费。Prior to implementing the payment infrastructure into your application, it’s essential to determine your business strategy, which pertains to how you’ll charge users of your web application. There are several ways to accomplish this:

+   **订阅计划**：这是一种定期付款模式，允许用户定期支付服务或产品费用，例如按月或按年支付。This is a recurring payment model that allows users to pay for a service or product on a regular basis, such as monthly or yearly. 它是**软件即服务** (**SaaS****)产品以及在线出版物中常见的支付选项，用户支付费用以获取服务或内容的访问权限。It is a common payment option for **Software-as-a-Service** (**SaaS****) products and online publications, where users pay for access to the service or content.

+   **一次性付款计划**：客户为产品或服务支付一次费用。make a single payment for a product or service.

+   **基于使用量的计划**：客户根据他们对产品或服务的使用情况付费，例如按次付费或按点击付费。are charged based on their usage of a product or service, such as pay-per-view or pay-per-click.

对于我们的代码错误修复应用程序，我们将设计一个订阅模式，为用户提供三个不同的计划进行选择。 此外，为了使用户入门过程更加顺畅，我们将包括一个 **免费试用期** ，允许用户在承诺支付计划之前尝试应用程序一定次数。

免费试用期 将为用户提供探索应用程序功能并确定其是否符合需求的机会。 在试用期间，用户可以在不收费的情况下使用应用程序特定次数。 试用期结束后，用户将被提示选择一个支付计划以继续使用应用程序。 这种基于订阅的方法对用户和作为应用程序所有者的您都有益。 用户可以选择最适合其预算和使用的订阅计划，而您可以从 订阅费中产生稳定的收入流。

为了实现我们应用程序的支付机制，我们需要建立一个数据库来跟踪用户访问和使用情况。 数据库需要记录两种类型的信息—— **浏览器 ID** (见 *表 4.1*)，这是每个访问应用程序的用户独有的，以及使用计数器，它记录每个用户访问应用程序的次数。 通过跟踪这些信息，我们可以识别唯一用户并确保他们在被提示选择一个 订阅计划之前没有超过允许的免费访问次数。

| **浏览器 ID** | **使用计数器** |
| --- | --- |
| 28ec523f092 | 3 |
| 58c9f5702fd | 6 |
| c59523926d | 10 |

表 4.1 – 数据库中收集的用户数据

现在我们已经制定了一个稳固的商业计划，我们可以继续前进并深入了解将支付服务集成到我们的 ChatGPT 应用程序中的技术细节。 然而，在我们能够这样做之前，我们需要初始化一个 SQL 数据库。

## 初始化 SQL 数据库

SQL 数据库对于跟踪用户在我们的应用程序上执行访问次数至关重要，这反过来又使我们能够使用 Stripe API 实现支付机制。 拥有数据库，我们可以轻松地存储和检索与我们的用户 `及其与我们的应用程序的交互相关的数据，使我们能够无缝地管理支付 `过程。`

现在，让我们打开我们的 Code Bug Fixer 应用程序的 `app.py` 文件，并导入所有必要的库以访问 Stripe 和 `SQL 数据库：`

app.py

```py
 from flask import Flask, request, render_template
from openai import OpenAI
import config import hashlib
import sqlite3
app.py file:

				*   `hashlib`: Provides interfaces to secure hash algorithms. It is used to generate hash values of data. In our case, we will use `hashlib` to hash user information before storing it in the database for security purposes.
				*   `sqlite3`: This library provides a lightweight disk-based database that doesn’t require a separate server process and allows us to access the database using SQL commands. We will use it to create and manage a database to store user information.
				*   `stripe`: This is a third-party library that provides a Python client for the Stripe API, which allows us to handle payments in our application. We will use it to process payments made by users through our application.

			While the `sqlite3` and `hashlib` are built-in libraries in Python, you will need to install `stripe`. You can simply do that by opening a new VSCode terminal by going to **Terminal** | **New Terminal** and typing the following:

```

$ pip install stripe

```py

			After successfully installing Stripe, you can proceed to configure the Stripe test key. To do so, simply navigate to the Stripe Dashboard, and then head over to the **Developers** tab, followed by the **API keys** section. From there, you can click on the option to reveal the test key, which will allow you to copy the test key for use.
			To create a new API key entry, make the required modifications to the files listed in your Code Bug Fixer project:
			config.py

```

API_KEY = "<YOUR_OPENAI_API_KEY>" STRIPE_TEST_KEY = "<YOUR_STRIPE_API_TEST_KEY>"

```py

			app.py

```

client = OpenAI(

api_key=config.API_KEY,

) config.py 文件，然后在 `app.py` 文件中检索此密钥，允许您的应用程序使用指定的密钥安全地与 Stripe API 通信。

            下一步是 在 `initialize_database()` 函数下添加到 `app.py`中。此函数将创建一个 SQLite 数据库和一个包含两个列的用戶表，用于 `指纹` 和 `使用计数器`，如 *表 4.1*所示：

```py
 def initialize_database():
    conn = sqlite3.connect('app.db')
    c = conn.cursor()
    c.execute(
        '''CREATE TABLE IF NOT EXISTS users (fingerprint text primary key, usage_counter int)''')
    conn.commit()
    conn.close()
```

            前面的函数通过使用 Python 中的 `SQLite` 库连接到一个新创建的 `app.db`数据库，然后 创建一个 `c`，用于执行 SQL 命令并从数据库中检索结果。 游标对象允许您对数据库执行各种操作，如创建表、插入数据以及 更新数据。

            然后，我们使用游标对象执行一个创建名为 `users` 的表的 SQL 命令，该表位于连接的 SQLite 数据库中。 此表有两个列， `fingerprint,` 和 `usage_counter`； `fingerprint` 列将存储用户浏览器 ID，而 `usage_counter` 将存储特定用户的程序使用次数。 `主键` 关键字指定 `fingerprint` 列是表的 `主键。`

            最后，我们可以 `提交` 之前 SQL 命令对数据库所做的更改，使它们成为永久更改，并且 `关闭` 数据库连接，释放连接所使用的任何资源。

            这是为 Code Bug Fixer 应用程序初始化 SQL 数据库的方法。 这些都是创建 SQL 数据库和配置 Stripe 测试密钥所必需的库。 在下一步中，您将了解获取 `指纹` 浏览器的过程，这将帮助您识别使用 您的应用程序的个别用户。

            获取浏览器指纹 ID

            为了跟踪和识别您 Web 应用程序的独特用户，您需要一种方法来获取他们的 浏览器指纹。 浏览器指纹是基于各种参数（如浏览器类型、屏幕分辨率和已安装字体）由浏览器生成的唯一 标识符。 在本节中，我们将探讨如何在 Python 中获取浏览器指纹 ID 。

            在我们的应用程序中，为每个单独的任务创建一个专门的 Python 函数被认为是一种良好的实践，生成浏览器指纹也不例外。 因此，在 `initialize_database()` 函数下创建一个名为 `get_fingerprint()` 的新函数是合适的：

```py
 def get_fingerprint():
    browser = request.user_agent.browser
    version = request.user_agent.version and float(
        request.user_agent.version.split(".")[0])
    platform = request.user_agent.platform
    string = f"{browser}:{version}:{platform}"
    fingerprint = hashlib.sha256(string.encode("utf-8")).hexdigest()
    print(fingerprint)
    return fingerprint
```

            `get_fingerprint()` `函数是一个 Flask 视图函数，它负责为与应用程序交互的每个用户生成一个唯一的浏览器指纹（在大多数情况下）。 指纹是根据用户的浏览器类型、版本和平台生成的字符串的散列，用以唯一标识用户。

            重要提示

            浏览器指纹并不能保证对每个用户都是 100%唯一的，但浏览器指纹通常可以生成一个相对唯一的标识符，用以区分不同的用户。 如 IP 地址或 MAC 地址这样的标识符更适合 现实世界应用。

            首先， 函数通过使用 `request.user_agent`从请求对象中获取 `user_agent` 对象。此对象包含有关用户浏览器、平台和版本的信息。

            接下来，该函数通过将浏览器、版本和平台信息连接起来，并用冒号（`:`）分隔，构建一个字符串。 这个字符串用作输入到 `hashlib.sha256()` 函数的输入，该函数使用 `print()` 语句生成输入字符串的哈希，以便我们可以在我们的 VSCode 日志中验证指纹。

            该函数返回生成的指纹字符串。 稍后，我们将指纹存储在 SQLite 数据库中，同时存储每个用户的用法计数器，以便应用程序可以跟踪用户访问应用程序的次数。 这些信息用于确定用户是否需要付费才能继续使用 该应用程序。

            现在，是时候通过获取每个用户在下一节中的应用使用次数来覆盖我们用户跟踪机制的最后一部分。

            跟踪应用程序用户

            在本节中，我们将 深入探讨应用程序后端的使用计数器功能。 具体来说，我们将检查 `get_usage_counter()` 和 `update_usage_counter()` 函数，这些函数负责检索和更新与用户浏览器指纹 ID 相关的使用计数器。 这些函数在确定用户是否超出使用限制并应被提示付费以继续使用 应用程序方面起着至关重要的作用。

            The `get_usage_counter()` 函数负责从 SQLite 数据库中检索特定浏览器指纹的使用计数器。 计数器跟踪具有该指纹的用户提交了多少次 代码错误：

```py
 def get_usage_counter(fingerprint):
    conn = sqlite3.connect('app.db')
    c = conn.cursor()
    result = c.execute('SELECT usage_counter FROM users WHERE fingerprint=?', [fingerprint]).fetchone()
    conn.close()
    if result is None:
        conn = sqlite3.connect('app.db')
        c = conn.cursor()
        c.execute('INSERT INTO users (fingerprint, usage_counter) VALUES (?, 0)', [fingerprint])
        conn.commit()
        conn.close()
        return 0
    else:
        return result[0]
```

            The 函数接受 `fingerprint` 作为参数，因为它用于检索给定浏览器指纹的使用计数器。 指纹作为应用程序每个用户的唯一标识符，该函数在数据库中查找与该指纹相关的使用计数器。 它连接到 `app.db` ，然后创建一个可以执行数据库上 SQLite 命令的光标对象。

            然后， `c.execute()` 命令创建一个名为 `users` 的新数据库表，其中包含名为 `fingerprint` 和 `usage_counter`的两列。 它仅在表不存在时创建表。

            该 `结果` 执行一个 SQL 查询以获取给定 `fingerprint` 的 `usage_counter` 列值 `users` 表。 它是通过使用参数化查询和 `fetchone()` 方法来完成的。 如果对于给定的指纹没有找到记录，结果将被设置为 `None` 对于 新用户。

            一旦 与数据库的连接关闭，函数将检查之前数据库查询的结果是否为 `None` 。 如果是 `None`，则意味着在 `fingerprint` 的 `users` 表中没有记录。 在这种情况下，函数将执行以下操作：

                1.  连接到 数据库。

                1.  创建一个 游标对象。

                1.  插入一个新记录用于 `指纹` ，并带有 `usage_counter` 值为 `0`。

                1.  将更改提交到 数据库。

                1.  关闭 连接。

                1.  返回 `0`。

            如果结果不是 `None`，则意味着在 `fingerprint` 的 `users` 表中已经存在一个记录。 在这种情况下，函数返回记录的 `usage_counter` 列的值。 本质上，这个函数旨在为新用户返回 `0` 的值，或者为那些已经在数据库中的用户返回使用次数。

            另一方面， `update_usage_counter()` 函数负责更新给定浏览器指纹的数据库中的使用计数器。 该函数接受两个参数——浏览器指纹和更新的使用 计数器值：

```py
def update_usage_counter(fingerprint, usage_counter):
    conn = sqlite3.connect('app.db')
    c = conn.cursor()
    c.execute('UPDATE users SET usage_counter=? WHERE fingerprint=?', [usage_counter, fingerprint])
    conn.commit()
    conn.close()
```

            一旦连接到数据库并创建了一个版本游标，该函数就负责在每次使用我们的 Code Bug Fixer 应用后更新使用计数器。 它使用游标对象的 `execute` 方法执行一个更新用户 `usage_counter` 字段的 SQL 语句，该用户具有提供的 `fingerprint`。

            该 SQL 语句使用 `占位符 ?` 来指示 `usage_counter` 和 `fingerprint` 的值应插入的位置。 要插入的值作为列表传递给 `execute()` 的第二个参数，其顺序与 SQL 语句中的顺序相同。 WHERE 子句确保更新仅应用于匹配指定 `fingerprint`的行。

            总结来说， `get_usage_counter()` 函数从 SQLite 数据库中检索用户浏览器指纹 ID 的使用计数器，而 `update_usage_counter()` 函数在每次使用 Code Bug Fixer 应用后更新给定指纹的数据库中的使用计数器值。 这些函数对于确定用户是否已超过使用限制并应提示他们支付以继续使用应用程序至关重要。 我们现在可以将迄今为止创建的所有函数集成到我们的 Code Bug Fixer 应用程序 `index()` 页面中。

            实现使用计数器

            在本节中， 我们将把我们之前创建的所有函数集成到我们的 Code Bug Fixer 应用程序的 `index()` 页面中。 通过这样做，我们将能够跟踪用户提交代码错误的次数以及他们是否已达到三次提交的限制，从而需要他们付费才能继续。 此外，我们还将能够将每个提交与一个唯一的浏览器指纹关联起来，以防止用户使用 不同的别名 提交多个错误：

```py
 @app.route("/", methods=["GET", "POST"])
def index(): initialize_database()
 fingerprint = get_fingerprint()
 usage_counter = get_usage_counter(fingerprint)
```

            前面的代码 片段执行了三个基本任务。 首先，它初始化数据库，然后从数据库中检索用户的浏览器指纹并获取他们的当前使用计数器。 如果用户数据库表不存在，则会创建该表。 指纹变量唯一标识用户，并允许我们跟踪他们的 使用计数器。

            初始化完成后，我们需要设置一条规则，当使用计数器超过一个特定数字时，将控制权传递到支付页面： 

```py
 if request.method == "POST": POST, which indicates that the user has submitted a form on the website. In our case, this means that the user has submitted code for fixing in the Code Bug Fixer. Then, our app will check whether the user’s usage counter is greater than 3. If it is, it means the user has exceeded their limit of free usage and should be directed to the payment page. The function returns a rendered template of the payment.html page. We also add a print() statement so that we can verify the counter increment in our logs.
			After the usage counter is initialized and we have a mechanism to check whether it is greater than the allowed number of usages, the last step is to make sure that the usage counter increments every time the user utilizes the Code Bug Fixer:

```

fixed_code_prompt = (f"修复此代码：\n\n{code}\n\n 错误：\n\n{error}." f" \n 仅回复修复后的代码。")

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

        fixed_code = fixed_code_completions.choices[0].message.content usage_counter += 1

print(usage_counter)

usage_counter变量增加 1，这意味着用户多使用了一次服务。之后，我们将更新数据库中用户的使用计数器值，该用户通过其浏览器指纹识别。这确保了使用计数器在用户会话之间持久存在，并反映了用户使用服务的总次数。我们还添加了一个print()语句，以便我们可以在日志中验证计数器的增加。

            现在，您可以运行代码错误修复应用程序来验证您的使用跟踪方法是否成功。 一旦代码错误修复应用程序启动并运行，您可以将一个有错误的代码和一个错误放入相关字段，然后点击 **代码修复** 按钮。 执行此操作两次，以便您的使用计数器增加两次。 一旦您收到 ChatGPT API 的响应以及 **修复代码** 和 **说明** 字段在您的代码错误修复应用程序中已填充，您就可以回到 **终端** 窗口并验证您的指纹和使用计数器是否显示（见 *图 4**.3*）。

            ![图 4.3 – 浏览器指纹和使用计数器记录](img/B22414_04_3.jpg)

            图 4.3 – 浏览器指纹和使用计数器记录

            `3ae64fc…` 这是您的特定浏览器指纹，而数字表示您当前使用该应用程序的次数。 由于您向 ChatGPT API 发出了两次请求，计数器 增加了两次。

            要确认新数据库的创建，您可以在项目目录中检查名为 `app.db` 的文件（见 *图 4**.4*）。 此文件将存储所有用户数据，并且即使您关闭并重新启动 应用程序，它也会持续存在。

            ![图 4.4 – 数据库显示](img/B22414_04_4.jpg)

            图 4.4 – 数据库显示

            这是将主要后端功能添加到我们的 Code Bug Fixer 应用程序的 最后一步。 在下一节中，您将学习如何构建 `/charge` 页面，当用户的免费 试用期结束时，用户将被重定向到该页面。

            向 ChatGPT 应用程序添加付款

            在本节中，我将指导您完成创建 Code Bug Fixer 中的付款页面和功能的流程。 我们将创建一个与 Stripe 连接的付款页面，为用户提供三种不同的 订阅计划：

                +   **月度计划**：用户每月将被收取 5 美元 费用

                +   **季度计划**：用户每季度将被收取 12 美元 费用

                +   **年度计划**：用户每年将被收取 50 美元 费用

            我们还将创建一个确认页面，该页面将使用如下的简单声明来确认用户已购买的方案： *您已成功支付 12 美元用于无限访问 Code * *Bug Fixer 的季度计划。*

            接下来，您将学习如何创建 `payment.html` 文件。

            构建付款页面

            在这里，我们将创建一个完整的 HTML 文档，其中包含一个用于从用户收集付款信息的表单。 我们将使用 **Bulma CSS** 框架来 美化页面并包含 **jQuery** 以及 Stripe API 来处理 付款处理。

            页面将被分为三个列，每个列显示不同的支付计划选项。 每个选项包括一个带有标题、副标题和计划描述的卡片。 支付表单位于每个卡片的页脚中，它包括用于计划类型和支付金额的隐藏输入字段。 使用 Stripe API 生成一个支付按钮，该按钮收集支付信息并启动支付处理（见 *图 4**.5*）。

            ![图 4.5 – 支付页面](img/B22414_04_5.jpg)

            图 4.5 – 支付页面

            生成 “ 模板 ” 文件夹。 这两个文件将被命名为 `payments.html` 和 `charge.html`。一旦完成，Code Bug Fixer 项目的结构将如下所示： 如下：

```py
 CodeBugFixer/
├── templates/
│   ├── charge.html
│   ├── index.html
│   └── payment.html
├── venv/
├── app.db
├── app.py
└── config.py
```

            我们将从构建我们的 `payments.html` 页面头部开始：

```py
 <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.0/css/bulma.min.css">
    <script src="img/jquery.min.js"></script>
    <script src="img/"></script>
    <title>Payment</title>
</head>
</html>
```

            头部 包含文档正确显示所需的元数据和外部资源。 字符集指定文档中使用的字符编码，在本例中为 `UTF-8` ，而 viewport 元标签确保文档在不同屏幕尺寸的设备上正确显示。

            `link` 标签用于导入外部框架。 我们将使用 Bulma CSS 框架，它提供了一套预设计的 CSS 样式，可以快速且容易地构建响应式网页。 我们将使用 Bulma CSS 框架，它提供了一套预设计的 CSS 样式，可以快速且容易地构建响应式网页。

            然后，我们将导入 jQuery 库，这是一个流行的 JavaScript 库，它简化了操作 HTML 文档和处理事件的过程。 第二个导入是 Stripe API 库，它提供了处理 在线支付 的功能。

            现在，在 `</head>`，我们可以开始构建 HTML 文件的主体部分，它被包含在 `<``body>` 标签内：

```py
 <body>
    <section class="section">
        <div class="container">
            <h1 class="title">Payment Options</h1>
            <div class="columns">
          </div>
        </div>
    </section>
</body>
```

            `我们将使用` `<section>` `元素` `，表示它是一个可以独立于其他元素进行样式的页面独立部分。` `有一个` `<div>` `元素，其类为` `columns` `。` `该元素用于创建一个网格系统，用于在列中布局内容。` `这是一种创建响应式布局的常见方法，其中列的数量` `可能根据屏幕大小或使用的设备而变化。` `在这种情况下，列将包含用户可以选择的不同支付计划选项，每个选项都有其自己的功能和定价。`

            `现在，在` `列` `<div>` `元素内部，我们可以创建三个列，代表之前提到的三个支付计划：` `：`

            `您可以在` `payments.html` `文件` `中找到完整的` [https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API/blob/main/Chapter04%20CodeBugFixer/templates/payment.html](https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API/blob/main/Chapter04%20CodeBugFixer/templates/payment.html)：

```py
 <div class="column">
    <div class="card">
        <div class="card-content">
            <p class="title">Monthly Plan</p>
            <p class="subtitle">$5 par month</p>
            <p>This plan will give you unlimited access
                to the code explanation and fixing services every month. You will be charged $5 every month.</p>
        </div>
        <footer class="card_footer">
            <form action="/charge" method="post">
                <input type="hidden" name="plan" value="monthly">
                <input type="hidden" name="amount" value="500">
                <script
                    src="img/checkout.js"
                    class="stripe-button"
                    data-key="<YOUR_PUBLIC_KEY>" data-amount="500"
                    data-name="Monthly Plan"
                    data-description="$5 per month"
                    data-image="https://stripe.com/img/documentation/checkout/marketplace.png"
                    data-locale="auto"
                    data-zip-code="false">
                </script>
            </form>
        </footer>
    </div>
```

            `重要提示`

            `请确保将` `"<YOUR_PUBLIC_KEY>"` `标签替换为您从` `Stripe 账户` `获得的公钥。`

            `第一个` `<div>` `具有` `列` `类别的元素` `负责创建一个类似卡片元素，用于展示提供给用户的月度计划信息。` `该卡片包含计划名称、订阅费用以及用户可以从该计划中期待的内容。`

            `使用` `<div>` `具有` `卡片` `类别的元素` `用于创建展示月度订阅计划信息的卡片。` `在卡片内部，卡片内容使用` `card-content` `类` `定义。` `卡片内容由三个段落组成，第一个包含计划的标题，` `月度计划` `，第二个包含订阅费用，` `$5 每月` `，第三个提供计划的描述，突出订阅该计划的好处。`

            在页脚内部，定义了一个 `表单` 元素，该元素用于将订阅请求提交到服务器。 表单的 action 属性设置为 `/charge`，这表示订阅请求将被发送到服务器的 `/charge` 路由。 我们将在稍后构建 `/charge` 页面的后端功能。 此功能将确保在购买支付计划后向用户显示确认信息。

            表单包含两个隐藏的输入字段，用于发送计划的名称和值。 在这种情况下，计划的名称设置为 `monthly`，而金额属性设置为 `500`，这代表订阅金额（以分计）。 这些信息将被用于渲染 确认页面。

            最后，在表单元素内部包含了一个脚本标签，该标签加载 Stripe 结账脚本。 此脚本负责创建一个 `data-locale` 属性并将其设置为 `auto`，这确保了支付表单的语言将根据用户的地理位置自动设置。 `data-zip-code` `属性设置为 `false`，这意味着支付表单将不需要用户输入他们的 邮编。

            如代码片段所示，季度计划和年度计划的列是以与月度计划相同的方式构建的。 唯一的区别是计划的名称、副标题和成本值，以及计划隐藏输入字段和要收费的金额值。

            您可以通过在任何浏览器中显示 `payment.html` 页面来验证您的页面是否正常工作。 您可以从 **打开方式** 选项中选择 HTML 文件并选择您喜欢的浏览器（见 *图 4**.6*）。 然后您应该看到一个支付页面，类似于在 *图 4**.5*中显示的页面。

            ![图 4.6 – 在浏览器中显示 HTML](img/B22414_04_6.jpg)

            图 4.6 – 在浏览器中显示 HTML

            这就是您如何构建一个包含收集用户支付信息的表单的支付页面。 我们使用 Bulma CSS 框架设计了支付页面，并使用 jQuery 和 Stripe API 处理支付处理。 页面分为三个列，每列显示不同的支付计划选项，支付表单位于每张卡的页脚中。 Stripe API 用于生成一个收集支付信息并启动 支付处理 的支付按钮。

            在下一节中，您将学习如何使用 Stripe API 在您的应用程序中创建处理用户支付的收费函数，并返回一个支付 确认页面。

            确认用户支付

            在上一个部分中，您了解到当用户选择购买支付计划以访问您的应用程序时，我们将立即触发 /charge 页面，并为其提供所选计划的名称以及相应的支付金额 （以分计）。

            我们可以在我们的 `app.py` 文件中的 `index()` 函数中构建 `charge()` 函数。 此函数是 Code Bug Fixer 网络应用程序的一部分，负责处理用户选择定价计划并提交其支付信息时的 `收费` 过程。 该函数在用户点击 `payment.html`:

```py
 @app.route("/charge", methods=["POST"]) def charge():
    amount = int(request.form["amount"])
    plan = str(request.form["plan"])
    customer = stripe.Customer.create(
        email=request.form["stripeEmail"], source=request.form["stripeToken"] )
    charge = stripe.Charge.create(
        customer=customer.id,
        amount=amount,
        currency="usd", description="App Charge" ) app.py file here: https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API/blob/main/Chapter04%20CodeBugFixer/app.py.
			The `@app.route("/charge", methods=["POST"])` decorator creates a route to handle a `POST` request sent to the `/charge` endpoint. This means that when the form in the HTML template is submitted, it will send a `POST` request to this endpoint.
			Both the `amount` and `plan` variables are assigned values that were previously sent by the user via a payment form, and they are used later in the function to create a new customer and charge the customer the appropriate amount based on the selected plan.
			Then, we can create a new Stripe customer object using the Stripe API. The `stripe.Customer.create()` method takes two arguments – the email address of the customer and the payment source. In this case, the email address is obtained from the `stripeEmail` parameter in the `POST` request sent by the Stripe checkout form, and the payment source is obtained from the `stripeToken` parameter.
			The `stripeToken` parameter is a unique identifier for the payment information provided by the user in the checkout form, such as credit card details or a payment app. Stripe uses this token to securely charge the user’s payment method for the specified amount. By passing the `stripeToken` parameter to the source argument of `stripe.Customer.create()`, the payment information is associated with the newly created customer object in Stripe.
			We can then use the Stripe API to create a charge object that is associated with the customer who provided their payment information. The `stripe.Charge.create()` method creates a new charge object in the Stripe API with the following arguments:

				*   `customer`: This is the ID of the Stripe customer object associated with the payment. The `customer.id` attribute is used to retrieve the ID of the customer object created in the previous step.
				*   `amount`: This is the amount of the charge in cents. The amount variable is set to the value passed in the `POST` request from the form.
				*   `currency`: This is the currency of the charge. In this case, it is set to `USD`.
				*   `description`: This is a brief description of the charge. In this case, it is set to `App Charge`.

			After creating the charge, the function uses Flask’s `render_template()` function to render the `charge.html` template and pass in the amount and plan variables. The `charge.html` template will be used to display a message to the user, indicating that their payment was successful.
			Our final task when building the application is to create the `charge.html` file. This file will be used to display a confirmation message to the user after a successful charge has been made. The `render_template` function used in the charge function of the `app.py` file specifies that the `charge.html` file will be used to render the message.
			You can now open the `charge.html` file that we created in the `templates` folder and add the following code:

```

<!DOCTYPE html>

<html lang="en">

<head>

    <meta charset="UTF-8">

    <title>支付确认</title>

</head>

<body>

<h1>支付确认</h1>

<p>您已成功支付${{ amount / 100 }}美元用于{{ plan }}计划，以获得对 Code Bug Fixer 的无限制访问</p>

</body>

</html>

```py

			In this simple HTML page, the `<body>` section contains a heading that says `Payment Confirmation`. The paragraph below the heading uses curly braces to display the amount and plan variables passed from the `charge()` function. Specifically, it displays the amount variable divided by `100` (because the amount variable is in cents) and the `plan` variable for the user to confirm the payment they have made.
			When the `charge()` function is called and executed, it returns the `charge.html` file as a response, with the amount and plan variables passed as arguments to be rendered in the appropriate places in the HTML code.
			Now that the payments infrastructure has been added to your app, you can test it by navigating to the relevant pages and clicking on the payment buttons. You can follow the following steps:

				1.  Run the `app.py` file to start up your Code Bug Fixer app.
				2.  Create more than three bug-fixing requests to the ChatGPT API to be prompted to the `Payments` page (see *Figure 4**.5*).
				3.  Click on the **Pay with Card** button on one of the payment plans.
				4.  Enter the following sample credit card details in the pop-up window (see *Figure 4**.7*):
    *   `mrsmith@gmail.com`
    *   `4242 4242` `4242 4242`
    *   `09 /` `30`
    *   `424`
				5.  Click the **Pay** button:

			![Figure 4.7 – Stripe payment information](img/B22414_04_7.jpg)

			Figure 4.7 – Stripe payment information
			Once you have submitted your payment, you should be redirected to a payment confirmation page that displays a message indicating that your payment has been successfully processed, as shown in *Figure 4**.8*:
			![Figure 4.8 – The confirmation page](img/B22414_04_8.jpg)

			Figure 4.8 – The confirmation page
			In this section, you saw how to confirm user payments using the Stripe API in a Code Bug Fixer web application. We built `charge()` to handle the charge process when a user selects a pricing plan and submits their payment information. We also tested the payment infrastructure of the app using sample credit card details.
			Summary
			This chapter focused on the implementation of a payment infrastructure into a web application using the Stripe API. It provided instructions on how to set up a Stripe account, create API keys, and configure the payment settings. You saw the importance of selecting the appropriate business strategy, such as subscription or one-time payment, before implementing payment options. You also saw how to track user visits and usage using a SQL database and how to create payment plans on a payment page. Additionally, the chapter outlined the functions required to retrieve and update the usage counter and described how to implement them to track a user’s payment status.
			You learned how to build the `charge()` function, which handles the payment process when a user selects a pricing plan and submits their payment information. We covered the use of the Stripe API to create a new customer and charge object and render the `charge.html` template to display a confirmation message to the user. This section also provided instructions on how to test the payment feature using sample credit card details. The chapter provided a comprehensive guide on implementing a payment infrastructure in a web application, integrating the Stripe API and the ChatGPT API, from creating a Stripe account to handling payments and confirming user payments.
			In *Chapter 5*, *Quiz Generation App with ChatGPT and Django*, you will learn how to integrate the ChatGPT API with **Django**, a full stack web framework that comes with many built-in features and is designed to handle larger and more complex web applications than Flask. Django includes everything, from URL routing, database ORM, and an admin interface to authentication and security features, providing a more comprehensive framework for web development.

```

```py

```

```py

```
