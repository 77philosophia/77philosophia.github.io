---
title: 从Transformer到LLM:Architecture,Training and Usage
categories:
  - 专业笔记
date: 2025-01-06 22:11:16
tags:
---

### NLP basic concepts

#### 1. 两个NLP的主要问题

表示和建模：

a.Representation:将语言表示为机器语言：如BERT,Openai embedding

b. Modeling:用统计方法建模，GPT, chatGPT



1. 怎么表示words

   ![image-20250106225834152](from-transformer-to-llm/image-20250106225834152.png)

将单词分散到向量空间：将token映射到向量空间，向量空间(vector space) 是用来表示文本数据的一种高纬空间。在这个空间中，每个token或文本单元都被表示为一个向量。这些向量捕捉了文本单元的语义信息，并使得数学计算（如距离计算、相似度计算）成为可能。

![image-20250106230157598](from-transformer-to-llm/image-20250106230157598.png)



2. 建模

   语言模型LLM: 语言模型预测任何单词序列在给定语言语料库中的可能性

   ![image-20250106230245416](from-transformer-to-llm/image-20250106230245416.png)

- 怎么学习语言模型

  - 自回归模型Autogressive LM:

    ![image-20250106230342821](from-transformer-to-llm/image-20250106230342821.png)

  - Bi-gram/N-gram model:

    ![image-20250106230354621](from-transformer-to-llm/image-20250106230354621.png)

  - Marked LM:

    ![image-20250106230523067](from-transformer-to-llm/image-20250106230523067.png)

  - 几种语言模型的学习方式

    ![image-20250106230534541](from-transformer-to-llm/image-20250106230534541.png)

  - 为什么语言模型有比较好的表示

    ![image-20250106230544997](from-transformer-to-llm/image-20250106230544997.png)

  - 总结

    ![image-20250106230557957](from-transformer-to-llm/image-20250106230557957.png)

### attention机制和transformer

#### 1. attention来源：翻译任务seq2seq

![image-20250106230706080](from-transformer-to-llm/image-20250106230706080.png)

![image-20250106230717889](from-transformer-to-llm/image-20250106230717889.png)

![image-20250106230732617](from-transformer-to-llm/image-20250106230732617.png)

![image-20250106230745379](from-transformer-to-llm/image-20250106230745379.png)

![image-20250106230757155](from-transformer-to-llm/image-20250106230757155.png)



#### 2. 结构

![image-20250106230817984](from-transformer-to-llm/image-20250106230817984.png)

![image-20250106230830430](from-transformer-to-llm/image-20250106230830430.png)



### 训练LM

### Pretrained model用法（微调和prompt）

### transformer除语言外其他应用

