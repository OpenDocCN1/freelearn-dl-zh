# 第六章：5

# 使用 ChatGPT 和 Django 创建问答应用

在本章中，我们将深入探讨将 **ChatGPT**，一个前沿的 语言 模型，与 **Django**，广受赞誉的 Python 应用开发框架 相结合的激动人心的世界。 我们将探索如何构建一个动态且交互式的考试 生成应用，该应用利用 **人工智能** (**AI**)。

在前几章中，我们主要关注了 Flask，一个轻量级和基本的 Web 框架。 然而，在本章中，我们将重点关注 Django，一个强大且高级的框架，它在构建一些最著名和最广泛使用的应用程序中发挥了关键作用，包括 Instagram、Dropbox 和 Pinterest。 您将有机会探索 Django 的功能，包括数据库管理、认证系统、管理界面以及 表单处理。

您将学习如何从头开始构建 Django 项目，包括设置环境和创建应用程序的基础组件。 我们将专注于创建问答应用的框架和视图。 您将探索 ChatGPT 和 Django 的集成，使您能够使用 AI 进行问答生成。 为了确保全面的学习体验，我们将介绍应用中的两个基本视图。 一个视图将允许用户通过输入学习材料和执行 ChatGPT API 来生成问答，相关的问题将被存储在数据库中。 另一个视图将使用户能够方便地下载之前 生成的测试。

在本章中，您将探索开发健壮的 Django 应用程序所必需的几个关键主题。 您将从安装 Django 并创建您的第一个运行项目开始，为您的应用程序打下基础。 之后，您将构建针对考试生成应用程序定制的 Django 视图，然后与 Django 集成 ChatGPT API 以在后台拦截响应。 为了提升用户体验，您将利用 Bootstrap 模板创建一个视觉上吸引人的界面。 此外，您将学习如何安全地将 ChatGPT 响应在数据库中存储和检索，确保数据完整性和安全性。 最后，本章将指导您将文件下载功能添加到您的应用程序中，完善您的技能集，并使您能够交付一个全面且功能齐全的 网络应用程序。

在本章中，我们将涵盖以下 主题：

+   构建一个 Django 项目

+   创建考试应用框架 和视图

+   将 ChatGPT 和 Django 集成用于 测验生成

+   存储和下载 生成的测验

在本章结束时，您将具备使用 ChatGPT API 生成任何提供的 学习材料派生的考试问题的知识和技能。

# 技术要求

在继续本章内容之前，您需要完成一些基本软件的安装。 此外，Django 网络框架的安装将在下一节中演示。

该项目有以下 技术先决条件：

+   在您的 本地机器上安装 Python 3.7 或更高版本

+   一个代码编辑器，推荐使用 VSCode 以获得 最佳体验

+   一个 OpenAI API 密钥，用于访问必要的 API 功能

您可以在提供的 存储库 https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API](https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API)中访问本章使用的代码示例。

# 构建 Django 项目

在本节中，我们将开始一段激动人心的旅程，构建一个作为我们测验生成应用基础的 Django 项目。 我们将引导您逐步设置和构建您的 Django 项目，确保您有一个坚实的基础来继续。 在本节结束时，您将拥有一个功能齐全的 Django 项目，可以基于 学习材料自动生成测验。

我们的冒险从 安装 Django 和创建新的 Django 项目开始。 我们将向您介绍设置开发环境的过程，包括 Python 和 Django 框架的安装。 在 Django 设置完成后，我们将使用命令行界面生成一个新的 Django 项目，为您提供必要的目录结构和初始 配置文件。

我们可以先创建一个名为 `QuizApp`的新 VSCode 项目。 现在，让我们点击 VSCode 的 `pip`：

```py
 $pip install Django
```

接下来，您可以在终端中运行以下命令来创建一个新的 Django 项目：

```py
 $django-admin startproject quiz_project
```

当您运行此命令时，Django 的命令行工具，称为 `django-admin`，将创建一个具有指定名称的新项目目录， `quiz_project`。此目录将包含启动 Django 项目所需的文件和文件夹。

`startproject` 命令 通过生成以下文件来初始化项目结构： 以下文件：

+   `manage.py`：一个命令行工具，允许您与各种 Django 命令交互并管理 您的项目。

+   `quiz_project`：项目目录，其名称将与命令中指定的名称相同。 此目录作为您 Django 项目的根目录，包含配置文件和其他 项目特定组件。

在项目目录内，您将找到以下文件和目录：

+   `__init__.py`：一个空文件，标记目录为一个 Python 包

+   `settings.py`：配置文件，您在其中定义 Django 项目的各种设置，包括数据库设置、中间件和 已安装的应用程序

+   `urls.py`: 定义您项目 URL 与视图之间映射的 URL 配置文件。

+   `wsgi.py`: 用于部署的**Web 服务器网关接口** (**WSGI**) 配置文件。

您可以通过定义模型、视图和模板以及根据应用程序的需求配置设置来进一步自定义和开发您的 Django 项目。

在 Django 中，一个项目是一组设置、配置和多个应用程序的集合，这些应用程序协同工作以创建一个网络应用程序。 另一方面，应用程序是 Django 项目中的一个模块化组件，它服务于特定的目的或功能。 因此，在我们的 `quiz_project`中，我们可以创建一个新的应用程序 如下：

```py
 $cd quiz_project
$python manage.py startapp quiz_app
```

我们新创建的应用程序的目的将是生成 AI 问题并允许用户下载它们。 将根据指定的应用程序名称生成一个新目录， `quiz_app`。 此目录包含开发您的 Django 应用程序所需的必要文件和文件夹。 让我们探索 应用程序目录 内的典型项目结构和文件：

+   `admin.py`: 此文件用于将您的应用程序模型注册到 Django 管理界面。 您可以根据需要自定义模型在管理站点上的显示和交互方式。

+   `apps.py`: 此文件定义了应用程序配置，包括应用程序的名称，并在应用程序启动时使用初始化函数。

+   `models.py`: 在这里，您可以使用 Django **对象关系映射** (**ORM**) 来定义您的应用程序的数据模型。 模型代表您数据的结构，并定义了数据库中的表。

+   `tests.py`: 此文件用于编写您应用程序的测试。 您可以创建测试用例并运行它们，以确保应用程序的功能。

+   `views.py`: 此文件定义了处理 HTTP 请求并返回 HTTP 响应的函数或类。 该文件处理数据，与模型交互，并渲染模板以生成显示给用户的内 容。

+   `migrations/`: 该文件夹用于存储数据库迁移文件。 迁移是管理数据库模式随时间变化的一种方式。 它们允许您跟踪和应用数据库结构的增量更改，例如创建新表、修改现有表以及添加或 删除列。

测验生成应用程序的完整高级项目结构如下： 如下：

```py
 quiz_project/
├── quiz_app
│   ├── migrations/
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── quiz_project/
│   ├── settings.py
│   ├── urls.py
│   ├── wsgi.py
│   └── __init__.py
└──  manage.py
```

默认情况下，Django 包含几个内置应用程序，例如 `auth`, `admin`, `contenttypes`，和 `sessions`。这些应用程序都有自己的迁移需要应用以在数据库中创建包含默认用户数据的所需表。

