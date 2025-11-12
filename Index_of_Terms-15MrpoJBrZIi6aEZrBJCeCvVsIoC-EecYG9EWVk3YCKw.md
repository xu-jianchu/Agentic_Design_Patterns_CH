# 术语表

## 基本概念

- **Prompt（提示）**：Prompt 是用户提供给 AI 模型以引发响应的输入，通常采用问题、指令或陈述的形式。Prompt 的质量和结构极大地影响模型的输出，使 prompt engineering 成为有效使用 AI 的关键技能。

- **Context Window（上下文窗口）**：上下文窗口是 AI 模型一次可以处理的最大 token 数量，包括输入和生成的输出。这个固定大小是一个关键限制，因为窗口外的信息被忽略，而更大的窗口使更复杂的对话和文档分析成为可能。

- **In-Context Learning（上下文学习）**：上下文学习是 AI 从直接在 prompt 中提供的示例中学习新任务的能力，无需任何重新训练。这个强大的功能允许单个通用模型即时适应无数特定任务。

- **Zero-Shot, One-Shot, & Few-Shot Prompting（零样本、单样本和少样本提示）**：这些是提示技术，其中模型被给予零个、一个或几个任务示例来指导其响应。提供更多示例通常有助于模型更好地理解用户的意图并提高其在特定任务上的准确性。

- **Multimodality（多模态）**：多模态是 AI 理解和处理跨多种数据类型（如文本、图像和音频）的信息的能力。这允许更多样化和类似人类的交互，例如描述图像或回答口语问题。

**Grounding（接地）**：Grounding 是将模型的输出连接到可验证的真实世界信息源以确保事实准确性并减少幻觉的过程。这通常通过 RAG 等技术来实现，使 AI 系统更加值得信赖。

## 核心 AI 模型架构

- **Transformers（Transformer）**：Transformer 是大多数现代 LLM 的基础神经网络架构。其关键创新是自注意力机制，它高效地处理长文本序列并捕获单词之间的复杂关系。

- **Recurrent Neural Network (RNN)（循环神经网络）**：循环神经网络是 Transformer 之前的基础架构。RNN 顺序处理信息，使用循环来维护先前输入的"记忆"，这使得它们适用于文本和语音处理等任务。

- **Mixture of Experts (MoE)（专家混合）**：专家混合是一种高效的模型架构，其中"路由器"网络动态选择一小部分"专家"网络来处理任何给定的输入。这允许模型拥有大量参数，同时保持计算成本可控。

- **Diffusion Models（扩散模型）**：扩散模型是擅长创建高质量图像的生成模型。它们通过向数据添加随机噪声，然后训练模型仔细地逆转该过程来工作，允许它们从随机起点生成新数据。

- **Mamba**：Mamba 是一种使用选择性状态空间模型（SSM）高效处理序列的最近 AI 架构，特别是对于非常长的上下文。其选择性机制允许它专注于相关信息，同时过滤掉噪声，使其成为 Transformer 的潜在替代方案。

## LLM 开发生命周期

- 强大语言模型的开发遵循一个独特的序列。它从预训练（Pre-training）开始，通过在大规模通用互联网文本数据集上训练来构建大规模基础模型，以学习语言、推理和世界知识。接下来是微调（Fine-tuning），这是一个专业化阶段，通用模型在较小的、特定任务的数据集上进一步训练，以使其能力适应特定目的。最后阶段是对齐（Alignment），调整专业化模型的行为以确保其输出有帮助、无害并与人类价值观保持一致。

- 预训练技术：预训练是模型从大量数据中学习一般知识的初始阶段。此阶段的顶级技术涉及模型学习的不同目标。最常见的是因果语言建模（CLM），其中模型预测句子中的下一个单词。另一个是掩码语言建模（MLM），其中模型填充文本中故意隐藏的单词。其他重要方法包括去噪目标（Denoising Objectives），其中模型学习将损坏的输入恢复到其原始状态；对比学习（Contrastive Learning），其中它学习区分相似和不同的数据片段；以及下一句预测（NSP），其中它确定两个句子是否在逻辑上相互跟随。

