---
title: Prompt Design and Engineering
categories:
  - 专业笔记
date: 2025-01-05 23:12:42
tags:
---

> [!TIP]
>
> Prompt engineering:指令+questions+input+example

### LLMs的limitations

- 短暂记忆
- 输出不一致性
- 过期信息
- 内容捏造
- 领域专业性



### 一些建议和技巧

- CoT prompting
- 让模型通过其他方式输出真实的东西
- 直接结束prompt
- 强势的态度
- 让ai自我纠正
- 产生不同的观点
- 保持状态+角色扮演（gpt的web端维护了一个session，api没有实现）
- 教算法
- **LLMs的本质是往前读并补全文本，因此examples的顺序和prompt的顺序很重要。**
- Affordances:达到一定条件触发的函数



### 高级建议和技巧

- CoT
  - Zero-shot: lets think step by step...
  - Manual-Cot:
- ToT(Tree of thought)



### 工具：

- ART
- 通过自一致性增强依赖性
- 反思
- 专家prompt：prompt chainer
- Streamlining Complex Tasks with Chains
- 引导性的输出with rails
- 流水线的Prompt设计with自动的prompt engineering



### Argmenting LLMs through External Knowledge -RAG

- RAG-aware Prompting Techniques



### LLM Agents

#### Agents

- Reasoning without Observation
- Reason And Act
- Dialog-Enabled Resolving Agents



### 工具和框架

- Langchain
- Semantic Kernel
- The Guidance
- Nemo Guardrails
- LlamaIndex
- FastRAG
- Auto-GPT



