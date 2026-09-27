---
layout: post
title: "如何使用 Platform Event 处理 Trigger 中的复杂数据"
date: 2023-08-27 18:03:37 +0800
author: jair
image: /assets/images/wechat/platform-event-for-complex-trigger-processing/cover.jpg
description: "如何使用单独的 Transaction （异步）来处理复杂的业务处理，包括耗时的计算和 Callout 操作，同时又能尽早的生成结果。避免使用 Scheduled 这种定时处理方式。"
categories: apex
tags: "platform-event trigger"
---
如何使用单独的 Transaction （异步）来处理复杂的业务处理，包括耗时的计算和 Callout 操作，同时又能尽早的生成结果。避免使用 Scheduled 这种定时处理方式。

## Trigger 中进行 Callout

如果想在 Salesforce 的 trigger 中进行 callout，就需要使用 Future 或者 Queueable，但两个都有限制。包括：

- 一个 Transaction 最多 50 次
- 每 24 小时 250,000 或者 License x 200 的数量，两者取最大值。Batch Apex, Queueable Apex, scheduled Apex, and future methods 共享。
- Future 不能在异步方法中调用
- Queueable 只能在异步方法中启动一个

基于上面的限制，因为 DML 最大数量是 10,000，trigger 默认的 size 是 200，所以 50 （10,000/200）个限制正常是没有问题的。但如果一个 Transaction 有下面的情况就会出问题：

- 多于 50 个 DML，比如因为特殊情况，把 DML 放到了 For 循环中。
- 操作不同的对象，对象都有 trigger 并启动了 future 或者 queueable，数量不是整数。比如 4,100/200 = 21，5,900/200=30，21 + 30 就超出了
- Trigger 的执行本身就是因为异步操作引起的，Future 就不能启动。或者 Queueable 不能大于一个。

## 使用 Platform Event 方案

### Platform Event 的特点

- 同一时间只有一个在执行，多个会进行合并，方便并发处理。默认是 2000，可以指定。
- Apex Trigger 订阅的话每小时有 250,000 个的限制，比异步 24 小时 25w 要多很多。

### Platform Event 限制

- 因为同一时间只处理一个，所以属于单线程。如果处理一次需要 1s，这样的话，一天最多可以处理 86400 次的请求。
- 数据处理变成一个特定的 User，比如默认是 Automated Process。也可以指定特定的 User。

### Platform Event 问题

Platform Event 可能会出现丢失的情况，虽然很罕见，但也需要注意。所以 Platform Events 需要是 stateless （无状态）, 这样可以接受丢失。无状态就意味着，需要有另外的 Object 进行储存，在要处理的对象上添加 Custom Field 进行标记。为了避免出现更新字段的时候并发问题，可以使用两个时间字段和一个 Formula 字段进行标记，当需要处理时间有值，处理时间没有值的时候，就可以将 Formula 设置成 True，在 SOQL 中进行查询，对数据进行处理。

### Platform Event 方案总结

1. 在 Trigger 中只发布一次 Platform Event，可以使用 Static 变量进行控制。作用就是启动一个异步线程，去对 Trigger 中的数据进行更复杂的处理。
2. Trigger 中需要对数据进行标记，方便 Platform Event 进行查询，获取要处理的数据。
3. Platform Event 使用 Formula 字段对数据查询，开始处理。对处理好的数据，再次更新，标记已经处理好。

## 参考

- [Demo code](https://github.com/sirephil/sf-async-from-trigger)
- [Governor friendly asynchronous processing from triggers](https://www.apexhours.com/governor-friendly-asynchronous-processing-from-triggers/)

---

> 本文首发于微信公众号「SalesforceTalk」，[阅读原文](https://mp.weixin.qq.com/s/jajqnXASQn5xWODaGP7e6Q)。