- 微调技术：微调是使用较小的、专业化的数据集将通用预训练模型适应特定任务的过程。最常见的方法是监督微调（SFT），其中模型在正确输入-输出对的标记示例上训练。一个流行的变体是指令调优（Instruction Tuning），它专注于训练模型以更好地遵循用户命令。为了使这个过程更高效，使用了参数高效微调（PEFT）方法，顶级技术包括 LoRA（低秩适应），它只更新少量参数，以及其内存优化版本 QLoRA。另一种技术，检索增强生成（RAG），通过在微调或推理阶段将模型连接到外部知识源来增强模型。

- 对齐和安全技术：对齐是确保 AI 模型的行为与人类价值观和期望保持一致，使其有帮助和无害的过程。最突出的技术是人类反馈强化学习（RLHF），其中基于人类偏好训练的"奖励模型"指导 AI 的学习过程，通常使用像近端策略优化（PPO）这样的算法来保持稳定性。已经出现了更简单的替代方案，例如直接偏好优化（DPO），它绕过了对单独奖励模型的需求，以及 Kahneman-Tversky 优化（KTO），它进一步简化了数据收集。为了确保安全部署，Guardrails 被实现为最终安全层，以过滤输出并实时阻止有害操作。

## 增强 AI Agent 能力

- AI agent 是能够感知其环境并采取自主行动以实现目标的系统。它们的有效性通过强大的推理框架得到增强。

- **Chain of Thought (CoT)（思维链）**：这种提示技术鼓励模型在给出最终答案之前逐步解释其推理。这种"大声思考"的过程通常在复杂推理任务上产生更准确的结果。

- **Tree of Thoughts (ToT)（思维树）**：思维树是一种高级推理框架，其中 agent 同时探索多个推理路径，就像树上的分支。它允许 agent 自我评估不同的思路并选择最有希望的一个来追求，使其在复杂问题解决方面更加有效。

**ReAct (Reason and Act)（推理与行动）**：ReAct 是一个在循环中结合推理和行动的 agent 框架。Agent 首先"思考"要做什么，然后使用工具采取"行动"，并使用产生的观察来通知其下一个想法，使其在解决复杂任务方面非常有效。

- **Planning（规划）**：这是 agent 将高级目标分解为一系列较小的、可管理的子任务的能力。然后，agent 创建一个计划来按顺序执行这些步骤，使其能够处理复杂的多步骤任务。

- **Deep Research（深度研究）**：深度研究是指 agent 通过迭代搜索信息、综合发现和识别新问题来自主深入探索主题的能力。这允许 agent 构建对主题的全面理解，远远超出单个搜索查询。

- **Critique Model（批判模型）**：批判模型是专门训练的 AI 模型，用于审查、评估和提供对另一个 AI 模型输出的反馈。它充当自动化批判者，帮助识别错误、改进推理并确保最终输出达到所需的质量标准。

## 术语索引

此术语索引是使用 Gemini Pro 2.5 生成的。prompt 和推理步骤包含在末尾，以演示节省时间的好处并用于教育目的。

### A

