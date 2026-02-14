---
title: AI Agent Workflow 模式分类
date: 2026-02-14
category: AI/提示词工程
tags: [agent,  workflow,  prompt-chaining,  routing,  parallelization,  orchestration]
source: 
---

# AI Agent Workflow 模式分类

不同Workflow和Agent模式：

Prompt chaining（提示词链）：
- 适用场景：任务可以轻松且清晰地分解为固定子任务
- 优点：通过使每次LLM调用成为更简单的任务，以延迟为代价换取更高的准确度
- 示例：大纲 → 校验 → 文档

Routing（路由）：
- 适用场景：存在明显的类别，且分类可以由LLM或更传统的分类模型/算法准确完成
- 示例：把问题路由，按不同难度分发给不同模型处理

Parallelization（并行化）：
- 适用场景：分解后可并行化提高速度，或需要多个视角获取更高置信度的结果
- 示例1：安全防护 - 一个模型处理请求，另一个模型并行处理请求筛选
- 示例2：代码审核 - 不同角度审核代码问题

Orchestrator-workers（编排器-工作者）：
- 适用场景：无法预测所需子任务的复杂任务
- 示例：编程中需要修改的文件数量及每个文件中修改的性质通常取决于具体任务

Evaluator-optimizer（评估器-优化器）：
- 适用场景：有明确的评估标准，且迭代优化能带来可衡量的价值
- 示例：Research任务 - evaluator评估是否继续进一步搜索

Agents（智能体）：
- 适用场景：开放式问题

---
*Created by Inspiration Manager*
*Timestamp: 2026-02-14 05:17*
