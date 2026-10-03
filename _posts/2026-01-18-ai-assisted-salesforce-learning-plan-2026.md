---
layout: post
title: "用 AI 辅助制定 2026 Salesforce 学习计划"
date: 2026-01-18 12:56:57 +0800
author: jair
image: /assets/images/wechat/ai-assisted-salesforce-learning-plan-2026/cover.jpg
description: "每年年初制定学习计划是惯例。作为 Salesforce 从业者，通常我会根据 Trailhead 官网路径结合经验人工筛选证书目标。"
categories: ai
tags: "certification learning-plan"
translation_key: ai-assisted-salesforce-learning-plan-2026
---
每年年初制定学习计划是惯例。作为 Salesforce 从业者，通常我会根据 Trailhead 官网路径结合经验人工筛选证书目标。今年我决定换个思路：引入 AI（ChatGPT 和 Gemini）作为“咨询顾问”，对我的计划进行交叉验证。

## 1. 数据准备 (ChatGPT)

**遇到的坑**：起初直接让 Gemini 给推荐，结果给出来的结果都是我已经考过的证书。还是缺少相关的数据，正好 Trailhead Profile 上数据都有。尝试直接把 Trailhead Profile 的 URL 投喂给 Gemini，但受限于页面反爬策略，无法读取有效内容。

**解决方案**：使用 ChatGPT (配合 Atlas 浏览器) 访问 Trailhead 主页，利用其强大的文本提取能力进行“人肉”抓取。

> **Prompt:**  
> 把这个页面的信息，详细输出成 markdown 格式，保留所有详细的信息，不要总结。英文输出就可以。

*注：Trailhead 主页内容不多，直接手动 Copy 页面文字也可以*

## 2. 分析与推荐 (Gemini)

将 ChatGPT 输出的 Markdown 内容（包含 Badge、Superbadge、Skill points 分布等）完整复制，保存为一个 **Google Docs** 文档。

- **Why Google Docs?**  
  Gemini 可以直接读取 Workspace 文档内容，不仅规避了对话框字数限制，该文档还可以作为后续复盘的基准数据。

在 Gemini 中直接挂载该文档，输入分析指令：

> **Analysis Prompt:**  
> 基于这份 Trailhead Profile 的学习历史和技能分布，结合当前的 Salesforce 生态趋势，为我推荐 2026 年最值得考取的两个证书，并给出理由。

## 分析结果

Gemini 在读取文档后，输出的分析报告带来了一次小小的“震撼”：

1. **结果一致性**：Gemini 推荐的两个证书，与我人工选定的目标**完全一致**。
2. **逻辑推导**：
   - 它准确识别了我现有技能树中的“进阶缺口”。
   - 推荐理由（Why）与我制定计划时的初衷高度重合。

## 总结

**给的数据越准确，AI 潜力越大。**如果效果不理想，可以尝试把你的职业信息补充一下，包括工作时间，工作职责等。

最后分享一个通用的年度计划 Prompt，可以让 AI 充当教练角色：

```text
write my 2026 goals for me. 
Ask me the most important questions 
to craft the best possible 
set of goals for me
```

---

> 本文首发于微信公众号「SalesforceTalk」，[阅读原文](https://mp.weixin.qq.com/s/muw35nl6bii01wvetOuD2g)。