Django 使用默认的 SQLite 数据库来存储数据。 我们可以运行以下命令来使用所有 现有模型初始化数据库： 现有模型初始化数据库：

```py
 $python manage.py migrate
```

当你运行此命令时，Django 将检查这些默认应用中是否有任何挂起的迁移，并在必要时应用它们。 这确保了数据库模式被正确设置以支持 Django 内置功能，并由 命令输出进行验证：

```py
 Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying auth.0012_alter_user_first_name_max_length... OK
  Applying sessions.0001_initial... OK
```

一旦迁移都设置好了，你就可以启动开发服务器来 在本地运行你的 Django 项目：

```py
$python manage.py runserver Watching for file changes with StatReloader
Performing system checks... System check identified no issues (0 silenced). May 26, 2023 - 18:59:08
Django version 4.2.1, using settings quiz_project.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```

当你在终端中运行此命令时，Django 的 `manage.py` 脚本启动一个轻量级 Web 服务器，允许你在本地运行和测试你的 Django 应用程序。 要访问你的应用程序，你可以点击终端输出中提供的链接（http://127.0.0.1:8000/）或在浏览器中输入它。 此链接包含一个本地 IP 地址和一个端口，你可以通过该端口访问你的新应用程序（见 *图 5**.1*）。

![图 5.1 – Django 欢迎屏幕](img/B22414_05_1.jpg)

图 5.1 – Django 欢迎屏幕

在浏览器中看到 Django 欢迎页面意味着我们已经成功创建了我们的 Django 项目和应用。 在下一节中，我们将根据我们的 项目需求，通过定义模型、视图和模板以及配置 URL 路由来开始构建我们的应用程序。

# 创建考试应用程序框架和视图

在本节中，我们将关注构建我们的 Django 应用程序的基本结构。 我们将从探索 Django 设置开始，在那里你将学习如何配置重要方面，例如数据库连接和中间件组件。 理解和正确设置这些配置对于应用程序的平稳运行至关重要。 你的应用程序。

我们还将了解 Django 中的 URL 处理。你将发现如何使用 `urls.py` 文件定义 URL 模式，从而实现应用程序内的无缝导航。 我们还将介绍使用正则表达式进行动态 URL，以实现灵活和动态的路由。 此外，我们将指导你通过将基础模板集成到应用程序中，这将提供一致菜单、侧边栏和视图。 你的应用程序。

## 连接 Django 视图和 URL

在 Django 中，视图、应用 URL 和项目 URL 之间的关系构成了处理和路由 Web 请求的关键结构。 流程从视图开始，视图是负责处理特定 HTTP 请求并返回适当响应的 Python 函数或类。 每个视图对应于应用程序中的特定功能或页面。 这些视图可以通过分配唯一的 URL 向用户展示。 由于在 Django 中，一个项目可以有多个应用程序，因此这些 URL 首先在 `quiz_app/urls.py`中的应用级别定义。然后，这些 URL 被传递到位于 `urls.py` 文件中的项目级别，该文件位于我们的项目目录 `quiz_project` 中（参见 *图 5**.2*）。

![图 5.2 – Django 视图/URL 架构](img/B22414_05_2.jpg)

图 5.2 – Django 视图/URL 架构

`默认情况下，Django 在初始化项目时会自动生成` `quiz_project/urls.py` `文件。 `然而，需要注意的是，我们将在` `urls.py` `文件中手动创建一个新文件，该文件位于` `quiz_app` `文件夹内。 `与项目级别的` `urls.py` `文件不同，应用级别的` `urls.py` `文件不会自动生成，并且组织并定义特定于` `quiz_app` `内部功能的 URL 模式是至关重要的。

`现在，您可以打开` `quiz_app/views.py` `并编写以下` `Python 代码：

```py
 from django.shortcuts import render
def home(request):
    return render(request, 'base.html')
```

`此代码` `演示了一个名为` `home` `的简单 Django 视图函数。 `该视图函数负责处理特定类型的请求，通常是 HTTP` `GET` `请求，并生成适当的响应。

在函数内部，Django 快捷方式 `render` `函数用于渲染 HTML 模板。 `第一个参数，` `request` `，代表传入的 HTTP 请求对象。 `该对象包含有关请求的信息，例如头部、数据和用户会话详情。 `第二个参数，` `base.html` `，指定要渲染的模板。

`在构建` `base.html` `文件之前，我们需要确保用户可以访问` `home` `视图。 `为了实现这一点，我们需要在我们的` `quiz_app/urls.py` `文件中指定一个唯一的 URL：

```py
 from django.urls import path
from . import views
urlpatterns = [
    path('', views.home, name='home'),
]
```

`在这里，` `urlpatterns` `变量是一个包含应用程序定义的 URL 模式的列表。 `目前，已指定了一个 URL 模式。 `` `path` `函数用于定义 URL 模式。 `它接受三个参数 - 第一个参数是 URL 模式本身，在本例中由空字符串` `''` `表示。 `这个空字符串表示应用程序的根 URL 或基本 URL。

第二个参数, `views.home`, 指的是将处理此 URL 模式请求的视图函数。 第三个参数, `name='home'`, 为此 URL 模式提供了一个唯一的名称。 此名称可用于从应用程序的其他部分 引用 URL 模式，例如模板或其他 URL 配置。

最后，我们可以通过 修改 `quiz_project/urls.py`:

```py
from django.contrib import admin from django.urls import include, path
urlpatterns = [
 path('admin/', admin.site.urls),
 path('', include('quiz_app.urls')), ]
