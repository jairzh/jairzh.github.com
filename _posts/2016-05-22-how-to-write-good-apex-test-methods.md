---
layout: post
title: "让 Apex 中的 Test Method 发挥更好的作用"
date: 2016-05-22 15:25:29 +0800
author: jair
comments: true
tags: salesforce test
categories: apex
---

在 Salesforce 中开发，Test Method 肯定是少不了的，因为在部署的时候 Production 是要求测试代码的整体覆盖率是不能小于 **75%** 的，而且 Trigger 是不能没有对应的测试代码的。测试代码的作用是什么，既然必须要写测试代码, 怎么才能让测试代码发挥它应该拥有的价值？这里给大家提供一些写测试代码的基本原则和注意的知识点。

## 4 Goals

#### 1. Positive Behavior（测试正确行为）

写测试代码首先验证一下的正确逻辑，尽量保证把所有的业务逻辑的代码覆盖到。比如代码里有 `try catch` 的话，Postitive 就是需要先把 `try` 里边的代码覆盖到。还有就是如果代码里有 `if else`，`else` 中是一个 `throw` 或者 `addMessage` 等用户提示信息的话，那么 `if` 中的代码就算 **Postitive** 了。

#### 2. Verify End State（验证结果）

测试数据准备好，执行完要测试的方法后，一定要在最后验证一下结果，这是写测试代码非常重要的目的。

``` java
System.assert();
System.assertEquals();
System.assertNotEquals();
```

通过 `assert` 来保证代码输出的结果是业务逻辑需要的值。这样在代码部署后，如果再加新代码，原有测试代码就可以很容易的帮忙检查新代码有没有破坏旧代码，就不需要再浪费时间把原来的逻辑再测试一遍。

#### 3. Negative Behavior（测试反面行为）

这个是 **Positive** 的一个补充，在验证完正确的业务逻辑以后，还需要在测试下反面的行为。这样能够保证用户在使用代码的时候，如果输入了不符合规定的数据，代码也能很好的处理。

#### 4. Bulk Actions（确保不触发 Limits）

如果测试代码达到了上面的 3 个 **Goal**，覆盖率应该已经很高了，肯定能够满足部署需求了，部署吗？等一等，测试代码里还有一个很关键的没有做 － **Bulk Test**。

什么是 Bulk Test？简单翻译就是大量动作测试，它需要更多的数据来完成测试，而不是只创建一条测试数据。

为什要做 Bulk Test? 我们经常会说，不要把 `SOQL` 和 `DML` 放到 Loop 中。但是有的时候还是会有走神的时候，不小心就写进入了。功能测试的时候，很难发现，因为基本上都是通过标准的 UI 去创建一条数据测试的。但当用户通过 Data Loader 等工具批量导入数据的时候，正好上面的代码是一个 Trigger，那基本就悲剧了。

怎样避免上面这中情况呢？**Bulk Test** 可以帮忙，在创建测试数据的时候创建多余1条的数据，建议 **200** 条，然后调用你的业务逻辑代码，验证结果。做了 **Bulk Test**，代码基本很难再触发 Limits 了。

## 5 Best Practices

#### 1. 不要依赖已经存在的数据

* 避免使用 `SeeAllDate=true`，让 `SOQL` 去初始化测试数据。数据在不同的 org 是不一样的，这样会让测试代码结果很难验证。
* 使用 `Test.loadData` 方法创建数据。如果数据比较复杂，可以使用这个方法，加载一个 .csv 文件来创建测试数据。
* 使用 `@testSetup` 方法在测试代码开始阶段统一准备数据，减少重复代码，让测试跑的更快。

#### 2. 使用 Test Utility Classes 创建测试数据

在 Org 中会写很多代码和对应的测试代码，检查一下代码，会发现在测试代码中有很多重复创建数据的方法，所以推荐使用 Utility Classes 去统一的创建测试数据，在测试代码中去调用 Utility Classes 中的方法，这样可以减少代码的重复率，同时也可以帮助快速修复一些因为数据导致测试代码报错的问题。

比如，Org 里在测试代码中创建了很多 Accounts, 有一天要在 Account 上加一个 Required 字段，测试代码就会开始报错，如果测试数据都是在一个地方产生的，简单就该就可以修复了。

#### 3. 测试代码的命名规则

为了让测试代码可以更容易的找到，推荐使用 **MyClass + Test** 的方式命名。

#### 4. 区分测试边境

使用 `Test.startTest()` 和 `Test.stopTest()` 把测试数据和要测试的代码区分开发。也就是在 `startTest()` 前面创建测试数据，在 `startTest()` 和 `stopTest()` 中调用要测试的代码，在 `stopTest()` 后，使用 `assert` 去验证测试结果。

#### 5. 避免一个方法测试所有的逻辑

Unit Tests 翻译一下是单元测试，所以不要在一个 Test Method 中，把整个的应用都测试了。尽量让一个 Test Method 只测试一个功能点。

## 6 Challenges

#### 1. 测试 Visualforce Controllers

* 使用 `Test.setCurrentPage()` 分配一个模拟的 VF page 去测试 controller 中的 `ApexPages.CurrentPage()`。
* 设置 HTTP 的参数，让 controller 可以获取到 URL 中的参数。
* 验证页面的跳转结果

```java
PageReference myPage = Page.MyPage;
myPage.getParameters().put('id', acc.Id);
Test.setCurrentPage(myPage);

MyPageController controller = new MyPageController();
```

#### 2. 测试 SOSL

SOSL 比较特殊，需要使用 `Test.setFixedSearchResults(List<Id>)` 后，才能得到正确的结果。

#### 3. 测试 Asynchronous Apex

如果测试 Batch，feature 等异步方法，需要使用 `Test.startTest()` 和 `Test.stopTest()`，得到正确结果。

#### 4. 测试 HTTP Callouts

在测试代码中是不允许测试 Callout 的，需要创建一个 class，实现 `HttpCalloutMock` Interface，来模拟 HTTP 的请求结果。

#### 5. 测试数据访问权限

如果代码中有需要特定 User 才能执行的逻辑，可以使用 `System.runAs()` 来模拟一个 User 来执行测试代码。

#### 6. 测试 Private 变量和方法

`@TestVisible` 可以让 class 中的 private 变量和方法可以在 test class 中访问。

## 测试不仅仅是为了覆盖率

很多人写测试代码的原因是不写不行，因为 Salesforce 要求必须写。看了上面说的 **4 Goals**，**5 Best Practices** 和 **6 Challenges** 希望大家能够对测试代码有一个重新的认识，让测试代码发挥作用，成为你的代码必不可少的一部分。


