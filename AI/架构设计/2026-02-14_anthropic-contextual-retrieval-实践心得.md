---
title: Anthropic Contextual Retrieval 实践心得
date: 2026-02-14
category: AI/架构设计
tags: [contextual-retrieval, RAG, embedding, BM25, reranking]
source: 
---

# Anthropic Contextual Retrieval 实践心得

## 原始用户输入

```
实践总结：
embedding+BM25：1）文档库切chunks；2）倒排索引+改进TF-IDF编码、语义向量；3）使用BM25相关性算法精准匹配chunks、使用embedding语义匹配chunks；4）排序融合合并；5）将top k结果放入prompt进行生成

contextual retrieval：在embedding+BM25之前，为每个独立切片（chunk）生成一段专属的、特定的上下文说明，并将其拼接到切片前面进行索引。

其他利用上下文的方法：在 chunk 中添加通用的文档摘要（提升非常有限）、假设性文档嵌入以及基于摘要的索引（性能较低）

实践经验：考虑chunk大小边界和重叠、embedding模型、领域contextualizer prompt、召回chunk数量20

增加reranking：1）初始检索获取潜在相关的150个chunk，2）将150个chunk连同用户query丢给reranking模型，根据每个chunk与prompt的相关性与重要性进行评分，选择前20个，3）传递k个chunk给模型生成答案

Voyage 和 Gemini 拥有表现最好的 embeddings
```

---

## 结构化内容

### 1. embedding + BM25 流程

| 步骤 | 内容 |
|------|------|
| 1 | 文档库切 chunks |
| 2 | 倒排索引 + 改进 TF-IDF 编码、语义向量 |
| 3 | 使用 BM25 精准匹配 chunks、使用 embedding 语义匹配 chunks |
| 4 | 排序融合合并 |
| 5 | 将 top-k 结果放入 prompt 进行生成 |

### 2. Contextual Retrieval

**核心思想**：在 embedding+BM25 之前，为每个独立切片（chunk）生成一段专属的、特定的上下文说明，并将其拼接到切片前面进行索引。

### 3. 其他利用上下文的方法

| 方法 | 效果 |
|------|------|
| 在 chunk 中添加通用文档摘要 | 提升非常有限 |
| 假设性文档嵌入 | 效果一般 |
| 基于摘要的索引 | 性能较低 |

### 4. 实践经验

- 考虑 chunk 大小边界和重叠
- embedding 模型选择
- 领域 contextualizer prompt
- 召回 chunk 数量：20

### 5. Reranking 流程

| 步骤 | 内容 |
|------|------|
| 1 | 初始检索获取潜在相关的 150 个 chunks |
| 2 | 将 150 个 chunks 连同用户 query 丢给 reranking 模型，根据每个 chunk 与 prompt 的相关性与重要性进行评分，选择前 20 个 |
| 3 | 传递 k 个 chunks 给模型生成答案 |

### 6. 推荐

- **Voyage** 和 **Gemini** 拥有表现最好的 embeddings

---

*Created by Inspiration Manager*
*Timestamp: 2026-02-14*
