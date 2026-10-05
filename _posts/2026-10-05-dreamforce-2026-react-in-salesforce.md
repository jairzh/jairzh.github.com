---
layout: post
title: "Dreamforce 2026：React 正式登陆 Salesforce，开发者怎么选"
date: 2026-10-05 00:00:00 +0800
author: jair
image: "https://article.asset.jairzh.com/img/2026/10/dbd4d4-5qr0ng.png"
description: "从 Dreamforce Mini Hacks 看 React on Salesforce：UIBundle、React 与 LWC 的应用边界、Microfrontends，以及 AI 编程下的框架选择。"
categories: development
tags: "dreamforce react lwc multiframework"
translation_key: dreamforce-2026-react-in-salesforce
---
今年 Dreamforce 的五个 Mini Hacks 里，有一道纯开发题：用 React 做一个运行在 Salesforce 上的应用。前两篇分别聊了 [MCP](/blog/2026/10/03/dreamforce-2026-salesforce-mcp) 和 [模型选择](/blog/2026/10/04/dreamforce-2026-agentforce-model-selection)，这篇聊前端。

过去做 Salesforce 前端，常见的演进路线是 Visualforce、Aura、LWC。现在通过 **Salesforce Multi-Framework**，我们可以用 React 开发应用，并把它部署到 Salesforce。

我在现场问过「React 还是 LWC」这个问题。官方的回答很直接：团队擅长 React，就用 React。这个回答让我觉得，真正该讨论的不是哪种框架更新，而是这个应用准备放在哪里、需要哪些平台能力，以及团队维护起来是否顺手。

### 1. React 应用如何成为 Salesforce 应用

关键概念是 **UIBundle**。它把前端应用作为 Metadata 纳入 Salesforce DX 项目；开发时使用前端工具链，部署时走 Salesforce 的流程。[Multi-Framework 概览](https://developer.salesforce.com/docs/platform/multiframework/guide/multiframework-overview.html)
![xu4nmb-sjsfy6](https://article.asset.jairzh.com/img/2026/10/xu4nmb-sjsfy6.png)
Salesforce Multi-Framework 前端代码能和对象、Apex、权限配置交付，都在 Salesforce DX 项目里。不必因为用了 React，就另起一套完全独立的应用管理方式。

同时，应用的受众要先定清楚：给员工用的内部应用，和面向客户、伙伴的外部应用，入口、模板和访问方式不同。

![mwtcuo-hnv92y](https://article.asset.jairzh.com/img/2026/10/mwtcuo-hnv92y.png)

![e9r8mi-g37ktq](https://article.asset.jairzh.com/img/2026/10/e9r8mi-g37ktq.png)
### 2. React vs LWC

选择 React 还是 LWC，我的判断是，**需要深度融入 Lightning 标准页面时，优先看 LWC；做一个边界清楚、交互复杂的完整应用时，认真考虑 React。**

例如给记录页加一个小组件，依赖现成的 Lightning Base Components、页面上下文和 App Builder 配置，LWC 通常更顺手。为了这个局部需求换框架，可能把时间花在重新连接平台能力上。

另一种场景是独立的工作台：有自己的页面导航、复杂交互，团队已经有 React 组件和开发经验。这时继续使用熟悉的生态，可能更有价值。
这一定位也可以对照官方文档中的 [LWC 与 Multi-Framework 比较](https://developer.salesforce.com/docs/platform/multiframework/guide/multiframework-overview.html)。

![wtib6w-aruohe](https://article.asset.jairzh.com/img/2026/10/wtib6w-aruohe.png)

### 3. 嵌进 Lightning

如果 React 应用需要出现在 Lightning 页面中，可以使用 Microfrontends，目前还是 beta；通过隔离的 iframe 和通信通道连接外层页面。[Microfrontends 嵌入指南](https://developer.salesforce.com/docs/platform/multiframework/guide/mfw-ui-embedding.html)

独立运行与嵌入页面是两种使用场景；嵌入后的尺寸、页面切换和事件通信，还需要结合实际应用验证。

### 4. AI 能帮我们写页面，选择仍然要由场景决定

Mini Hack 中的代码通过 Agentforce Vibes 的 Prompt 生成，这也是我觉得 React 值得关注的原因之一：标准前端生态里已有大量组件、工具和示例，可以帮助我们更快搭出一个原型。

目前 AI coding 能力已经非常强，所以技术已经不是选择 React 和 LWC 的关键要素，还是要弄清楚两个的优势和劣势之后，结合业务场景进行选项。

### 现在就能动手试什么

如果想体验，可以直接使用 [Multi-Framework 官方 Quick Start](https://developer.salesforce.com/docs/platform/multiframework/guide/mfw-quick-start.html)。

想看具体数据操作，可以参考 [multiframework-recipes](https://github.com/trailheadapps/multiframework-recipes) 的 React 示例。

### 总结

React 加入 Salesforce，让前端方案多了一个有实际价值的选择。对我来说，选择标准还是一样：应用边界是否清楚，平台集成需要多深，团队是否能持续维护。

**先定场景，再选框架。** 让 LWC 做它擅长的集成组件，让 React 发挥完整应用和生态复用的优势。另外，现场分享提到 Angular 支持计划在 2026 年 10 月 GA，Vue 预计在明年春季支持。这些是当时的路线图预期，实际可用性以官方发布和目标环境为准。当前官方概览已经列有 React、Angular 模板。

### 参考资料

- [Salesforce Multi-Framework 概览与框架选择](https://developer.salesforce.com/docs/platform/multiframework/guide/multiframework-overview.html)
- [项目结构与 UIBundle Metadata](https://developer.salesforce.com/docs/platform/multiframework/guide/mfw-project-structure.html)
- [React Multi-Framework GA 更新与 Beta 迁移说明](https://developer.salesforce.com/blogs/2026/07/build-with-react-on-salesforce-multi-framework-is-now-ga)：旧教程中的 Data SDK 和应用入口配置有变化，使用时请核对当前文档。
- [Multi-Framework Quick Start](https://developer.salesforce.com/docs/platform/multiframework/guide/mfw-quick-start.html)
- [Microfrontends 嵌入指南（Beta）](https://developer.salesforce.com/docs/platform/multiframework/guide/mfw-ui-embedding.html)
- [官方 multiframework-recipes 示例](https://github.com/trailheadapps/multiframework-recipes)
- [Trailhead：Salesforce Multi-Framework: Quick Look](https://trailhead.salesforce.com/content/learn/modules/salesforce-multi-framework-quick-look)

> 本文基于 Dreamforce 2026 现场体验与个人判断。博客版补回了官方文档、示例和学习链接；产品状态按 2026 年 10 月 5 日查阅的资料说明，路线图时间以实际发布为准。
