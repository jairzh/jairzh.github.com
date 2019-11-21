---
layout: post
title: "Using Ant in Salesforce.com"
date: 2014-02-22 15:58
author: jair
comments: true
tags: [ ant, force.com ]
categories: tool
---

[Ant](http://ant.apache.org/) (Another Neat Tool) 作为一个的部署工具，Salesforce 在开发的时候同样支持，Force.com Migration Tool 就是基于 Ant 的 SFDC 的部署工具。SFDC 的文档中建议下面的情况使用 Migration Tool:


* Development projects where you need to populate a test environment with large amounts of setup change
* Multistage release processes
* Repetitive deployment using the same parameters
* When migrating from stage to production is done by IT

在项目中使用 Ant 的话，如果是整个项目的迁移的话，Ant 是非常好用的。因为操作速度和等待的结果的时间都是要比 Eclipse 快很多。

Ant 在运行的是会读取 `build.xml` 中的配置信息，执行对应的 task。

在 SFDC 中 Ant 可以实现下面的功能：

* Deploying Changes
* Retrieving Metadata
* Deleting Files
* Run All Tests

<!--more-->

## Installation

因为 Force.com Migration Tool 运行在命令行的，所以需要使用之前需要在电脑上按照 Java 和 Ant。这里话就只介绍 Mac 系统如何配置。关于其他系统可以参考 SFDC 的在线文档－[Force.com Migration Tool Guide](http://www.salesforce.com/us/developer/docs/daas/index_Left.htm#StartTopic=Content/forcemigrationtool_prereq.htm)

### Installing Ant

1. Install Ant to use the [Homebrew](http://brew.sh/) at the command prompt: `brew install ant`
2. Type the `ant` to check the Ant has been installed.

### Installing Force.com Migration Tool
1. Log into a Salesforce organization on your deployment machine.
2. From **Setup**, click **Develop > Tools**, and then click **Force.com Migration Tool**.
3. Save the .zip file locally and extract the contents to the directory of your choice.
4. Copy **ant-salesforce.jar** and paste into your Ant installation's lib directory. (usually `/usr/local/Cellar/ant/1.9.3/libexec/lib`)


## Configration

解压在上一步骤中下载的 .zip 包一会，可以看到下面的目录结构：

```
salesforce_ant/
├── sample/
│   ├── codepgk/
│   ├── mypkg/
│   ├── removecodepkg/
│   ├── unpackaged/
│   ├── build.properties
│   └── build.xml
├── ant-salesforce.jar
├── Readme.html
```
其中最重要的两个文件就是 `build.xml` 和 `build.proerties`

* build.xml: 包含了需要 custom Ant tasks
* build.proerties: 用来存储配置信息，如 org username 和 password. 当然你也可以直接把 UN 和 Pwd 直接写在 build.xml 文件中。

#### build.proerties

``` ruby
# Specify the login credentials for the desired Salesforce organization
sf.usernameDev = jair@test.com
sf.passwordDev = xxxxxxxxxxx

sf.usernameSandbox = jair@test.com
sf.passwordSandbox = xxxxxxxxxxx

#sf.pkgName = <Insert comma separated package names to be retrieved>
#sf.zipFile = <Insert path of the zipfile to be retrieved>
#sf.metadataType = <Insert metadata type name for which listMetadata or bulkRetrieve operations are to be performed>

# Use 'https://login.salesforce.com' for production or developer edition (the default if not specified).
# Use 'https://test.salesforce.com for sandbox.
sf.serverurl = https://login.salesforce.com

sf.maxPoll = 20
# If your network requires an HTTP proxy, see http://ant.apache.org/manual/proxy.html for configuration.

```

可以配置多个 Org 的 login credentials 在里边，用于不同的 task.

#### build.xml


``` xml
<project name="Sample usage of Salesforce Ant tasks" default="test" basedir="." xmlns:sf="antlib:com.salesforce">
    <property file="build.properties"/>
    <property environment="env"/>

    <!-- Shows deploying code & running tests for code in directory -->
    <target name="deployCode">
      <!-- Upload the contents of the "codepkg" directory, running the tests for just 1 class -->
      <sf:deploy username="${sf.usernameDev}" password="${sf.passwordDev}" serverurl="${sf.serverurl}" maxPoll="${sf.maxPoll}" deployRoot="codepkg">
        <runTest>SampleDeployClass</runTest>
      </sf:deploy>
    </target>
</project>

```

## Run Task

做好相应的配置一会，打开命令行，`cd` 到 `sample/` 目录，在 command line 执行 `ant deployCode`，如果 login credentials 没有问题，代码就可以部署到对应的 org 了。这里的话是介绍了一个 taks，如果想了解更多的内容的话，可以看 Salesfroce 提供的
Force.com Migration Tool Guide。

Note：Ant 在执行过程中，具体那些 metadata files 被部署了，是由 `package.xml` 这个文件决定的。关于如何配置 `package.xml` 可以参考 Salesforce 的文档，[Sample package.xml Manifest Files](http://www.salesforce.com/us/developer/docs/api_meta/Content/manifest_samples.htm)

## CONCLUSION

Ant 在平时的工作中用的会相对少一些，因为开发的还是用 IDE 会更方便一些。不过在持续集成和大量文件下需要部署的时候还是非常用用的。下面提供两个不错的应该，有兴趣可以看看:

* [grunt-ant-sfdc](https://github.com/kevinohara80/grunt-ant-sfdc): 一个基于 grunt 和 ant 的工具，可以用于 static resource 的部署等。更多可以查看作者在 df13 上的一个 session [Automate Static Resource Builds With Node.js and Grunt](http://youtu.be/W4Fyiz4Gocw)
* [How to Setup Continuous Integration With Git, Jenkins, and Force.com](http://youtu.be/a0u1CBUsj_I)：和 Jenkins 做持续集成，如果有多人需要持续开发的项目的话，这是一个很好的选择。



## Related Posts

- [Force.com Migration Tool Guide](http://www.salesforce.com/us/developer/docs/daas/salesforce_migration_guide.pdf)
- [Salesforce.com Ant Migration Tool](http://www.soliantconsulting.com/blog/2013/07/salesforcecom-ant-migration-tool)

