---
layout: post
title: "Git Your Force.com - Part1"
date: 2013-05-25 11:49
author: jair
comments: true
tags: [ git, force.com]
categories: tool
---

在 force.com 这个平台上使用 Git 已经有很长的一段时间了，在应用上面有一些体会，所以准备整理出来。这篇主要写了一些基本的配置和学习资料。

PS: 本文所有的内容只针对 Mac 系统，所以其他操作系统仅供参考了。

## Installing Git

在 Mac 系统上安装的话，推荐下面的两种安装方式：

### 使用 Git package

直接从下面的地址下载 Git 的安装包，安装就可以了。下载的时候注意选择适合自己系统的版本。

`https://code.google.com/p/git-osx-installer/`

### 使用 `Homebrew` 安装

[Homebrew], 是 OS X 上非常好的一个工具，能帮助你快速的安装上百种开源的工具。如果你准备长期使用 Mac 作为你的主要开发平台的话，推荐的安装 [Homebrew]。

<!--more-->

1. Install Homebrew

    在命令行执行下面的命令：

    ```
    $ ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"
    ```

    命令执行成功以后，使用下面的命令检查一下是否能正常运行：

    ```
    $ brew doctor
    ```

    如果出现错误或者警告，可以安装提示修复，更多可以参考这里。

    `https://github.com/mxcl/homebrew/wiki/Installation`

2. Install Git

    Homebrew 正常安装后，就可以直接使用下面的命令安装 Git 了。

```
$ brew update
$ brew install git
```

## 初次运行 Git 前的配置

Git 使用 gitconfig 文件来存储 Git 的配置信息的，如果你是第一次安装 Git 的话，需要做一次初始化配置，来保证以后的 Git 的正常运行。

### 用户信息

第一个要配置的是你个人的用户名称和电子邮件地址。这两条配置很重要，每次 Git 提交时都会引用这两条信息，说明是谁提交了更新:

```
$ git config --global user.name "Your Full Name"
$ git config --global user.email "Your Email Address"
```

因为使用 --global 选项，所以信息存储在了 ~/.gitconfig 文件里，可以打开 Terminal，执行下面的命令查看内容。

```
$ cat ~/.gitconfig
```

如果日后需要修改信息，就可以直接更新这个文件就可以了。因为这个是 global 的设置，如果一个项目需要单独设置 `user name` 和 `email` 的话，可以进行项目内去掉 `--global`， 直接执行上面的命令。更多信息

### Diff Tool

使用 Git 用来比较代码差异和 merge code 是非常方便的，但这里需要一个比较顺手的 diff tool，推荐使用 [p4merge]。

