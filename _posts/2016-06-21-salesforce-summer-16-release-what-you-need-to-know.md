---
layout: post
title: "Salesforce Summer '16 Release 亮点功能介绍，Lightning 未来的方向"
date: 2016-06-21 09:25:29 +0800
author: jair
image: "assets/images/2016/06/14657982205119.jpg"
comments: true
tags: salesforce summer16
categories: release
---

> Lightning is the fastest way to connect with customers, employees, and partners

时间过的好快，感觉总结 Spring '16 还没有出来多久，Summer '16 就来了！这次 Release 是官方的第 50 次更新！对于成立 17 年的 Salesforce 来说，应该算是一个里程碑 － 新产品 Lightning 有很多新功能。 6／11号过后，所有的 Org 都已经升级到了 Summer '16 了。

在开始介绍 Summer '16 的新功能前，发表下个人的见解。虽然 Lightning 还不是很成熟，但从发布会的 3 次 release 和一些消息来看，Lightning 已经成为 Salesforce 接下来发展非常重要的一部分。所以作为 Admin 和 Developer，建议对它保持持续的关注，让自己能有一个更好的知识过渡。这里推荐一个 Trailhead 的 [Migrate to Lightning Experience](https://developer.salesforce.com/trailhead/trail/lex_admin_migration) trail, 可以来整体了解 Lightning。

> 从今年的 Q2 开始 Salesforce 把产品线也进行了更新：
> Salesforce’s **Professional Edition**, **Enterprise Edition** and **Unlimited Edition** for Sales Cloud and Service Cloud will be replaced by new **Lightning Professional Edition**, **Lightning Enterprise Edition** and **Lightning Unlimited Edition** for Sales Cloud and Service Cloud.

## Admin and Customers

**1. Create and Edit Lightning Experience Home Pages**

Lightning Experience 的 Home 页面再经历了两次更新后，在这次终于可以进行[定制化](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_sales_home_customize_create.htm)了。发布之初，因为 Lightning 主要是针对 Sales 来发布的，首页显示的是一些销售数据和报表等，所以其他的部门用起来，基本首页就废掉了。这次 Salesforce 把他们平台的灵活可定制的优点也带到了 Home 页，和传统的 Home 页原理相似，可以定制首页的不同模块，然后根据 Profile 就行分配。给你多了一个选择 Lightning 的理由。

![](/assets/images/2016/06/14663334851527.png)

**2. Create a Calendar from Anything in Salesforce**

Lightning 最大的改变对用户来说应该就是 UI，所以 Salesforce 也不断的再推出能够提供更好数据显示的方式。这次推出了可以把不同 objects 根据日期字段显示在 [Calendar](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_sales_activities_calendar_create.htm) 上的功能，让我们有了一种更直观的方式显示数据的日期。

这里给出一些使用场景，比如：Opportunity close dates, Task due dates, Campaign start dates 等，让你能用直观的方式把握每一项工作的日期。

![](/assets/images/2016/06/14663248627054.png)

**3. Shortcuts for Currency and Number Fields in Lightning Experience**

一个用户使用上一个小细节的体现，我们在录入数据的时候，如果遇到数字类型的，就可以 **k**, **m**, **b**, **t** 去表示 千，百万，十亿 和 万亿。比如输入 1k = 1,000，非常方便。[这个功能](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_general_currency_shortcuts.htm)在 Lightning Experience, Salesforce Classic, 和 Salesforce1 mobile app 上都支持。

**4. Associate a Contact with Multiple Accounts**

在 Salesforce 的标准对象的数据模型中，Account 和 Contact 是**1对多**，就是一个公司可以有多个员工。但是现实世界是复杂的，很多时候在公司和员工之间可以是一个多对多的关系，比如一个人，他是公司的负责人，但同时他也参加了一个公益组织，在里边负责相关工作。正好这两家公司都和你们有业务来往，那按照之前的标准模型，只能在系统里再创建一条重复的 Contact 了。

现在你可以把[一个 Contact 关联到多个 Account](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_sales_shared_contacts.htm)了。在 UI 上是区分主次的，在 Related list 中的是所有相关的 Accounts, 勾选了 **Direct** 的 Account，会显示在 **1** 的位置。

![](/assets/images/2016/06/14663266956525.png)

**5. Save Time by Cloning Sandboxes(Pilot)**

