---
layout: post
title: "Git 简明教程"
categories: Manual
tags: Git
---

* content
{:toc}

## Git 简介

### Git 是何方神圣？

Git 是用 C 语言开发的分布版本控制系统。版本控制系统可以保留一个文件集合的历史记录，并能回滚文件集合到另一个状态（历史记录状态）。另一个状态可以是不同的文件，也可以是不同的文件内容。举个例子，你可以将文件集合转换到两天之前的状态，或者你可以在生产代码和实验性质的代码之间进行切换。文件集合往往被称作是“源代码”。在一个分布版本控制系统中，每个人都有一份完整的源代码（包括源代码所有的历史记录信息），而且可以对这个本地的数据进行操作。分布版本控制系统不需要一个集中式的代码仓库。






当你对本地的源代码进行了修改，你可以标注他们跟下一个版本相关（将他们加到 index 中），然后提交到仓库中来（commit）。git 保存了所有的版本信息，所以你可以转换你的源代码到任何的历史版本。你可以对本地的仓库进行代码的提交，然后与其他的仓库进行同步。你可以使用Git来进行仓库的克隆（clone）操作，完整的复制一个已有的仓库。仓库的所有者可以通过 push 操作（推送变更到别处的仓库）或者 Pull 操作（从别处的仓库拉取变更）来同步变更。

Git 支持分支功能（branch）。如果你想开发一个新的产品功能，你可以建立一个分支，对这个分支的进行修改，而不至于会影响到主支上的代码。

Git 提供了命令行工具；这个教程会使用命令行。你也可以找到图形工具，譬如与 Eclipse 配套的 EGit 工具，但是这些都不会在这个教程中进行描述。

### 重要术语

* 仓库（Repository）

一个仓库包括了所有的版本信息、所有的分支和标记信息。在Git中仓库的每份拷贝都是完整的。仓库让你可以从中取得你的工作副本。

* 分支（Branches）

一个分支意味着一个独立的、拥有自己历史信息的代码线（code line）。你可以从已有的代码中生成一个新的分支， 这个分支与剩余的分支完全独立。默认的分支往往是叫 master。用户可以选择一个分支，选择一个分支叫做checkout。

* 标记（Tags）

一个标记指的是某个分支某个特定时间点的状态。通过标记，可以很方便的切换到标记时的状态，例如2009年1月25号在testing分支上的代码状态。

* 提交（Commit）

提交代码后，仓库会创建一个新的版本。这个版本可以在后续被重新获得。每次提交都包括作者和提交者，作者和提交者可以是不同的人。

* URL

URL用来标识一个仓库的位置。

* 修订（Revision）

用来表示代码的一个版本状态。Git通过用SHA1 hash算法表示的id来标识不同的版本。每一个SHA1 id都是160位长，16进制标识的字符串.最新的版本可以通过HEAD来获取。之前的版本可以通过"HEAD~1"来获取，以此类推。

### 索引

Git 需要将代码的变化显示的与下一次提交进行关联。举个例子，如果你对一个文件继续了修改，然后想将这些修改提交到下一次提交中，你必须将这个文件提交到索引中，通过 `git add file` 命令。这样索引可以保存所有变化的快照。

新增的文件总是要显示的添加到索引中来。对于那些之前已经提交过的文件，可以在 `commit` 命令中使用 `-a` 选项达到提交到索引的目的。

## Git安装

在 Ubuntu 上，你可以通过 apt 来安装 git 命令行工具

~~~bash
$ sudo apt-get install git-core
~~~

对于其他的 Linux 版本，请查看相关的软件包安装工具使用方法。msysgit 项目提供了 Windows 版本的 Git，地址是 <http://code.google.com/p/msysgit/>

## Git 配置

你可以在 `.gitconfig` 文件中防止 git 的全局配置，文件位于用户的 home 目录。上述已经提到每次提交都会保存作者和提交者的信息，这些信息都可以保存在全局配置中。后续将会介绍配置用户信息、高亮显示和忽略特定的文件。