- A/B Testing - Chapter 3: Parallelization  
- Action Selection - Chapter 20: Prioritization  
- Adaptation - Chapter 9: Learning and Adaptation  
- Adaptive Task Allocation - Chapter 16: Resource-Aware Optimization  
- Adaptive Tool Use & Selection - Chapter 16: Resource-Aware Optimization  
- Agent - What makes an AI system an Agent?  
- Agent-Computer Interfaces (ACIs) - Appendix B  
- Agent-Driven Economy - What makes an AI system an Agent?  
- Agent as a Tool - Chapter 7: Multi-Agent Collaboration  
- Agent Cards - Chapter 15: Inter-Agent Communication (A2A)  
- Agent Development Kit (ADK) - Chapter 2: Routing, Chapter 3: Parallelization, Chapter 4: Reflection, Chapter 5: Tool Use, Chapter 7: Multi-Agent Collaboration, Chapter 8: Memory Management, Chapter 12: Exception Handling and Recovery, Chapter 13: Human-in-the-Loop, Chapter 15: Inter-Agent Communication (A2A), Chapter 16: Resource-Aware Optimization, Chapter 19: Evaluation and Monitoring, Appendix C  
- Agent Discovery - Chapter 15: Inter-Agent Communication (A2A)  
- Agent Trajectories - Chapter 19: Evaluation and Monitoring  
- Agentic Design Patterns - Introduction  
- Agentic RAG - Chapter 14: Knowledge Retrieval (RAG)  
- Agentic Systems - Introduction  
- AI Co-scientist - Chapter 21: Exploration and Discovery  
- Alignment - Glossary  
- AlphaEvolve - Chapter 9: Learning and Adaptation  
- Analogies - Appendix A  
- Anomaly Detection - Chapter 19: Evaluation and Monitoring  
- Anthropic's Claude 4 Series - Appendix B  
- Anthropic's Computer Use - Appendix B  
- API Interaction - Chapter 10: Model Context Protocol (MCP)  
- Artifacts - Chapter 15: Inter-Agent Communication (A2A)  
- Asynchronous Polling - Chapter 15: Inter-Agent Communication (A2A)  
- Audit Logs - Chapter 15: Inter-Agent Communication (A2A)  
- Automated Metrics - Chapter 19: Evaluation and Monitoring  
- Automatic Prompt Engineering (APE) - Appendix A  
- Autonomy - Introduction  
- A2A (Agent-to-Agent) - Chapter 15: Inter-Agent Communication (A2A)

### B

- Behavioral Constraints - Chapter 18: Guardrails/Safety Patterns  
- Browser Use - Appendix B

### C

- Callbacks - Chapter 18: Guardrails/Safety Patterns  
- Causal Language Modeling (CLM) - Glossary  
- Chain of Debates (CoD) - Chapter 17: Reasoning Techniques  
- Chain-of-Thought (CoT) - Chapter 17: Reasoning Techniques, Appendix A  
- Chatbots - Chapter 8: Memory Management  
- ChatMessageHistory - Chapter 8: Memory Management  
- Checkpoint and Rollback - Chapter 18: Guardrails/Safety Patterns  
- Chunking - Chapter 14: Knowledge Retrieval (RAG)  
- Clarity and Specificity - Appendix A  
- Client Agent - Chapter 15: Inter-Agent Communication (A2A)  
- Code Generation - Chapter 1: Prompt Chaining, Chapter 4: Reflection  
- Code Prompting - Appendix A  
- CoD (Chain of Debates) - Chapter 17: Reasoning Techniques  
- CoT (Chain of Thought) - Chapter 17: Reasoning Techniques, Appendix A  
- Collaboration - Chapter 7: Multi-Agent Collaboration  
- Compliance - Chapter 19: Evaluation and Monitoring  
- Conciseness - Appendix A  
- Content Generation - Chapter 1: Prompt Chaining, Chapter 4: Reflection  
- Context Engineering - Chapter 1: Prompt Chaining  
- Context Window - Glossary  
- Contextual Pruning & Summarization - Chapter 16: Resource-Aware Optimization  
- Contextual Prompting - Appendix A  
- Contractor Model - Chapter 19: Evaluation and Monitoring  
- ConversationBufferMemory - Chapter 8: Memory Management  
- Conversational Agents - Chapter 1: Prompt Chaining, Chapter 4: Reflection  
- Cost-Sensitive Exploration - Chapter 16: Resource-Aware Optimization  
- CrewAI - Chapter 3: Parallelization, Chapter 5: Tool Use, Chapter 6: Planning, Chapter 7: Multi-Agent Collaboration, Chapter 18: Guardrails/Safety Patterns, Appendix C  
- Critique Agent - Chapter 16: Resource-Aware Optimization  
- Critique Model - Glossary  
- Customer Support - Chapter 13: Human-in-the-Loop

