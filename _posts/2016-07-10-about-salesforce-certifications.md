---
layout: post
title: "关于 Salesforce 认证的整体介绍"
date: 2016-07-10 09:25:29 +0800
author: jair
comments: true
categories: certification
---

做为一名 Salesforce 相关的从业者，无论你是 Admin 还是 Developer 有一件事情肯定是这个行业里经常提起的，就是 Salesforce 的认证。因为这个认证是 Salesforce 官方推出，并且考试的内容会随着 Salesforce 新功能推出持续更新，保证了知识的有效性和证书质量的可靠性。所以在行业内，大家对这个认证体系都是比较认可。为了让大家能更好的了解和准备 Salesforce 的认证，我们将陆续给大家介绍一些相关认证的资料和准备方式，希望对大家能有所帮助。这次先给大家整体介绍一下常见的认证。

介绍之前先说两个常见的概念：

- Salesforce.com: 一个 SaaS （软件即服务）平台，提供行业标准的 CRM 服务。
- Force.com: 一个 PaaS （平台即服务） 平台，针对开发人员，有平台独特的语言，开发人员可以在这个平台上开发各种 App。

## [Administrator (管理员)](http://certification.salesforce.com/administrators)

Administrator 主要是针对 Salesforce.com 这个平台，一个企业要实施和使用 Salesforce，一个管理员会起到了非常重要的角色。所以 Salesforce 专门针对这部分人群推出了下面两个考试认证：

### Administrator (201)

Administrator 初级认证，代号 201，主要针对接触 Salesforce 时间不长，想全面了解 Salesforce.com 这个 CRM 平台推出的。考试内容的话，主要涉及：

1. 系统配置：Organization, User，User Interface, Security
2. 数据模型：Standard 和 Custom Objects
3. Sales cloud 和 Service cloud
4. 数据管理：导入和导入数据等
5. 数据分析：Reprot 和 Dashboard
6. 基本自动的化：Workflow 的使用

**考试要求**

- 60 multiple-choice questions
- 90 minutes allotted to complete the exam
- 65% is the passing score
- Registration fee is USD 200

上面是考试要求，60 道多选题，考试时间 90 分钟，65% 算考过，也就是你需要答对 40 道题。考试费的话是 200 美金。如果第一次没过考过，第二次需要再交 100 美金。

### Advanced Administrator (211)

Advanced Administrator 认证，代号 211，是 201 的升级，需要拥有 201 证书以后才可以报名这个考试。考试的范围和 201 相差不大，不过需要你对每个部分的有一个更深入的了解，会使用更高级的功能。一个好的管理员，必备证书之一。

**考试要求和 201 一样**

- 60 multiple-choice questions
- 90 minutes allotted to complete the exam
- 65% is the passing score
- Registration fee is USD 200

## [Developer (开发）](http://certification.salesforce.com/platform-developers)

Developer 的认证的话，主要针对 Force.com 这个平台。之前大家可能比较常听说的就 401 和 501，但去年 Salesforce 做了一次升级，变成了 Developer I 和 Developer II，最大的改变应该就是开发人员的初级认证，之前的考 401 的话，你是不需要懂代码，也是可以拿到这个证书的，所以一个不懂开发的管理员也可以比较容易的拿到这个证书。升级后的 Developer I，加入了代码部分考核。

### Developer I (450)

Developer I，开发人员的初级认证，代号 450，考试要去的话，比原来的 401 难了不少，除了 Data Model 这一块和 Admin 重叠以外，其他的都是代码了。主要是 Apex 和 Visualforce，还有要大体了解下 Lightning Component。如果之前没有接触过 Salesforce，但有其他语言开发经验，推荐 2－3 个月的准备时间。

**考试要求**

- 60 multiple-choice questions
- 105 minutes allotted to complete the exam
- 68% is the passing score
- Registration fee is USD 200

105 分钟，60 道多选题，68% 才可以过，你需要答对 41 道题，比 Admin 的多了一道 :)，不过时间更充裕了。

### Developer II

Developer II，开发人员的高级认证，更有含金量一些，因为考试步骤跟多，费用更高。建议至少拥有一年以上 Force.com 开发经验的人准备。考试的要求就不细说了，有兴趣的可以留言。下面详细说一下考试步骤，主要分三步：

**一. 选择题**