### 用户信息

通过如下命令来配置用户名和 Email：

~~~bash
$ git config --global user.name "Example Username"
$ git config --global user.email "your.email@gmail.com"

# Set default so that all changes are always pushed to the repository
$ git config --global push.default "matching"
~~~

获取 Git 配置信息，执行以下命令：

~~~bash
$ git config --list
~~~

### 高亮显示

以下命令会为终端配置高亮：

~~~bash
$ git config --global color.status auto
$ git config --global color.branch auto
~~~

### 忽略特定的文件

可以配置 Git 忽略特定的文件或者是文件夹。这些配置都放在 `.gitignore` 文件中。这个文件可以存在于不同的文件夹中，可以包含不同的文件匹配模式。为了让 Git 忽略 bin 文件夹，在主目录下放置 `.gitignore` 文件，其中内容为 bin。 

同时 Git 也提供了全局的配置，`core.excludesfile`。

### 使用 .gitkeep 来追踪空的文件夹

Git 会忽略空的文件夹。如果你想版本控制包括空文件夹，根据惯例会在空文件夹下放置 .gitkeep 文件。其实对文件名没有特定的要求。一旦一个空文件夹下有文件后，这个文件夹就会在版本控制范围内。

## 开始操作 Git

后续将通过一个典型的 Git 工作流来学习。在这个过程中，你会创建一些文件、创建一个本地的 Git 仓库、提交你的文件到这个仓库中。这之后，你会克隆一个仓库、在仓库之间通过 pull 和 push 操作来交换代码的修改。注释（以 # 开头）解释了命令的具体含义，让我们打开命令行开始操作吧。

### 创建内容

下面创建一些文件，它们将会被放到版本控制之中

~~~bash
# Switch to home
$ cd ~

# Create a directory
$ mkdir repo01

# Switch into it
$ cd repo01

# Create a new directory
$ mkdir datafiles

# Create a few files
$ touch test01
$ touch test02
$ touch test03
$ touch datafiles/data.txt

#Put a little text into the first file
$ ls > test01
~~~

### 创建仓库、添加文件和提交更改

每个 Git 仓库都是放置在 `.git` 文件夹下.这个目录包含了仓库的所有历史记录，`.git/config` 文件包含了仓库的本地配置。

以下将会创建一个Git仓库，添加文件倒仓库的索引中，提交更改。

~~~bash
# Initialize the local Git respository
$ git init

# Add all (files and directories) to Git repository
$ git add .

# Make a commit of your file to the local repository
$ git commit -m "Initial commit"

# Show the log file
$ git log
~~~

### diff命令与commit更改

通过 `git diff` 命令，用户可以查看更改。通过改变一个文件的内容，看看 `git diff` 命令输出什么，然后提交这个更改到仓库中。

~~~bash
# Make some changes to the file
$ echo "This is a change" > test01
$ echo "This is another change" > test02

# Check the changes via the diff command
$ git diff

# Commit the changes, -a will commit changes for modified files
# but will not add automatically new files
$ git commit -a -m "These are new changes"
~~~

### status, diff and commit log

下面会向你展示仓库现有的状态以及过往的提交历史

~~~bash
# Make some changes in the file
$ echo "This is a new change" > test01
$ echo "This is anotner new change" >test02

# See the corrent status of your repository
# Which files are changed / new / deleted
$ git status

# Show the differemces netween the uncommitted files
# and the last commit in the current branch 
$ git diff

# Add the changes to che index and commit
$ git add . && git commit -m "More changes - typo in the commit message"

# Show the history of commit in the current branch
$ git log

# This starts a nice graphical view of the changes
$ gitk --all
~~~

### 更正提交的信息 - `git -amend`

通过 `git amend` 命令，我们可以修改最后提交的信息。上述提交的信息中心存在错误，下面我们修改这个错误。

