---
title: "How Agentic AI Changes the Work of Salesforce Admins and Developers"
date: 2026-05-25 20:00:00 +0800
last_modified_at: 2026-10-03 21:48:42 +0800
permalink: /en/blog/2026/05/25/agentic-era-salesforce-development
translation_key: agentic-era-salesforce-development
description: "An English text edition of the original infographic on intent, tools, evaluation, and changing Salesforce roles."
categories: ["ai"]
tags: ["agentic-ai", "tdx"]
---

Agentic AI changes what Salesforce administrators and developers build, and how they build it. My original article presented this idea as a Chinese infographic based on TDX 2026. This English text edition makes its main points available without requiring the reader to interpret the image.

The shift is toward **intent-driven development**: describe the outcome, let AI assist with planning and implementation, then evaluate the result against the business requirement.

## 1. What makes an agent different?

An agent combines interpreting an instruction with reasoning, planning, and taking actions through available tools. A request such as “send the report to the sales team” can involve several steps rather than one response.

This is a useful design direction, not a guarantee that an agent always understands the request correctly. The outcome still depends on its instructions, tools, permissions, and the evidence supplied to it.

## 2. Why it matters for Salesforce work

AI can help interpret requirements, generate code and configuration, handle repetitive tasks, and connect work across systems. That changes the allocation of effort: less time on repetitive production of artifacts, more on architecture, design, evaluation, and improvement.

The developer's responsibility does not disappear when a model generates the code. Someone still has to decide whether the implementation fits the requirement and operates safely in the platform.

## 3. From writing every line to guiding the work

The infographic describes a workflow with four parts:

1. Express the intended outcome.
2. Let an AI assistant reason about and generate an implementation.
3. Review the resulting artifact.
4. Correct and improve it through feedback.

The skills this requires include making intent clear, evaluating generated code, understanding architectural trade-offs, and shaping the behavior of an agent. Producing code is one part of a wider engineering process.

## 4. Openness, choice, and trust

Another theme is using AI tools alongside the Salesforce platform rather than limiting all work to one interface. MCP is one way to connect a compatible AI client to capabilities exposed by a service.

The original infographic contrasted external coding assistants with the platform's own developer experience. The practical question is which environment fits the work, while maintaining the required controls around code, data, and access.

For more detail on exposing useful capabilities through MCP, see my [Dreamforce 2026 MCP article](/en/blog/2026/10/03/dreamforce-2026-salesforce-mcp).

## 5. An application and an agent can work together

An application provides visible structure: components, forms, validation, and a familiar workflow. An agent provides a conversational way to interpret a request and coordinate several steps.

These strengths are complementary. A conversation can guide the task, while a form or record view supplies a clear interface for entering details and reviewing the result. The best experience does not have to force every interaction into text.

## 6. Evaluate behavior, not only artifacts

Traditional application tests check defined behavior under particular inputs. Agent evaluation also needs to examine how the agent interprets varied requests, chooses tools, and handles uncertainty.

The infographic highlights Testing Center and observability as parts of that process. The underlying practice is to test representative prompts, inspect failures, and improve the behavior with evidence.

A successful demonstration is not enough. Check variations in wording, missing information, tool failures, and whether the agent asks for clarification at the right point.

## 7. What this means for administrators

For administrators, the opportunity is to use natural-language assistance with configuration and automation, and to spend more time understanding business problems.

Flows, reports, pages, and formulas still need a coherent design. AI assistance is most useful when the administrator can explain what the business needs and verify that the resulting configuration satisfies it.

## 8. The skills worth developing

The infographic brings together several capabilities:

- Understanding the business and its users.
- Expressing intent clearly.
- Working with AI while judging the output critically.
- Reviewing quality and applying governance.
- Testing and observing agent behavior.
- Continuing to learn as the tools change.

These build on existing platform knowledge. Knowing the data model, security model, and transaction boundaries remains important when another system helps produce the implementation.

## 9. Learn with the community

Hands-on experiments, hackathons, and shared Trailblazer examples help turn an abstract idea into practical experience. Study how others approach the same problem, then test the pattern against your own requirements.

My main takeaway from the infographic is that administrators and developers increasingly act as designers and reviewers of AI-assisted work. Their business understanding and technical judgment become more important as more of the implementation can be generated.

Source for the original infographic: [TDX 2026 Highlights, S1E33](https://www.salesforce.com/plus/experience/tdx_2026/series/tdx_2026_highlights/episode/episode-s1e33).

---

> Originally published on 2026-05-25. English edition revised on October 3, 2026. Adapted the Chinese infographic into an English text article. [Read the Chinese original](/blog/2026/05/25/agentic-era-salesforce-development).
