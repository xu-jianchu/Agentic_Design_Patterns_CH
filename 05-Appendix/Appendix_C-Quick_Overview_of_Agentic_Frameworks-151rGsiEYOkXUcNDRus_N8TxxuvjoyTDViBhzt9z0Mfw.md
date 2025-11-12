# 附录 C - 智能体框架快速概览

## LangChain

LangChain 是一个用于开发由 LLM 驱动的应用程序的框架。其核心优势在于其 LangChain 表达式语言（LCEL），它允许您将组件"管道"连接成一个链。这创建了一个清晰的、线性的序列，其中一步的输出成为下一步的输入。它构建用于有向无环图（DAG）的工作流，意味着过程单向流动，没有循环。

将其用于：

* 简单 RAG：检索文档、创建提示、从 LLM 获取答案。  
* 摘要：获取用户文本，将其输入到摘要提示，并返回输出。  
* 提取：从文本块中提取结构化数据（如 JSON）。

Python

```python
# A simple LCEL chain conceptually # (This is not runnable code, just illustrates the flow) 
chain = prompt | model | output_parse
```

## LangGraph

LangGraph 是一个构建在 LangChain 之上的库，用于处理更高级的智能体系统。它允许您将工作流定义为具有节点（函数或 LCEL 链）和边（条件逻辑）的图。其主要优势是创建循环的能力，允许应用程序循环、重试或以灵活的顺序调用工具，直到任务完成。它显式管理应用程序状态，该状态在节点之间传递并在整个过程中更新。

将其用于：

* 多智能体系统：监督智能体将任务路由到专门的 worker 智能体，可能循环直到达到目标。  
* 规划和执行智能体：智能体创建计划、执行步骤，然后循环返回以根据结果更新计划。  
* 人在回路：图可以等待人工输入，然后再决定转到哪个节点。

| 功能 | LangChain | LangGraph |
| :---- | :---- | :---- |
| 核心抽象 | 链（使用 LCEL） | 节点图 |
| 工作流类型 | 线性（有向无环图） | 循环（带循环的图） |
| 状态管理 | 通常每次运行无状态 | 显式和持久状态对象 |
| 主要用途 | 简单、可预测的序列 | 复杂、动态、有状态的智能体 |

### 您应该使用哪一个？

* 当您的应用程序具有清晰、可预测和线性的步骤流时，选择 LangChain。如果您可以将过程从 A 到 B 到 C 定义为不需要循环返回，则使用 LCEL 的 LangChain 是完美的工具。  
* 当您需要应用程序进行推理、规划或在循环中操作时，选择 LangGraph。如果您的智能体需要使用工具、反思结果，并可能以不同方式重试，您需要 LangGraph 的循环和有状态特性。

```python
# Graph state
class State(TypedDict):
    topic: str
    joke: str
    story: str
    poem: str
    combined_output: str


# Nodes
def call_llm_1(state: State):
    """First LLM call to generate initial joke"""
    msg = llm.invoke(f"Write a joke about {state['topic']}")
    return {"joke": msg.content}


def call_llm_2(state: State):
    """Second LLM call to generate story"""
    msg = llm.invoke(f"Write a story about {state['topic']}")
    return {"story": msg.content}


def call_llm_3(state: State):
    """Third LLM call to generate poem"""
    msg = llm.invoke(f"Write a poem about {state['topic']}")
    return {"poem": msg.content}


def aggregator(state: State):
    """Combine the joke and story into a single output"""
    combined = f"Here's a story, joke, and poem about {state['topic']}!\n\n"
    combined += f"STORY:\n{state['story']}\n\n"
    combined += f"JOKE:\n{state['joke']}\n\n"
    combined += f"POEM:\n{state['poem']}"
    return {"combined_output": combined}


# Build workflow
parallel_builder = StateGraph(State)

# Add nodes
parallel_builder.add_node("call_llm_1", call_llm_1)
parallel_builder.add_node("call_llm_2", call_llm_2)
parallel_builder.add_node("call_llm_3", call_llm_3)
parallel_builder.add_node("aggregator", aggregator)

# Add edges to connect nodes
parallel_builder.add_edge(START, "call_llm_1")
parallel_builder.add_edge(START, "call_llm_2")
parallel_builder.add_edge(START, "call_llm_3")
parallel_builder.add_edge("call_llm_1", "aggregator")
parallel_builder.add_edge("call_llm_2", "aggregator")
parallel_builder.add_edge("call_llm_3", "aggregator")
parallel_builder.add_edge("aggregator", END)

parallel_workflow = parallel_builder.compile()

# Show workflow
display(Image(parallel_workflow.get_graph().draw_mermaid_png()))

# Invoke
state = parallel_workflow.invoke({"topic": "cats"})
print(state["combined_output"])
```