```

提供的代码展示了我们 Django 项目中 URL 模式的配置。 就像在应用级别的 URL 文件中一样，这里 `urlpatterns` 变量包含项目 URL 名称的列表。 在这种情况下，指定了两个 URL 模式。

第一个 URL 模式是使用 `path` 函数和 `admin/` 参数定义的。 这将 `admin/` URL 模式与 Django 管理界面关联起来，从而允许访问管理仪表板和相关功能。 Django 管理页面通常默认设置，与我们 的 quiz 应用程序的功能无关。

`path('', include('quiz_app.urls'))` path 函数从 `quiz_app/urls.py`获取应用级别的 URL。第一个参数，一个空字符串 `''`，表示应用程序的根 URL 或基本 URL。 这意味着当用户访问项目的根 URL 时，此 URL 模式将被 匹配。

第二个参数, `include('quiz_app.urls')`, 指示 Django 包含在 `quiz_app.urls` 模块中定义的 URL 模式。 这意味着任何在 `quiz_app` 应用程序中指定的 URL 都将被匹配并 相应处理。

这是 URL 处理分层结构的模样。 我们从处理应用级 URL 的视图开始，这些 URL 定义了特定的 URL 模式，最终到项目级 URL，这些 URL 将请求路由到适当的应用级 URL。 这个过程通过代码示例进行了说明，突出了在应用和项目级别正确配置 URL 对于在 Django 应用程序中实现顺畅导航和功能的重要性。 Django 应用程序。

在接下来的部分中，我们将深入探讨构建 HTML 模板，包括必要的 `base.html` 文件。 模板在定义 Django 应用程序中网页的视觉结构和布局方面发挥着至关重要的作用。 通过构建和定制 HTML 模板，我们可以为我们的 测验应用程序 创建一个一致且视觉上吸引人的用户界面。

## 开发 Django 模板

在本节中，我们将专注于构建 `base.html` 模板并将 CSS 集成到我们的 Django 应用程序中。 `base.html` 模板是我们网页视觉结构和布局的基础。 为了增强我们应用程序的设计和样式，我们将利用外部 CSS 资源。 一个宝贵的资源是 Bootstrap 框架，它提供了一系列预设计的模板和组件。 我们将通过从官方 Bootstrap 网站（[https://getbootstrap.com/docs/5.3/examples/](https://getbootstrap.com/docs/5.3/examples/)）中寻找合适的模板的过程，您可以在那里探索各种示例并选择与您期望的美学 和功能相匹配的模板。

如果您使用 **Google Chrome**，可以轻松访问任何 Bootstrap 模板背后的代码。例如，对于这个项目，我们可以使用 **Dashboard** 模板，您可以通过以下链接访问： [https://getbootstrap.com/docs/5.3/examples/dashboard/](https://getbootstrap.com/docs/5.3/examples/dashboard/)。正如您在 *图 5.3**.3*中可以看到的，可以通过右键单击 **Dashboard** 模板并选择 **查看页面** **源代码** 选项来访问 HTML 代码。

![Figure 5.3 – Viewing the page source in Chrome](img/B22414_05_3.jpg)

图 5.3 – 在 Chrome 中查看页面源代码

通过这种方式 ，你可以从该模板中提取特定部分并将其集成到自己的应用程序中，从而实现易于定制和扩展。 这个过程为你的 Django 应用程序 奠定了坚实的基础。

首先，你可以在应用程序和项目文件夹之外的一个单独位置创建一个 `templates` 文件夹，遵循以下概述的推荐结构。 这种有组织的设置确保了你的 模板文件 的有效管理：

```py
 quiz_project/
├── quiz_app
├── quiz_project/
└──  templates
```

在 `templates` 目录中，你将创建一个名为 `base.html`的新 HTML 文件，作为应用程序结构的基石。 在这个文件中，你可以开始包括相关的链接并定义设置应用程序整体上下文的头部部分：

```py
 {% load static %}
<html lang="en" data-bs-theme="auto">
<head>
  <script src="img/color-modes.js"></script>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Quiz App</title>
  <!-- Bootstrap CSS -->
  <link href="{% static 'css/bootstrap.min.css' %}" rel="stylesheet" />
<!--  ; Custom styles for this template &ndash;&gt;-->
  <link href="{% static 'css/dashboard.css' %}" rel="stylesheet" />
</head>
```

第一 行 `{% load static %}`，是一个 Django 模板标签，允许我们加载静态文件，例如 CSS 样式表和 JavaScript 脚本，到我们的 HTML 模板中。 它使我们能够在 Django 项目中访问和使用这些文件。 在本节后面的内容中，我们将创建 `.css` 文件，并且它们将被自动传递到 HTML 中。

然后， `data-bs-theme` 属性用于自动主题处理。 此属性通常与 Bootstrap（一个流行的 CSS 框架）一起使用，根据用户的 设备设置自动在浅色和深色主题之间切换。

`head` 部分包含各种元标签和外部脚本和样式表引用。 这些元标签提供了有关网页字符编码和视口设置的详细信息。 它们确保网页正确渲染，并调整其布局以适应不同的 屏幕尺寸。

接下来，我们包含外部 CSS 样式表。 `{% static 'css/bootstrap.min.css' %}` 和`{% static 'css/dashboard.css' %}` 标签指的是我们将在 Django 项目中创建的静态 CSS 文件。 这些 CSS 文件提供了样式规则和格式化指令，定义了网页上元素的视觉外观。

一旦完成头部 ，我们可以使用以下代码片段来构建页面的主体：

```py
 <body>
  <header class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0 shadow">
    <a class="navbar-brand col-md-3 col-lg-2 me-0 px-3 fs-6" href="#">Quiz App</a>
    <div class="navbar-nav">
    </div>
  </header>
  <div class="container-fluid">
    <div class="row">
      <nav id="sidebarMenu" class="col-md-3 col-lg-2 d-md-block bg-body-tertiary sidebar collapse">
      <div class="position-sticky pt-3 sidebar-sticky">
        <ul class="nav flex-column">
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="">
              <span data-feather="home" class="align-text-bottom"></span>
              Questions Generator
            </a>
          </li>
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="/history">
              <span data-feather="file" class="align-text-bottom"></span>
              My Quiz
            </a>
          </li>
        </ul>
      </div>
    </nav>
    </div>
  </div>
  </body>
</html>
```

在这里，主体 由一个 `<header>` 部分组成，其中包含一个导航栏。 导航栏包含测验应用的商标名称。 元素可以用来添加额外的导航项 （如果需要的话）。

在这个导航中，有一个 `<nav id="sidebarMenu">` 元素，它充当侧边菜单。 它包含可以按需定制的导航项列表。 每个导航项都由 `<li class="nav-item">` 及其关联的链接表示。 侧边栏内部我们将有两个主要链接： 。

+   **“问题生成器”标签页**：此标签页显示了 ChatGPT 测验生成工具 。

+   **“我的测验”标签页**：此标签页显示了用户迄今为止创建的所有测验，准备好 下载

现在我们已经完成了 `base.html`，我们可以添加样式文件。 为此，在你的 `quiz_project`中创建两个新的目录，分别命名为 `static/css` ，如下所示：

```py
 quiz_project/
├── quiz_app
├── quiz_project/
│   ├── static/
│   └── css/
│      ├── dashboard.css
│      └── bootstrap.min.css
└──  templates
```

我们不必自己编写样式文件，可以直接从 `css` 文件夹中下载 CSS 文件。 您可以在以下位置找到我们项目所需的每个 CSS 文件的链接：

+   `dashboard.css`: [https://getbootstrap.com/docs/5.3/examples/dashboard/dashboard.css](https://getbootstrap.com/docs/5.3/examples/dashboard/dashboard.css)

+   `bootstrap.min.css`: [https://getbootstrap.com/docs/5.3/dist/css/bootstrap.min.css](https://getbootstrap.com/docs/5.3/dist/css/bootstrap.min.css)

或者，您可以从我们的 GitHub 仓库 https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API访问所有项目文件。

现在，让我们看看如何将这些 HTML 和 CSS 文件集成到我们的 Django 项目中。通过修改`settings.py` `文件`，我们可以指定 Django 应该查找静态文件（包括 HTML 模板和 CSS 文件）的目录。这种配置将确保我们的 Django 应用程序可以访问和利用所需的模板和 CSS 样式，在整个应用程序中提供视觉上吸引人且统一的用户体验。

首先，要将 HTML 文件添加到你的项目中，你可以在`settings.py` `文件`中简单地引用`templates` `目录`。为此，找到`TEMPLATES` `字典`并编辑`DIRS` `键`，如下所示：

```py
 TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates', 'DIRS': [os.path.join(BASE_DIR, 'templates')], 'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