~~~bash
$ git commit --amend -m "More changes - now correct"
~~~

### 删除文件

如果你删除了一个在版本控制下的文件，那么使用 `git add .` 不会在索引中删除这个文件。需要通过带 `-a` 选项的 `git commit` 命令和 `-A` 选项的 `git add` 命令来完成。

~~~bash
# Create a file and put it under version control
$ touch nosense.txt
$ git add . && git commit -m "a new files has been created"

# Remove the file
$ rm -f nosense.txt

# Try standard way of committing -> will not work
$ git add . && git commit -m "a new file has been created"

# Now commit with the -a flag
$ git commit -a -m "File nonsense.txt is now removed"

# Alternatively you could add deleted files to the staging index via
$ git add -A .
$ git commit -m "File nonsense.txt is now removed"
~~~

## 远端仓库（remote repositories）

### 设置一个远端的 Git 仓库

我们将创建一个远端的 Git 仓库。这个仓库可以存储在本地或者是网络上。

远端 Git 仓库和标准的 Git 仓库有如下差别：一个标准的 Git 仓库包括了源代码和历史信息记录。我们可以直接在这个基础上修改代码，因为它已经包含了一个工作副本。但是远端仓库没有包括工作副本，只包括了历史信息。可以使用 `–bare` 选项来创建一个这样的仓库。

为了方便起见，示例中的仓库创建在本地文件系统上。

~~~bash
# Switch to the first repository
$ cd ~/repo01

$ git clone --bare . ../remote-repository.git

# Check the content, it is identical to the .git directory in repo01
$ ls ~/remote-repository.git
~~~

### 推送更改到其它的仓库

~~~bash
$ cd ~/repo01

$ echo "Hello! Turn your radio on." > test01
$ echo "Hello! Turn your radio off." > test02

$ git commit -a -m "Some changes"
$ git push ../remote-repository.git
~~~

### 添加远端仓库

除了通过完整的 URL 来访问 Git 仓库外，还可以通过 `git remote add` 命令为仓库添加一个短名称。当你克隆了一个仓库以后，origin 表示所克隆的原始仓库。即使我们从零开始，这个名称也存在。

~~~bash
# Add ../remote-repository.git with the name origin
$ git remote add origin ../remote-repository.git

# Again some changes
$ echo "I added a remote repo" > test02

# Commit
$ git commit -a -m "This is a test for the new remote origin"

# If you do not label a repository it will push to origin
$ git push origin
~~~

### 显示已有的远端仓库

通过以下命令查看已经存在的远端仓库

~~~bash
# Show the existing defined remote repositories
$ git remote
~~~

### 克隆仓库

通过以下命令在新的目录下创建一个新的仓库

~~~bash
# Switch to home
$ cd ~

# Make new directory
$ mkdir repo02

# Switch to new directory
$ cd ~/repo02

# clone
$ git clone ../remote-repository.git .
~~~

### 拉取（pull）更改

通过拉取，可以从其他仓库中获取最新的更改。

在第二个仓库中，做一些更改，然后将更改推送到远端仓库中。然后第一个仓库拉取这些更改。

~~~bash
# Switch to home
$ cd ~

#Switch to second directory
$ cd ~/repo02

# Make Changes
$ echo "A change" > test01

# Commit
$ git commit -a -m "A change"

# Push the change to the remote repository
# Origin is automatically maintained as we cloned from this repository
$ git push origin

# Switch to the first repository and pull in the changes
$ cd ~/repo01
$ git pull ../remote-repository

# Check the changes
$ less test01
~~~

### 还原更改

如果在你的工作副本中，你创建了不想被提交的文件，你可以丢弃它。

~~~bash
# Create a new file with content
$ touch test04
$ echo "This is trash" > test04

#Make a dry-run to see what would happen
# -n is the same as --dry-run
$ git clean -n

# Now delete
$ git clean -f
~~~