此代码定义并运行一个并行操作的 LangGraph 工作流。其主要目的是同时生成关于给定主题的笑话、故事和诗歌，然后将它们组合成单个、格式化的文本输出。

## Google 的 ADK

Google 的 Agent Development Kit（ADK）提供了一个高级、结构化的框架，用于构建和部署由多个、交互的 AI 智能体组成的应用程序。它与 LangChain 和 LangGraph 相比，提供了一个更有主见和面向生产的系统来编排智能体协作，而不是提供智能体内部逻辑的基本构建块。

LangChain 在最基础级别运行，提供组件和标准化接口来创建操作序列，如调用模型和解析其输出。LangGraph 通过引入更灵活和强大的控制流来扩展这一点；它将智能体的工作流视为有状态图。使用 LangGraph，开发人员显式定义节点（函数或工具）和边（决定执行路径）。此图结构允许复杂的循环推理，其中系统可以循环、重试任务，并基于在节点之间传递的显式管理的状态对象做出决策。它给开发人员对单个智能体思考过程的精细控制，或从第一原理构建多智能体系统的能力。

Google 的 ADK 抽象了大部分这种低级图构建。与其要求开发人员定义每个节点和边，它提供了用于多智能体交互的预构建架构模式。例如，ADK 具有内置智能体类型，如 SequentialAgent 或 ParallelAgent，它们自动管理不同智能体之间的控制流。它围绕"团队"智能体的概念构建，通常由主要智能体将任务委托给专门的子智能体。状态和会话管理由框架更隐式地处理，提供比 LangGraph 的显式状态传递更统一但不太精细的方法。因此，虽然 LangGraph 为您提供详细工具来设计单个机器人或团队的复杂布线，但 Google 的 ADK 为您提供了一个工厂装配线，设计用于构建和管理已经知道如何协作的机器人舰队。

```python
from google.adk.agents import LlmAgent
from google.adk.tools import google_Search

dice_agent = LlmAgent(
    model="gemini-2.0-flash-exp",
    name="question_answer_agent",
    description="A helpful assistant agent that can answer questions.",
    instruction="""Respond to the query using google search""",
    tools=[google_search],
)
```

此代码创建了一个搜索增强的智能体。当此智能体收到问题时，它不仅仅依赖其预先存在的知识。相反，按照其指令，它将使用 Google Search 工具从网络中找到相关的、实时信息，然后使用该信息构建其答案。

## Crew.AI

CrewAI 提供了一个编排框架，通过专注于协作角色和结构化流程来构建多智能体系统。它在比基础工具包更高的抽象级别上运行，提供了一个反映人类团队的概念模型。与其将细粒度的逻辑流定义为图，开发人员定义参与者和他们的分配，CrewAI 管理他们的交互。

此框架的核心组件是智能体（Agents）、任务（Tasks）和 Crew（团队）。智能体不仅由其功能定义，还由其角色定义，包括特定角色、目标和背景，这些指导其行为和沟通风格。任务是具有清晰描述和预期输出的离散工作单元，分配给特定智能体。Crew 是包含智能体和任务列表的统一单元，它执行预定义的过程（Process）。此过程决定工作流，通常是顺序的，其中一个任务的输出成为下一个任务的输入，或者是分层的，其中类似管理者的智能体将任务委托给其他智能体并协调工作流。

与其他框架相比，CrewAI 占据独特位置。它远离 LangGraph 的低级、显式状态管理和控制流，其中开发人员连接每个节点和条件边。与其构建状态机，开发人员设计团队章程。虽然 Google 的 ADK 为整个智能体生命周期提供了全面的、面向生产的平台，但 CrewAI 专门专注于智能体协作的逻辑和模拟专家团队。

```python
@crew
def crew(self) -> Crew:
   """Creates the research crew"""
   return Crew(
     agents=self.agents,
     tasks=self.tasks,
     process=Process.sequential,
     verbose=True,
   )
```

此代码为 AI 智能体团队设置顺序工作流，其中它们按特定顺序处理任务列表，并启用详细日志记录以监控其进度。

## 其他智能体开发框架