这用于构建 Django 项目内`templates` `目录`的绝对路径。《st c="24977">BASE_DIR` `指的是你的 Django 项目的基础目录，而`templates` `是存储你的模板文件的目录名称。` 

通过设置`DIRS` `键`，你告诉 Django 将`templates` `目录`作为搜索模板文件的位置之一。这允许你将模板文件与你的应用程序目录分开组织，并提供一个中心位置来存储你的`HTML 模板`。

我们还需要在`setting.py` `文件`的开头导入`os` `库`：

```py
import os from pathlib import Path
```

其次，要将所有 CSS 文件传递到你的 Django 项目中，你可以滚动到`settings.py` `文件`的底部并添加以下内容：

```py
 # Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/4.2/howto/static-files/
STATIC_URL = 'static/' STATICFILES_DIRS = [os.path.join(BASE_DIR, 'quiz_project/static')]
STATIC_ROOT = os.path.join(BASE_DIR, 'static') # Default primary key field type
# https://docs.djangoproject.com/en/4.2/ref/settings/#default-auto-field
DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
```

在这里，`STATICFILES_DIRS` `告诉 Django 在开发期间在哪里查找静态文件，而`STATIC_ROOT` `指定了静态文件将被收集以供部署的目录。` 这些设置确保了你的静态文件在开发和生产环境中都组织良好且可访问。这将使我们能够选择项目中的那些 CSS 文件。

本节解释了通过构建 `base.html` 模板，整合 CSS 资源如 Bootstrap，以及配置 `settings.py` 文件 来管理 Django 项目中的静态文件和模板的过程。 完成这些步骤后，您现在可以进入下一节运行您的 Django 应用程序。

## 运行您的 Django 应用程序

一旦您完成了上一节中涵盖的步骤，您就可以运行您的 Django 项目。 要运行 Django 应用程序，您需要再次访问您的终端或导航到您的 Django 项目的根目录， `quiz_project`，。 这是包含 `manage.py` 文件 的目录。 一旦您处于正确的目录，请执行以下命令：

```py
 $python manage.py runserver
```

此命令启动开发服务器，允许您在本地运行 Django 应用程序进行测试和 开发。

重要提示

一个需要注意的重要提示是，Django 的开发服务器有一个有用的功能 称为 **自动重新加载**。这意味着您可以在修改代码的同时让服务器运行，服务器将自动检测这些更改并应用它们，而无需手动重启。

一旦 Django 开发服务器启动，您可以通过打开网页浏览器并输入以下 URL 来访问您的应用程序： `http://127.0.0.1:8000/` (见 *图 5.4**.4*)。 默认情况下，Django 在本地主机 `127.0.0.1` 上运行并使用端口 `8000`。前面的 URL 将使您的浏览器指向 Django 应用程序的首页。

![Figure 5.4 – The quiz app initial page view](img/B22414_05_4.jpg)

图 5.4 – Quiz 应用初始页面视图

在我们的 Django 应用程序中，我们可以观察到三个不同的部分：

+   首先，在最上面，我们有一个标题区域，显示名称 **Quiz App** 在深色背景上。

+   在左侧，我们找到了一个导航栏，其中包含两个标签，我们将其整合到了 `base.html` 文件中 – **问题生成器** 和 **我的测验**。这些标签便于在应用程序的不同功能之间进行导航。 **问题生成器** 标签将用户引导到一个可以构建测验的区域，而 **我的测验** 标签则提供了一个下载测验的视图。

+   最后，在导航栏的右侧，我们有工作区，用户可以与之交互并探索 **我的测验** 和 **问题生成器** **功能。

这是您应用程序的初始视图，它展示了一个时尚现代的界面，具有用户友好的设计。 在下一节中，我们的注意力现在可以转向 开发应用程序的基本方面，这涉及到创建核心的 ChatGPT 测验 生成组件。

# 集成 ChatGPT 和 Django 用于测验生成

在本节中，我们将探讨使用 ChatGPT API 在 Django 应用程序中生成问题的激动人心的 任务。 这项强大的功能将允许你根据用户提供的文本输入创建交互式测验。 通过处理用户发出的 `POST` 请求并利用 ChatGPT API 的功能，你将能够动态生成有洞察力的提问，从而增强用户参与度和 知识获取。

该过程首先在你的 Django 应用程序中引入一个字段，用户可以在其中输入文本。 一旦用户通过点击提交按钮提交文本，应用程序就会利用 ChatGPT API 根据提供的文本生成相关且上下文准确的提问，魔法随之展开。 这些问题无缝地显示给用户，从而提供了一种互动和教育的体验。 通过自动化提问生成过程，你可以节省时间和精力，同时为用户提供有价值的内容。 因此，让我们深入探讨处理 `POST` 请求、构建用于提问创建的 ChatGPT API 函数以及最终以用户友好和直观的方式向用户展示生成的提问所涉及的步骤。

## `构建测验生成文本区域和提交按钮`

在对时间顺序略有偏离的情况下，我们将首先对`base.html` `文件进行修改，以包含我们问题生成功能所需的组件。` `具体来说，我们将添加一个文本区域，用户可以在其中输入他们希望输入的文本，以及一个提交按钮，该按钮将触发问题生成过程。`

`您可以在导航栏菜单代码（在` `</nav>` `标签下）下添加这些字段，如下所示：` ``

`base.html`

```py
 </nav> <main class="col-md-9 ms-sm-auto col-lg-10 px-md-4">{% block content %}<h1>Create a Quiz</h1><form method="post" action="">{% csrf_token %}<textarea name="text" rows="5" cols="50" placeholder="Enter some text..."></textarea><br><input type="submit" value="Generate Questions"></form>{% if questions %}<h2>Generated Questions:</h2>{{ questions|linebreaks }}{% endif %}{% endblock %}</main> </div>
  </div>
  </body>
</html>
```

`这代表了` `` `base.html` `文件中的部分，负责显示我们测验生成应用程序中与问题生成功能相关的用户界面元素。` ``

`在代码中，我们可以观察到 HTML 标签和 Django 模板语法的使用。` `` `<main>` `标签定义了主要内容区域。` `在` `<main>` `标签内部，我们有一个由` `{% block content %}` `和` `{% endblock %}` `` 定义的 Django 模板块，这允许动态内容插入。` `这将确保我们可以稍后连接此块与我们的 Django` `视图。`

`在这个块中，我们首先显示一个标题，` `{% csrf_token %}` `模板标签确保表单提交的安全性。`

`在表单内部，我们发现` `textarea` `，它作为输入字段，用户可以输入他们希望生成问题的文本。` `它指定了行和列属性来定义文本输入区域的大小。` `此外，还有占位文本，` `Enter some text...` `，为用户提供说明。` ``

`在` `textarea` `之后，我们有一个标题为` `生成问题` `的提交按钮，用户可以点击它来启动问题生成过程。` `当表单提交时，用户的输入将被发送到适当的视图进行进一步处理。` ``

在表单之后，有一个 `{% if questions %}` 语句，用于检查是否有生成的问答题。 如果有，则会出现一个标题， `{{ questions|linebreaks }}`。这种语法确保生成的问答题中的换行符在文本字段下正确显示。 。

最后，我们可以将 一个新的 样式表（稍后我们将构建它）添加到 `base.html` 文件中的 `head`部分：

```py
 <!--  &lt;!&ndash; Custom styles for this template &ndash;&gt;-->
  <link href="{% static 'css/dashboard.css' %}" rel="stylesheet" /> <link rel="stylesheet" type="text/css" href="{% static 'css/quiz_style.css' %}"> </head>
