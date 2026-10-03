---
title: "Dreamforce 2026: Five Takeaways on Salesforce MCP"
date: 2026-10-03 00:00:00 +0800
permalink: /en/blog/2026/10/03/dreamforce-2026-salesforce-mcp
translation_key: dreamforce-2026-salesforce-mcp
image: "https://article.asset.jairzh.com/img/2026/10/26-mini-hack-x3e6ff.png"
description: "A developer's perspective on Salesforce MCP at Dreamforce 2026: AIforce, tool design, authentication and permissions, Flex Credits, and greater choice in models and front-end frameworks."
categories: [ai]
tags: [dreamforce, mcp, agentforce, aiforce]
---

I attended Dreamforce 2026 in person this year. Every year, Dreamforce offers small hands-on challenges called **Mini Hacks** to introduce the technologies Salesforce is focusing on. This year there were five challenges: **three involved MCP**, while the other two covered model selection and React.

That mix of topics tells a story: **Salesforce is bringing CRM capabilities into the AI tools people already use.**

Rather than walk through the setup steps, I want to share five things that stood out to me as a developer working with Salesforce every day.

### 1. A new interface layer: AIforce

According to this year's keynote, Salesforce now has four layers:

![The four Salesforce layers presented at Dreamforce 2026](https://article.asset.jairzh.com/img/2026/10/scr-20261003-jvml-d83cf5.jpeg)

- **Interface layer:** AIforce. The initial entry points include Claudeforce in Claude, Slackforce in Slack, and Agentforce Coworker in Lightning.
- **Agent layer:** Agentforce.
- **Application and semantic layer:** Sales Cloud, Service Cloud, and other applications.
- **Data layer:** Data 360.

Why add an interface layer? Employees spend their day across email, documents, presentations, and communication tools as well as CRM. More people are doing their work directly in AI tools. CRM needs to participate in that ecosystem; asking users to return to Salesforce every time they need an agent adds friction.

The mechanism for bringing these capabilities into other tools is **MCP (Model Context Protocol)**, a standard protocol connecting AI clients to external services. A service exposes its capabilities as tools. Each tool has a natural-language description that helps the AI decide which one to call.

In Salesforce, MCP configuration lives in Setup. Standard servers provide CRUD and search tools. Custom servers can expose Flows, Apex, agents, and Prompt Templates as tools. Once configured, a server gives you a URL to add to an MCP-compatible client. The Tableau Next and Slack Mini Hacks follow the same pattern, with different sources of capabilities and different entry points.

### 2. Development is shifting from building pages to designing tools

During the Mini Hack, I first tried using only the standard server. The AI could complete a multi-step booking workflow using CRUD, but it had to query and create records object by object. **It was slow, and it had to work out the relationships between objects each time.** The results were inconsistent, too.

When I wrapped the entire workflow in a Flow and exposed it as a single tool, one call completed the process.

My takeaways:

- **Package frequently used core workflows into tools with the right scope.** Do not expect the AI to assemble them from individual CRUD operations every time.
- **The tool description is the new UI.** Previously, people interacted with buttons and forms. Now the model interprets tool names, descriptions, and parameter explanations. Their clarity directly affects whether it selects the right tool and passes the right inputs.
- **Standard servers cannot be modified, but their tools can be combined.** Create a custom server, use Add from Server to include the standard tools you need, then add your own Apex actions. This lets you assemble different servers for different roles.

**An agent has a brain, but its ability to act depends on the tools we give it.**

### 3. Plan authentication and security early

Connections use an **External Client App (ECA)**. A few configuration details are easy to miss:

- Select the `mcp_api` OAuth scope; the ordinary `api` scope is not sufficient. Add `refresh_token` as well.
- Enable Require PKCE and Issue JWT-based access tokens for named users.

The bigger question is **whose identity the tools run under**. When an external AI client calls your exposed Apex, it acts as the authorized user. Custom Apex actions therefore need to respect sharing rules and field permissions, using mechanisms such as `with sharing` and `WITH USER_MODE`. Treat parameters from the model as user input, and do not concatenate them directly into dynamic SOQL. In the MCP era, you can no longer assume that only an internal page will call your code.

Salesforce is also introducing a permission configuration called **Scoped Access**. It addresses the situation where an AI client acts on behalf of a user whose permissions are broader than the client needs. Scoped Access lets you adjust the permission scope for that context rather than simply relying on the user's full set of permissions.

### 4. Cost: MCP calls will consume Flex Credits

This is easy to overlook when designing a solution. A breakout session noted that **successful MCP calls in production will consume Flex Credits**. According to published reports, each successful call will count as a “Headless Platform Interaction,” tracked in Digital Wallet. The exact credit rate has not yet been announced. The rollout is planned for around November, alongside agent registration and security controls, with 30 days' notice before billing begins.

This reinforces the point about tool design: **well-scoped tools require fewer calls, which can reduce cost.** Having an AI assemble a workflow one CRUD operation at a time can be expensive as well as slow.

### 5. More choice in models and front-end frameworks

The other two Mini Hacks may seem unrelated to MCP, but they reflect the same direction: **greater openness**.

- **Models can be changed.** Prompt Templates and agents can use different models. In the new Builder, which uses Agent Script, each subagent can specify its own model through `model_config`. My approach: if prompt refinement stops improving inconsistent results, test another model. Start with a less expensive model, and reserve stronger models for the subagents handling complex tasks. Always rerun your test cases after changing models, because models differ in how they use tools. Agents built in the older Builder can move to the new version using the official Upgrade feature.
- **React is an option for the front end.** Multi-Framework is now generally available, and React applications can be deployed to Salesforce as UI Bundles. When I asked “React or LWC?” at the event, the answer was direct: if your team knows React, use React. My view is that React fits complex, customer-facing single-page applications, while LWC remains the main choice for internal applications that need standard Lightning components.

### Try it yourself

Trailhead now has modules covering AIforce. Search for them on Trailhead if you want to explore these capabilities hands-on.

### Closing thoughts

My biggest takeaway from Dreamforce 2026 is that **what Salesforce developers build for is changing**. We still build interfaces for people, and now we also design tools for AI. Those tools need the right scope, clear descriptions, carefully enforced permissions, and attention to AI costs.

---

> Translated from my Chinese article, originally published on the SalesforceTalk WeChat account. [Read the original on WeChat](https://mp.weixin.qq.com/s/fECELCxZHQV_L_18pKWbCg).
