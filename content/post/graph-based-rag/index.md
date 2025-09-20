---
title: 基于图的各种检索增强生成 (RAG) 方法
description: GraphRAG、LightRAG 和 HyperGraphRAG
slug: graph-based-rag
date: 2025-09-20 12:22:54+0000
image: cover.jpg
categories:
    - Papers
tags:
    - LLM
    - RAG
    - Paper
weight: 1
---

去年 4 月，微软在 Arxiv 上发表了 GraphRAG 的论文，并在 7 月将有关代码在 GitHub 上[开源](https://github.com/microsoft/graphrag)出来，大约在 7 天内就拿到了 6.7k stars。而在今年暑假笔者恰好接触到了一些在实际生产环境中使用 RAG 系统的业务，因此尝试在这里追踪和解读一下基于图的 RAG 方法的学术研究。

## NaiveRAG

通常在各种论文里用 NaiveRAG 或者 BaselineRAG 表示最基础的 RAG 方法。**RAG** *(Retrieval-Augmented Generation, 检索增强生成)* 是一种设法将**与问题相关的**特定领域知识放入大模型上下文，从而使其能准确回答该领域问题的技术。其优势在于知识库规模不受大模型上下文长度制约，且无需在知识领域发生变化时重新调整模型本身，具有高度**可扩展**性。

好吧，这种定义性文字看着很头疼，所以让我们来换个角度。

- 众所周知，大模型在回答专业领域问题时经常会出现**幻觉**而产生不准确的答案。

- 现在，你被安排做一个**专门回答法律领域**相关问题的聊天机器人。

- 如果机器人回答出错，你可能会**被甲方青蒜**。

- 你想到可以简单地把法律条文一股脑塞进问答的提示词 (Prompts) 里。

- 但一番尝试后你发现这根本行不通：
  
  - 法律条文太多，大模型上下文不够用；
  
  - 就算够用，大模型的指令遵循等能力也会**明显下降**。

- 你突然想到了平时使用的联网搜索功能，用联网搜索能解决这个问题吗？

- 甲方告诉你他们还有一些网上搜不到的**内部**文档，联网搜索也搞不定了。

- 那能不能在本地想办法构建一个“法律知识库”呢？

- 你试着把法律文档全部放到一起组成一个**数据库**。

- 然后你开始研究如何让大模型在回答问题前从数据库里**搜索**知识。

恭喜你，你发明了最简单的检索增强生成方法——NaiveRAG。把过程分解成两步就是下面这样。

这是用文档建立知识库的流程：
{{< mermaid >}}
graph TD;
    subgraph 构建知识库
        A(原始文档) --> B[文档分块];
        B --> C[嵌入知识块];
        C --> D[存入数据库];
        D --> E(建立索引);
    end

{{< /mermaid >}}

这是使用知识库来做回答的流程：
{{< mermaid >}}
graph TD;
    subgraph 回答生成
        F(用户问题) --> G[寻找知识];
        E[数据库] -- 检索 --> G;
        G --> H[返回的知识块];
        F --> I[放入上下文];
        H --> I;
        I --> J[生成回答];
        J --> K(最终答案);
    end

{{< /mermaid >}}

网上有很多传统 RAG 方法的资料，包括怎么做嵌入，怎么做向量搜索，以后笔者可能会再单独写一篇文章聊 NaiveRAG，这里就先不赘述了。

## GraphRAG

原论文的 [Arxiv 链接](https://arxiv.org/pdf/2404.16130)。

GraphRAG 是微软开源的一套基于图结构的 RAG 方法。[微软官方](https://microsoft.github.io/graphrag)的说法：

> GraphRAG is a structured, hierarchical approach to Retrieval Augmented Generation (RAG), as opposed to naive semantic-search approaches using plain text snippets. The GraphRAG process involves extracting a knowledge graph out of raw text, building a community hierarchy, generating summaries for these communities, and then leveraging these structures when perform RAG-based tasks.

简单来说，它通过从文档中提取出“知识图谱”，并采用算法划分为社群，调用 LLM 为每个社群生成一份总结，在回答问题时分别在这些社群上生成一份答案，最终再使用 LLM 在这些回答的基础上生成最终答案。

### 为什么要这么做？

在解释为什么要使用 GraphRAG 之前，我们可以先仔细审视传统 RAG 采用的方法，看看它有什么缺陷。

| 方法              | 缺陷                   | 改进                              |
| --------------- | -------------------- | ------------------------------- |
| 对文本进行分块后嵌入      | 知识被切断，导致文本块不能表示完整信息  | 适应文档的分块方法，如分隔符（对法律条文），或引入 LLM   |
| 采用向量化方法进行嵌入和匹配  | 对“关键词”的匹配能力非常弱       | 使用“混合检索”策略，即关键词相似度和向量相似度进行加权    |
| 基于相似度分数排序返回的文本块 | 相似度得分无法完全表示和问题的相关程度  | 引入重排序模型 (Reranker)，代价是检索速度会明显变慢 |
| 直接用问题文本匹配文本块    | 多轮对话中由于代词等原因，召回率显著降低 | 利用 LLM 等进行查询重写，即构造出更容易检索知识的提问   |

而 GraphRAG 指向了传统 RAG 的一个顽固问题：**无法**有效利用所有知识块，导致无法回答**全局性**的问题，例如“这些文档的主题是什么？”，传统 RAG 使用这个提问很难找到有效的知识块，因此也就很难合理地回答这个问题。

为了让大模型回答时能使用所有文本块提高的知识，提高答案的**全面性**，微软提出了 GraphRAG 的方法。

### 那它是怎么做的？

WIP.