```

当你此时启动你的测验生成器时，你会看到虽然所有字段都是可见的，但它们的视觉呈现可能并不美观。 这是因为我们迄今为止只包含了 HTML 元素。 为了增强它们的视觉效果，我们需要引入一些 CSS 样式。 为此，你可以在 `quiz_style.css` 文件中创建一个新的 CSS 文件，该文件位于 `quiz_project/static/css` 目录下。 你可以在这里找到 CSS 文件的内容 ： [https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API/blob/main/Chapter05%20QuizApp/quiz_project/quiz_project/static/css/quiz_style.css](https://github.com/PacktPublishing/Building-AI-Applications-with-ChatGPT-API/blob/main/Chapter05%20QuizApp/quiz_project/quiz_project/static/css/quiz_style.css)。

在这里，我们定义了 `body`、 `container`和 `main` 元素的风格。 `body` 元素被设置为使用 Arial 字体家族，并具有白色背景。 `container` 类指定最大宽度为 `960px`，而包含我们的应用程序的`main` 元素，也具有阴影效果和 `5px`的边框半径。。

然后，我们可以定义 `textarea` 和 `submit` 按钮元素的样式。 `textarea` 元素被设置为具有其父容器宽度的 `70%` ，颜色为浅灰色。 代表提交按钮的`input[type="submit"]` 元素具有蓝色背景色，当鼠标悬停 在其上时，光标变为指针。

`响应式样式` 部分使用媒体查询在视口的最大宽度为 `767px` 或更小时应用特定样式。 在这种情况下，它减少了`.container` 和 `main` 元素的填充，将标题的字体大小减少到 `20px`，并调整了`textarea` 和 `input[type="submit"]` 元素的填充。 这些样式确保了测验生成应用程序在视觉上吸引人，并能很好地适应不同的屏幕尺寸，提供更好的 用户体验。

到目前为止，我们已经 成功 构建了我们应用程序测验生成功能的整个用户界面。 我们的下一步是集成 ChatGPT API 服务并实现必要的视图。 正确答案应该在哪里以及如何显示

## 使用 Django 创建 ChatGPT API 视图

现在我们已经 完成了前端开发，我们将 探索如何将这个强大的 ChatGPT API 集成到我们的 Django 视图中，使我们能够利用 ChatGPT 的能力来生成 测验问题。

要求 ChatGPT 以特定结构生成响应可能是一项具有挑战性的任务，因为我们需要定义非常具体的指令，说明我们的测验中的问题应该如何生成。 考虑到这一点，我们的 ChatGPT 提示应该满足以下要求： 以下要求：

+   一个变量应该传递我们想要创建问题的文本。 这将使我们的应用程序能够处理不同类型的 文本。

+   提示应该包含以下指令，以指定 以下：

    +   问题的数量 个

    +   提示应该包含以下指令： 以下：

    +   ChatGPT 应生成哪种类型的问题（多项选择、开放式回答等）

每次您想要设计一个 ChatGPT 提示并测试 API 响应是否符合您的提示定义时，创建这样的要求是一个好习惯。 基于您的 提示定义，检查 API 响应是否符合预期。

在您的 Django 项目中的`quiz_app` `文件夹内，您需要创建两个基本文件—— `config.py` `和 `services.py`。` `config.py` `文件作为存储您的 ChatGPT API 密钥的容器，允许您在应用程序中安全地存储和访问它。 此密钥对于建立与 ChatGPT API 服务的连接至关重要。 另一方面，`services.py` `文件在容纳必要的函数中扮演着关键角色，这些函数将使您能够在 Django 视图中与 ChatGPT API 交互。 这些函数将促进与 API 的通信，允许您根据 用户输入 动态生成测验问题：

services.py

```py
 from openai import OpenAI
from . import config
# API Token
client = OpenAI(
  api_key=config.API_KEY,
)
def generate_questions(text):
    # Define your prompt for generating questions
    prompt = f"Create a practice test with multiple choice questions on the following text:\n{text}\n\n" \
             f"Each question should be on a different line. Each question should have 4 possible answers. " \
             f"Under the possible answers we should have the correct answer." # Generate questions using the ChatGPT API
    response = client.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": f"{prompt}"}],
        max_tokens = 3500,
        stop = None,
        temperature = 0.7
    )
    # Extract the generated questions from the API response
    questions = response.choices[0].message.content
    return questions
```

config.py

```py
 API_KEY = "YOUR_API_KEY"
```

在这里，我们有一个 Python 函数，它使用 OpenAI 库与 ChatGPT API 交互，以生成测验问题。 首先，导入必要的依赖项，包括 OpenAI 库和本地的 `config` `模块，该模块包含 API 密钥。 使用导入的 `config.API_KEY` `值设置 API 密钥，以建立与 `ChatGPT API` 的连接。

The `generate_questions` 函数接受一个文本输入作为参数，该参数表示将从中生成问题的内容或上下文。 该函数通过将提供的文本整合到特定格式中，为 AI 模型创建基于给定内容的多个选择题做好准备。 该格式指导 AI 模型根据给定内容创建多项选择题。

正如您所看到的，给定 ChatGPT 提示的设计重点在于提供清晰的指令，以根据给定文本生成包含多项选择题的练习测试。 以下是设计背后的原因：

+   **清晰的指令**：提示语首先明确说明任务的目的是创建一个包含多项选择题的练习测试。 这有助于设定生成内容的上下文和期望。

+   `{text}` 占位符表示应在该位置插入给定的文本。 这允许用户提供基于该文本的问题。

+   **结构化格式**：提示指定每个问题应单独一行。 这有助于创建练习测试的结构化布局，使其更容易被人类和其他 Python 脚本 理解。

+   **多项选择题格式**：提示中说明，每个问题应有四个可能的答案。 这表明生成的题目应遵循在测试和测验中常用的多项选择题格式 。

+   **指示正确答案**：提示中提到，在可能的答案下，应提供正确答案。 这确保了生成的题目包括正确答案和选项，使学习者能够轻松地识别 正确答案。

使用 ChatGPT API，该函数 将带有准备好的提示的请求发送到 API。 API 响应包含生成的题目。 该函数从 API 响应中提取这些问题，并将它们作为 字符串变量 返回。

“ `问题` 变量现在可以在 `views.py` 文件中使用，以便将生成的题目传递到我们 `base.html` 前端文件中：

