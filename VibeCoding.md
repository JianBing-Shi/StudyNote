# Vibe Coding
`Vibe Coding`的核心概念是**氛围编程**。在AI快速发展的当下，使用AI编辑工具帮助人们完成对代码的编写，让更多的时间花费在创造的思路上，这种方式从根本上改变了传统编程（“古法编程”）。

## LLM 大语言模型
LLM（Large Language Model, 大语言模型）

## Token 词元
大模型处理文本的基本单元，即为Token词元。

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
现在的Agent智能体，主要有 *Claude Code* 、 *Copilot* 、 *Gemini* 、*CodeX*。
* Claude Code -> 深度代码理解
* Copilot -> GitHub生态深度绑定，企业级支持
* Gemini -> 多模态 + 百万Token上下文
* CodeX -> 轻量级开源方案，完全可控

## Agent Skill