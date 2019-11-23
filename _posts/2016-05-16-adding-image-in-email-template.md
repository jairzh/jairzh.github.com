---
layout: post
title: "在 Email Template 中引用 Salesforce 中的图片"
date: 2016-05-16 09:25:29 +0800
author: jair
comments: true
tags: salesforce email
categories: admin
---

Salesforce 做为一个 CRM 系统，给客户发邮件肯定是必不可少的功能，Salesforce 提供了非常灵活的 Email Template 功能，来满足你的业务需求。如果你们公司的要求相对高一些的话，肯定希望给客户发一封图文并茂的 Email，这里给大家介绍如何在 Email Tempalte 中引用图片。

## HTML (using Letterhead) Template

**HTML (using Letterhead)** 模版可以使用已经定制好的头部和底部（Letterheads），头部可以放公司的 Logo，底部放公司的联系信息等。在选择好一个 Letterheads 后，再选择 Salesforce 提供的布局模式：
![](/assets/images/2016/05/What-Is-a-Letterhead.png)

就可以利用 Salesforce 提供的可视化编辑器在 Template 中插入存放在 **Documents** 里的图片文件了。
![](/assets/images/2016/05/editor.png)

## Custom 和 Visualforce Template

Custom 和 Visualforce 给我们提供了更多的灵活度，但同时也增加了编辑的难度，因为这两种模式都没有可视化的编辑器可供选择了。它们的编辑方式相差不多，都是需要自己写代码，然后保存到模版当中。当我们把 HTML 在本地写好之后，在 Salesforce 中创建模版的时候，其他的文字和 CSS（坑很多，有需求的可以交流）都可以正常工作，但是图片的话就会出现，因为我们需要给 Tempalte 提供一个绝对路径的图片地址。这时候我们可以有两种选择：

1. 图片上传到自己 CDN 服务器，比如 Amazon S3, 阿里云等，得到 URL。
2. 图片保存到 Documents 中，做相关配置，得到 URL。

>  Visualforce 中也可以使用 Static Resource

因为第一方式情况也比较多，就不做过多介绍。这里详细介绍下第二个方式。

### 图片存到 Documents

首选我们需要把图片存到 Documents 中，然后获取图片地址：

1. 点击 Documents tab, 然后点击 **Create New Folder* link。
2. 输入 Label 和 Name, 选择 **This folder is accessible by all users** 选项。
3. 进入上面创建的 Folder，然后点击 **New Document** button。
4. 输入 Name 等信息，选择 **Externally Available Image**。
5. 选择文件，点击 **Save**。
6. 在查看文件页面，找到刚刚上传的图片，点击鼠标右键，选择 **复制图片地址**，得到下面格式的URL：
```
https://xx.xx.content.force.com/servlet/servlet.ImageServer?id=015i000000Azct4&oid=00Di0000000HNo4&lastMod=1462469426000
```
![](/assets/images/2016/05/copy-url.png)

上面的图片地址，就可以直接放到我们的邮件模版中，建议在替换之前，找一个没有登陆过你当前 Org 的浏览器，输入上面的 URL 进行访问，测试一下，确保外部可以访问。

上面的图片地址，就可以直接放到我们的邮件模版中，建议在替换之前，找一个没有登陆过你当前 Org 的浏览器，输入上面的 URL 进行访问，测试一下，确保外部可以访问。

### URL 说明

关于上面的 URL，可能有人会发现用的是 `servlet.ImageServer`，不是我们平时下载 Attachment 的 `servlet.filedownload`，这两个地址的区别：

1. servlet/servlet.imageserver?id=<record id>&oid=<orgid>
2. servlet/servlet.filedownload?file=<recordid>

> The first URL allows us only to view the image in browser window while the download, downloads the file from your browser to download location.

当你通过 Chrome Developer Tools 去查看两个地址的 `Response Headers` 的具体内容：

![](/assets/images/2016/05/cache-1.jpg)

可以看到 `servlet.imageserver` 多了 ｀Expires｀, 浏览器会通过这个属性进行 Cache，所以推荐使用 `servlet.imageserver`

上面方式得到的图片地址，因为可以在外部访问，所以除了在 Email 中引用以外，还可以在其他不需要登陆的地方进行使用，比如 Force.com Site 中的 Visualforce page 中，不过如果和你的 page 结合紧密的，不经常更换的图片，推荐使用 **Static Resouce**。

