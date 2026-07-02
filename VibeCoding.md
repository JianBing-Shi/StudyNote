# Vibe Coding
`Vibe Coding`的核心概念是**氛围编程**。在AI快速发展的当下，使用AI编辑工具帮助人们完成对代码的编写，让更多的时间花费在创造的思路上，这种方式从根本上改变了传统编程（“古法编程”）。

## LLM 大语言模型
LLM（Large Language Model, 大语言模型）

## Token 词元
大模型处理文本的基本单元，即为Token词元。

在实际的Vibe Coding时，会出现以下的Token：
* Input Tokens：输入词元
* Cached Tokens：命中缓存词元
* Uncached Tokens：未命中缓存词元
* Output Tokens：输出词元

如果你经常进行持续数小时的大型 Vibe Coding，会很建议关注** Cached Tokens 的比例**：
* 95%以上：说明上下文复用得很好，成本较低。
* 70~90%：正常。
* 低于50%：通常意味着上下文变化太大，或者线程已经变得很长，可以考虑新建会话。

## Context 上下文
大模型每次处理任务的信息总和大小，也是大模型的临时记忆体。

## RAG 检索增强生成

### Context 上下文 && RAG 检索增强生成
Context越做越大，未来的大模型还需要RAG吗？

## Prompt 提示词
现在AI智能体设计了 `Plan Mode`，其意图就是让模型在执行任务之前，获取更多的提示词与用户交流，使得模型能更加准确、精确地完成用户的任务需求，不再盲目地在少量提示词下去做偏离用户的任务需求的行为。

## Tool

## MCP 服务器
在现代AI开发中，Model Context Protocol（MCP）允许通过外部进程扩展模型能力，而 [npx](https://nodejs.org/en/download)（Node.js 生态）和 [uvx](https://docs.astral.sh/uv/getting-started/installation/#github-releases)（Python 生态）则是两种即装即用的客户端工具，帮助你快速下载并运行 MCP 服务器或工具包，无需全局安装。

## Agent 智能体
Agent的本质是LLM和人类的**翻译官**，这是因为人类的自然语言很模糊和LLM需要的输入非常精准之间的矛盾。所以，Agent在输入端接受人类最朴素、最随意的自然语言，Agent平台在将人类的简单意图进行包装、拼接、翻译成大模型听得懂的高级结构化语言，在输出端大模型吐出标准的暗号，Agent再次将其翻译成人类看得懂的精美UI卡片。

现在的Agent智能体，主要有 *Claude Code* 、 *Copilot* 、 *Gemini* 、*CodeX*。
* Claude Code -> 深度代码理解
* Copilot -> GitHub生态深度绑定，企业级支持
* Gemini -> 多模态 + 百万Token上下文
* CodeX -> 轻量级开源方案，完全可控
Agent

### Agent Skill 技能
Agent Skill（智能体技能）指的是赋予 Agent 执行特定任务、扩展其原生能力的可复用工具、函数、代码段或工作流。

一个标准的Agent Skill通常包含两个关键部分：
* 技能描述（Metadata / Prompt Description）：这是 Skill 最核心的部分。
* 执行逻辑（Execution Logic）：实际干活的代码或 API。

在Skill文件夹中被命名为`SKILL.md`文件，这是**专门给大模型阅读的深度说明书**，用自然语言规定该技能的使用边界、输出格式（如表格、JSON）以及少样本示例（Few-shot Examples），防止大模型幻觉或滥用。然后在目录下，存在`*.yaml`文件，它定义技能的元数据（如 display_name）和核心触发指令（如 default_prompt），负责告诉平台何时激活它，并向大模型注入核心“暗号”。

### Prompt Engineering 提示词工程
教你怎么"跟 AI 说话"

### Context Engineering 上下文工程
教你怎么"给 AI 喂信息"

### Harness Engineering 马具工程
教你怎么"给 AI 造一条高速公路，配上护栏、限速牌和加油站"。  
Harness Engineering 是一套围绕 AI Agent 构建的约束、反馈与控制系统，让 Agent 在人类设定的边界内自主、可靠、可持续地工作——它不优化模型本身，而是优化模型运行的"环境"。