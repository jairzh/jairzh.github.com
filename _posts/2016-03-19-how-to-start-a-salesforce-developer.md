---
layout: post
title: "快速入门 Salesforce Developer"
date: 2016-03-19 08:25:29 +0800
author: jair
comments: true
tags: salesforce apex force.com
categories: apex
---

Salesforce 作为 CRM 解决方案领域的全球领导者，得到了越来越多企业的青睐。相对于传统 CRM，Salesforce 能够取得今天的成绩，除了它在一开始就选择基于云计算的架构，走 SaaS 这条路之外。还归功于，它利用 Force.com 这个 PaaS 平台，打造了一个非常完善的开发者生态圈。

在这个环境下，越来越多的人选择进入 Salesforce 这个平台，在国内也是上升趋势。为了让大家能够更方便的入门 Salesforce，这次给大家介绍如何快速的了解 Salesforce 平台，成为一名 Developer。

## 基本条件
这里先列出我们公司招聘的基本条件，供大家参考：
1. 了解一门 OO 语言，Java、C# 或 C++ 等
2. 有一定的英语阅读能力，因为 Salesforce 官方还未提供中文开发者资料
3. 想独立开发，还要掌握基本的前端知识, HTML、CSS 和 Javascript

如果你满足上面的第一个条件，那就继续往下读吧。如果没有学过任何一门开发语言，建议不要把 Salesforce 平台提供的 Apex 作为你第一门语言，因为中文学习资料较少，整个平台相对复杂。

## Trailhead

![](/assets/images/2016/03/trailhead.png)

Trailhead 作为 Salesforce 官方推出的一个学习系列，给 Admin，User 和 Developer 提供了不同的学习路径，并在每一个部分都会有对应的练习，题目答对以后会给你对应 Point，当你一个 Module 完成后，会给你颁发一个 Bagde，成就感满满。

![](/assets/images/2016/03/badges.png)

同时因为 Trailhead 由官方维护，它能够保持与新版本同步（Salesforce 每年 Release 3 次）。每次 Release 推出的新功能和新技术，都会同步推出对应的 Module 和 Project。所以作为入门的 Developer 学习资料, 这里给大家推荐 [Developer Beginner Trail]。

## Developer Beginner Trail

![](/assets/images/2016/03/developer-beginner.png)

Developer Beginner Trail 作为官方的 Developer 入门系列，列出了作为一个 Salesforce Developer 需要掌握的核心知识点，并把这些知识分别放到了 11 个 Module 中，包含了平台介绍，数据模型，数据安全，自动化处理，Apex 和 Visualforce page。下面给大家一一介绍下每个模块涵盖的内容。

##### 1. Salesforce Platform Basics
介绍了 Salesforce 这个平台的整体结构，使用场景，平台中常用的术语: Record、Field、Object、Org、 Force.com 和 App 等；指导你如何创建一个 Developer Edition Org, 开始使用 Salesforce；对开发者常用的 Setup 做了详细介绍。最后介绍了一下 Salesforce 的应用商店: AppExchange。

![](/assets/images/2016/03/intro_arch_basics.png)

##### 2. Data Modeling
介绍了在 Force.com 平台中如何定义和存储数据，建立 Object（表） 与 Object 之间的关系。标准 Object 和 Custom Object 的区别，利用 Schema Builder 更高效的定义和维护你的 Objects 和 Fields。

> 重点推荐：理解 Lookup 和 Master-detail 的区别

##### 3. Data Management
如何利用 Force.com 平台提供的工具进行 Import 和 Export 数据。

##### 4. Data Security
如何在平台中控制你的数据安全性，包括利用 Password 策略，IP 和 登陆时间限制提高用户安全级别；通过 Profile 和 Permission Set 对 Object 和 Field 访问权限控制；使用 OWD (Organisation-wide defaults), Sharing Rule, Role hierarchies 和 Manual sharing 去控制和分享 Records(数据)。

![](/assets/images/2016/03/record_access_triangle.png)

这一章对于之前没有接触过 Salesforce 的开发者来说相对困难，但对于一个企业是非常重要的。理解不好这一章，很容易让你的数据出现问题，重点掌握。推荐在学习过程中，用多个用户去查看控制效果，加深理解。

##### 5. Formulas & Validations
利用 point-and-click 方式，在不需要写代码的情况下实现需求。Formula 和 Roll-up 功能同 Excel 中提供的公式非常相似，它可以帮助我们很方便的实现一些数据处理方面的需求。Validation Rule 可以帮助我们保证数据质量，在页面中对用户给出错误提示。

##### 6. Process Automation
Force.com 平台提供了 Process Builder, Visual Workflow, Workflow 和 Approvals 这些 point-and-click 的工具，帮助我们实现业务的自动化。通过这一个 Module，我们可以学习到每个 Tool 提供了什么功能，如何根据具体的业务场景，合理的选择和使用它们。

![](/assets/images/2016/03/pb_understanding_ui.png)

> Salesforce 官方推荐需求实现，Automation Tool > Coding，所以如果工具能帮助我们实现需求，就可以先不考虑写代码。
 
##### 7. Salesforce1 Mobile Basics
了解 Salesforce1 这个 Mobile 平台，学会如何配置 Navigation, Layout 和 Action 等。如果你们的公司正在使用 Salesforce1，推荐学习。因为它和 Web 端的使用和配置还是有很多区别。
 
##### 8. Apex Basics & Database
Apex 终于登场，可能很多人会问，既然是开发，为什么不第一时间介绍 Apex 语言。我感觉 Salesforce 这么安排还是合理的，因为我们要学习和使用一个平台，在 Force.com 上开发，不单单是 Coding, 同时我们要利用前面介绍的每一个 Modle 中的功能去实现我们的需求。对于 Apex，如果你学过 Java，入门应该是很容易的。

##### 9. Apex Triggers
Apex Trigger 是 Force.com 平台相对其它语言比较特殊的一个点，能够保证我们很方便的处理和使用数据。但是 Trigger 写太多，如果没有注意优化代码，到后面的开发会越来越难，可能会有一种欲哭无泪的感觉。所以强烈推荐新手仔细学习这一个 Module。

##### 10. Apex Testing
Test method 是 Apex 中非常重要的点，因为 Salesforce 强制要求你 Production 里的测试代码覆盖率要达到75%，否则新的 change 就不能部署。写好测试，争取写 100% 覆盖率的测试，是一个好的开始。这一 Module 也给出了一些最佳实践，推荐学习。

##### 11. Visualforce Basics
如何使用 Visualforce 来构建 Web 和 Mobile 上的页面。Visualforce 支持 MVC 设计模式，是 Force.com 中的 Web 开发框架。 在这个 Module 让你对 Standard 和 Custom controller 有一个基础了解。

![](/assets/images/2016/03/visualforce_request_processing.png)

通过上面的介绍，希望大家能过对 Salesforce 的知识点有一个大致的理解。对于上面 11 个 Module 官方推荐的学习时间是 15 个小时，我感觉这个时间是相对于有一定基础的 Salesforce 开发者。因为我们公司很多新同事做下来，至少要两周左右的时间。所以这里推荐可以先挑选一部分学习，然后再回过头学习比较难的模块。推荐先学习 1, 2, 8, 9, 10, 11，再学习 5，6，7 最后 3，4，仅供大家参考。