```py
 from django.shortcuts import render from .services import generate_questions def home(request): if request.method == 'POST':text = request.POST['text']questions = generate_questions(text)context = {'questions': questions}return render(request, 'base.html', context) return render(request, 'base.html')
```

在这里，我们升级了 `home` 函数，该函数处理主页的渲染和基于用户输入生成测验问题。 收到请求后， `view` 函数检查请求的 HTTP 方法。 如果是 `POST` 请求，表示用户已提交表单，该函数将继续从请求的 `POST` 数据中提取文本输入，使用 `text` 键。 实际上，提交是通过选择我们的问题 `text` 文件来启动的。

然后，提取的文本被传递给 generate_questions 函数，该函数我们之前在 `services.py` 文件中定义。 此函数利用 ChatGPT API 根据提供的文本生成测验问题。 生成的测验问题存储在 `questions` 变量中。

为了在网页上显示生成的测验问题，创建了一个名为 `context` 的字典，其中包含 `questions` 键和包含生成的测验问题的相应值。 此 `context` 字典作为参数传递给 `render` 函数，以及 `request` 对象和模板文件 `base.html`。渲染的页面将能够访问 `questions` 变量并适当地显示它。

通过在你的 Django 应用程序的 `views.py` 文件中实现此代码， `home` 视图函数将根据用户输入处理测验问题的生成，并在收到 `POST` 请求时显示生成的测验问题。

在测试你的 Django 应用程序并使用 ChatGPT API 之前，你需要确保已安装 `openai` 库。 OpenAI 库提供了与 ChatGPT API 交互所需的工具和功能。 要安装库，你可以在终端窗口中键入以下命令：

```py
 pip install openai
```

为了测试你的测验生成器应用程序，请按照以下步骤操作： 这些步骤：

1.  请确保你的 Django 开发服务器正在运行。 如果尚未运行，请在终端或命令提示符中导航到你的 Django 项目根目录，并执行以下命令： 以下命令：

    ```py
    http://127.0.0.1:8000/. This will direct you to the home page of your quiz generator application.
    ```

1.  在主页上，你会看到一个文本区域，你可以输入一些文本。 粘贴或输入你想要生成测验问题的文本（见 *图 5**.5*）。

![图 5.5 – 向测验生成应用程序添加文本](img/B22414_05_5.jpg)

图 5.5 – 向测验生成应用程序添加文本

1.  一旦你 输入了文本，点击 `POST` 请求到 服务器。

1.  服务器将处理请求并根据提供的文本使用 ChatGPT API 生成测验问题。 生成的测验问题将在文本区域下方显示（见 *图 5**.6*）。

![图 5.6 – 测验生成应用程序显示的问题](img/B22414_05_6.jpg)

图 5.6 – 测验生成应用程序显示的问题

遵循这些 步骤后，你现在可以测试你的测验生成应用程序 并查看它是如何根据文本输入生成问题的。 请随意尝试不同的输入来探索你应用程序的功能。

在本节中，我们探讨了 ChatGPT 和 Django 在测验生成中的集成。 我们涵盖了创建用于生成问题的用户界面，包括 CSS 样式以增强视觉效果，使用 Django 实现 ChatGPT API 视图，并通过输入文本并基于它生成问题来测试测验生成应用程序 。 现在，我们可以构建一个数据库来存储生成的测验，并允许用户下载它们，这将在下一节中介绍。 。

# 存储和下载生成的测验

在本节中，你 将学习如何为你的测验生成应用程序构建测验下载功能。 通过集成数据库并为下载测验创建一个专门的页面，你将赋予用户访问和使用他们生成的测验的能力。 此功能将通过提供一种无缝保存和检索测验的方式，增强整体用户体验，使用户能够轻松地回顾和分享他们生成的 内容。

我们将探讨使用 Django 的模型和数据库管理功能将生成的测验保存到数据库中的过程。 通过定义适当的数据库结构和实现必要的视图和模板，你将能够创建一个用户友好的页面，用户可以在其中浏览和下载他们生成的测验。 有了这个功能，你的测验生成应用程序将提供一种全面的解决方案来生成、存储和访问测验，进一步丰富用户的互动和教育体验。

## 将测验保存到 SQLite 数据库

构建测验下载功能的关键方面之一是能够将生成的测验保存到数据库中。 SQLite 为此提供了一个简单 且高效的解决方案。 SQLite 是一个轻量级、无服务器、自包含的数据库引擎，它需要最少的设置和配置。 它非常适合小型应用，并且与 Python 无缝集成 。 \

首先，您需要修改 `generate_questions()` 函数以包含数据库功能。 在此函数中，您可以包含代码将生成的测验问题和选项保存到 SQLite 数据库中。 通过使用 Django 的模型定义适当的表结构，您可以轻松地将测验数据映射到数据库中的相应字段。 。 \

除了修改 `generate_questions()` 函数外，您还需要创建一个单独的函数来初始化数据库。 此函数将处理创建必要表和建立数据库连接等任务。 要创建它，请打开您的 `services.py` 文件，您可以在您的 `API_KEY` 定义下编写初始化函数： \

```py
import sqlite3 from openai import OpenAI
from . import config
# API Token
client = OpenAI(
  api_key=config.API_KEY,
) initialize_database() function is responsible for setting up the SQLite database to store the generated quizzes. It begins by establishing a connection to the SQLite database named questions.db. Then, the cursor serves as a handle for executing SQL statements and fetching results from the database. It allows us to execute SQL commands and retrieve data from the database.
			The function then checks whether a table named `questions` exists in the database. This statement creates a table with three columns – `id`, `key`, and `value`. The `key` column will later take the first two words from the text you pass to ChatGPT and use them as the quiz name, while the `value` column will hold the complete quiz.
			After executing the table creation statement, the changes are committed to the database using `conn.commit()`. This ensures that the table creation is finalized and persisted in the database before the connection to the database is closed, using `conn.close()` to free up system resources and maintain good coding practices.
			Additionally, make sure to import the SQLite package to enable the utilization of the database functionalities.
			Now, we can modify our `generate_questions()` function to incorporate the database:

```

def generate_questions(text): initialize_database()# 连接到 SQLite 数据库conn = sqlite3.connect('questions.db')cursor = conn.cursor() # 定义用于生成问题的提示

    prompt = f"根据以下文本创建一个包含多项选择题的练习测试：\n{text}\n\n" \

            "每个问题应该单独占一行。每个问题应该有 4 个可能的答案。" \

            "在可能的答案下方，我们应该有正确答案。" # 使用 ChatGPT API 生成问题 \

    response = client.chat.completions.create( \

        model="gpt-3.5-turbo", \

        messages=[{"role": "user", "content": f"{prompt}"}], \

        max_tokens = 3500, \

        stop = None, \

        temperature = 0.7 \

    ) \

    # 从 API 响应中提取生成的问答 \

    questions = response.choices[0].message.content # Generate a unique key for the questionbase_key = ' '.join(text.split()[:2])key = base_keyindex = 1while key_exists(cursor, key):key = f"{base_key} {index}"index += 1# Insert the questions into the databasevalue = questionscursor.execute("INSERT INTO questions (key, value) VALUES (?, ?)", (key, value))conn.commit() return questions def key_exists(cursor, key):cursor.execute("SELECT COUNT(*) FROM questions WHERE key = ?", (key,))count = cursor.fetchone()[0]return count > 0