你可以提取老版本的代码，通过提交的ID。`git log` 命令可以查看提交 ID

~~~bash
# Switch to home
$ cd ~/repo01

# Get the log
$ git log

# Copy one of the older commits and checkout the older revision via
# 译者注：checkout 后加 commit id 就是把 commit 的内容复制到 index 和工作副本中 
$ git checkout commit_name
~~~

如果你还未把更改加入到索引中，你也可以直接还原所有的更改

~~~bash
# Some nosense change
$ echo "nosense change" > test01

# Not added to the staging index.
# Therefore we can just checkout the old version
# 译者注：checkout后如果没有commit id号，就是从index中拷贝数据到工作副本，不涉及commit部分的改变
$ git checkout test01

# Check the result
$ cat test01

# Another nonsense change
$ echo "another nonsense change" > test01

# We add the file to the staging index
$ git add test01

# Restore the file in the staging index
# 译者注：复制 HEAD 所指 commit 的 test01 文件到 index 中
$ git reset HEAD test01

# Get the old version from the staging index
# 译者注：复制 index 中 test01 到工作副本中
$ git checkout test01

# 译者注，以上两条命令可以合并为 git checkout HEAD test01

也可以通过 revert 命令进行还原操作

~~~bash
# Revert a commit
$ git revert commit_name
~~~

即使你删除了一个未添加到索引和提交的文件，你也可以还原出这个文件

~~~bash
# Delete a file
$ rm test01

# Revert the deletion
$ git checkout test01
~~~

如果你已经添加一个文件到索引中，但是未提交。可以通过 `git resetfile` 命令将这个文件从索引中删除

~~~bash
# Create a file
$ touch incorrect.txt
   
# Accidently add it to the index
$ git add .

# Remove it from the index
$ git reset incorrect.txt
   
# Delete the file
$ rm -f incorrect.txt
~~~

如果你删除了文件夹且尚未提交，可以通过以下命令来恢复这个文件夹。

译者注：即使已经提交，也可以还原

~~~bash
git checkout HEAD -- your_dir_to_restore
~~~

### 标记

Git 可以使用对历史记录中的任一版本进行标记。这样在后续的版本中就能轻松的找到。一般来说，被用来标记某个发行的版本。可以通过 `git tag` 命令列出所有的标记，通过如下命令来创建一个标记和恢复到一个标记

~~~bash
$ git tag version1.6 -m 'version 1.6'
$ git checkout <tag_name>
~~~
## 分支、合并

### 分支

通过分支，可以创造独立的代码副本。默认的分支叫master。Git消耗很少的资源就能创建分支，Git鼓励开发人员多使用分支。

下面的命令列出了所有的本地分支，当前所在的分支前带有*号

~~~bash
$ git branch
~~~

如果你还想看到远端仓库的分支，可以使用下面的命令

~~~bash
$ git branch -a
~~~

可以通过下面的命令来创建一个新的分支

~~~bash
# Syntax: git branch <name> <hash>
# <hash> in the above is optional 
# if not specified the last commit will be used
# If specified the corresponding commit will be used
$ git branch testing

# Switch to your new branch
$ git checkout testing

# Some changes
$ echo "Cool new feature in this branch" > test01
$ git commit -a -m "new feature"

# Switch to the master branch
$ git checkout master

# Check that the content of test01 is the old one
$ cat test01
~~~

### 合并

通过 Merge 我们可以合并两个不同分支的结果。Merge 通过所谓的三路合并来完成。分别来自两个分支的最新 commit 和两个分支的最新公共 commit。可以通过如下的命令进行合并：

~~~bash
# Syntax: git merge <branch-name>
$ git merge testing
~~~

一旦合并发生了冲突，Git 会标志出来，开发人员需要手工的去解决这些冲突。解决冲突以后，就可以将文件添加到索引中，然后提交更改

### 删除分支

删除分支的命令如下：

~~~bash
# Delete branch testing
$ git branch -d testing

