---
layout: post
title: "Dreamforce 2026：Agentforce 里怎么选模型"
date: 2026-10-04 00:00:00 +0800
author: jair
image: "https://article.asset.jairzh.com/img/2026/10/hsr9xe-9p9imq.png"
description: "Prompt 调了很多遍，Agent 还是不稳定？从 Dreamforce Mini Hacks 看 Agentforce 模型配置层次、按任务选择模型，以及怎样用测试做取舍。"
categories: ai
tags: "dreamforce agentforce models koa"
translation_key: dreamforce-2026-agentforce-model-selection
---
今年去了 Dreamforce 2026 现场。Mini Hacks 的五个题目里，三个讲 MCP，另外两个分别讲模型选择和 React。上一篇写的是把 Salesforce 能力接进 AI 工具，这篇换个角度：**给 Agent 装上了 Tools，接下来由哪个模型决定怎么用？**

模型选择的题目由 Google 赞助。我的体会是，模型不应该只在项目开始时选一次，之后所有效果问题都靠改 Prompt 解决。它应该和 Action 设计、上下文准备、测试一起，成为开发时可以调整的一项配置。

### 1. 先弄清楚：到底是哪一层在用这个模型

Agentforce 可以在 Org 层设置默认模型，也可以在 Agent Script 中用 `model_config` 为 Agent 或 Subagent 覆盖默认值。优先级是 **Subagent → Agent → Org 默认**。这不是让每个 Agent 都重新配置一遍，而是给需要特殊处理的任务留出选择空间。[官方配置说明](https://developer.salesforce.com/docs/ai/agentforce/guide/ascript-model.html)
![psbudt-mv1k7e](https://article.asset.jairzh.com/img/2026/10/psbudt-mv1k7e.png)
Prompt Template 的模型则是另一个检查点。Agent 调用一个 Prompt Template 时，不要直接假设它和 Agent 的推理过程用的是同一个模型；排查时需要分别看配置。
![scr-20261004-pnee-8p5dh0](https://article.asset.jairzh.com/img/2026/10/scr-20261004-pnee-8p5dh0.png)

### 2. Prompt 调不动的时候，先诊断，再换模型

**Prompt 优化到头了，可以换个模型试试。** 这句话的前提，是已经把更基础的问题排除了。

可以先分清三类失败：

- 没拿到必要信息：检查上下文、检索和 Action 返回值。
- 选错 Tool、传错参数：检查名称、描述、参数约束和指令，再比较模型。
- Tool 执行失败：先看权限、校验和业务逻辑，不把调用错误算成模型理解能力的问题。

换模型值得试，但它不能替代这些检查。尤其是操作业务数据的 Agent，回答读起来很顺，并不等于任务完成了。

### 3. 按任务分配，而不是全局升级

选择原则可以是：先用一个响应速度和成本都能接受的模型建立基线，再看哪些任务需要更强的推理能力。

简单的信息查询，与需要理解模糊输入、补齐条件、连续调用多个 Action 的任务，难度不一样。可以把后一类任务放到独立的 Subagent 中比较模型，而不是因为一个复杂场景就把整个 Agent 都升级。

这是一种开发取舍，**不是“价格低的模型一定够用”的结论**。也不要把模型名称里的档位直接当成效果排名。同一个模型在不同 Tools、不同语言和不同上下文下，表现可能不同。

不是“哪个模型最强”，而是“这类任务需要什么能力，现有模型具体在哪一步失败”。把问题排除清楚，下一轮测试才有方向。

### 4. 比模型之前，先准备一批不会随手改的例子

如果每次换模型都临时想一句话试试，最后很容易只记住最好的一次回答。我建议固定一批小而有代表性的测试输入，用同样的 Tools、同样的数据和同样的指令比较。同时配合 Salesforce 的 Agentforce Test Suites 进行自动化的测试。

换完模型，再把这些例子重跑一遍，并多运行几次观察波动。更换时尽量只改一个变量，否则很难知道改善来自模型、Prompt，还是数据。

### 5. Salesforce 新模型 Koa

Salesforce 这次介绍了基于 NVIDIA Nemotron 的 CRM 推理模型 Koa，面向多步骤业务任务。官方页面目前仍列有 Pilot 和美国区域 GA 预期；[Koa 官方介绍](https://www.salesforce.com/agentforce/koa/)

对开发者来说，我觉得更值得观察的是：面向 CRM 任务优化的模型，能否减少选错 Action、漏条件和多步骤执行中的错误。整体指标可以帮助了解方向，但最终仍要用自己的任务和测试数据判断。

### 相关推荐

选一个已经能运行的 Agent，保存当前版本和一组典型输入，再在另一个版本中比较模型。暂时不同时修改 Prompt 和 Tools，也不急着激活新版本。

如果还没用过新版 Builder，可以先通过 Trailhead 熟悉 Agent Script。老版 Agent 的迁移也应作为独立工作处理，迁移前后行为和换模型效果分开验证。

- [Quick Start: Prompt Builder](https://trailhead.salesforce.com/content/learn/projects/quick-start-prompt-builder)
- [Build an Agent Using Agentforce DX](https://trailhead.salesforce.com/content/learn/projects/create-an-agent-using-pro-code-tools)

### 总结

LLM Model 是 Agent 的大脑，所以要了解模型能力，基于当前的业务场景选择合适的模型。

配置和调试过程 **先找清失败在哪一步，再决定改上下文、改 Tool、改 Prompt，还是换模型。** 具体可以了解一下 Salesforce 推出的 Agentforce Studio 中的 Observe & Optimize 的相关功能。

---

> 同步自 SalesforceTalk 公众号。[阅读微信原文](https://mp.weixin.qq.com/s/3PmUq6d1Joe2fIW5Le7Ijg)。
