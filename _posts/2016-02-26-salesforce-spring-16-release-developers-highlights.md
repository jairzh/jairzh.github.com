---
layout: post
title: "Salesforce Spring ’16 Release 总结 | Developers"
date: 2016-02-26 19:25:29 +0800
author: jair
comments: true
tags: salesforce release
categories: release
---

经过一个假期的等待，Salesforce Spring ’16 终于全面升级，在过去的两周内大家都在 Production 里体验到了 Spring ’16 带来的新功能。我们在代码里也可以开始使用API 36.0 的版本了，今天给大家带来针对开发者的 Release 总结。

**Set and Modify the CreatedDate Field in Apex Tests**

大家都知道 Apex 中是不能修改 CreatedDate，这个在实际代码中是没有问题的。但是如果我们的一段代码是依赖于它去做条件判断的，比如需要处理7天前创建的数据，这时候想让我们的代码覆盖率达到100%，正常的方式是不行的，因为我们没有办法在 Test Method 里创建7天前的数据。

现在我们可以了，因为在 Apex 中新加了一个 `System.Test.setCreatedDate`, 它可以在测试代码中修改数据的 CreatedDate，下面是示例代码：

```java
Account a = new Account(name='myAccount');
insert a;
Test.setCreatedDate(a.Id, DateTime.newInstance(2012,12,12));
```

**Create Test Suites of Test Classes**

如果你做一个相对独立的项目，为这个项目写了很多的 Test Method, 这时候希望每次部署的之前都一起跑一下，提前发现错误。在以前的时候只能是一个个的在 Developer Console 里手动选，然后下次再跑时候还是同样的再选一次。现在你可以用 Test Suites 这个新功能把你需要跑的 Test Method 加到一个 Suite 里，然后每次去跑这个 Suite 就可以了。试了一下，很方便。

**Stop a Test Run That’s Failing Miserably**

现在你可以通过配置一个测试方法失败的上限，来决定你跑的测试什么时候停下来。如果你同时需要处理很多 Test Method，当你有 Test method 报错的时候，这个配置就可以让所有的测试停下来，让你省掉不少的等待时间。

**Test Method Bug Fixs**

* **Call Test.startTest() to Reliably Reset Limits in Apex Tests**: 修复了之前一些 Limits 不能在 `startTest` 和 `stopTest` 之前重置的问题。
* **Use @future to Avoid the Dreaded MIXED_DML_OPERATION Error**: 修复了不能使用 @future 去避免在 test method 里同时对 Setup sObject(如 User) 和 non-setup sObject 做 DML 操作的问题。
* **Compare Currency to Decimals in Apex Tests**: 修复了在 Test 中不能用 Currency 和 Decimals 做比较的问题。

**Locate Jobs in the Apex Flex Queue**

如果你的 Org 里有很多的 Apex Job, 同一时刻可能会启动好多个 Job，这些 Job 之间还有一些关联的话，你可以使用了 Flex Queue 去管理他们，因为在 Felx Queue 里可以放100排队的 Job，然后通过 FlexQueue class 或者手动的管理你的 Job 的执行的顺序。现在你可以通过 FlexQueueItem 这个 Object 去查询你想操作的 Job 的位置了，这样你可以更好的用代码去控制 Queue 里 Job 的执行顺序。
```
SELECT JobPosition FROM FlexQueueItem WHERE JobType = 'BatchApex' AND AsyncApexJobId = '707xx000000DABC'
```

**Make Calls to PageReference.getContent() After DML and Savepoints**

就像 callout 一样，以前在 DML 之后我们是不能执行  `getContent()` 和 `getContentAsPdf()` 方法的，现在 Salesforce 放开了这个限制。生成 PDF 应该更加方便灵活了。

**New and Changed Apex Classes**

这次添加了不少新的方法，这里给大家介绍两个自己感觉比较大众的：
* System.Approval.isLocked: 可以通过这个方法查看 records 是否被 Lock 了。
* System.System.isQueueable: 判断当前是不是 Queueable 在调用当前的方法。

**Serve Static Resources from the Visualforce Domain**

这是一个 Summer ’15 的一个 Patch，这次更新会自动激活。在这给大家提示一下，如果你之前在 page 或者其它地方用绝对路径使用了 Static Resource 的资源，你是需要更新的。因为 Salesforce 修改了 Static Resource 的 Domain，导致 URL 发生了变化，资源不能被访问。

友情提示: $Resource，是一种更安全的引用方式。

**Run Script After Sandbox Creation and Refresh**

现在你可以在 Sandbox 刷新完成之后执行一段 Apex 代码，帮助自动完成一些配置工作。

![Sandbox](/content/images/2016/03/sandbox-script-1.png)

```java
global class HelloWorld implements SandboxPostCopy {
   global void runApexClass(SandboxContext context) {
     System.debug('Hello Tester Pester ' +
      context.organizationId() + '' ' ' +
      context.sandboxId() + context.sandboxName());
   }
}
```

**SOQL 更新**

* **Query Records Saved in All Users’ Private Folders with the allPrivate Query Scope**: SOQL 语句终于可以通过加入 `USING SCOPE allPrivate` 的方式去查询在 User 的 Private Folder 里的报表了。还记得我们一个同事，当时为了删除这些 Private 的 Report，只能是一个个 User 手动去 Login，然后去删除 :(
* **Aliasing in SELECT convertCurrency()**: convertCurrency 也可以支持别名了。
* **Formatting for Number, Date, Time, and Currency Fields in SELECT Clauses**: 根据当前 user 的地区，`Format` 关键词可以返回 Number，Date，Time 和 Currency 字段本地相对应的格式。
```
1. SELECT FORMAT(amount) Amt, FORMAT(lastModifiedDate) editDate FROM Opportunity
2. editDate = "7/2/2015 3:11 AM"
3. Amt = "AED 1,500.000000 (USD 1,000.00)"
```

**其它 API 更新**

* **REST API: Friendly URLs**: REST API 新增加了一种请求资源的方式，通过这种方式可以较少你的请求次数，获得到你想要的数据。
	`/services/data/v36.0/sobjects/contact/id/account`
* **JSON Support for Bulk API**: Bulk API 支持 JSON 的通讯方式了。
* **Wave API has gone GA**: 很酷很炫，可惜有点贵。
