GitHub行动--2021年的最佳CI
====================

> id: c1fe3b77-a506-4f20-bf1e-90ee82817c7d
> slug:
> 	cs: github-actions-nejlepsi-ci-pro-rok-2021
> 	zh: github-xing-dong-2021nian-de-zui-jiaci
> 
> perex:
> 	- 'Protože už nějaký čas vyvíjíte webové aplikace, určitě jste si všimli, že se pro vás celá řada věcí rutinně opakuje, i když by nemusela. Velmi často jde o technickou správu projektů, verzování souborů, automatická kontrola kódu, zpracování různých dat, nebo třeba deploy na server a bezpečnostní věci kolem toho.'
> 	- 由于你已经开发了一段时间的网络应用程序，你可能已经注意到，许多事情对你来说是例行公事式的重复，尽管它们不应该是这样。很多时候，它是技术项目管理、文件版本管理、自动代码审查、各种数据处理，或者也许是部署到服务器和围绕这个的安全问题。
> 
> publicationDate: '2021-02-07 15:46:39'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: d3d151927a02dde744d1202449c904dc

由于你已经开发了一段时间的网络应用程序，你可能已经注意到，许多事情对你来说是例行公事式的重复，尽管它们不应该是这样。很多时候，它是技术项目管理、文件版本管理、自动代码审查、各种数据处理，或者可能是部署到服务器和围绕这些的安全问题。

在公司的咨询中，我经常遇到这样的问题，即预防被大大低估了。原因往往是开发人员认为有些事情非常具有挑战性，会给他们增加工作。但事实是，你通常只需要设置一次整个设置，然后就可以获得长期的收益。

什么是CI（持续集成）？
----------

CI/CD是一种工具，可以将具有类似基础并在项目中不断重复的常规任务自动化。CI最常见的用途是运行自动测试、代码审查、自动部署（将应用程序部署到网络服务器上）、项目管理，或许还有安全审计。

把CI想象成一个虚拟的操作系统，执行预定义的命令到终端或运行特定的程序。CI的输出要么是成功，要么是失败，直接显示在项目中，但也发送给管理员，管理员可以对此作出反应。一个好的做法是在特定的事件（例如，提交到版本库、收到合并请求/拉取请求、标签/版本/发布，或 cron timeloop 运行）之后运行 CI 工作。

CI具体如何工作
------------

CI的基础是一个配置文件，我们在其中定义其行为和对事件的响应。在GitHub的情况下，你只需要创建一个辅助目录`.github/workflows`并在里面创建任何带有`.yml`扩展名的文件。GitHub 会在每次提交时解析它，并根据需要执行它。

让我们想象一下，我们要定义一个新的配置文件，并有一个特定的任务。由于这是一个将由GitHub或虚拟机上的另一个环境直接运行的任务，我们需要定义环境的基本内容，包括。

- 任务的名称（例如，测试的名称）。
- 触发事件（根据哪个事件来触发任务，例如，每次推送到版本库或收到拉动请求的时候）
- 任务本身的定义（可以有许多任务并行运行）。

在工作的情况下，我们进一步定义。

- 工作名称
- 运行环境（通常是指操作系统的名称）。
- 建造容器的步骤

上述容器已经是为**项目的完整docker化做了第一个准备。CI的基本使用比较简单，你根本不需要了解Docker，只需要知道终端的命令。

作为任务中具体步骤的一部分，我们需要运行各个命令，以建立我们刚刚安装的操作系统的安装。所以在PHP的情况下，重要的是只安装PHP语言（并指定版本），克隆项目库到运行器，安装项目依赖（最常见的是[Composer]（/composer）），并运行测试本身。

为什么要使用 GitHub Actions (CI)？
--------

在IT界有一个普遍的迷信，即微软是制造低质量东西的邪恶公司。然而，就GitHub Actions而言，这完全不是事实，因为客观地说，他们拥有世界上最好的CI平台。

GitHub CI的根本优势在于符号的简单和优雅。只需几行字，你就可以配置你自己的测试环境并自动管理它，即使没有事先的知识。同时，我认为处理特定任务的速度是一个巨大的优势。例如，一个关于 codestyle（代码格式）和 PhpStan（检查数据类型、逻辑和一般正确性）的测试，GitHub Actions 可以在 50 秒内对普通项目进行评估，而以 GitLab 为例，评估相同的测试需要近 8 分钟像Travis这样的其他解决方案相对昂贵，而且大多数开发者无论如何都不会欣赏其特殊功能的好处。

准备的行动
-----

GitHub有一个庞大的数据库，里面有现成的动作和包，你可以马上使用。这些通常是操作系统、编程语言或社区的库。