# Check if branch has been deleted
$ git branch
~~~

### 推送（push）一个分支到远端仓库

默认的，Git 只会推送匹配的分支的远端仓库。这意味在使用 `git push` 命令默认推送你的分支之前，需要手工的推送一次这个分支。

~~~bash
# Push testing branch to remote repository
$ git push origin testing

# Switch to the testing branch
$ git checkout testing

# Some changes
$ echo "News for you" > test01
$ git commit -a -m "new feature in branch"

# Push all including branch
$ git push
~~~

通过这种方式，你可以确定哪些分支对于其他仓库是可见的，而哪些只是本地的分支

## 解决合并冲突

如果两个不同的开发人员对同一个文件进行了修改，那么合并冲突就会发生。而 Git 没有智能到自动解决合并两个修改。

在这一节中，我们会首先制造一个合并冲突，然后解决它，并应用到 Git 仓库中。

下面会产生一个合并冲突

~~~bash
# Switch to the first directory
$ cd ~/repo01

# Make changes
$ touch mergeconflict.txt
$ echo "Change in the first repository" > mergeconflict.txt

# Stage and commit
$ git add . && git commit -a -m "Will create merge conflict 1"

# Switch to the second directory
$ cd ~/repo02
# Make changes
$ touch mergeconflict.txt
$ echo "Change in the second repository" > mergeconflict.txt
# Stage and commit
$ git add . && git commit -a -m "Will create merge conflict 2"
# Push to the master repository
$ git push

# Now try to push from the first directory
# Switch to the first directory
$ cd ~/repo01
# Try to push --> you will get an error message
$ git push
# Get the changes
$ git pull origin master
~~~

Git 将冲突放在收到影响的文件中，文件内容如下：

~~~
<<<<<<< HEAD
Change in the first repository
=======
Change in the second repository
>>>>>>> b29196692f5ebfd10d8a9ca1911c8b08127c85f8
~~~

上面部分是你的本地仓库，下面部分是远端仓库。现在编辑这个文件，然后 commit 更改。另外的，你可以使用 `git mergetool` 命令

~~~bash
# Either edit the file manually or use
$ git mergetool

# You will be prompted to select which merge tool you want to use
# For example on Ubuntu you can use the tool "meld"
# After  merging the changes manually, commit them
$ git commit -m "merged changes"
~~~

## 变基（Rebase）

### 在同一分支中应用 Rebase Commit

通过 rebase 命令可以合并多个 commit 为一个。这样用户 push 更改到远端仓库的时候就可以先修改 commit 历史

接下来我们将创建多个 commit，然后再将它们 rebase 成一个 commit

~~~bash
# Create a new file
$ touch rebase.txt

# Add it to git
$ git add . && git commit -m "rebase.txt added to index"

# Do some silly changes and commit
$ echo "content" >> rebase.txt
$ git add . && git commit -m "added content"
$ echo " more content" >> rebase.txt
$ git add . && git commit -m "added more content"
$ echo " more content" >> rebase.txt
$ git add . && git commit -m "added more content"
$ echo " more content" >> rebase.txt
$ git add . && git commit -m "added more content"
$ echo " more content" >> rebase.txt
$ git add . && git commit -m "added more content"
$ echo " more content" >> rebase.txt
$ git add . && git commit -m "added more content"

# Check the git log message
$ git log
~~~

我们合并最后的七个 commit。你可以通过如下的命令交互的完成

~~~bash
$ git rebase -i HEAD~7
~~~

这个命令会打开编辑器让你修改 commit 的信息或者 squash / fixup 最后一个信息。Squash 会合并 commit 信息而 fixup 会忽略 commit 信息（待理解）

### Rebasing 多个分支

你也可以对两个分支进行 rebase 操作。如下所述，merge 命令合并两个分支的更改。rebase 命令为一个分支的更改生成一个补丁，然后应用这个补丁到另一分支中

