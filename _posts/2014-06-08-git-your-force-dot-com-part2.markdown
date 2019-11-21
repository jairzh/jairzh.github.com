---
layout: post
title: "Git Your Force.com - Part2"
date: 2014-06-08 21:04:37 +0800
author: jair
comments: true
tags: [git, force.com]
categories: git
---

### Config Your Develpment Environment

Git is working on the local, so if you want to use the git in your force.com project, you need to move your IDE from cloud to local. For now, I think the MavenMate is the best choice.

1. Install git and config it
2. Install [MavensMate](http://mavensmate.com/Plugins/Sublime_Text/Installation)
3. Create a new account in Meginfo Gitlab server and upload your SSH public key


### Use the Git In Your Project

#### Start New Project

If you want to use the git in you new project, please use the following setps to initialize it.

1. Create a new Force.com project by MavensMate in Sublime
2. Open the project and use git to init it
3. Create a new file `.gitignore` below the root of your project and put the following content into it
```
config/
*.sublime-project
*.sublime-workspace
*.log
deploy/
src/staticresources/
```
If you want to check the `static resources`, pelase remeber to remove the end line.

4. Add and commit all the code by git
5. Log in the Gitlab and create a new project
6. Link the remote repository with your local repos and push your code to the remote service

```bash
$ cd existing_git_repo
$ git remote add origin git@scm.meginfo.com:xxxx/xxxx.git
$ git push -u origin master
```

### Work on a Existing Project

If you are new member in a team that is using git and Gitlab as a workflow. You need to do these steps to jion the team to code with them.

<!--more-->

1. Get the git remote link in Gitlab
2. Clone the project from Gitlab into your MavensMate workspace
3. Create the project as a MavesMate project, so you can save your code to the Force.com platform

    * Open the project folder via Sublime
    * Click the right mouse on project name in Sublime sidebar, go to `MavensMate -> Create a new project`
    * Fill out the login credential that you want to work in the force.com org
4. Create a new branch or checkout a existing remote branch
5. Coding follow with our workflow

### Our Workflow

We use the [GitHub Flow](https://guides.github.com/introduction/flow/) which is a lightweight, branch-based workflow in our projects.

![image](http://jairblog.qiniudn.com/git-flow-github-flow.png)

**There's only one rule: Anything in the master branch is always deployable.**

1. Create a branch right from the repository
2. Develop and commit in the feature branch
3. Open a merge request from your branch with your changes to kick off a discussion
4. Others review your code and testing
5. Merge the feature branch into master branch
6. Update the change status and bump a tag with change number, such as `c-31502`
6. Delete the feature branch and deploy your code

### ProTip

- Your branch name should be descriptive (e.g., `refactor-authentication`, `make-retina-avatars`), so that others can see what is being worked on.
- You can also continue to push to your branch after opening a merge request. If someone comments that you forgot to do something or if there is a bug in the code, you can fix it in your branch and push up the change
- **Commit messages** is important, please check here [how to write a good commit message](https://gist.github.com/jairzh/404c8b33b68aae2c39f0)