### D

- Data Extraction - Chapter 1: Prompt Chaining  
- Data Labeling - Chapter 13: Human-in-the-Loop  
- Database Integration - Chapter 10: Model Context Protocol (MCP)  
- DatabaseSessionService - Chapter 8: Memory Management  
- Debate and Consensus - Chapter 7: Multi-Agent Collaboration  
- Decision Augmentation - Chapter 13: Human-in-the-Loop  
- Decomposition - Appendix A  
- Deep Research - Chapter 6: Planning, Chapter 17: Reasoning Techniques, Glossary  
- Delimiters - Appendix A  
- Denoising Objectives - Glossary  
- Dependencies - Chapter 20: Prioritization  
- Diffusion Models - Glossary  
- Direct Preference Optimization (DPO) - Chapter 9: Learning and Adaptation  
- Discoverability - Chapter 10: Model Context Protocol (MCP)  
- Drift Detection - Chapter 19: Evaluation and Monitoring  
- Dynamic Model Switching - Chapter 16: Resource-Aware Optimization  
- Dynamic Re-prioritization - Chapter 20: Prioritization

### E

- Embeddings - Chapter 14: Knowledge Retrieval (RAG)  
- Embodiment - What makes an AI system an Agent?  
- Energy-Efficient Deployment - Chapter 16: Resource-Aware Optimization  
- Episodic Memory - Chapter 8: Memory Management  
- Error Detection - Chapter 12: Exception Handling and Recovery  
- Error Handling - Chapter 12: Exception Handling and Recovery  
- Escalation Policies - Chapter 13: Human-in-the-Loop  
- Evaluation - Chapter 19: Evaluation and Monitoring  
- Exception Handling - Chapter 12: Exception Handling and Recovery  
- Expert Teams - Chapter 7: Multi-Agent Collaboration  
- Exploration and Discovery - Chapter 21: Exploration and Discovery  
- External Moderation APIs - Chapter 18: Guardrails/Safety Patterns

### F

- Factored Cognition - Appendix A  
- FastMCP - Chapter 10: Model Context Protocol (MCP)  
- Fault Tolerance - Chapter 18: Guardrails/Safety Patterns  
- Few-Shot Learning - Chapter 9: Learning and Adaptation  
- Few-Shot Prompting - Appendix A  
- Fine-tuning - Glossary  
- Formalized Contract - Chapter 19: Evaluation and Monitoring  
- Function Calling - Chapter 5: Tool Use, Appendix A

### G

- Gemini Live - Appendix B  
- Gems - Appendix A  
- Generative Media Orchestration - Chapter 10: Model Context Protocol (MCP)  
- Goal Setting - Chapter 11: Goal Setting and Monitoring  
- GoD (Graph of Debates) - Chapter 17: Reasoning Techniques  
- Google Agent Development Kit (ADK) - Chapter 2: Routing, Chapter 3: Parallelization, Chapter 4: Reflection, Chapter 5: Tool Use, Chapter 7: Multi-Agent Collaboration, Chapter 8: Memory Management, Chapter 12: Exception Handling and Recovery, Chapter 13: Human-in-the-Loop, Chapter 15: Inter-Agent Communication (A2A), Chapter 16: Resource-Aware Optimization, Chapter 19: Evaluation and Monitoring, Appendix C  
- Google Co-Scientist - Chapter 21: Exploration and Discovery  
- Google DeepResearch - Chapter 6: Planning  
- Google Project Mariner - Appendix B  
- Graceful Degradation - Chapter 12: Exception Handling and Recovery, Chapter 16: Resource-Aware Optimization  
- Graph of Debates (GoD) - Chapter 17: Reasoning Techniques  
- Grounding - Glossary  
- Guardrails - Chapter 18: Guardrails/Safety Patterns

