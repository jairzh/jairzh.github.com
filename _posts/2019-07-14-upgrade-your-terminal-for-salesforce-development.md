---
layout: post
title: "升级你的 Terminal，让 Salesforce 开发更高效"
date: 2019-07-14 08:25:29 +0800
author: jair
image: /assets/images/2019/07/15630875251257.jpg
comments: true
tags: terminal
categories: tool
---

Salesforce 开始主推 Salesforce DX 的开发模式之后，用到 Salesforce CLI 的频率也越来越高了。

使用 sfdx cli 除了 [Salesforce Extensions for VS Code](https://forcedotcom.github.io/salesforcedx-vscode/) 提供的 UI 封装之外，还有很多场景，需要在 Terminal 里直接执行相关命令。但 Mac 内置的 Terminal 并不是很好用，本篇给大家介绍一下，目前我在使用的一套方案，希望能帮助你提升生成效率。

## 亮点

- Terminal 中的 sfdx cli 自动提示
- Terminal 自定义主题
- Terminal 中显示 Git Branch 名字
- Terminal 支持历史命令搜索

## 安装 Homebrew

[Homebrew](https://brew.sh/) 是 macOS(or Linux) 的一款 package 的管理工具，大部分和开发相关的 package 都可以使用它来安装和升级。

复制下面这一行到 Terminal 中，回车确认进行安装。

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

> Note: 安装 Homebrew 的时候会有一些依赖，在安装过程中会有提示，请仔细阅读具体的提示信息。

## 安装 iTerm2

[iTerm2](https://www.iterm2.com/) 相对于 macOS 内置的 Terminal，提供了很多新的特性，大部分的开发人员都选择了 iTerm2 作为主要的执行命令行工具。可以直接到 iTerm2 官网进行[下载安装](https://www.iterm2.com/downloads.html)。可以顺便了解一下提供的[功能特性](https://www.iterm2.com/features.html)

## 安装 ZSH

Zsh 是一种以交互为目的设计的 shell script。macOS 内置了 Zsh，不过这里用 **brew** 重新安装，升级到最新版本。

```bash
brew install zsh
```

## 安装 Oh My Zsh

[Oh My Zsh](https://github.com/robbyrussell/oh-my-zsh) 是一个管理 Zsh 配置的开源项目，提供了丰富的插件、主题和配置。

安装方式

```bash
$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

安装过程中会提示选择 `zsh` 作为默认的 shell script，选择 `Y`。安装成功之后，重启 iTerm2 查看效果。


## 修改 Oh My Zsh 的默认主题

Oh My Zsh 提供了丰富的主题，具体可以在[Github wiki 页面查看](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes)，找到适合自己的主题。推荐我正在使用的 [agnoster](https://github.com/agnoster/agnoster-zsh-theme)，主题提供：

- [Solarized](https://ethanschoonover.com/solarized/) 配色
- Git 信息提示

安装主题，需要修改 `~/.zshrc`，使用默认编辑器打开配置文件。

```bash
open ~/.zshrc
```

修改 ZSH_THEME 这一行

```
ZSH_THEME="agnoster"
```

在 Terminal 的路径中隐藏 “user@hostname”，并控制路径的长度，可以在 `~/.zshrc` 添加：

```
prompt_context(){}

prompt_dir() {
  prompt_segment blue black "%$(( $COLUMNS - 61 ))<...<%3~%<<"
}
```

设置后的效果和更多配置方式可以点击这里查看[Trim long paths in prompt](https://github.com/wesbos/Cobalt2-iterm/issues/15#issuecomment-224957918)

## 安装字体

Oh My Zsh 的主题需要配合 [powerline fonts](https://github.com/powerline/fonts)，才可以保证主题内容的完整显示。

使用下面方式安装

```bash
# clone
git clone https://github.com/powerline/fonts.git --depth=1

# install
cd fonts
./install.sh

# clean-up a bit
cd ..
rm -rf fonts
```

安装成功后，修改 iTerm 的默认字体 `iTerm2 > Preferences > Profiles > Text > Change Font.`

![](/assets/images/2019/07/15625000896823.jpg)

字体可以选自己喜欢的，不过需要有 `for Powerline` 结尾的。


## 修改 iTerm2 的主题

[Iterm2-color-schemes](https://iterm2colorschemes.com/) 在这个网站上下载主题包，然后安装下面方式启动主题：

`iTerm2 > Preferences > Profile > Colors > Color Presets > Import`

![](/assets/images/2019/07/15630709159672.jpg)

上面几步配置好，重启 Terminal，就可以拥有一个全新的 Terminal 了 🎉

![](/assets/images/2019/07/15625003271577.jpg)


## 安装 Plugins

Zsh 有丰富的插件，可以提供各种扩展，这里介绍几个插件：

### [salesforce-cli-zsh-completion](https://github.com/wadewegner/salesforce-cli-zsh-completion)

Salesforce 团队成员写了一个 sfdx cli 的插件，可以实现自动提示，可以帮助我们在使用 sfdx cli 的时候快速找到需要的命令。

安装方式

```bash
git clone https://github.com/wadewegner/salesforce-cli-zsh-completion.git ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/salesforce-cli-zsh-completion
```

编辑 `~/.zshrc`，添加 `salesforce-cli-zsh-completion` 到 plugins 中

```
# ~/.zshrc
plugins=(salesforce-cli-zsh-completion)

source $ZSH/oh-my-zsh.sh

autoload -U compinit && compinit
```

执行下面的命令重新加载配置

```bash
source ~/.zhrc
```

`cd` 到一个 sfdx cli 初始化的项目，然后输入 `sfdx`，点击两次 tab 键，就会有提示了，enjoy it！

### 推荐其他插件

- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting): This package provides syntax highlighting for the shell zsh. It enables highlighting of commands whilst they are typed at a zsh prompt into an interactive terminal. This helps in reviewing commands before running them, particularly in catching syntax errors.
- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions): It suggests commands as you type based on history and completions.
- [git](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins#git): Adds a lot of git aliases and functions for day-to-day git operations.


插件安装配置好之后，`.zshrc` 文件中的 plugins:

```
plugins=(
  git
  salesforce-cli-zsh-completion
  zsh-autosuggestions
  zsh-syntax-highlighting
)
```


## 快捷键

在命令行操作的时候，因为对光标的支持并不是很好，可以掌握一些基本的快捷键，帮助你快速的完成一些操作。

- `Ctrl + a`: 移动到当前行的最前面
- `Ctrl + e`: 移动到当前行的最后面
- `Ctrl + r`: 搜索之前命令
- `Ctrl + w`: 删除光标前的单词
- 在 Tab 之间切换: `Cmd + →` 和 `Cmd + ←`
- `Cmd + D` 打开一个新的 Pane
- 在 Pane 之间切换 `Cmd + ]` 和 `Cmd + [`

配置 iTerm 的全局切换 hotkey:

`iTerm2 > Preferences > Keys > Show/hide iTerm with a system-wide hotkey` 指定一个你想使用的一个快捷键，比如 `Ctrl + space`。

## 别名 Aliases

添加一些 alias 可以让你更方便的输入一些你常用的命名或者命令组合。配置方式很简单，只需要在 `.zshrc` 中添加就可以了。比如：

```bash
alias sfpush="sfdx force:source:push --json --loglevel fatal"
```

另外 `oh-my-zsh` 内置了很多 alias，比如 [git alias](https://github.com/robbyrussell/oh-my-zsh/blob/master/plugins/git/git.plugin.zsh)

## VS Code 配置

在 Settings 里加入下面两行，让上面的配置在 VS Code 的 Terminal 生效。

```json
"terminal.integrated.fontFamily": "Source Code Pro for Powerline",
"terminal.integrated.shell.osx": "/bin/zsh"
```

## 参考

- [How to Configure your macOs Terminal with Zsh like a Pro](https://www.freecodecamp.org/news/how-to-configure-your-macos-terminal-with-zsh-like-a-pro-c0ab3f3c1156/)
- [5 ways to upgrade your Terminal](https://medium.com/@sahanarajasekar/5-ways-to-upgrade-your-terminal-2fb8ab447949)


