---
layout: post
title: "使用 octopress 创建博客"
date: 2013-03-17 18:05
author: jair
comments: true
tags: blog
categories: tool
---

一直想弄个自己的博客，买了个域名已经很长时间了，一直也没有弄。在 happycasts.net 上看到了一个关于 [octopress 的介绍](http://happycasts.net/episodes/36)，感觉这个比较适合自己，所以决定折腾下。

## Installation

好的东西，一般官方的文档都比较不错，所以先去官网看一下。在首页发现了熟悉的 [start here](http://octopress.org/docs/setup) 链接。按照文档安装，结果发现 rvm 的 ruby 版本不对，安装提示安装需要的ruby 版本。

``` bash
    git clone git://github.com/imathis/octopress.git octopress
    cd octopress    # If you use RVM, You'll be asked if you trust the .rvmrc file (say yes).
```

进入 octopress 目录后，有下面的提示

    ruby-1.9.3-p385 is not installed.
    To install do: 'rvm install ruby-1.9.3-p385'

使用下面的命令安装对应版本的 ruby

``` bash
    rvm install ruby-1.9.3-p385
    bundle install
```

<!--more-->

结果再次悲剧，执行bundle 命令失败，得到下面的错误

    Gem::InstallError: liquid requires RubyGems version >= 1.3.7. Try 'gem update --system' to update RubyGems itself.
    An error occurred while installing liquid (2.3.0), and Bundler cannot continue.
    Make sure that `gem install liquid -v '2.3.0'` succeeds before bundling.

按照提示，执行下面的命令，然后继续bundle，成功安装需要的gems。

``` bash
    gem update --system
    gem install liquid -v '2.3.0'
    bundle install
```

主题先不准备折腾，所以先使用默认的

``` bash
    rake install
```

上面的命令执行完成后，就可以使用 `rake preview` 在本地进行访问了。

## 更改Blog基本信息

安装完以后，在使用前需要更改一下配置文件，具体步骤可以参考：

[Basic Configuration - enable third party services and personalize your blog](http://octopress.org/docs/blogging)

## 使用Blog

新建博客和页面可以使用下面的命令

``` bash
    rake new_post["hello"]
    rake new_page["about.markdown"]
```

更多参数 [Blogging Basics](http://octopress.org/docs/blogging)

## 部署到GitHub Pages

Octopress 网站上给出了三种发布方式，因为Github的空间比较不错，所以我选择了部署到GitHub。

按照 [Deploying to Github Pages](http://octopress.org/docs/deploying/github/) 给出的步骤进行发布，结果对 [Github Pages][GitHubPages] 的工作方式不太理解，还是出现了一写问题，所以简单写一下。同时推荐首先查看下 [Github Pages help][GitHubePagesHelp]。

1. 创建一个新的Repo 在GitHub 上。我在安装文档上创建的时候，仓库名上遇到点小问题，因为我想使用我的username 去创建一个pages，所以这个地方我要创建一个 username.github.com，也就是 jairzh.github.com 的这样一个 Repository。*.githbu.com* 是一定要有的。

2. 创建完成后，使用octopress 的rake 命令，和上面创建的repo 关联起来

``` bash
    rake setup_github_pages
```

    按照提示输入Github Pages repository 的SSH协议的URL

3. 执行下面命令，把blog 部署到Github 上。

``` bash
    rake generate
    rake deploy
```

    部署完成后就可以使用上面 repo 的地址访问你的blog了

4. 最后一步就是，跟踪 octopress 的原文件，以后更新博客需要在这上面改动

``` bash
    git add .
    git commit -m 'your message'
    git push origin source ＃ 'source' branch is created by above `rake` command
```

## 绑定自己的域名

之前在 Godaddy 上买了一个域名，现在把这个域名用起来，绑定到我Github Pages 的地址上来。
列出一些有用的 links ：

 - [Setting up a custom domain with Pages][GHDomain] *GitHub Pages 的官方配置文章*
 - [Godaddy注册商域名修改DNS地址][dnspod] *据说 Godaddy 的 DNS 解析不稳定，所以这里使用了国内的 DNS 服务器*

## 更改主题

在 octpress Github wiki 上找到了 [3rd Party Octopress Themes][Octopress-Themes]，查看几个以后，最后选择了[Fabric][Fabric] 这个主题。安装下面的命令安装完成

``` bash
    cd octopress
    git clone git://github.com/panks/fabric.git .themes/fabric
    rake install['fabric']
    rake generate
```

`rake preview` 在本地预览没有问题以后，用`rake deploy` 部署到Github

## 参考

- [happycasts for octopress](http://happycasts.net/episodes/36?autoplay=true)
- [使用Github Pages建独立博客](http://beiyuu.com/github-pages/)

[GitHubPages]: http://pages.github.com
[GitHubePagesHelp]: https://help.github.com/categories/20/articles
[GHDomain]: https://help.github.com/articles/setting-up-a-custom-domain-with-pages
[dnspod]: https://support.dnspod.cn/Kb/showarticle/tsid/42/
[Octopress-Themes]: https://github.com/imathis/octopress/wiki/3rd-Party-Octopress-Themes
[Fabric]: https://github.com/panks/fabric