### H

- Haystack - Appendix C  
- Hierarchical Decomposition - Chapter 19: Evaluation and Monitoring  
- Hierarchical Structures - Chapter 7: Multi-Agent Collaboration  
- HITL (Human-in-the-Loop) - Chapter 13: Human-in-the-Loop  
- Human-in-the-Loop (HITL) - Chapter 13: Human-in-the-Loop  
- Human-on-the-loop - Chapter 13: Human-in-the-Loop  
- Human Oversight - Chapter 13: Human-in-the-Loop, Chapter 18: Guardrails/Safety Patterns

### I

- In-Context Learning - Glossary  
- InMemoryMemoryService - Chapter 8: Memory Management  
- InMemorySessionService - Chapter 8: Memory Management  
- Input Validation/Sanitization - Chapter 18: Guardrails/Safety Patterns  
- Instructions Over Constraints - Appendix A  
- Inter-Agent Communication (A2A) - Chapter 15: Inter-Agent Communication (A2A)  
- Intervention and Correction - Chapter 13: Human-in-the-Loop  
- IoT Device Control - Chapter 10: Model Context Protocol (MCP)  
- Iterative Prompting / Refinement - Appendix A

### J

- Jailbreaking - Chapter 18: Guardrails/Safety Patterns

### K

- Kahneman-Tversky Optimization (KTO) - Glossary  
- Knowledge Retrieval (RAG) - Chapter 14: Knowledge Retrieval (RAG)

### L

- LangChain - Chapter 1: Prompt Chaining, Chapter 2: Routing, Chapter 3: Parallelization, Chapter 4: Reflection, Chapter 5: Tool Use, Chapter 8: Memory Management, Chapter 20: Prioritization, Appendix C  
- LangGraph - Chapter 1: Prompt Chaining, Chapter 2: Routing, Chapter 3: Parallelization, Chapter 4: Reflection, Chapter 5: Tool Use, Chapter 8: Memory Management, Appendix C  
- Latency Monitoring - Chapter 19: Evaluation and Monitoring  
- Learned Resource Allocation Policies - Chapter 16: Resource-Aware Optimization  
- Learning and Adaptation - Chapter 9: Learning and Adaptation  
- LLM-as-a-Judge - Chapter 19: Evaluation and Monitoring  
- LlamaIndex - Appendix C  
- LoRA (Low-Rank Adaptation) - Glossary  
- Low-Rank Adaptation (LoRA) - Glossary

### M

- Mamba - Glossary  
- Masked Language Modeling (MLM) - Glossary  
- MASS (Multi-Agent System Search) - Chapter 17: Reasoning Techniques  
- MCP (Model Context Protocol) - Chapter 10: Model Context Protocol (MCP)  
- Memory Management - Chapter 8: Memory Management  
- Memory-Based Learning - Chapter 9: Learning and Adaptation  
- MetaGPT - Appendix C  
- Microsoft AutoGen - Appendix C  
- Mixture of Experts (MoE) - Glossary  
- Model Context Protocol (MCP) - Chapter 10: Model Context Protocol (MCP)  
- Modularity - Chapter 18: Guardrails/Safety Patterns  
- Monitoring - Chapter 11: Goal Setting and Monitoring, Chapter 19: Evaluation and Monitoring  
- Multi-Agent Collaboration - Chapter 7: Multi-Agent Collaboration  
- Multi-Agent System Search (MASS) - Chapter 17: Reasoning Techniques  
- Multimodality - Glossary  
- Multimodal Prompting - Appendix A

### N

- Negative Examples - Appendix A  
- Next Sentence Prediction (NSP) - Glossary

### O

