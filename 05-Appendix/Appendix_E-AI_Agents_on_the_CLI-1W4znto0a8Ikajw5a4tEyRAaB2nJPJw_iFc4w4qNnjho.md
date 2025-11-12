# 附录 E - CLI 上的 AI Agent

## 引言

开发者的命令行，长期以来一直是精确、命令式命令的堡垒，正在经历深刻的变革。它正在从简单的 shell 演变为由新型工具驱动的智能协作工作空间：AI Agent 命令行界面（CLI）。这些 agent 不仅仅执行命令；它们理解自然语言，维护关于整个代码库的上下文，并可以执行复杂的多步骤任务，自动化开发生命周期的显著部分。

本指南深入探讨了这个新兴领域的四个主要参与者，探索它们的独特优势、理想用例和不同理念，以帮助您确定最适合您工作流程的工具。需要注意的是，为特定工具提供的许多示例用例通常也可以由其他 agent 完成。这些工具之间的关键区别通常在于它们为给定任务能够实现的结果的质量、效率和细微差别。有专门的基准测试旨在衡量这些能力，这将在以下部分中讨论。

## Claude CLI (Claude Code)

Anthropic 的 Claude CLI 被设计为高级编码 agent，对项目架构有深入、全面的理解。其核心优势在于其"agentic"特性，使其能够为复杂的多步骤任务创建代码库的心理模型。交互是高度对话式的，类似于结对编程会话，在执行之前解释其计划。这使得它非常适合从事涉及重大重构或实现具有广泛架构影响的功能的大型项目的专业开发人员。

**示例用例：**

1. **大规模重构：** 您可以指示它："我们当前的用户身份验证依赖于会话 cookie。重构整个代码库以使用无状态 JWT，更新登录/注销端点、中间件和前端令牌处理。" Claude 将读取所有相关文件并执行协调的更改。
2. **API 集成：** 在提供了新天气服务的 OpenAPI 规范后，您可以说："集成这个新的天气 API。创建一个服务模块来处理 API 调用，添加一个新组件来显示天气，并更新主仪表板以包含它。"
3. **文档生成：** 指向一个代码文档不完整的复杂模块，您可以问："分析 `./src/utils/data_processing.js` 文件。为每个函数生成全面的 TSDoc 注释，解释其目的、参数和返回值。"

Claude CLI 作为专门的编码助手，具有用于核心开发任务的固有工具，包括文件摄取、代码结构分析和编辑生成。它与 Git 的深度集成促进了直接的分支和提交管理。该 agent 的可扩展性通过多工具控制协议（MCP）进行中介，使用户能够定义和集成自定义工具。这允许与私有 API、数据库查询以及执行项目特定脚本进行交互。这种架构将开发者定位为 agent 功能范围的仲裁者，有效地将 Claude 描述为由用户定义工具增强的推理引擎。

## Gemini CLI

Google 的 Gemini CLI 是一个多功能的开源 AI agent，专为功能和可访问性而设计。它以先进的 Gemini 2.5 Pro 模型、巨大的上下文窗口和多模态能力（处理图像和文本）而脱颖而出。其开源性质、慷慨的免费层级和"推理与行动"（ReAct）循环使其成为从爱好者到企业开发者的广泛受众的透明、可控且优秀的全能工具，特别是那些在 Google Cloud 生态系统内的开发者。

**示例用例：**

1. **多模态开发：** 您提供设计文件中 Web 组件的截图（gemini describe component.png）并指示它："编写 HTML 和 CSS 代码以构建一个看起来完全像这样的 React 组件。确保它是响应式的。"
2. **云资源管理：** 使用其内置的 Google Cloud 集成，您可以命令："找到生产项目中运行版本低于 1.28 的所有 GKE 集群，并生成一个 gcloud 命令来逐个升级它们。"
3. **企业工具集成（通过 MCP）：** 开发者向 Gemini 提供一个名为 get-employee-details 的自定义工具，该工具连接到公司内部的 HR API。提示是："为我们的新员工起草欢迎文档。首先，使用 get-employee-details --id=E90210 工具获取他们的姓名和团队，然后用该信息填充 welcome_template.md。"
4. **大规模重构：** 开发者需要重构大型 Java 代码库，用新的结构化日志记录框架替换已弃用的日志记录库。他们可以使用 Gemini，提示如下：读取 'src/main/java' 目录中的所有 *.java 文件。对于每个文件，将所有 'org.apache.log4j' 导入及其 'Logger' 类的实例替换为 'org.slf4j.Logger' 和 'LoggerFactory'。重写记录器实例化和所有 .info()、.debug() 和 .error() 调用，以使用带有键值对的新结构化格式。

