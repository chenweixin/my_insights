---
title: Anthropic Contextual Retrieval 实践心得
date: 2026-02-12
category: AI/提示词工程
tags: [contextual-retrieval,  RAG,  embedding,  BM25,  reranking]
source: 
---

# Anthropic Contextual Retrieval 实践心得

anthropic关于contextual retrieval的实践心得：

实践总结：
embedding+BM25：
1）文档库切chunks；
2）倒排索引+改进TF-IDF编码、语义向量；
3）使用BM25相关性算法精准匹配chunks、使用embedding语义匹配chunks；
4）排序融合合并；
5）将top k结果放入prompt进行生成

contextual retrieval：
在embedding+BM25之前，为每个独立切片（chunk）生成一段专属的、特定的上下文说明，并将其拼接到切片前面进行索引。

其他利用上下文的方法：
- 在chunk中添加通用的文档摘要（提升非常有限）
- 假设性文档嵌入
- 基于摘要的索引（性能较低）

实践经验：
- 考虑chunk大小边界和重叠
- embedding模型
- 领域contextualizer prompt
- 召回chunk数量20

增加reranking：
1）初始检索获取潜在相关的150个chunk
2）将150个chunk连同用户query丢给reranking模型
3）根据每个chunk与prompt的相关性与重要性进行评分，选择前20个
4）传递k个chunk给模型生成答案

Voyage和Gemini拥有表现最好的embeddings

---
*Created by Inspiration Manager*
*Timestamp: 2026-02-12 11:24*