使用 merge 和 rebase，最后的源代码是一样的，但是使用 rebase 产生的 commit 历史更加的少，而且历史记录看上去更加的线性

~~~bash
# Create new branch 
$ git branch testing

# Checkout the branch
$ git checkout testing

# Make some changes
$ echo "This will be rebased to master" > test01

# Commit into testing branch
$ git commit -a -m "New feature in branch"

# Rebase the master
$ git rebase master
~~~

### Rebase 最佳实践

在 push 更改到其他的 Git 仓库之前，我们需要仔细检查本地分支的 commit 历史

在 Git 中，你可以使用本地的 commit。开发人员可以利用这个功能方便的回滚本地的开发历史。但是在 push 之前，需要观察你的本地分支历史，是否其中有些 commit 历史对其他用户来说是无关的

如果所有的 commit 历史都跟同一个功能有关，很多情况下，你需要 rebase 这些 commit 历史为一个 commit 历史。

交互性的 rebase 主要就是做重写 commit 历史的任务。这样做是安全的，因为 commit 还没有被 push 到其它的仓库。这意味着 commit 历史只有在被 push 之前被修改。

如果你修改然后 push 了一个已经在目标仓库中存在的 commit 历史，这看起来就像是你实现了一些别人已经实现的功能。

### 创建和应用补丁

一个补丁指的是一个包含对源代码进行修改的文本文件。你可以将这个文件发送给某人，然后他就可以应用这个补丁到他的本地仓库。

下面会创建一个分支，对这个分支所一些修改，然后创建一个补丁，并应用这个补丁到 master 分支

~~~bash
# Create a new branch
$ git branch mybranch

# Use this new branch
$ git checkout mybranch

# Make some changes
$ touch test05

# Change some content in an existing file
$ echo "New content for test01" >test01

# Commit this to the branch
$ git add .
$ git commit -a -m "First commit in the branch"

# Create a patch --> git format-patch master
$ git format-patch origin/master
# This created patch 0001-First-commit-in-the-branch.patch

# Switch to the master
$ git checkout master

# Apply the patch
$ git apply 0001-First-commit-in-the-branch.patch

# Do your normal commit in the master 
$ git add .
$ git commit -a -m "Applied patch"

# Delete the patch 
$ rm 0001-First-commit-in-the-branch.patch
~~~

## 定义同名命令

Git 允许你设定你自己的 Git 命令。你可以给你自己常用的命令起一个缩写命令，或者合并几条命令道一个命令上来。

下面的例子中，定义了 `git add-commit` 命令，这个命令合并了 `git add . -A` 和 `git commit -m` 命令。定义这个命令后，就可以使用 `git add-commit -m "message"` 了。

~~~bash
$ git config --global alias.add-commit '!git add . -A && git commit'
~~~

但是非常不幸，截止写这篇文章之前，定义同名命令在 msysGit 中还没有支持。同名命令不能以！开始。

## 放弃跟踪文件

有时候，你不希望某些文件或者文件夹被包含在 Git 仓库中。但是如果你把它们加到 `.gitignore` 文件中以后，Git 会停止跟踪这个文件。但是它不会将这个文件从仓库中删除。这导致了文件或者文件夹的最后一个版本还是存在于仓库中。为了取消跟踪这些文件或者文件夹，你可以使用如下的命令：

~~~bash
# Remove directory .metadata from git repo
$ git rm -r --cached .metadata

# Remove file test.txt from repo
$ git rm --cached test.txt
~~~

这样做不会将这些文件从 commit 历史中去掉。如果你想将这些文件从 commit 历史中去掉，可以参考 `git filter-branch` 命令。

## 其他有用的命令

下面列出了在日常工作中非常有用的Git命令

~~~bash
# 谁创建了或者是修改了这个文件
$ git blame filename
# 以上上个commit信息为起点，创建一条新的分支
$ git checkout -b mybranch master~1
~~~