```py

			Here is how the database implementation works:

				1.  Firstly, the function establishes a connection to the SQLite database named `questions.db` using the `sqlite3.connect()` method, similar to the previous function.

    To ensure the uniqueness of each question, the function generates a unique key by combining the first two words of the input text. If a question with the same key already exists in the database, it appends a numerical index to the key until a unique key is obtained. We verify the key by using the `key_exists()` method.

				2.  The function then inserts the generated questions into the SQLite database using the `cursor.execute()` method. The key and value (`questions`) are passed as parameters to the SQL query. Once the insertion is complete, the function calls `conn.commit()` to save the changes made to the database.
				3.  The `key_exists()` function takes a cursor object and a key as parameters, executes a SQL query to count the number of rows with the given key in the `questions` table, and returns `True` if a key exists in the table, or `False` otherwise. This function provides a convenient way to check for the existence of a key in the database before inserting new data or performing other operations.
				4.  To fetch the database, we can create the `print_all_questions()` function. It retrieves all the rows from the `questions` table in the SQLite database and returns them as a result:

    ```

    def print_all_questions():

        initialize_database()

        conn = sqlite3.connect('questions.db')

        cursor = conn.cursor()

        # Retrieve all rows from the database

        cursor.execute("SELECT * FROM questions")

        rows = cursor.fetchall()

        return rows

    ```py

			First, the function executes a `SQL SELECT` statement using the `SELECT * FROM questions` query. This query selects all the columns from the `questions` table.
			After executing the `SELECT` statement, the function retrieves all the rows returned by the query. The `fetchall()` method returns the result as a list of tuples, where each tuple represents a row from the table. Later, we will use this function as a part of the download functionality to download our quizzes from the database.
			Building the download quiz view
			Now, it’s time to explore the process of integrating download quiz views into your Django application. Download quiz views are essential to enable users to access and obtain the quizzes generated by the quiz generation app. First, navigate to the `views.py` file and add the following code under the `home` function:

```

def history(request):

    return render(request, 'donwload.html')

data = print_all_questions()

class TestListView(TemplateView):

    template_name = 'download.html'

    def get_context_data(self, **kwargs):

        context = super().get_context_data(**kwargs)

        context['data'] = data

        return context

def download(request, test_id):

    test = next((t for t in data if t[0] == test_id), None)

    if test:

        header = test[1]

        questions = test[2]

        filename = f'test_{test_id}.txt'

        with open(filename, 'w') as f:

            f.write(questions)

        file_path = os.path.join(os.getcwd(), filename)

        response = HttpResponse(open(file_path, 'rb'), content_type='text/plain')

        response['Content-Disposition'] = f'attachment; filename="{header}.txt"'

        return response

    else:

        return HttpResponse("Test not found.")

```py

			The provided code demonstrates the additional functions included in the `views.py` file of our Django quiz generator. Let’s go through each function and its purpose:

				1.  The `history` function is a view function that renders the `download.html` template, which we will build in the next section. When a user visits the specified URL associated with this function, the template will be displayed, allowing them to view the history of quizzes available to download.
				2.  The `data` variable is assigned the value returned by the `print_all_questions()` function. This function retrieves all the rows from the database containing the generated quizzes. By storing the data in the `data` variable, it can be used later to populate the context for the `TestListView` view.
				3.  The `TestListView` class is a subclass of `TemplateView`, which provides a generic class-based view. It defines the template that will be used to display the downloaded quizzes – in this case, the `download.html` template. The `get_context_data()` method is overridden to add the `data` variable to the context, making it available in the template for rendering.
				4.  The `download()` function handles the download functionality for a specific quiz, identified by its `test_id`. It first searches for the quiz in the `data` variable using `test_id`. If the quiz is found, the header, questions, and filename are extracted. The questions are then written to a text file with the appropriate filename. The file path is determined, and an `HttpResponse` object is created with the file content and content type. The Content-Disposition header is set to specify the filename for the downloaded file. Finally, the response is returned to initiate the file download. If the quiz is not found, an `HttpResponse` object is returned, with a message indicating that the test was not found.

			At the beginning of the `views.py` file, you can add all the necessary imports for those functions to operate properly:

```

import os

from django.http import HttpResponse

from django.shortcuts import render

from django.views.generic import TemplateView

from .services import generate_questions, print_all_questions

```py

			As you can see, the `HttpResponse` and `render` functions are imported from the Django framework. These functions are essential to generate and return HTTP responses. The `HttpResponse` class allows you to create custom HTTP responses, while the `render` function is used to render HTML templates and return the result as an `HttpResponse` object. The `TemplateView` class will be used later as a class-based view that will render the templates for the download view.
			By adding these functions to your `views.py` file, you enable the rendering of the download `download.html` template. In the next section, we will create the download template and add all necessary URLs to make the **My Quiz** download page accessible to users.
			Designing the download template
			If you have a close look at your `base.html` file, you will see that there are two buttons displayed inside the navigation bar – `home` view, allowing them to generate quizzes using the ChatGPT API. In this section, we will focus on the **My Quiz** page. We will ensure that once the user clicks the **My Quiz** button, they are sent to the **127.0.0.1/history** page. This page will show all views related to our download functionality, which we built in the previous section.
			To do that, we need to create a new HTML file that will be rendered once the user clicks on the `base.html`, with the only difference being that it will run a different set of functions (the download functions) in its `main` area. You can create your `downloads.html` inside the `templates/` directory:

```

{% load static %}

<html lang="en" data-bs-theme="auto">

<head>

<script src="img/color-modes.js"></script>

<meta charset="utf-8">

<meta name="viewport" content="width=device-width, initial-scale=1">

<title>Quiz App</title>

<!-- Bootstrap CSS -->

<link href="{% static 'css/bootstrap.min.css' %}" rel="stylesheet" />

<!-- 为此模板自定义样式 -->

<link href="{% static 'css/dashboard.css' %}" rel="stylesheet" />

</head>

<body>

<header class="navbar navbar-dark sticky-top bg-dark flex-md-nowrap p-0 shadow">

    <a class="navbar-brand col-md-3 col-lg-2 me-0 px-3 fs-6" href="#">Quiz App</a>

    <div class="navbar-nav">

    </div>

</header>

<div class="container-fluid">

    <div class="row">

    <nav id="sidebarMenu" class="col-md-3 col-lg-2 d-md-block bg-body-tertiary sidebar collapse">

    <div class="position-sticky pt-3 sidebar-sticky">

        <ul class="nav flex-column">

        <li class="nav-item">

            <a class="nav-link active" aria-current="page" href="/">

            <span data-feather="home" class="align-text-bottom"></span>

            Questions Generator

            </a>

        </li>

        <li class="nav-item">

            <a class="nav-link active" aria-current="page" href="/history">

            <span data-feather="file" class="align-text-bottom"></span>

            我的测验

            </a>

        </li>

        </ul>

    </div>

    </nav>