- Observability - Chapter 18: Guardrails/Safety Patterns  
- One-Shot Prompting - Appendix A  
- Online Learning - Chapter 9: Learning and Adaptation  
- OpenAI Deep Research API - Chapter 6: Planning  
- OpenEvolve - Chapter 9: Learning and Adaptation  
- OpenRouter - Chapter 16: Resource-Aware Optimization  
- Output Filtering/Post-processing - Chapter 18: Guardrails/Safety Patterns

### P

- PAL (Program-Aided Language Models) - Chapter 17: Reasoning Techniques  
- Parallelization - Chapter 3: Parallelization  
- Parallelization & Distributed Computing Awareness - Chapter 16: Resource-Aware Optimization  
- Parameter-Efficient Fine-Tuning (PEFT) - Glossary  
- PEFT (Parameter-Efficient Fine-Tuning) - Glossary  
- Performance Tracking - Chapter 19: Evaluation and Monitoring  
- Persona Pattern - Appendix A  
- Personalization - What makes an AI system an Agent?  
- Planning - Chapter 6: Planning, Glossary  
- Prioritization - Chapter 20: Prioritization  
- Principle of Least Privilege - Chapter 18: Guardrails/Safety Patterns  
- Proactive Resource Prediction - Chapter 16: Resource-Aware Optimization  
- Procedural Memory - Chapter 8: Memory Management  
- Program-Aided Language Models (PAL) - Chapter 17: Reasoning Techniques  
- Project Astra - Appendix B  
- Prompt - Glossary  
- Prompt Chaining - Chapter 1: Prompt Chaining  
- Prompt Engineering - Appendix A  
- Proximal Policy Optimization (PPO) - Chapter 9: Learning and Adaptation  
- Push Notifications - Chapter 15: Inter-Agent Communication (A2A)

### Q

- QLoRA - Glossary  
- Quality-Focused Iterative Execution - Chapter 19: Evaluation and Monitoring

### R

- RAG (Retrieval-Augmented Generation) - Chapter 8: Memory Management, Chapter 14: Knowledge Retrieval (RAG), Appendix A  
- ReAct (Reason and Act) - Chapter 17: Reasoning Techniques, Appendix A, Glossary  
- Reasoning - Chapter 17: Reasoning Techniques  
- Reasoning-Based Information Extraction - Chapter 10: Model Context Protocol (MCP)  
- Recovery - Chapter 12: Exception Handling and Recovery  
- Recurrent Neural Network (RNN) - Glossary  
- Reflection - Chapter 4: Reflection  
- Reinforcement Learning - Chapter 9: Learning and Adaptation  
- Reinforcement Learning from Human Feedback (RLHF) - Glossary  
- Reinforcement Learning with Verifiable Rewards (RLVR) - Chapter 17: Reasoning Techniques  
- Remote Agent - Chapter 15: Inter-Agent Communication (A2A)  
- Request/Response (Polling) - Chapter 15: Inter-Agent Communication (A2A)  
- Resource-Aware Optimization - Chapter 16: Resource-Aware Optimization  
- Retrieval-Augmented Generation (RAG) - Chapter 8: Memory Management, Chapter 14: Knowledge Retrieval (RAG), Appendix A  
- RLHF (Reinforcement Learning from Human Feedback) - Glossary  
- RLVR (Reinforcement Learning with Verifiable Rewards) - Chapter 17: Reasoning Techniques  
- RNN (Recurrent Neural Network) - Glossary  
- Role Prompting - Appendix A  
- Router Agent - Chapter 16: Resource-Aware Optimization  
- Routing - Chapter 2: Routing

### S