你可以在[市场](https://github.com/marketplace?type=actions)中找到一个行动数据库，例如我最喜欢的行动是[通过Twillio在失败的情况下发送短信](https://github.com/marketplace/actions/twilio-sms) 。

完成的例子
------

非常多的例子[我在自己的资料库中发布](https://github.com/baraja-core)，你可以透明地看到我使用的具体配置。

例如，在2021年2月，我在一个项目中使用这种配置进行代码风格检查。

```yaml
name: Coding Style

on:
  push:
    branches:
      - master
  pull_request:
    types: [ assigned, opened, synchronize, reopened ]
  schedule:
    - cron: '1 * * * *'

jobs:
  nette_cc:
    name: Nette Code Checker
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 7.4
          coverage: none

      - run: composer create-project nette/code-checker temp/code-checker ^3 --no-progress
      - run: php temp/code-checker/code-checker --strict-types --no-progress

  nette_cs:
    name: Nette Coding Standard
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 8.0
          coverage: none

      - run: composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
      - run: php temp/coding-standard/ecs check src
```

这个配置文件安装了最新的Ubuntu（`runs-on: ubuntu-latest`），克隆了项目库（`uses: actions/checkout@v2`），安装了PHP（`uses: shivammathur/setup-php@v2`）7.4版本（`php-version: 7.4`），安装了代码检查程序包，然后通过PHP运行作业。

对于经验不足的用户，还有一些注意事项。

- CI每次运行时都会启动一个具有完整操作系统的虚拟机，它可以用来在终端调用命令，就像你的服务器一样，例如
- 为了检查测试结果，使用了Linux自己定义的所谓返回代码。具体来说，当一个程序返回`0'（零）时，意味着成功，任何其他数字都意味着失败。例如，shell脚本的工作方式是一样的
- 当我们想写一个自定义的测试或更复杂的逻辑时，我们可以使用PHP，例如，作为CLI作业（`php file.php`命令）运行，它将运行并可以做任何它有权利做的事情（与文件工作，通过互联网通信，输出到屏幕，...）。
- 当CI运行时，所有的输出都记录在屏幕上，可以直接在网络浏览器中查看。
- CI不是交互式的，即使终端可以和用户一起运行交互式操作，但CI不支持这样做，必须设计成一个有时启动、有时结束的任务。
- 为CI容器的运行定义了1小时的超时，如果在这段时间内没有处理好所有的事情（例如，程序停滞），整个系统将被强行终止，并报告一个错误
- GitHub 可以通过 cron 自动运行 CI 工作，但很多时候它并没有在正确的时间运行，而是延迟运行。
- 你也可以将所有的CI逻辑包裹在一个Docker容器中，并在所有机器上或服务器上本地运行，例如
- 如果你需要访问秘密信息（如数据库密码），有一个选项可以直接在GitHub中定义秘密变量，并且CI在运行时可以使用它们。
- 部署到网络服务器最好通过SSH进行
- CI工作的总运行时间是有限制的，通常是每月2千分钟（很多时候这对你来说是足够的，我从来没有用完过，因为一个工作最多运行一分钟），或者你可以付费增加时间
- 你也可以在你的服务器上托管CI运行器，绕过分钟限制，但很可能你的服务器不会快很多，因为微软在托管GitHub Actions的Azure上投入了大量资金，它在非常强大的服务器上运行。
- 很多时候，为CI安装特定的软件包是很方便的（通常是PhpStan和各种安全功能），所以把它们放在`require-dev`文件的`composer.json`部分，这样它们就不会被作为强制依赖项安装在特定的项目中。

GitHub应用程序和机器人
-----

与其他存储库主机不同，GitHub支持所谓的GitHub应用程序和自动化机器人。这是一个现成的机器人大数据库，可以对你的项目进行自动操作（即使没有CI），当它们遇到一些事情时，它们会，例如，提交一个问题或发送一个修复的拉动请求。

我个人使用并向你推荐它。

- [Dependabot](https://dependabot.com) - 在Composer中自动进行依赖性检查，例如，当你使用的软件包的新版本出来并且兼容时，它会自动发送一个拉动请求。
- [Codecov](https://github.com/marketplace/codecov) - 用自动测试检查代码覆盖率，并绘制出代码中哪些部分还没有被覆盖的图形
- [Snyk](https://github.com/marketplace/snyk) - 自动检查安全漏洞
- [Imgbot](https://github.com/apps/imgbot) - 自动优化图像数据大小

在[Marketplace](https://github.com/marketplace?type=apps)可以找到许多其他应用程序。

更多方便的技巧和窍门
-----

我越来越喜欢在GitHub中使用自动化任务。

我在我所有的存储库中都设置了自动检查，所以当我提交任何提交时（或社区中的任何其他人），我都会得到实时反馈，了解代码质量（代码风格和PhpStan）、安全性等是否被违反。

我有一些测试每小时自动运行。例如，如果有一个安全漏洞或CVE，我最迟在一个小时内就会知道，甚至是具体项目的水平，以及具体发生了什么。

随着时间的推移，在测试了不同的配置后，我得出结论，我认为以下对Composer的依赖性是理想的最低设置。

- `phpstan/phpstan` - 检查代码的正确性和逻辑。
- `tracy/tracy` - 高水平的错误报告和记录
- `phpstan/phpstan-nette`-Nette中的Phpstan扩展和特长
- `spaze/phpstan-disallowed-calls` - Michal Spacek的一个出色的库，处理代码中（不）使用危险的东西（从被遗忘的变量转储到作为shell命令运行用户字符串的能力）。
- `roave/security-advisories` - 检查软件包的不安全版本的安装（如果在特定的软件包中发现了安全漏洞或发布了CVE，这个软件包将防止安装损坏的版本）。

理想情况下，你应该将基本安全设置为至少每8小时自动运行一次，并将错误发送到你的电子邮件。因为每当项目安装失败或发现新的漏洞时，我都想尽快知道并立即做出反应。

请记住，一切都自动工作，所以你总是可以确定，即使你使用复杂的程序来检查安全和其他事情，也不会花费你任何额外的努力，你只需要一个执行良好的初始配置。

因为在预防和自动化方面有巨大的潜力!
