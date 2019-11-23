---
layout: post
title: "Salesforce Spring '16 Release 总结 | Admin/Users"
date: 2016-01-30 17:25:29 +0800
author: jair
comments: true
tags: salesforce release
categories: release
---

这寒冷的冬季，Salesforce 用一支蝴蝶（Spring '16 Logo）给我们送来了春天。再过几周, Salesforce 的 Spring '16 就要开始推送到生产环境了，按照官方给出的时间，大多数的 org 应该在 2/14 之前都会升级到新版本。如果你运气不错，有一些地区的 org 已经在1月份升级了，具体可以参考下面这张图：

![](/assets/images/2016/03/milestone.jpg)

看了下 Spring '16 的 Release Note, 第一眼的感觉就是 Salesforce 的产品线是越来越多，现在一个 Release Note 就已经到达了400多页，想全部读完是需要一些的时间。估计大家都有一样的感觉，所以抽时间整理一下自己感觉还不错的功能和大家分享。

**Global Picklist**

这应该是 Admin 非常喜欢的一个功能了。在以前的版本，如果你有一个相同内容 Picklist 的 Field 在很多 Object 上都有，并且他们的 Option 都是一样的，当公司业务有了变动，这时候你就需要把每个都手动更新一下，感觉好累？Okay，Global Picklist 现在可以帮忙解决这个问题了，现在可以创建一个 Global Picklist，然后在 Object 上就可以根据这个 Global 的 Picklist 去创建新的 Picklist field 了。并且这种 Picklist 是不能通过 API（包括Apex）去添加不在预定义 Option 里的值的，保证你的数据质量。比如你加了 iPhone6, iPhone5 在 Global Picklist, 当有人通过API去插入一条包含 iPhone7 的数据的时候，系统就会报错阻止这条数据的插入。

![](/assets/images/2016/03/global-picklist-3.png)

开启 Global Picklist

![](/assets/images/2016/03/global-picklist-2.png)

填加一个 Global Picklist

![](/assets/images/2016/03/global-picklist-1.png)
 Object 上创建一个新的 Picklist

实际测试了一下，已经有的 Picklist 是不能改为根据 Global Picklist 的，感觉还是有些不方便，希望下一个 Release 能加上这个功能。

> Note: 现在这个功能还在 Beta 阶段，如果要在 Production 里使用，需要联系 Salesforce 打开。

**编辑 File 和 Link 的 Chatter Post**

如果你发了一个 Chatter，结果忘记 @ 了一些人或者打错了一些字，在之前的版本是修改不了的，只能补发或者删掉重发。Spring '16发布以后，可以允许用户对发出的 Chatter Post，包括 Text, File 和 Link 这三种类型的进行编辑修改了。
￼
![](/assets/images/2016/03/edit-chatter.png)

打开这个功能， Setup | Build | Customize | Chatter | Chatter Settings，然后选中 Allow users to edit posts and comments。

**Broadcast Groups (Generally Available)**

这次新增加一种 Chatter Group - Broadcast, 和其它类型的区别是，只有 group owners and managers 可以创建 Post，member 只能做 comment。如果你只是想发一些通知类型的消息，又不想让其它 member 的 Post 把它淹没的话，这种类型的 Group 比较适合。

**Files Without Chatter**

Salesforce 现在逐步加强文件系统。新的 Release 之后，你不打开 Chatter 也可以使用 Files 这个功能了。什么是 Salesforce Files？简单举个例子，你可以认为和 Box, Google Drive 或者 Dropbox Business 类似的一款云端文件管理系统，客户端涵盖各个平台，和 Salesforce 高度集成，同时也逐步提供和其它产品的 Connector - Google Drive, Box, SharePoint, OneDrive 等等。

> 不知道什么时候能支持百度，金山这些国内云盘。

**Track User Identity Verifications**

作为 Admin 你可以通过 Identity Verification History 查看 User 在过去 6个月内的 identity verification 的活动记录。 例如 user 使用两步验证机制登录或者在客户端使用了基于时间的一次性密码，这些动作相关的信息都会记录到 Identity Verification History 里。

**View and Address Security Risks Using Health Check**

Salesforce 这次提供了一个 Health Check 功能，可以帮助你检查你的 org 的 Session Settings, Password Policies, and Network Access settings 的安全级别，帮助你更好的保护公司的数据。这个功能第一次在企业级别软件中看到，不过对于我们国内用户并不是陌生，因为支付宝，JD等个人账号早就有这个功能了，所以记得给你的 org 也"体检"一下吧。

![](/assets/images/2016/03/health-check.png)

**Process Builder Enhancement**

这次 Process Builder 提供一个很 cool 的功能，你可以直接通过拖拽的方式，修改执行 Process 的顺序，节省了很多的时间，因为 criteria 是要有很多 action 的，如果你想调整下 criteria 的顺序，没有拖拽这个功能，就只能是重新创建了，很是麻烦。

![](/assets/images/2016/03/process-builder.png)
￼
顺便一下提一下 Process Builder 真的是一个很强大，如果你还没有用过，建议尝试。有了它，Admin 可以少找几次 Developer, Developer 可以少写不少代码和测试。

**Chatter Messenger Unavailable in New Orgs**

Chatter Messager 这个存在了3，4年的产品，宣布放弃！不过 Spring '16 之前开通的 org 还是会保留这个功能的。


**Data Loader**

Data Loader 再次更新，这次提供了一种新的登录方式，可以通过 OAuth 的方式去登录 Data Loader 了，支持 Mac 和 Windows。

**Increased Developer Sandbox Licenses**

非常有诚意的一次更新，直接上图，看一下数字的变化：

![](/assets/images/2016/03/sandbox-limit.png)


**关于 Lightning Experience**

考虑到大多的客户还对 Lightning Experience 这个新产品处于观望状态，没有在公司推行，所以准备把关于它的更新，放到后面再去单独写，如果感兴趣的，可以直接给我们留言。