**Microsoft AutoGen：** AutoGen 是一个以编排多个智能体为中心的框架，这些智能体通过对话解决任务。其架构使具有不同能力的智能体能够交互，允许复杂的问题分解和协作解决。AutoGen 的主要优势是其灵活的、对话驱动的方法，支持动态和复杂的多智能体交互。然而，这种对话范式可能导致不太可预测的执行路径，并可能需要复杂的提示工程以确保任务有效收敛。

**LlamaIndex：** LlamaIndex 从根本上是一个数据框架，设计用于将大型语言模型与外部和私有数据源连接。它擅长创建复杂的数据摄取和检索管道，这对于构建能够执行 RAG 的知识渊博的智能体至关重要。虽然其数据索引和查询能力对于创建上下文感知智能体非常强大，但其用于复杂智能体控制流和多智能体编排的本机工具与智能体优先框架相比不太发达。当核心技术挑战是数据检索和综合时，LlamaIndex 是最优的。

**Haystack：** Haystack 是一个开源框架，设计用于构建由语言模型驱动的可扩展和生产就绪的搜索系统。其架构由模块化、可互操作的节点组成，形成用于文档检索、问题回答和摘要的管道。Haystack 的主要优势是其专注于大规模信息检索任务的性能和可扩展性，使其适用于企业级应用程序。潜在的权衡是其设计，针对搜索管道进行了优化，对于实现高度动态和创造性的智能体行为可能更加严格。

**MetaGPT：** MetaGPT 通过基于预定义的标准操作程序（SOP）集分配角色和任务来实现多智能体系统。此框架构建智能体协作以模拟软件开发公司，智能体承担产品经理或工程师等角色来完成复杂任务。此 SOP 驱动的方法产生高度结构化和连贯的输出，这对于像代码生成这样的专门领域来说是一个显著优势。该框架的主要限制是其高度专业化，使其在核心设计之外的一般用途智能体任务中不太适应。

**SuperAGI：** SuperAGI 是一个开源框架，设计用于为自主智能体提供完整的生命周期管理系统。它包括智能体配置、监控和图形界面功能，旨在增强智能体执行的可靠性。关键好处是其专注于生产就绪性，具有内置机制来处理常见的故障模式（如循环）并提供对智能体性能的可观察性。潜在的缺点是其综合平台方法可能引入比更轻量级、基于库的框架更多的复杂性和开销。

**Semantic Kernel：** 由 Microsoft 开发，Semantic Kernel 是一个 SDK，通过"插件"和"规划器"系统将大型语言模型与常规编程代码集成。它允许 LLM 调用本机函数并编排工作流，有效地将模型视为更大软件应用程序中的推理引擎。其主要优势是其与现有企业代码库的无缝集成，特别是在 .NET 和 Python 环境中。其插件和规划器架构的概念开销与更直接的智能体框架相比可能呈现更陡峭的学习曲线。

**Strands Agents：** 一个 AWS 轻量级和灵活的 SDK，使用模型驱动的方法来构建和运行 AI 智能体。它设计为简单且可扩展，支持从基本对话助手到复杂的多智能体自主系统的所有内容。该框架是模型无关的，为各种 LLM 提供商提供广泛支持，并包括与 MCP 的本机集成，以便轻松访问外部工具。其核心优势是其简单性和灵活性，具有易于入门的可定制智能体循环。潜在的权衡是其轻量级设计意味着开发人员可能需要构建更多周围的操作基础设施，如高级监控或生命周期管理系统，这些更全面的框架可能开箱即用提供。

## 结论

智能体框架的格局提供了多样化的工具，从定义智能体逻辑的低级库到编排多智能体协作的高级平台。在基础级别，LangChain 支持简单的、线性的工作流，而 LangGraph 引入了有状态的、循环的图以进行更复杂的推理。更高级的框架（如 CrewAI 和 Google 的 ADK）将重点转向编排具有预定义角色的智能体团队，而其他框架（如 LlamaIndex）专门用于数据密集型应用程序。这种多样性为开发人员呈现了基于图的系统的精细控制与更有主见平台的简化开发之间的核心权衡。因此，选择正确的框架取决于应用程序是需要简单序列、动态推理循环还是管理的专家团队。最终，这个不断演进的生态系统通过选择项目所需的精确抽象级别，使开发人员能够构建越来越复杂的 AI 系统。

参考文献

1. LangChain, [https://www.langchain.com/](https://www.langchain.com/)
2. LangGraph, [https://www.langchain.com/langgraph](https://www.langchain.com/langgraph)
3. Google's ADK, [https://google.github.io/adk-docs/](https://google.github.io/adk-docs/)
4. Crew.AI, [https://docs.crewai.com/en/introduction](https://docs.crewai.com/en/introduction)
