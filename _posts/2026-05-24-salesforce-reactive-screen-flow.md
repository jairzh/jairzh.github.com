---
layout: post
title: "Salesforce Reactive Screen Flow"
date: 2026-05-24 11:33:23 +0800
author: jair
image: /assets/images/wechat/salesforce-reactive-screen-flow/cover.jpg
description: "Salesforce Screen Flow 通过 Reactivity 支持实时 UI 渲染：组件之间可以相互传值，借助 Formula 中转实现复杂计算和相互控制。"
categories: flow
tags: "screen-flow"
translation_key: salesforce-reactive-screen-flow
---
Salesforce Screen Flow 通过 Reactivity 支持实时 UI 的渲染，可以通过组件之间相互传值，利用 Formula 作为中转实现复杂的计算，实现相互的控制，比如禁用组件，动态改变组件的值等。

比如：

- **组件控制组件（直接联动）**：将复选框组件（Confirmation）的布尔输出直接绑定到文本组件（Subject）的 Disabled（禁用）属性。当用户勾选确认时，文本框瞬间置灰，防止二次修改。
- **公式中转控制（逻辑路由）**：利用文本组件输入的值作为公式（Formula）的入参，通过公式进行文本模糊匹配（如判断是否包含 "urgent" ）。公式计算出的结果（如返回 "High"）再实时塞给单选按钮（Priority）作为默认值，实现智能引导。
- **多源输入计算（复杂数学/日期运算）**：将单选按钮选择的“优先级天数”与滑块组件（Slider）拖动的“微调天数”共同作为输入源，交由日期公式进行实时加减，直接驱动最终日期组件（Due Date）的实时渲染。
- **条件显示控制（UI 显隐分流）**：将滑块组件的数值输出作为下游 Display Text 组件的“条件可见性（Conditional Visibility）”判定条件。当拖动滑块 > 3 天时瞬间弹出审批警告，<3 天时则自动切换为绿色通行提示，实现单页内的动态交互引导。

---

> 本文首发于微信公众号「SalesforceTalk」，[阅读原文](https://mp.weixin.qq.com/s/3TXnZwBdlrJxRXBdXvyDWQ)。
