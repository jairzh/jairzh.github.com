---
layout: post
title: "Salesforce Archive 解决 Org 数据膨胀与存储成本的归档方案"
date: 2026-01-31 18:12:53 +0800
author: jair
image: /assets/images/wechat/salesforce-archive/cover.jpg
description: "Salesforce Archive 官方推出的数据归档服务"
categories: data
tags: "archive storage"
---
随着 Org 使用时间的增长，面临的一个挑战就是：**数据爆炸**。

当 Standard Objects（如 Case, Task, Event）或 Custom Objects 的记录数突破千万级，不仅会频繁触发 Storage Limit 警告，还会直接拖慢 Report 运行速度和 Global Search 的响应时间。

面对这种情况，单纯的 “Delete” 往往不可行（由于审计和合规要求），而购买额外的 Data Storage 费用又相当昂贵。今天在 Trailhead 上看到 **Salesforce Archive** 这个产品，详细了解下，这是一个专门解决“过期数据去留”问题的归档方案，简单介绍下。

> Note: **Salesforce Archive** 需要单独购买，目前官方没有给出明确的价格。

## 什么是 Salesforce Archive？

![Salesforce Archive 界面截图](/assets/images/wechat/salesforce-archive/01.png)

**Salesforce Archive** 是 Salesforce 提供的一种解决方案，旨在将 Org 中“低频访问”或“过期”的数据，迁移到低成本的外部存储中。

它的核心价值在于**分离冷热数据**：

- **热数据 (Hot Data)**：保留在 Salesforce 主存储中，用于日常高频业务操作。
- **冷数据 (Cold Data)**：迁移至外部存储（如 AWS, Azure），但在 Salesforce Org 内部依然保持可见和可搜索。

## 核心特性 (Key Features)

根据官方文档和网上的实践总结，Archive 方案主要包含以下能力：

### 1. 自动化归档策略 (Automated Policies)

你不需要手动去写 Batch Apex 来迁移数据。系统支持配置基于规则的策略（例如：`CreatedDate > 3 years` 且 `Status = 'Closed'`），自动识别并移动符合条件的数据。

![配置归档策略](/assets/images/wechat/salesforce-archive/02.png)

### 2. 无缝的可视化访问 (Visual Access)

这是 Archive 区别于传统 “Data Export” 备份最大的不同。

数据虽然物理上移出了 Salesforce 的核心 Storage，但在 UI 层面上，用户依然可以通过相关的 Component 或列表查看到这些归档数据。对于 End User 来说，体验是无缝的，不需要切换系统去查找历史记录。

![在 Salesforce 中查看已归档的数据](/assets/images/wechat/salesforce-archive/03.png)

### 3. 外部低成本存储

底层存储通常对接 AWS S3、Azure 等云存储服务。相比于 Salesforce 原生的 Data Storage 价格，这些外部存储的成本可以更低。

### 4. 合规与审计 (Compliance)

针对医疗、金融等强监管行业，数据往往需要保留 7-10 年。Archive 方案能够满足这种长期保留（Retention）的需求，同时不占用昂贵的生产环境资源。

## 典型使用场景

如果你的 Org 遇到以下瓶颈，可以考虑引入 Archive 策略：

- **存储告警**：Data Storage 使用率长期处于 90%+，且主要被数年前的历史数据（History, Logs, Closed Cases）占用。
- **性能下降**：由于单表数据量过大（Data Skew），导致 Report 超时、List View 加载缓慢或 SOQL 查询效率低下。
- **审计需求**：业务部门要求必须保留所有历史记录以备审计，但 IT 预算无法支撑无限制购买 Salesforce Storage。

## 定价模型 (Pricing)

*注：Salesforce 的具体 SKU 定价通常不公开透明，且取决于合同（Contract）、地区和企业规模，以下信息主要整理自社区讨论，仅供参考。*

Salesforce Archive 并不像 License 那样有固定的 List Price，它通常采用 **定制报价** 的模式。

根据社区（Community）反馈的信息：

- **参考单价**：约为 $10/GB/年。
- **起售门槛**：部分案例提到有最低存储量要求（如 50GB 起），这意味着每年的起步预算可能在 $6,000 左右。

相比于 Salesforce 原生 Data Storage 的扩容价格，Archive 的单位成本显然更具优势，特别是对于 TB 级别的数据量。

## 总结与建议

**Salesforce Archive** 本质上是用“架构复杂度”换取“成本与性能”的平衡。

对于数据量尚未达到千万级的 Org，配置合理的 **Retention Policy** 并定期通过 ETL 工具导出备份后物理删除，可能是一个更轻量级的选择。但对于企业级 Org，引入自动化的 Archive 机制是必然趋势。

## References

- [Salesforce Archive 官方介绍](https://www.salesforce.com/platform/data-archiving-solutions/)

---

> 本文首发于微信公众号「SalesforceTalk」，[阅读原文](https://mp.weixin.qq.com/s/VpsO5Xo0e_TmpsRp1s8LZQ)。