- 60 multiple-choice questions
- 120 minutes allotted to complete the exam
- 63% is the passing score
- Registration fee is USD 400

和其他考试一样，首先要考过选择题，考试形式和 450 相同。120 分钟 60 到选择题，63% 过，答对 38 道题。费用是 450 的两倍，400 美金。

**二. Programming Assignment**

考过选择题以后，你就需要去预约这个 Programming Assignment，注意这个一年就开放几次，错过了可能就要等几个月，所以一定要留意邮件通知和官网通告。预约上以后，Salesforce 会单独给你发邮件，会给你一个他们定义好的需要文档，让你在 30 天的时间去完成这个程序。官方给出的推荐时间是 20 个小时。规定的时间到了以后，你就不能再改代码了。然后进行第 3 部分。

**三. Essay Exam**

这部分的话，需要你去登陆考试系统预约，然后和做选择题类似，到指定的时间，就可以去考试了。考试内容是 5 到问答题，需要再 60 分钟内完成。

这三部分完成后，剩下的就是等待考试结果了。整个过程大致需要需要 2－3个月。

## [App Builder](http://certification.salesforce.com/app-builders)

App Builder 是 Salesforce 去年升级 Developer 多出来的一个证书，整体应该是原来 401 的一个升级，加入更多 Lightning 的东西，建议 Developer 考一下这个证书。如果只是会写 code 的话，其实不能算上一个合格的 Salesforce 开发，因为 Force.com 除了代码，还提供了很多强大的功能，比如：Formula, Roll-up fields，workflow, process builder 等。

考试要求：

- 60 multiple-choice questions
- 90 minutes allotted to complete the exam
- 63% is the passing score
- Registration fee is USD 200

## [Implementation Experts](http://certification.salesforce.com/implementation-experts)

这部分主要是对每个不同领域的细分，主要分为：

- Sales Cloud Consultant
- Service Cloud Consultant
- Pardot Consultant credential
- Marketing Cloud Consultant credential

如果你对每个具体的产品有更深入的要求的话，可以考虑上面的证书。

考试形式都是选择题，费用 200 美金。

## 其他

Salesforce 除了上面介绍的这些认证之外，还有一个就是压轴的 Technical Architect，含金量相当高，因为考试费加起来就需要 $6,500 (美金哦，努力吧，少年！)。因为这些相对小众，所以就不介绍了，有兴趣的可以去[官网](http://certification.salesforce.com/)了解详情。

## 维护证书

因为 Salesforce 每年都会 release 3次，所以每次 release 后，你都需要维护一下你的证书，在线上做一下相应的题目，好处就是开卷的，你可以查资料。

除了每次 release 考试以外，还需要**每年**交 **$100** 美金的维护费（对于国内这个费用还是有点小高）。

PS: 如果不维护，证书就过期了，需要重新再考。

## 关于考试

首先需要注册一个账号，打开认证官网: http://www.webassessor.com/salesforce, 点击右上角的 **REGISTER FOR EXAM**，按照步骤提示注册完成。

然后根据你的情况，选择考试类型，主要同一个类型的考试，一般会有两个选项，主要是考试形式的不同：

1. 线下考试中心
  可以在考试的网站上，预约线下的考试中心，国内主要的城市是有的。如果你所在城市没有，也不用担心，可以用第二种方式。

2. 线上方式
   只需要准备一摄像头（笔记本自带不可以)，下载一个软件安装在电脑上就可以了。最后有比较稳定的 VPN，以为考试的过程中，对方是会通过摄像头全程监控的是考试过程的。如果网络不好，或者你有姿势不符合规定，对方会把你暂停，不过不用着急，你可以在线聊天的方式和对方沟通，一般问题都不大。

关于多选题：考试的题目中选择题是有多选的，不过比较好的就是提供会告诉你需要选择几个正确的，稍微降低了一点难度。

## 证书验证

可以在这个地址验证一个人所持有的证书：http://certification.salesforce.com/verification

## 结语

这篇 Blog 主要根据 Salesforce 的认证官方提供的资料整理，让大家对认证体系有一个整体的认识，提供的信息都具有时效性，所以考试前，建议大家要上官方查看最新的资料：http://certification.salesforce.com

> Note: 通过考试，系统的完善自己的知识体系，增加个人能力，享受准备考试的过程吧。