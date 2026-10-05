---
title: "Dreamforce 2026: React on Salesforce, and When to Choose It"
date: 2026-10-05 00:00:00 +0800
permalink: /en/blog/2026/10/05/dreamforce-2026-react-in-salesforce
translation_key: dreamforce-2026-react-in-salesforce
image: "https://article.asset.jairzh.com/img/2026/10/dbd4d4-5qr0ng.png"
description: "A developer's perspective on React on Salesforce: UIBundle, the boundary between React and LWC, Microfrontends, and framework choices with AI coding tools."
categories: [development]
tags: [dreamforce, react, lwc, multiframework]
---

One of the five Mini Hacks at Dreamforce this year was a development challenge: build an application with React and run it on Salesforce. The first two articles covered [MCP](/en/blog/2026/10/03/dreamforce-2026-salesforce-mcp) and [model selection](/en/blog/2026/10/04/dreamforce-2026-agentforce-model-selection). This one is about the front end.

For Salesforce front-end development, the familiar progression has been Visualforce, Aura, and LWC. With **Salesforce Multi-Framework**, we can now develop React applications and deploy them to Salesforce.

At the event, I asked whether to choose React or LWC. The answer was direct: if your team knows React, use React. That made me think the real questions are where the application will run, which platform capabilities it needs, and how comfortably the team can maintain it.

### 1. How a React application becomes a Salesforce application

The key concept is **UIBundle**. It brings the front-end application into a Salesforce DX project as metadata. Development uses a front-end toolchain, while deployment follows Salesforce workflows. [Multi-Framework overview](https://developer.salesforce.com/docs/platform/multiframework/guide/multiframework-overview.html)

![Salesforce Multi-Framework development](https://article.asset.jairzh.com/img/2026/10/xu4nmb-sjsfy6.png)

The front-end code can be delivered alongside objects, Apex, and permission configuration in the same Salesforce DX project. Choosing React does not require a completely separate application-management approach.

Define the audience early. An internal application for employees differs from an external application for customers or partners in its entry points, templates, and access arrangements.

![Salesforce Multi-Framework application example](https://article.asset.jairzh.com/img/2026/10/mwtcuo-hnv92y.png)

![A React application running on Salesforce](https://article.asset.jairzh.com/img/2026/10/e9r8mi-g37ktq.png)

### 2. React vs. LWC

My approach is: **start with LWC when you need deep integration into standard Lightning pages; seriously consider React for a complete application with clear boundaries and complex interactions.**

For example, a small record-page component may depend on Lightning Base Components, page context, and App Builder configuration. LWC is usually a natural fit. Switching frameworks for that local requirement could mean spending extra time reconnecting platform capabilities.

A standalone workspace is a different case. It may have its own navigation and complex interactions, and the team may already have React components and experience. Using that familiar ecosystem can offer more value.

You can compare this distinction with the official documentation's [LWC and Multi-Framework guidance](https://developer.salesforce.com/docs/platform/multiframework/guide/multiframework-overview.html).

![React and LWC comparison from the event](https://article.asset.jairzh.com/img/2026/10/wtib6w-aruohe.png)

### 3. Embedding in Lightning

If a React application needs to appear inside a Lightning page, Salesforce Microfrontends provides an embedding option. It is currently documented as **Beta**, using an isolated iframe and a communication channel to connect the application with the surrounding page. [Microfrontends embedding guide](https://developer.salesforce.com/docs/platform/multiframework/guide/mfw-ui-embedding.html)

Running an application independently and embedding it are distinct usage scenarios. Test sizing, page transitions, and event communication in the actual embedded application.

### 4. AI can help build pages; the use case still guides the choice

The Mini Hack code was generated through prompts in Agentforce Vibes. That is one reason React interests me: its established ecosystem has many components, tools, and examples that can help us build a prototype quickly.

AI coding tools are now very capable. In my view, implementation effort alone is therefore becoming less decisive when choosing between React and LWC. Understand the strengths and limitations of each, then choose for the business scenario.

### What to try now

Start with the official [Multi-Framework Quick Start](https://developer.salesforce.com/docs/platform/multiframework/guide/mfw-quick-start.html).

For specific data operations, explore the React examples in [multiframework-recipes](https://github.com/trailheadapps/multiframework-recipes).

### Closing thoughts

React gives Salesforce developers another valuable front-end option. My criteria remain the same: clear application boundaries, the required depth of platform integration, and the team's ability to maintain the solution.

**Define the use case first, then choose the framework.** Let LWC handle the integrated components it is well suited to, and use React where complete applications and ecosystem reuse offer an advantage.

The event roadmap also described Angular support reaching GA in October 2026 and Vue support arriving around Lunar New Year 2027. Those were roadmap expectations; actual availability depends on the official release and your target environment. The current official overview already lists React and Angular templates.

### References

- [Salesforce Multi-Framework overview and framework comparison](https://developer.salesforce.com/docs/platform/multiframework/guide/multiframework-overview.html)
- [Project structure and UIBundle metadata](https://developer.salesforce.com/docs/platform/multiframework/guide/mfw-project-structure.html)
- [React Multi-Framework GA release and beta migration](https://developer.salesforce.com/blogs/2026/07/build-with-react-on-salesforce-multi-framework-is-now-ga): older tutorials use different Data SDK and application-entry configurations; check them against current documentation.
- [Multi-Framework Quick Start](https://developer.salesforce.com/docs/platform/multiframework/guide/mfw-quick-start.html)
- [Microfrontends embedding guide (Beta)](https://developer.salesforce.com/docs/platform/multiframework/guide/mfw-ui-embedding.html)
- [Official multiframework-recipes examples](https://github.com/trailheadapps/multiframework-recipes)
- [Trailhead: Salesforce Multi-Framework: Quick Look](https://trailhead.salesforce.com/content/learn/modules/salesforce-multi-framework-quick-look)

> Based on my Dreamforce 2026 experience and personal assessment. The blog edition restores official documentation, examples, and learning links. Product status reflects documentation checked on October 5, 2026; roadmap dates depend on actual releases. [Read the Chinese version](/blog/2026/10/05/dreamforce-2026-react-in-salesforce).
