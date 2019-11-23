---
layout: post
title: "迁移到 Salesforce Extensions for Visual Studio Code"
date: 2019-04-22 08:25:29 +0800
author: jair
comments: true
tags: "vs-code apex"
categories: tool
---

Salesforce 新推出的 IDE 经过不断的迭代升级之后，现在已经很成熟，所有现在完全可以从 Sublime 或者 Eclipse 迁移到 [Salesforce Extensions for Visual Studio Code](https://forcedotcom.github.io/salesforcedx-vscode/articles/user-guide/org-development-model)。

### 安装 Salesforce Extensions for VS Code

安装步骤可以参考官方文档 [Install Salesforce Extensions for VS Code](https://forcedotcom.github.io/salesforcedx-vscode/articles/getting-started/install)

### VS Code 的优势

VS Code 的插件是官方在维护，这里列几点和其他 IDE 相比的优势：

- 官方团队维护，更新速度快，不断会有新功能的推出
- 强大的智能提示
- 运行测试代码方便
- 支持所有 Salesforce 相关的开发，包括 Apex, VF, Aura 和 LWC
- 支持两种开发模式，Org Development 和 Package Development Model

### 开发新项目

如果是新项目，就可以直接使用 Salesforce 提供的文档进行开发就可以了，具体可以看这里：[https://forcedotcom.github.io/salesforcedx-vscode](https://forcedotcom.github.io/salesforcedx-vscode)

### 迁移旧项目

很多现有项目，基本在使用 Sublime 或者 Eclipse 开发，这里提供一下具体的迁移步骤:

1 打开 VS Code，然后 打开 command palette (press Ctrl+Shift+P on Windows or Linux, or Cmd+Shift+P on macOS) 运行 **SFDX: Create Project with manifest**，输入和旧项目相同的名字，然后找到原来项目的上级目录，回车确认，等待成功之后，会把原来的项目初始化成一个 VS Code 的项目。

2 更新 **.gitignore** 文件，忽略 VS Code 相关不需要提交的文件和文件夹。

```
# Ignore OS system files that are usually auto-added to directories
[Tt]humbs.db
.DS_Store

# Ignore Salesforce DX config files
.sfdx
.salesforce

# Ignore VSCode config files
.vscode
```

保存之后，git 提交一下。

3 执行下面的命令转化 Source format 并且保留 git 的提交历史。

```bash
git config merge.renameLimit 999999
sfdx force:mdapi:convert -r src -d src2
rm -rf src
mv src2 src
git add -A
git commit -m "Converted from metadata to source format"
git config --unset merge.renameLimit # Return the git config option to the default
```

4 打开 command palette 执行 **SFDX: Authorize an Org**，登陆一个 org，然后就可以正常开发了。


## References

- [Org Development Model with VS Code](https://forcedotcom.github.io/salesforcedx-vscode/articles/user-guide/org-development-model)
- [Converting Salesforce Metadata to Source Format While Maintain Git History](https://ntotten.com/2018/05/11/convert-metadata-to-source-format-while-maintain-git-history/)

