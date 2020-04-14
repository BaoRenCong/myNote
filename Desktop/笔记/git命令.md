## git命令

#### 获取git仓库

###### 新建仓库

1. 新建目录后初始化 git仓库， `git init`；
2. 追踪文件并进行初始提交：
	> `git add *`  提交文件到暂存区；
	> `git commit -m ''` 将暂存区的内容提交到本地仓库；

###### 克隆现有仓库

`git clone 远程仓库url`	在url后添加文件夹名以修改clone下的仓库名；

#### 文件状态

###### 查看文件状态

> `git status`
> 查看当前哪些文件处于什么状态，还会显示当前所在分支；

> `git status -s` 或 `git status --short`
> 精简输出内容
> 精简输出中： ?? 指新添加的未跟踪的文件、A 新添加到暂存区中的文件、M 已修改的文件；
> 精简内容输出中有两列，左侧指明暂存区状态，右侧指明工作区状态；

###### 忽略文件

新建 .gitignore 文件 ，规则类似正则表达式；

```
# 忽略所有的 .a 文件
*.a

# 但跟踪所有的 lib.a，即便你在前面忽略了 .a 文件
!lib.a

# 只忽略当前目录下的 TODO 文件，而不忽略 subdir/TODO
/TODO

# 忽略任何目录下名为 build 的文件夹
build/

# 忽略 doc/notes.txt，但不忽略 doc/server/arch.txt
doc/*.txt

# 忽略 doc/ 目录及其所有子目录下的 .pdf 文件
doc/**/*.pdf
```

###### 查看已暂存或为暂存的修改

- `git diff --staged` || `git diff --cached` 查看已经暂存文件中修改的内容；
- `git diff` 查看未暂存的文件中修改的内容；

###### 提交更新 - 本地仓库

`git commit -m '提交描述'` 

`git commit -a -m '提交描述'` 不需要使用 `git add` 命令将文件提交到暂存区

######  移除文件

1. 删除文件：
	在工作目录下删除文件
	`git rm [文件名]` 记录移除文件的操作

2. 使文件仅在 git 仓库中删除：
	`git rm --cached [文件名]`

`gti rm` 命令后的文件名可以使用 glob 模式匹配

###### 移动文件

`git vm [原文件名] [新文件名]`，该命令等价于：

```
mv README.md README	//修改文件名
git rm README.md	//在git库记录删除旧文件的操作
git add README		//添加新文件
```

#### 查看文件提交历史

1. ` git log `  这个命令会列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明；
```
$ git log
commit ca82a6dff817ec66f44342007202690a93763949	//校验和
Author: Scott Chacon <schacon@gee-mail.com>		//作者名、邮箱
Date:   Mon Mar 17 21:52:11 2008 -0700			//提交时间

    changed the version number					//提交说明

```
2. ` git log [-p || -patch] -num ` 显示每次提交的差异，-num 可以限制显示的数目；
3. ` git log --stat ` 在每次提交的下面列出所有被修改过的文件、有多少文件被修改了以及被修改过的文件的哪些行被移除或是添加了，在每次提交的最后还有一个总结；
4. `git log --pretty=oneline` 将提交信息放在一行显示，只显示校验和、提交说明；
5. `git log --pretty=short` 只显示校验和、作者邮箱、提交说明；
6. `git log --pretty=format:"%h - %an, %ar : %s"` 自定义信息的显示格式；
|选项|说明|
|:---:|:---:|
|%H|提交的完整哈希值|
|%h|提交的简写哈希值|
|%an||作者名字|
|%ae|作者的电子邮件地址|
|%ad|作者修订日期（可以用 --date=选项 来定制格式）|
|%ar|作者修订日期，按多久以前的方式显示|

7. `git log --[since || after]=2.weeks //之后` `git log --[until || before]=2.weeks //之前`，该命令可用的格式十分丰富——可以是类似 "2008-01-15" 的具体的某一天，也可以是类似 "2 years 1 day 3 minutes ago" 的相对日期；
8. `git log --author=name` 指定作者名；
9. `git log -S='txt' ` 指定显示有该内容的添加、修改；

#### 撤销操作

- `git commit --amend` 在提交命令后添加--amend，重新提交暂存区中的内容，第二次提交的内容会代替第一次提交的结果，第一次的提交信息会消失；
- `git reset HEAD CONTRIBUTING.md` 将暂存区中的CONTRIBUTING.md从暂存区删除；
- `git checkout -- CONTRIBUTING.md` ，用git仓库中最后一次提交的版本内容替换掉现有的CONTRIBUTING.md文件中的内容；

#### 远程仓库

###### 添加远程仓库

`git remote add <shortname> <url>`，shortname--自定义仓库名、url--远程仓库地址

###### 从远程仓库中拉取

`git fetch <remote>`，remote--自定义的仓库名，该命令会访问远程仓库，从中拉去所有本地没有的数据，但是并不会修改工作目录中的内容；

###### 推送到本地仓库

`git push origin master`，origin--本地仓库名，master--要推送到的远程分支名，只有当你有所克隆服务器的写入权限，并且之前没有人推送过时，这条命令才能生效；

###### 查看某个远程仓库

`git remote show origin`，origin--本地仓库名，指代远程仓库链接；

###### 远程仓库的重命名与移除

- 重命名 `git remote rename origin target`，将远程仓库 origin 重命名为 target；
- 移除远程仓库 `git remote [remove || rm] `；

#### 打标签


#### 别名

` git config --global alias.[自定义命令名] [命令名] `，Git 只是简单地将别名替换为对应的命令，例如：

```
$ git config --global alias.last 'log -1 HEAD'

$ git last
commit 66938dae3329c7aebe598c2246a8e6af90d04646
Author: Josh Goebel <dreamer3@example.com>
Date:   Tue Aug 26 19:48:51 2008 +0800

    test for current head

    Signed-off-by: Scott Chacon <schacon@example.com>
```

#### 分支

###### branch

`git branch`，查看所有分支，可以看到当前处于哪一分支；
`git branch -v`，查看所有分支以及分支的最后一次提交信息；
`git branch [分支名]`，创建新分支；
`git branch -d [Brc]`，删除分子Brc；

###### checkout

`git checkout [分支名]`，切换分支，会将工作目录整个切换为要切换分支的内容；
`git checkout -b [分支名]`，新建分支并切换；

###### merge

`git merge [Brc]`，将分支Brc 合并到当前所在的分支；

###### 远程分支

`git ls-remote [remote]`，查看远程仓库的分支；
`git push origin --delete [远程分支名]`，删除远程分支；

###### 快进（fast-forward）

当你试图合并两个分支时， 如果顺着一个分支走下去能够到达另一个分支，那么 Git 在合并两者的时候， 只会简单的将指针向前推进（指针右移），因为这种情况下的合并操作没有需要解决的分歧；

###### 合并提交

由于要合并的分支并不在同一条提交历史上，无法直接向前推进；所以git将此次三方合并的结果做了一个新的快照并且自动创建一个新的提交指向它；

###### 合并冲突

如果在两个不同的分支中，对同一个文件的同一个部分进行了不同的修改，Git 就没法干净的合并它们；

###### 变基（rebase）

你可以提取在 C4 中引入的补丁和修改，然后在 C3 的基础上应用一次；

![rebase示例](img\rebase.png)

它的原理是首先找到这两个分支（即当前分支 experiment、变基操作的目标基底分支 master） 的最近共同祖先 C2，然后对比当前分支相对于该祖先的历次提交，提取相应的修改并存为临时文件， 然后将当前分支指向目标基底 C3, 最后以此将之前另存为临时文件的修改依序应用；

