---
title: "Dreamforce 2026: Choosing Models in Agentforce"
date: 2026-10-04 00:00:00 +0800
permalink: /en/blog/2026/10/04/dreamforce-2026-agentforce-model-selection
translation_key: dreamforce-2026-agentforce-model-selection
image: "https://article.asset.jairzh.com/img/2026/10/hsr9xe-9p9imq.png"
description: "When prompt changes stop helping, diagnose the failure and compare models: configuration levels, task-specific choices, repeatable tests, and Salesforce Koa."
categories: [ai]
tags: [dreamforce, agentforce, models, koa]
---

I attended Dreamforce 2026 in person this year. Of the five Mini Hacks, three focused on MCP; the other two covered model selection and React. My [previous article](/en/blog/2026/10/03/dreamforce-2026-salesforce-mcp) explored how Salesforce capabilities connect to AI tools. This one looks at the next question: **once an agent has tools, which model decides how to use them?**

The model-selection challenge was sponsored by Google. My takeaway was that we should not choose a model once at the start of a project and then try to solve every performance problem by changing prompts. Model selection belongs in the development process alongside action design, context preparation, and testing.

### 1. Understand which layer is using the model

Agentforce supports an org-level default model. In Agent Script, `model_config` can override that default for an agent or an individual subagent. The precedence is **subagent → agent → org default**. This gives particular tasks their own model choice without requiring every agent to be configured separately. [Official configuration guide](https://developer.salesforce.com/docs/ai/agentforce/guide/ascript-model.html)

![Agentforce model configuration](https://article.asset.jairzh.com/img/2026/10/psbudt-mv1k7e.png)

A Prompt Template's model is a separate setting to check. When an agent calls a Prompt Template, do not assume the template uses the same model as the agent's reasoning process. Inspect both configurations when troubleshooting.

![Prompt Template model settings](https://article.asset.jairzh.com/img/2026/10/scr-20261004-pnee-8p5dh0.png)

### 2. Diagnose the failure before switching models

**When prompt optimization stops helping, try another model.** But first rule out more basic problems.

I would separate failures into three groups:

- **Missing information:** inspect the context, retrieval results, and action outputs.
- **Wrong tool or parameters:** check names, descriptions, parameter constraints, and instructions, then compare models.
- **Tool execution errors:** investigate permissions, validation, and business logic. Do not count an execution failure as evidence that the model misunderstood the task.

Changing models is worth testing, but it cannot replace those checks. For an agent that operates on business data, a fluent answer does not prove that the task was completed.

### 3. Assign models by task rather than upgrading everything

Start with a model whose latency and cost are acceptable, and establish a baseline. Then identify the tasks that need stronger reasoning.

A straightforward lookup differs from a task that involves ambiguous input, missing details, and a sequence of actions. You can isolate the latter in a subagent and compare models there, rather than upgrading the entire agent because one workflow is difficult.

This is a development tradeoff, **not a claim that a cheaper model will always be sufficient**. Model names and tiers are not a substitute for evaluating results. The same model can behave differently with different tools, languages, and context.

The useful questions are: what capabilities does this task require, and at which step does the current model fail? A clear diagnosis gives the next test a purpose.

### 4. Prepare consistent examples before comparing models

If you invent a new test sentence every time you change models, you may remember only the best response. I recommend keeping a small, representative set of test inputs and comparing models with the same tools, data, and instructions. Use Salesforce's Agentforce Test Suites to support automated testing as well.

After a model change, rerun those examples several times to observe variation. Change one variable at a time where possible; otherwise, it becomes difficult to tell whether the improvement came from the model, the prompt, or the data.

### 5. Salesforce's new Koa model

Salesforce introduced Koa, a CRM reasoning model built on NVIDIA Nemotron for complex, multi-step business tasks. As of October 4, 2026, its official page still describes availability for selected pilot customers and an expected GA release in U.S. regions. [Official Koa introduction](https://www.salesforce.com/agentforce/koa/)

What interests me as a developer is whether a model optimized for CRM tasks can reduce incorrect action selection, missed conditions, and errors across multi-step workflows. Aggregate metrics can indicate a direction, but the decision still needs to come from your own tasks and test data.

### A practical way to start

Take an agent that already works, save its current version and a set of typical inputs, and compare models in another version. Keep the prompt and tools unchanged initially, and do not rush to activate the new version.

If you have not used the newer Builder, start by learning Agent Script through Trailhead. Treat migration from an older agent as a separate change, so you can distinguish migration-related behavior from the effects of switching models.

- [Quick Start: Prompt Builder](https://trailhead.salesforce.com/content/learn/projects/quick-start-prompt-builder)
- [Build an Agent Using Agentforce DX](https://trailhead.salesforce.com/content/learn/projects/create-an-agent-using-pro-code-tools)

### Closing thoughts

The LLM is the agent's brain. Understand its capabilities and choose it for the business scenarios you actually need to support.

During configuration and debugging, **first identify where the failure occurs, then decide whether to change the context, tools, prompt, or model**. Agentforce Studio's Observe & Optimize capabilities are also worth exploring for that process.

---

> Translated from my Chinese article, published on October 4, 2026. [Read the Chinese blog version](/blog/2026/10/04/dreamforce-2026-agentforce-model-selection) or [the original on WeChat](https://mp.weixin.qq.com/s/3PmUq6d1Joe2fIW5Le7Ijg).
