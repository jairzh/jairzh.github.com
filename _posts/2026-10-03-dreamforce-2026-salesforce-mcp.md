---
layout: post
title: "Dreamforce 2026：Salesforce MCP 篇"
date: 2026-10-03 00:00:00 +0800
author: jair
image: "https://article.asset.jairzh.com/img/2026/10/26-mini-hack-x3e6ff.png"
description: "从 Dreamforce 2026 Mini Hacks 看 Salesforce MCP：AIforce 界面层、Tools 设计、认证与权限、Flex Credits 成本，以及模型和 React 的开放。"
categories: ai
tags: "dreamforce mcp agentforce aiforce"
---
今年去了 Dreamforce 2026 现场。每年 Dreamforce 都有一些动手的小实验，叫 **Mini Hacks**，用来让大家体验当年主推的技术。今年一共五个题目，**三个都和 MCP 有关**，剩下两个一个讲模型选择，一个讲 React。

题目的分布本身就说明了方向：**Salesforce 不再只在自己的界面里做 AI，而是要把 CRM 的能力送到用户正在用的 AI 工具里。**

这篇不复述操作步骤（文末有官方 Workshop 和 Trailhead 链接），只讲作为一线开发者，我觉得最值得关注的五件事。

### 1. 多了一层「界面层」：AIforce

按今年 Keynote 的说法，Salesforce 现在分成四层：
![scr-20261003-jvml-d83cf5](https://article.asset.jairzh.com/img/2026/10/scr-20261003-jvml-d83cf5.jpeg)
- **界面层**：AIforce。首批入口有 Claude 里的 Claudeforce、Slack 里的 Slackforce，以及 Lightning 里的 Agentforce Coworker
- **Agent 层**：Agentforce
- **应用与语义层**：Sales Cloud、Service Cloud 等
- **数据层**：Data 360

为什么要加一层界面层？员工一天的工作不只在 CRM 里，还有邮件、文档、PPT 和各种沟通工具。越来越多的人直接在 AI 工具里干活，CRM 如果不进入这个生态，就会被孤立；让用户专门回到 Salesforce 里去用 Agent，体验并不好。

把能力送出去的方式就是 **MCP（Model Context Protocol）**：AI 客户端和外部服务之间的标准协议。服务把能力暴露成 Tools，每个 Tool 带一段自然语言描述，AI 根据描述决定调用哪个。

在 Salesforce 里，Setup 里有 MCP 设置，标准 Server 自带 CRUD 和搜索；自定义 Server 可以把 Flow、Apex、Agent、Prompt Template 加成 Tools。配好之后得到一个 URL，填进任何支持 MCP 的客户端就能用。Tableau Next 和 Slack 两个 Mini Hacks 也是同一个套路，只是能力的来源和入口不同。

### 2. 开发的重心：从做页面，到做 Tools

Mini Hack 里我先试了只用标准 Server：AI 确实能靠 CRUD 完成一个多步骤的预约流程，但要逐个对象查询、逐个创建，**很慢，而且每次都要重新理解对象之间的关系**，结果也不稳定。

换成把整个流程封装成一个 Flow、再作为一个 Tool 暴露出去，一次调用就完成了。

我的体会是：

- **高频的核心流程，要封装成一个粒度合适的 Tool**，不要指望 AI 用 CRUD 自己拼。
- **Tool 的描述就是新的 UI**。以前用户看的是按钮和表单，现在「看」的是模型，名字、描述、参数说明写得清不清楚，直接决定 AI 会不会选对、传对。
- **标准 Server 不能改，但可以组合**。新建自定义 Server，用 Add from Server 把需要的标准 Tools 加进来，再加上自己写的 Apex Action，按角色组合成不同的 Server。

**Agent 有大脑，没有手脚，能做什么取决于我们给它的 Tools。**

### 3. 认证和安全要提前想清楚

连接走的是 **External Client App（ECA）**，几个关键配置容易踩坑：

- OAuth Scope 要选 `mcp_api`，普通的 `api` 不行，另外加上 `refresh_token`
- 勾选 Require PKCE，以及 Issue JWT-based access tokens for named users

比配置更重要的是**以谁的身份运行**。外部 AI 调用你暴露的 Apex 时，是以授权用户的身份来的，所以自己写的 Apex Action 要老老实实遵守共享规则和字段权限（`with sharing`、`WITH USER_MODE`），模型传进来的参数也要当成用户输入来对待，不能直接拼进动态 SOQL。以前「只有内部页面会调用」的假设，在 MCP 时代不成立了。

另外 Salesforce 在推出了新的权限配置，叫 **Scoped Access**，这个可以解决在 AI 客户端使用 user 什么去运用，但 user 什么权限又太大的问题。**Scoped Access** 可以单独在 User 的基础上去掉/增加相关的权限，保证在 AI 环境下的安全性。

### 4. 成本：MCP 调用将按 Flex Credits 计费

这一点做方案时很容易忽略。现场分会场提到，**生产环境里成功的 MCP 调用会消耗 Flex Credits**。按公开报道，每次成功调用会计为一次「Headless Platform Interaction」，在 Digital Wallet 里统计；具体倍率官方还没公布，计划 11 月前后随 Agent 注册和安全管控一起推出，开始计费前会提前 30 天通知。

这反过来又印证了第 2 点：**Tool 粒度越合适，调用次数越少，成本越低。** 让 AI 用 CRUD 一步步拼，不只是慢，还贵。

### 5. 模型和前端也在同步开放

另外两个 Mini Hacks 看起来和 MCP 无关，其实是同一个思路：**更开放**。

- **模型可以换**：Prompt Template、Agent 可以用不同的模型，新版 Builder（Agent Script）里每个 Subagent 还能用 `model_config` 单独指定。经验是：Prompt 优化到头了效果还不稳，就换个模型测；先用低档模型打底，只给复杂的 Subagent 配强模型；换完模型一定要把测试用例重跑一遍，不同模型调 Tools 的习惯不一样。老版 Builder 的 Agent 可以用官方 Upgrade 功能转成新版。
- **前端可以用 React**：Multi-Framework 已经 GA，React 应用以 UI Bundle 的形式部署在 Salesforce 上。现场我问了「React 还是 LWC」，官方的回答很直接：团队擅长 React 就用 React。我的判断是，对外、复杂的单页应用适合 React；需要 Lightning 标准组件的内部应用，LWC 依然是主力。

### 动手实践

目前 Trailhead 已经同步推出 AIforce 相关的模块，有兴趣可以去 Trailhead 搜索，动手实践体验下。

### 总结

Dreamforce 2026 给我最大的感受是：**Salesforce 开发者的工作对象正在改变。** 过去我们给人做页面，现在还要给 AI 做 Tools：粒度要合适，描述要写清楚，权限要守住，另外还有相关的 AI 成本问题。。

---

> 本文首发于微信公众号「SalesforceTalk」，[阅读原文](https://mp.weixin.qq.com/s/fECELCxZHQV_L_18pKWbCg)。