- Safety - Chapter 18: Guardrails/Safety Patterns  
- Scaling Inference Law - Chapter 17: Reasoning Techniques  
- Scheduling - Chapter 20: Prioritization  
- Self-Consistency - Appendix A  
- Self-Correction - Chapter 4: Reflection, Chapter 17: Reasoning Techniques  
- Self-Improving Coding Agent (SICA) - Chapter 9: Learning and Adaptation  
- Self-Refinement - Chapter 17: Reasoning Techniques  
- Semantic Kernel - Appendix C  
- Semantic Memory - Chapter 8: Memory Management  
- Semantic Similarity - Chapter 14: Knowledge Retrieval (RAG)  
- Separation of Concerns - Chapter 18: Guardrails/Safety Patterns  
- Sequential Handoffs - Chapter 7: Multi-Agent Collaboration  
- Server-Sent Events (SSE) - Chapter 15: Inter-Agent Communication (A2A)  
- Session - Chapter 8: Memory Management  
- SICA (Self-Improving Coding Agent) - Chapter 9: Learning and Adaptation  
- SMART Goals - Chapter 11: Goal Setting and Monitoring  
- State - Chapter 8: Memory Management  
- State Rollback - Chapter 12: Exception Handling and Recovery  
- Step-Back Prompting - Appendix A  
- Streaming Updates - Chapter 15: Inter-Agent Communication (A2A)  
- Structured Logging - Chapter 18: Guardrails/Safety Patterns  
- Structured Output - Chapter 1: Prompt Chaining, Appendix A  
- SuperAGI - Appendix C  
- Supervised Fine-Tuning (SFT) - Glossary  
- Supervised Learning - Chapter 9: Learning and Adaptation  
- System Prompting - Appendix A

### T

- Task Evaluation - Chapter 20: Prioritization  
- Text Similarity - Chapter 14: Knowledge Retrieval (RAG)  
- Token Usage - Chapter 19: Evaluation and Monitoring  
- Tool Use - Chapter 5: Tool Use, Appendix A  
- Tool Use Restrictions - Chapter 18: Guardrails/Safety Patterns  
- ToT (Tree of Thoughts) - Chapter 17: Reasoning Techniques, Appendix A, Glossary  
- Transformers - Glossary  
- Tree of Thoughts (ToT) - Chapter 17: Reasoning Techniques, Appendix A, Glossary

### U

- Unsupervised Learning - Chapter 9: Learning and Adaptation  
- User Persona - Appendix A

### V

- Validation - Chapter 3: Parallelization  
- Vector Search - Chapter 14: Knowledge Retrieval (RAG)  
- VertexAiRagMemoryService - Chapter 8: Memory Management  
- VertexAiSessionService - Chapter 8: Memory Management  
- Vibe Coding - Appendix B  
- Visual Perception - Appendix B

### W

- Webhooks - Chapter 15: Inter-Agent Communication (A2A)

### Z

- Zero-Shot Learning - Chapter 9: Learning and Adaptation  
- Zero-Shot Prompting - Appendix A

## Prompt

以下是用于创建此术语索引的 prompt。

> *请阅读 <https://docs.google.com/document/d/1rsaK53T3Lg5KoGwvf8ukOUvbELRtH-V0LnOIFDxBryE/edit?tab=t.0> 和第一页上的所有相关文档。请提取每个章节的所有标题并打印它们。然后处理所有单独章节的内容，并创建一个用于发布的详细索引，即单词或短语列表（'标题'）和关联指针（'定位器'），指向在文档或文档集合中可以找到与该标题相关的有用材料的位置。我不想要示例索引。我想要完整的索引，包含 100 个索引术语*

## Gemini 2.5：推理步骤

### 开始索引创建

我已经通过阅读提供的 Google Doc 启动了该过程。目前，我专注于理解用户的请求：提取章节标题以构建详细索引。我正在仔细审查文档，寻找将指示章节分隔的清晰结构元素。我希望识别一致的格式或关键词，这些将指导索引过程。

### 定义索引提取过程

我现在完全沉浸在这个任务中。我已经将用户的请求分解为可操作的步骤。我最初的重点是使用 Workspaces 访问主 Google Doc。接下来，我将解析文档以定位和检索相关文档，这些文档也将使用相同的工具访问。最后，我将从所有文档中提取所有章节标题，并创建一个全面的索引。我已经规划了详细的过程。

### 完善提取策略

