---
layout: post
title: "Using Sublime for Force.com"
date: 2013-03-24 23:22
author: jair
comments: true
tags: [sublime, salesforce, force.com]
categories: tool
---
在 Force.com 这个平台已经开发了两年多得时间了，一直感觉 Eclispe 的 Force.com IDE 不是很好用。有句古话叫”工欲善其事，必先利其器“，本身我自己来说也是一个工具控。所以我一直在寻找一个更好的工具。中间有尝试过 [VIM 配合 Ant][vim-forcedotcom] 来进行开发，但有很多的限制。直到在 Github 上发现了 [MavensMate-SublimeText]，试用之后，非常不错。在使用了 MavensMate-SublimeText 一段时间以后，并且在公司部分同事一起使用后，发现了安装和试用问题，所以决定写些东西出来。

## 选择 Sublime 的几点原因

1. Sublime 本身是一个非常好的文本编辑器，如果你不了解，可以先看看下面的几篇文章。
	* [Sublime Text 2 官网]
	* [Sublime Text 2 入门及技巧]
	* [视频: sublime text2 简介]
2. Fore.com IDE 启动太慢，由于国内网络的原因，一些操作经常出现卡死得现象
3. Developer Console 这个 Salesforce 一直主推的在线编辑器，经过最近几次 Release 以后，确实有不小得改进，但是还是网络的问题，保存或者智能提示的时候，会出问题。而且最主要的问题是，不能很好的 git 配合使用。
4.  [MavensMate-SublimeText] 这个 Sublime Package 经过作者更新和修复以后，已经非常稳定。能够满足正常的开发需求。更多功能得介绍，可以 Github 查看。这里放两个视频地址：
	* [http://v.youku.com/v_show/id_XNDU2MDI4MzQ4.html](http://v.youku.com/v_show/id_XNDU2MDI4MzQ4.html)
	* [http://www.youtube.com/watch?v=3cwZxIaxhY8](http://www.youtube.com/watch?v=3cwZxIaxhY8)


## 在 Mac OS X 中安装 MavensMate-SublimeText

这里要说明一下，我这边文章里所以得配置都是针对 Mac OS 系统进行的，因为我现在不使用其他的系统，其他的平台可以看一下[这里](https://github.com/joeferraro/MavensMate-SublimeText/pull/53)

<!--more-->

### 1. 安装 Rails

因为 MavensMate-SublimeTex 这个 Package 是使用 Rails 进行开发的，所以需要在你们的系统里安装 Rails 的运行环境。下面介绍两种安装方式：

#### Using the [Railsinstaller](http://railsinstaller.org/)

在 Railsinstaller 的官方网站上下载到适合你系统的包以后，按照提示安装就可以了。

Packages included are:

> * Ruby 1.9.3-p194
> * Rails 3.2.8
> * Git 1.7.9.6
> * Sqlite 3070500
> * osx-gcc-installer 4.1
> * JewelryBox 1.3.0
> * RVM 1.15.7
> * SM 0.10.4

上面是这个包里边包含得软件，它会把所有的 Rails 需要的包全部安装完。
如果你不想做 Rails 的开发，推荐这种安装方式，方便快捷。

#### Using command line

> 1. Install [osx-gcc](https://github.com/kennethreitz/osx-gcc-installer)
> 2. Install [Homebrew](http://mxcl.github.com/homebrew/)
   `$ ruby <(curl -fsSkL raw.github.com/mxcl/homebrew/go)`

> 3. Install Git
	`$ brew install git`

> 4. Install RVM, Ruby1.9.3 and Rails:  (https://rvm.io/rvm/install/)
    * RVM: `$ curl -L get.rvm.io | bash -s stable`
    * Ruby1.9.3:  `$ rvm install 1.9.3`
    * Set default ruby version:  `$ rvm use 1.9.3 --default`
    * Rails: `$ gem install rails`

### 2. 安装 MavensMate-SublimeTex

Rails 安装成功以后，就可以安装 MavensMate-SublimeTex 这个 Package 了。这个 Package 的话，如果你的 Rails 环境没有问题的话，使用作者提供的方式安装方式是应该没有问题的。

```
$ gem install mavensmate
$ ruby < <(curl -s https://raw.github.com/joeferraro/MavensMate-SublimeText/master/install.rb)
```
安装成功以后，重新打开 Sublime 就可以在 Help
的左边可以看到 MavensMate 这个 Menu 了。

<img src="http://wearemavens.com/images/mm/menu3.png" width="400"/>

如果安装出现问题，可以参考 GitHub 上的 [Install Help](https://github.com/joeferraro/MavensMate-SublimeText/wiki/Install-Help)

## 使用MavensMate-SublimeTex

### 1. 初始化配置

#### 配置 Workspace

在 "MavensMate --> Settings --> User"，打开文件后修改 `mm_workspace`，这个目录会存放你以后创建的项目。  (需要试用绝对路径):

	"mm_workspace": "/Users/your_username/Projects"

#### 配置 ruby 环境

如果你是安装我上面介绍的方式安装的 ruby 的话，那么需要修改 `mm_ruby` 这个选项，具体的值需要在 terminal 里使用 which rvm 查看:

	"mm_ruby": "~/.rvm/bin/rvm-auto-ruby" // 命令行安装

### 2. 创建项目

Package 提供了两种创建项目的方式，`New Project` 和 `Checkout project`

#### New Project

如果你是一个新项目，你需要使用这种方式创建项目，具体的创建方式大家打开以后应该都可以看清楚。

#### Checkout Project

这个选项是配合 Source control 来试用的，如果你的项目使用了 Git 等源代码管理工具在团队共享代码的话，你可以选择这个选项，需要填写你的账号密码，还有远程仓库的URL。这个功能配合 Git 试用的话，非常不错。

### 3. 编写代码

下面是作者给出的功能点，推荐大家再安装完成后，找一个 developer org 把所有功能都使用一下。

* Create & Edit Salesforce.com projects with specific package metadata
* SVN & Git support
* Create & compile Apex Classes, Apex Trigger, Visualforce Pages, and Visualforce Components
* Compile and retrieve other Salesforce.com metadata
* Run Apex test methods and visualize test successes/failures & coverage
* Play Pacman while your Apex unit tests run
* Deploy metadata to other Salesforce.com orgs
* Apex Execute Anonymous
* Apex Code Assist (beta!!)
* [Resource Bundles](http://www.youtube.com/watch?v=g4M5zr8Q8MY) （PS：这个功能在 ReadMe 里并没有，看了视频以后才发现，是编辑 static resource 很不错的功能）

Apex Code Assist:

<img src="http://wearemavens.com/images/mm/code_3.png"/>


## 推荐安装的 Package

### [Sublime Text 2 SFDC Assist][SFDC Assist]

这个我自己写一个很简单的 Sublime Package，里边主要包含了两个功能：

* Apex Language Snippets
* Visualforce Component Completions

安装方式，[Install SFDC Assist][SFDC Assist]。功能还不是很多，需要完善的地方也很多，如果各位谁有兴趣，可以一起来做。

----------

下面的这些 Package 是我经常使用的，安装的话，只需要在 Package control 里搜索就可以了。

### All Autocomplete

Extends the default autocomplete to find matches in all open files.By default Sublime only considers words found in the current file.

### [SideBarEnhancements](https://github.com/titoBouzout/SideBarEnhancements)

Provides enhancements to the operations on Side Bar of Files and Folders for Sublime Text 2

### [Surround](https://github.com/mgnandt/sublime-text-surround)

It's easiest to explain with examples.  Press `cs"'` inside

    "Hello world!"

to change it to

    'Hello world!'

### [SublimeCodeIntel](https://github.com/Kronuz/SublimeCodeIntel)

Provides the following features:

* Jump to Symbol Definition - Jump to the file and line of the definition of a symbol.
* Imports autocomplete - Shows autocomplete with the available modules/symbols in real time.
* Function Call tooltips - Displays information in the status bar about the working function.

### [Git](https://github.com/kemayo/sublime-text-2-git)

Git integration: it's pretty handy. Who knew, right?

-------
#### *Enjoy Sumlime and Salesforce*

[MavensMate-SublimeText]: https://github.com/joeferraro/MavensMate-SublimeText
[SFDC Assist]: https://github.com/jairzh/sublime-sfdc-assist
[vim-forcedotcom]: https://github.com/jairzh/vim-forcedotcom
[Sublime Text 2 官网]: http://www.sublimetext.com/)
[Sublime Text 2 入门及技巧]: http://lucifr.com/2011/08/31/sublime-text-2-tricks-and-tips/#alignment)
[视频: sublime text2 简介]: http://v.youku.com/v_show/id_XMzU5NzQ5ODgw.html)