```py

			After the navigation bar, where our working area is located, we can create a section where users can download quizzes. We can generate a list of quiz download links based on the data provided, displaying the quiz headers as clickable links for users to access and download the quizzes.
			Inside the `<main>` element, there is an `<h1>` heading tag that displays the text `Download Quiz`. This heading provides a clear title for the section, indicating its purpose to the users:

```

<main class="col-md-9 ms-sm-auto col-lg-10 px-md-4"><h1>下载测验</h1><ul>{% for test in data %}<li><a href="/download/{{ test.0 }}" class="test-header">{{ test.1 }}</a></li>{% endfor %}</ul></main> </div>

</div>

</body>

</html>

```py

			Below the heading, there is an `<ul>` element that represents an unordered list. Within this list, there is a loop construct using Django’s template syntax, denoted by `{% for test in data %}` and `{% endfor %}`. This loop iterates over the `data` variable, which contains a list of quizzes or test data.
			For each iteration of the loop, a new list item, `<li>`, is generated. Inside the list item, there is an anchor (`<a>`) tag that serves as a hyperlink. The `href` attribute of the anchor tag is dynamically generated using Django’s template syntax and the `test` variable. This link points to the `/download/` URL, followed by the test.
			This section of `download.html` generates a main content area with a heading and a list of quiz download links. The loop ensures that each quiz in the `data` variable is represented as a separate list item with an appropriate hyperlink and display text.
			As a final step to set up the `urls.py` files. Those files are responsible for routing requests to the appropriate views. You need to define a URL pattern that maps to the history view, which will display the `urls.py` files, users will be able to access the **My Quiz** page and view their downloaded quizzes:
			quiz_project/urls.py

```

from django.contrib import admin

from django.urls import include, path

urlpatterns = [

    path('admin/', admin.site.urls),

    path('', include('quiz_app.urls')),

    path('history/', include('quiz_app.urls')),

]

```py

			quiz_app/urls.py

```

from django.urls import path

from . import views

from .views import TestListView, download

urlpatterns = [

    path('', views.home, name='home'),

    path('history/', TestListView.as_view(), name='test_list'),

    path('download/<int:test_id>', download, name='test_download'),

]

```py

			In the `quiz_project/urls.py` file, we define the URL patterns for the entire project. The `history/` path is mapped to the `quiz_app.urls` module, which contains the URL patterns specific to the quiz application. This configuration allows users to access the **My** **Quiz** page.
			In the `quiz_app/urls.py` file, we define the URL patterns for the quiz application specifically. We add the `history/` path that is mapped to the `TestListView` view class, which is responsible for displaying the `download/<int:test_id>` path is mapped to the download view function, which handles the downloading of specific quizzes that the user selects.
			By configuring the URL patterns in these files, you enable users to navigate to different pages of your quiz app, such as the **Question Generator** page or the **My Quiz** page, from which the download page can be activated by accessing the corresponding URLs.
			Configuring the URL files marks the final stage in the creation of your application. To test the Django quiz app, you can run it from the terminal using the following command:

```

`$python manage.py runserver`

```py

			Once the application is up and running, you can access it through your web browser. Start by navigating to the **Question Generator** page, where you can generate a few quizzes by providing the necessary inputs. After generating the quizzes, you can reboot the application by restarting the server.
			Next, navigate to the **My Quiz** tab, which should be available in the navigation menu. Clicking on this tab will take you to the page where you will see the names of all the quizzes you previously generated. This list will be populated based on the quizzes stored in the database. It provides a convenient overview of the quizzes you have created (see *Figure 5**.7*).
			![Figure 5.7 – Downloading quizzes from the My Quiz page](img/B22414_05_7.jpg)

			Figure 5.7 – Downloading quizzes from the My Quiz page
			To download a specific quiz as a text file, simply click on the desired quiz from the list. This action will trigger the download functionality implemented in the application. The quiz will be downloaded as a text file, which you can save to your local machine. This allows you to access and review the quiz offline, print it, or share it with others if needed.
			This is how we integrated the download quiz views into our Django application. We added functions to the `views.py` file, such as `history` to render the download `TestListView` to display downloaded quizzes, and `download` to handle quiz downloads. These functions enable users to access and obtain quizzes generated by the quiz generation app. We also created the `downloads.html` template, modified from `base.html`, to display the download functionality. The `urls.py` files were modified to define URL patterns, allowing access to the **My Quiz** page and quizzes to be downloaded. To test the application, we generated quizzes using the **Question Generator** page, and then we navigated to the **My Quiz** page to view and download our quizzes.
			Summary
			In this chapter, the focus was on building a Django project for a quiz generation application. We provided a step-by-step guide to set up and structure the project, including the installation of Django and important aspects such as database initialization, running a development server, and understanding the flow of views and URLs in Django. We also explored the concept of templates and demonstrated how to incorporate external CSS resources such as the Bootstrap framework.
			We also explained how to integrate ChatGPT and Django for quiz generation. We illustrated the process of adding a text input field and a submit button to the application’s interface, enabling users to generate relevant questions using the ChatGPT API. We also discussed how to test the quiz generator application and outlined the next steps, which involve building a database to store the generated questions. You learned how to store the generated quizzes in a SQLite database, using Django’s models and database management capabilities, and download the stored quizzes.
			This project was a comprehensive guide to building a Django-based quiz generation application, integrating ChatGPT for question generation, and implementing the functionality to download generated quizzes. You gained valuable experience in creating your own interactive applications with Django.
			In the next chapter, *Chapter 6*, *Language Translation Desktop App with the ChatGPT API and Microsoft Word*, you will learn how to create a language translation desktop app using Python and the ChatGPT API. We will explain how to integrate the ChatGPT API with Microsoft Word to translate Word documents into different languages. The chapter will cover the creation of a user interface using the *tkinter* library, allowing users to choose a target language and view real-time translations. It will also provide information on implementing language selection and file-browsing features, emphasizing the use of the powerful gpt-3.5-turbo language model for accurate translations.

```

# 第三部分：用于桌面应用程序开发的 ChatGPT、DALL-E 和 Whisper API

在本部分，您的注意力将转向桌面应用程序的创建，扩展您在一系列人工智能 API 方面的专业知识。 您将通过无缝集成 ChatGPT 和 DALL-E API 来探索人工智能艺术，为您的应用程序注入吸引人的艺术能力。 此外，您将通过使用 Whisper API 探索语言翻译和转录领域。 在本部分中，您将接触到各种桌面应用程序框架，如 Tkinter、PyQt 和 Microsoft Office API，使您能够构建强大且功能丰富的应用程序。

本部分包含以下章节： ：

+   *第六章*， *使用 ChatGPT API 和 Microsoft Word 构建语言翻译桌面应用程序*

+   *第七章*， *使用 PyQt 和 ChatGPT API 构建 Outlook 电子邮件回复生成器*

+   *第八章**，使用 PyQt 和 ChatGPT API 构建论文生成工具*

+   *第九章**，集成 ChatGPT 和 DALL-E API：构建端到端演示文稿生成器*

+   *第十章**，使用 Whisper API 进行语音识别和语音合成*