1. 下载 p4merge: [http://www.perforce.com/product/components/perforce-visual-merge-and-diff-tools][p4merge]
2. 把 p4merge 放到 `Applications` 中

    <img src="https://www.evernote.com/shard/s54/sh/08b31d02-338b-4995-a897-a87c73443e83/57c483f26d1c380167f2e8d032346b6f/deep/0/p4merge.png" alt="p4merge" />

3. 更改 `.gitconfig` 配置信息

   - 在命令行使用 `vim ~/.gitconfig`，按 `i` 进去编辑模式，update，按`esc` 退出编辑模式，输入 `:wq`，退出保存。
   - 或者使用 `sumlime` 等其他文本编辑工具直接编辑，但在打开文件的时候需要按一下 `cmd+shift+.` 显示隐藏文件才可以。

   具体配置：

```
[merge]
    tool = mymerge

[mergetool "mymerge"]
    cmd = /Applications/p4merge.app/Contents/MacOS/p4merge  \"$BASE\" \"$LOCAL\" \"$REMOTE\" \"$MERGED\"
    trustExitCode = false

[mergetool]
    keepBackup = false

[diff]
    tool = mydiff
    trustExitCode = false

[difftool "mydiff"]
    cmd = /Applications/p4merge.app/Contents/MacOS/p4merge  \"$BASE\" \"$LOCAL\"
    trustExitCode = false
```

通过上面的配置，就可以在命令行，使用 `git difftool` 和 `git mergetool` 来打开 `p4merge` 了。

## Git 使用

Git 如果以前没有接触过的会感觉有些难，因为需要使用一些命令行才可以。但是现在的 `GUI` 工具已经有不错的了。所以基本使用上，不太需要去刻意记 Git 的命令。

### Git GUIs
这里推荐俩款 Mac 上不错的图形化工具。

[GitX(L)](http://gitx.laullon.com/)

GitX(L) 是我一直在使用的一个工具，界面简洁，提交操作步骤简单，能够满足我平时的各种需求。如果你选择 GitX 的话，推荐安装一下面的辅助工具：

1. [OpenInGitX](https://code.google.com/p/git-osx-installer/wiki/OpenInGitX): 下载以后放到 `Applications` 中，然后将其拖放到 Finder 的工具栏上，就可以在有 Git 的项目里，直接用 GitX 查看当前目录的 Git 信息了。
2. `Enable Terminal Usage`: 在 GitX 的配置中开启这个选项，可以直接在 terminal 中，使用 `gitx` 命令打开 GitX 工具。
3. [SublimeTextGitX](https://github.com/fabiocorneti/SublimeTextGitX): Sublime 中打开 GitX 的一个工具，直接用 Package Controle 安装就可以了，具体可以看一下[这里](https://github.com/fabiocorneti/SublimeTextGitX#installing)。


[SourceTree](http://www.sourcetreeapp.com/)

这是一个功能非常强大的工具，强大到你机器上不用单独安装 Git(因为内置了)，都可以让你正常使用 Git 的功能。而且应该是有一个很不错的团队在维护这个工具，所以这里推荐尝试。
而且有一个不错的功能就是集成了，我非常喜欢的 [Git-flow]。

个人感觉关于工具的话，找到最适合自己的就可以了，所以没有必要太纠结，可以多尝试一些，更多的工具可以看一下 [Git 官网的推荐](http://git-scm.com/download/gui/mac)。


### Git 学习资源

我不准备写如何使用一些基本的 git 命令了，因为网上的资源已经非常多了，所以我这里给大家列出一些平时看到的不错的资料。

PS：学习 Git 的命令，最好在命令行多敲一敲，不要上来就直接使用 GUI 工具。掌握了一些基本命令后，再使用 GUI 就很得心应手了。

- [git - the simple guide](http://rogerdudler.github.io/git-guide/)：让你了解 Git 里最常用的命令的作用。
- [Try Git](http://try.github.io/): 了解以后，可以在这里进行实践。
- [Pro Git](http://git-scm.com/book/zh): 如果真的想用好 Git，推荐这本书。
- [Code School Git Real](http://www.codeschool.com/courses/git-real): 非常不错的视频教程

### Git Terminal

#### 自动提示

虽然图像的话工具很方便，但是不得不说 Git 的命令行更加强大，但是命令行都是需要手动敲入的，有的时候单词写错是很容易的事，所以这里一个 `tab` 自动提示还是很有用的。

1. Install bash-completion: `brew install git bash-completion`
2. Add bash-completion to your `.bash_profile`:

```
if [ -f `brew --prefix`/etc/bash_completion ]; then
    . `brew --prefix`/etc/bash_completion
fi
```

`.bash_profile` 是 Terminal 的配置文件，在根目录下，可以使用 `vim ~/.bash_profile` 直接打开编辑。如果没有可以新创建一个。

通过上面的配置以后，在使用 Git 的时候，敲 `Tab` 键，会减少你很多的输入量。

#### 显示 Branch Name:

把下面的内容添加到 ` .bash_profile` 中：

```
PS1='\[\033[32m\]\u@\h\[\033[00m\]:\[\033[34m\]\W\[\033[31m\]$(__git_ps1)\[\033[00m\]\$ '
```

如果提示 `__git_ps1` 不存在，试着执行 `brew link git` 来修复。


## Git 常用命令

平时使用 Git 记录了一些自己常用，但是不太容易记忆的命令。

- 比较两次提交之间的文件，只显示文件名

``` bash
$ git diff --stat
```

- Git cherry-pick 和 rebase 的混合用法

``` bash
$ git checkout master
$ git cherry-pick D
$ git cherry-pick F
$ git checkout topic
$ git rebase master
```

- Removing Git Tag On Remote Repository

``` bash
$ git push origin :refs/tags/<mytag>
```

- Check the conflict, but do not merge branch

``` bash
# In the master branch
$ git merge dev --no-ff --no-commit  git merge —abort
```

- Removing the Remote Branch

``` bash
$ git push origin :branch_name
```

- How many lines of code were added/changed by one person

``` bash
$ git log --shortstat --author "jair" --since "2012-01-01" --until "2013-09-01" | grep "files changed" | awk '{files+=$1; inserted+=$4; deleted+=$6} END {print "files changed", files, "lines inserted:", inserted, "lines deleted:", deleted}’
```

- Change commit author at one specific commit

``` bash
$ git commit --amend --author="Author Name <email@address.com>"
```


通过上面的安装和配置，希望大家能够正常在电脑里使用 Git。最后分享一些其他资源，有我自己在 Github 上的 Git 和 Terminal 的配置文件。

## Links

>[Progit: Getting Started - First-Time Git Setup](http://git-scm.com/book/en/Getting-Started-First-Time-Git-Setup)

>[Git 和 Terminal 的配置文件](https://github.com/jairzh/dotfiles)

>[Git help](https://help.github.com/)







[git-download]:https://code.google.com/p/git-osx-installer/
[Homebrew]: http://mxcl.github.io/homebrew/
[p4merge]: http://www.perforce.com/product/components/perforce-visual-merge-and-diff-tools
[Git-flow]: https://github.com/nvie/gitflow