用户的指令很明确：阅读 Google Doc、其相关文档，并构建章节标题的详细索引。我已经制定了一个多步骤计划。首先，Workspaces 将访问主 Google Doc。然后，我将解析它以查找和读取相关文档，也使用 Workspaces。我现在专注于提取标题并使用所需的术语形成详细索引。我优先考虑准确性以满足用户对全面索引的特定要求。

#### 1. 建立用户意图：

我已经确定用户需要来自 Google Doc 和相关文档的章节标题列表和全面索引。现在我需要找出最有效的方法来进行这个提取和索引过程。我正在探索潜在的 API 功能以简化此任务，并需要评估处理不同文档结构的最佳策略。

#### 2. 考虑工具使用

我已经使用正则表达式从主文档中提取了章节 URL。现在，我准备利用 Workspaces 检索每个章节的实际内容。一旦我有了每个章节的文本，我将继续进行索引。

### 访问章节内容：

我已经成功识别了主文档中每个章节的 URL，我现在可以使用 Workspaces。我的下一步是从这些 URL 检索内容，这将使我能够生成索引。我专注于效率，旨在快速处理此信息。

### 获取章节内容

我已经成功识别了主文档中的章节 URL，并准备利用 Workspaces。我的计划涉及提取这些 URL，批量调用以提高效率，然后处理检索到的内容以创建所需的索引。这确保了捕获所有相关信息，为全面和准确的结果铺平道路。

### 启动 URL 检索

我已经成功从主文档中提取了章节 URL。目前，我正在设置对 Workspaces 工具的调用，准备获取每个章节的内容。目标是获取索引所需的文本。然后，此信息将促进创建至少 100 个术语的索引，如请求的那样。

### 启动章节提取

我已经成功识别并提取了主文档中的章节 URL，我已将其包含在上面。我现在正在使用 Workspaces 为每个 URL 获取章节内容。目标是获取索引所需的文本。然后，此信息将促进创建至少 100 个术语的索引，如请求的那样。我已经开始检索此信息。

### 编排章节检索

我已经提取了章节 URL 并启动了对 Workspaces 的调用。我现在正在检索每个章节的内容。为了确保过程顺利，我正在批量调用以防止任何速率限制问题。这应该使我能够高效地编译索引所需的文本。

### 解码用户请求

我已经确定了用户的需求。他们需要两个主要交付成果：首先，从 Google Docs 提取的章节标题的全面列表；其次，包含至少一百个关键术语的详细索引。提取标题的过程似乎很简单，而策划索引将涉及更细致的分析。

### 提取标题和索引

我已经成功识别了用户的需求：从提供的文档中提取章节标题并编译全面索引。我已经获取了 31 个 Google Docs 的内容，包括主文档及其章节。我已经确认不需要其他工具。我的计划涉及两个步骤：首先，从文档文件名中提取章节标题；其次，细致地处理文本以构建包含关键术语和章节定位器的索引。

### 制定提取计划

我已经完善了处理用户请求的方法。我现在已经制定了一个精确的计划。首先，我将从 31 个获取的文档的文件名中编译章节标题。其次，更复杂的是，我将细致地分析内容以创建一个全面的、按字母顺序排列的索引，超过 100 个条目。我已经完成了提取和索引策略，现在可以开始执行。

### 制定提取策略

我的方法是明确定义的。我将从 31 个获取的文档的文件名中编译章节标题。随后，我将细致地分析内容以构建一个按字母顺序组织的索引，超过 100 个条目。我已经完成了提取和索引策略，现在可以开始实施。用户的详细索引目标，涵盖 Agentic Design Patterns 系列中不同章节和附录的内容，现在触手可及。我将专注于提供全面的响应，超越仅仅是示例索引。

### 定义方法

我已经澄清了用户的需求。我将从文档文件名中提取章节标题。接下来，我将仔细创建一个超过 100 个条目的索引，按字母顺序排列并注明它们的章节位置。我已经准备好开始索引和提取过程。