Gemini CLI 配备了一套内置工具，使其能够与其环境交互。这些包括用于文件系统操作的工具（如读取和写入）、用于运行命令的 shell 工具，以及用于通过网络获取和搜索访问互联网的工具。为了更广泛的上下文，它使用专门的工具来一次读取多个文件，并使用记忆工具来保存信息以供以后的会话使用。此功能建立在安全的基础上：沙箱隔离模型的操作以防止风险，而 MCP 服务器充当桥梁，使 Gemini 能够安全地连接到您的本地环境或其他 API。

## Aider

Aider 是一个开源 AI 编码助手，通过直接处理您的文件并将更改提交到 Git 来充当真正的结对程序员。其定义特性是其直接性；它应用编辑、运行测试以验证它们，并自动提交每个成功的更改。作为模型无关的工具，它使用户完全控制成本和能力。其以 Git 为中心的工作流程使其非常适合重视效率、控制和所有代码修改的透明、可审计跟踪的开发人员。

**示例用例：**

1. **测试驱动开发（TDD）：** 开发者可以说："创建一个测试函数来计算数字的阶乘。" 在 Aider 编写测试并且测试失败后，下一个提示是："现在，编写代码以使测试通过。" Aider 实现该函数并再次运行测试以确认。
2. **精确的错误修复：** 给定错误报告，您可以指示 Aider："billing.py 中的 `calculate_total` 函数在闰年失败。将文件添加到上下文，修复错误，并针对现有测试套件验证您的修复。"
3. **依赖项更新：** 您可以指示它："我们的项目使用过时版本的 'requests' 库。请浏览所有 Python 文件，更新导入语句和任何已弃用的函数调用以与最新版本兼容，然后更新 requirements.txt。"

## GitHub Copilot CLI

GitHub Copilot CLI 将流行的 AI 结对程序员扩展到终端，其主要优势在于它与 GitHub 生态系统的原生深度集成。它理解 *GitHub 内*项目的上下文。其 agent 功能允许将 GitHub issue 分配给它，处理修复，并提交 pull request 以供人工审查。

**示例用例：**

1. **自动化 Issue 解决：** 管理者将错误票证（例如，"Issue #123：修复分页中的差一错误"）分配给 Copilot agent。然后，该 agent 检出新分支，编写代码，并提交引用该 issue 的 pull request，所有这些都无需手动开发者干预。
2. **仓库感知问答：** 团队中的新开发者可以问："在这个仓库中，数据库连接逻辑定义在哪里，它需要哪些环境变量？" Copilot CLI 使用其对整个仓库的了解来提供带有文件路径的精确答案。
3. **Shell 命令助手：** 当不确定复杂的 shell 命令时，用户可以问：gh? 找到所有大于 50MB 的文件，压缩它们，并将它们放在归档文件夹中。Copilot 将生成执行该任务所需的确切 shell 命令。

## Terminal-Bench：CLI 中 AI Agent 的基准测试

Terminal-Bench 是一个新颖的评估框架，旨在评估 AI agent 在命令行界面中执行复杂任务的熟练程度。终端被确定为 AI agent 操作的最佳环境，因为其基于文本、沙箱化的性质。初始版本 Terminal-Bench-Core-v0 包含 80 个手动策划的任务，涵盖科学工作流程和数据分析等领域。为了确保公平比较，开发了 Terminus（一个简约的 agent）作为各种语言模型的标准化测试平台。该框架设计为可扩展，允许通过容器化或直接连接集成各种 agent。未来的开发包括启用大规模并行评估和合并已建立的基准。该项目鼓励开源贡献以扩展任务和协作框架增强。

## 结论

这些强大的 AI 命令行 agent 的出现标志着软件开发的根本性转变，将终端转变为动态和协作的环境。正如我们所看到的，没有单一的"最佳"工具；相反，正在形成一个充满活力的生态系统，每个 agent 都提供专门的优势。理想的选择完全取决于开发者的需求：Claude 用于复杂的架构任务，Gemini 用于多功能和多模态问题解决，Aider 用于以 Git 为中心的直接代码编辑，GitHub Copilot 用于无缝集成到 GitHub 工作流程中。随着这些工具的不断发展，熟练使用它们将成为一项基本技能，从根本上改变开发者构建、调试和管理软件的方式。

## 参考文献

1. Anthropic. *Claude*. [https://docs.anthropic.com/en/docs/claude-code/cli-reference](https://docs.anthropic.com/en/docs/claude-code/cli-reference)
2. Google Gemini Cli [https://github.com/google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli)
3. Aider. [https://aider.chat/](https://aider.chat/)
4. GitHub *Copilot CLI* [https://docs.github.com/en/copilot/github-copilot-enterprise/copilot-cli](https://docs.github.com/en/copilot/github-copilot-enterprise/copilot-cli)
5. Terminal Bench: [https://www.tbench.ai/](https://www.tbench.ai/)
