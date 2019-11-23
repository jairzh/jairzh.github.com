---
layout: post
title: "详解 Salesforce 15 和 18 位的 Record IDs"
date: 2016-04-16 09:25:29 +0800
author: jair
comments: true
tags: salesforce
categories: apex
---

在 Salesforce 中 Record (记录) ID 是非常重要的，它代表了一条唯一的数据，在做数据操作的时候是离不开 ID 的，这次给大家详细介绍一下 Record ID。

## Record ID 的构成

Record ID 是一个 base-62 编码的字符串。ID 中的字符可以是 a-z, A-Z 和 0-9 中 62 个字符任意一个。正常情况下是 15 位，每个位置有不同意义的：

![](/assets/images/2016/04/salesforce_id_structure_jzujyk.png)

- 前 3 位： Key Prefix，它可以代表一个 Object，也就是说如果你拿到一个 ID，通过前三位就可以判断这条数据是存在哪个 Object 上的。
- 4-5 和 6 都是 Salesforce 保留值，可以判断这个 ID 属于哪个 `instance`。
- 剩下的 9 位是随机值。


除 15 位的 ID，Salesforce 的 ID 还有一个 18 位的版本，两个版本的主要区别：

- 15 位大小写敏感（case-sensitive）的，主要被用在 UI 上，如 URL，Report。
- 18 位不区分大小（case-insensitive）的，主要用在 API 调用上。

![](/assets/images/2016/04/url-15-id.png)

## 为什么会出现两个版本的 ID

在 Salesforce 中 ID 是 case-sensitive 的，但是有些外部的系统是 case-insensitive，如果只有 15 位的 ID，就会出现问题。比如，当你需要把数据导出到 Excel 中去处理的时候，在 Excel 是不区分大小写的，这是时候在 Excel 中 `00Qi000000g4YE2` 和 `00Qi000000g4ye2` 这本来在 Salesforce 代表两条数据的 IDs，结果就变成了同一条数据了。所以 Salesforce 为了解决这个问题，引入了 18 位的 ID，它在所有字母全部小写的情况下也是不会重复的。

## 15 位转化为 18 位的算法

两个版本的 ID 代表了同一条 Record，所以它们之间有一个转化规则，下面是转化的算法：

![](/assets/images/2016/04/picture23-1.png)

1. 把 15 个的字符平均分成 3 组，每组 5 个字符。
2. 将每组的字符进行反转，如有一组是 `1, 2, 3, 4, 5`，改变后顺序是 `5, 4, 3, 2, 1`。
3. 把每个位置的字符转换成 `0` 或者 `1`，大写字母是 `1`，小写和数字是 `0`。
4. 构造一个 `A-Z` + `0-6` 的字符串，长度正好是 32，索引从 0 开始
5. 把每组队应二进制数转换为一个整数，到 4 中的字符串中找对应的字符，如 `g4YE2` 反转后对应的是 `01100`，对应的整数是 `12`，所以这组字符得到的字符就是 `M`。
6. 3 组转换完后，把得到的 3 个字符依次附加到 15 位 ID 后面，得到 18 位 ID。

### 转化方法

把 15 位转化为 18 位 ID 有很多方法，这里介绍几种常用的：

- **Formula Field**: 在 Salesforce 中创建一个 Formula Field，里边有一个 `CASESAFEID()` 的 Function，可以把 15 位的 ID 转化为 18 位。

- **Apex**: 在 Apex 中可以利用下面的代码转化：

```java
String outputId = (String) Id.valueOf(inputId);
```

- **Excel 插件**: 推荐 [Force.com Excel Connector]( https://developer.salesforce.com/page/Force.com_Excel_Connector) ，可以在 Excel 中将 Salesforce 的 ID 从 15 位转换为 18 位。

- **其他语言实现算法**: 这里提供一个 `javascript` 的版本

``` javascript
function convertToCaseInsensitiveId(id) {
    var hash = '';
    for (var c = 0; c < 3; c++) {
        var h = 0;
        for (var i = 0; i < 5; i++) {
            var ch = id.charCodeAt(i + c * 5);
            if (ch >= 65 && ch < 91) h += Math.pow(2, i);
        }
        hash += String.fromCharCode(h > 25 ? h + 48 - 26 : h + 65);
    }
    return id + hash;
}
```

- 使用工具，这里推荐一个 Chrome 插件 [Force.com Utility Belt](https://chrome.google.com/webstore/detail/forcecom-utility-belt/bchgkjmjnmekbampjoenadmoekocpbhp)

## 总结

Salesforce 中的 Record ID，有的时候可能会因为 15／18 位的问题导致很难发现的 Bug，所以大家在使用的时候需要多留意。在利用外部工具处理数据或者做集成开发的时候，推荐使用 18 位的 ID。

> 虽然 18 位是大小写不敏感的，但是在 Salesforce SOQL 中 15 和 18 都是大小写敏感的，`00Qi000000g4YKyEAM` 和 `00Qi000000g4YKyeam` 后者是查不出来数据的。