[Clone Sandbox](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_deployment_sandbox_to_sandbox.htm#rn_deployment_sandbox_to_sandbox) 非常不错的功能，可以在创建 Sandbox 的时候，选择一个已经正在使用的 Sandbox 去 clone，有的时候能帮我们节省掉很多的时间，比如你开发到一半的时候，突然有了两个不同的想法，都想试一试。这时候就可以 clone 一下当前的 Sandbox，复制出一个新的 Sandbox，实现另外一个想法，省掉了回滚和部署的时间。

**6. Processes Can Execute Actions on More Than One Criteria**

Process 又更新了，这次是一个更加强大的功能，可以在 Process 中[触发多个条件对应的 Actions](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_forcecom_process_multiple_actions.htm#rn_forcecom_process_multiple_actions)。详细解释之前，先来看一下之前的版本。下面这个 Process 的执行顺序是：如果满足 **1**，就执行 `Post to Support` 这个 action，完成后，整个 Process 就结束，不管后续的条件。

![](/assets/images/2016/06/14663292392642.png)

新版本中，看图中 **5** 的位置，多出了一个 **Evaluate the next criteria** 的选项，选择这个以后，在执行了一个条件对应的 Actions 之后，Process 可以继续下一个条件，检查是否满足，如果满足再执行这个条件对应的 Actions。这样的话，我们就可以在一个 Process 中来集中管理一个对象对应的不同业务场景了。避免出现创建太多的 Process，维护起来很难处理的情况（有的时候出了问题，Workflow 有时候检查起来就是噩梦啊）。

![](/assets/images/2016/06/14656870703395.png)

**7. Eliminate Picklist Clutter with Restricted Picklists (Generally Available)**

Spring '16 推出的一个功能，可以让我们限制一个 Picklist 字段中存储的值只能是在 Picklist 中定义好的选项，这样可以保证我们的数据整洁，在运行报表等情况下更好的筛选和分组。具体的开启方式可以看[这里](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_forcecom_picklists_restricted_picklists_ga.htm)。

**8. Dress up Your Data with New Charts in Lightning Experience**

在 Lightning Experience 中[新增几种图标类型](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_rd_reports_dashboards_charts.htm)：funnel, scatter, combo, and cumulative line charts。具体表现形式看图：

![](/assets/images/2016/06/14663334851527-1.png)

**9. Give Your Lightning Experience and Salesforce1 Users the Power of Flows (Pilot)**

如果你使用 Flow 的话，就会感觉功能好强大，但 UI 好难看啊！这次 Salesforce 推出了可以让 [Flow 运行在 Lightning Experience 和 Salesforce1](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_forcecom_flow_lab.htm) 的新功能，同时拥有了 Lightning 现代感的界面。

图中 Survey Customers 是一个 Flow:

![](/assets/images/2016/06/14665085324369.png)

> 如果没有用过 Flow 的话，可以看一下我们的这篇 Blog: [如何使用 Visual Workflow (Flow) 创建表单](http://blog.meginfo.com/visual-workflow-flow-create-form/)

**10. Remove Custom Tabs limit**

> If you use a Professional, Enterprise, or Unlimited Edition org, you can now use as many custom tabs as you want.

[Salesforce 终于把 25 tab 的限制去掉了](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_editions_general_limits_increased.htm)，现在的数量很难用完了。具体多少个，可以看 Org 的 System overview。

**11. Export Reports as Files from Lightning Experience**

现在可以在 Lightning Experience 导出报表了。

**12. Create New Accounts Lightning Fast with Account Autofill**

在创建 Account 的时候，可以[自动帮你填充一些数据](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/rn_sales_accounts_suggestions.htm)。实际测试了一下，对国内公司都不支持。如果你的客户是国外的话，可以考虑开起这个功能。开启的时候可以顺便打开 Account Logos 这个功能，不过国内公司无用！

![](/assets/images/2016/06/14665120742706.png)

## Developer

**1. Create and Edit Lightning Experience Record Pages (Generally Available)**

Lightning Experience record page 可以自定了。用了一下这个功能，感觉比 Page layout 还要强大！

**2. Visualforce for Lightning Experience is Generally Available**

Visualforce 可以运行在 Lightning Experience 中了，不过这并不是意味着 Visualforce 所有的功能都可以正常运行。所以建议，可以在 Sandbox 测试完成以后，在决定是不是开启 Lightning。

**3. View Apex Test Results More Easily**

可以通过 **Apex Test History** page 去查看 Test method 的历史纪录，不过这个功能竟然只在 Lightning Experience 才有。

![](/assets/images/2016/06/14665097955216.jpg)

**4. View Visualforce Controller Exceptions in Debug Logs**

Visualforce Controller 的异常现在可以在 Debug Log 里看到了。之前只能在 Page 上才能看到。

**5. Suppress Null Values When Serializing Apex Objects**

`JSON.serialize` 是非常好用的方法，不过之前的话，会出现 `Null` 这种值。现在我们可以使用 `suppressApexObjectNulls = true`，去掉结果中的 `Null`。

**6. Get a Map of Populated SObject Fields**

SObject 新增加了一个方法，可以让我们更方便的获取已经在内存中的 Fields，比如用 SOQL 查询出来了一个 SObject，之前相关通过代码获取有哪些 Fields 别查询出来的，是很难的。现在可以用下面这个方法，获取在内存中 Fields，这样我们可以更加灵活的使用这些字段或者检查当前在内存中SOjbect 有没有你需要的 Fields。

```java
Map<String, Object> getPopulatedFieldsAsMap()
```

具体的使用方式：

```java
Account a = new Account();
a.name = 'TestMapAccount1';
insert a;
a = [select Id,Name from Account where id=:a.Id];
Map<String, Object> fieldsToValue = a.getPopulatedFieldsAsMap();

for (String fieldName : fieldsToValue.keySet())
{
    System.debug('field name is ' + fieldName + ', value is ' +
        fieldsToValue.get(fieldName));
}

// Example debug statement output:
// DEBUG|field name is Id, value is 001R0000003EPPkIAO
// DEBUG|field name is Name, value is TestMapAccount1
```
> A field is populated in memory in the following cases.
	- The field has been queried by a SOQL statement.
	- The field has been explicitly set before the call to the getPopulatedFieldsAsMap() method.


上面介绍的新功能希望对大家有所帮助，要想全面了解 Summer '16 可以在这里查看最新的 [HTML release note](https://releasenotes.docs.salesforce.com/en-us/summer16/release-notes/salesforce_release_notes.htm)。 Salesforce 的第 50 个 release 给我们带来了很多有价值的东西，也代表一种趋势 － **Lightning**。

## 相关推荐

- [Developer Preview Live – Release Readiness LIVE, Summer ’16](https://www.salesforce.com/video/245969/?d=sessions-filter)
- [Summer '16 Release on Trailhead](https://developer.salesforce.com/trailhead/module/summer_16)
- [Summer '16 new Apex Method - Get a Map of Populated SObject Fields](http://www.fishofprey.com/2016/05/ive-just-found-my-new-favorite-apex.html)


