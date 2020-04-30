# Git 笔记

## 安装

### Mac 安装 Git

`brew install git`

### Ubuntu 安装 Git

`apt install git`

### Centos 安装 Git

`dnf install git`

### 安装后设置名字和邮箱

`git config --global user.name "XXX"`  
`git config --global user.email "XXX@XXX"`

## 使用 Git

### 创建版本库(Repository)

首先进入要作为版本库的文件夹 然后创建版本库

`cd <dir>`

`git init`  
（创建版本库）

### 修改完的文件添加到暂存区

`git add <File>`  
（单独文件）

`git add .`  
（所有文件）

### 将暂存区里的文件提交到仓库

提交一定要写提交说明

`git commit -m <message>`

### 查看状态

`git status`

### 查看差异

`git diff <File>`

## 文件/版本管理

### 查看提交日志

`git log`

### 查看历史提交日志

`git reflog`

### 选择回退版本

HEAD 指的是当前分支

`git reset --hard <commit id>`

### 管理修改

`git checkout -- <file>`  
（丢弃工作区的修改 将文件退回到最近一次 git commit 或 git add 状态）

`git reset HEAD <file>`  
（撤销暂存区的修改 重新放回工作区）

### 删除文件

`rm <file>`  
（手动删除文件）

`git rm <file>`  
（从版本库中删除该文件）

`git commit -m <message>`  
（提交删除）

### 误删撤回

`git checkout -- <file>`  
（将版本库替换工作区的版本）

## 远程仓库

### 关联远程库

`git remote add origin <git address>`

### 推送内容

`git push -u origin master`  
（第一次推送加 -u 参数 以后就不用了）

`git push origin master`  
（推送内容）

### 克隆仓库

`git clone <git address>`

## 分支管理

### 创建与合并分支

Git 鼓励大量使用分支来完成某一任务 合并后再删除分支

`git switch -c <name>`  
（加上-c 参数表示创建并切换此分支）

> Git 新版本提供了新的命令替代老命令  
> `git checkout -b <name>`
> （加上-b 参数表示创建并切换此分支）
>
> > 相当于以下两条命令  
> > `git branch <name>` （创建此分支）  
> > `git checkout <name>` （切换到此分支）

`git branch`  
（查看分支 当前分支前会标\*号）

`git add <file>`  
（提交到暂存区）

`git commit -m <message>`  
（提交到仓库）

`git switch <name>`  
（切换到此分支）

> Git 新版本提供了新的命令替代老命令  
> `git checkout <name>`

`git merge <name>`  
（合并此分支到当前分支）

`git branch -d <name>`  
（删除此分支）

### 解决冲突

Git 无法自动合并分支时需要手动修改冲突内容 再提交

`git log --graph`  
（查看分支合并图）

### 查看历史分支

`git log --pretty=oneline --abbrev-commit`  
(查看历史提交的 commit id)

### 分支管理策略

通常合并分支时 使用 Fast forward 模式 但此模式删除分支后 会丢掉分支信息  
禁用 Fast forward 模式 就会在 merge 时生成一个新的 commit  
这样从分支历史上就可以看出来分支信息

> 演示操作  
> `git switch -c dev`  
> （创建并切换到 dev 分支）  
> `git add <file>`  
> （修改文件提交到暂存区）  
> `git commit -m <message>`  
> （提交修改到仓库）  
> `git switch master`  
> （切回到 master 分支）  
> `git merge --no-ff -m <message> dev`  
> （合并时使用 --no-ff 参数禁用 Fast forward 模式）  
> `git log`  
> （查看分支历史）

在实际开发中 master 分支应该是非常稳定的 仅用来发布新版本 平时不能在上面干活  
干活应该都在 dev 分支上 当发布时再把 dev 分支合并到 master 上 在 master 分支上发布  
多人开发时都在 dev 分支上干活 每个人都有自己到分支 然后合并 dev 分支就可以了

### Bug 分支

`git stash`  
（先将工作现场”存储“起来 等以后恢复现场继续工作）

`git stash list`  
（查看工作现场）

有两种恢复工作现场方法

> 第一种
>
> > `git stash apple`  
> > （恢复工作现场 但 stash 内容并不删除）  
> > `git stash drop`  
> > （删除 stash 里的工作现场）  
> > 第二种  
> > `git stash pop`  
> > （恢复工作现场 并删除 stash 里的内容）

`git stash apply stash@{0}`  
（恢复指定的 stash）

在 master 分支上修复的 bug 想要合并到当前 dev 分支  
可以用 `git cherry-pick <commit>` 命令  
把 bug 提交的修改“复制”到当前分支 避免重复劳动

### Feature 分支

如果要丢弃一个没有被合并过的分支  
可以通过 `git branch -D <name>` 强行删除

### Rebase

`git rebase`  
（把分叉的提交历史“整理”成一条直线）

### 标签管理

使用好记的 tag 名字 绑定某一个 commit 多用于提交

`git tag <tag name>`  
（打上标签）

`git tag <tag name> <commit id>`  
（给历史分支打上标签）

`git tag`  
（查看所有标签）

`git show <tag name>`  
（查看标签信息）

`git tag -a <tag name> -m <message> <commit id>`  
（创建带有说明的标签 用 -a 指定标签名 -m 指定说明文字）

`git tag -d <tag name>`  
（删除标签）

`git push origin <tag name>`  
（推送某个标签到远程库）

`git push origin --tags`  
（一次性推送全部尚未推送到远程库的本地标签）

`git tag -d <tag name>`  
`git push origin :refs/tags/<tag name>`  
（删除远程库的标签）

### 连接其他远程仓库

`git remote -v`  
（查看远程库信息）

`git remote rm origin`  
（删除已有远程库）

`git remote add origin <git address>`  
（关联到远程库）

`git remote add <git name> <git address>`  
（关联多个远程库）

`git push <git name> <branch name>`  
（推送到对应远程库的某分支）

## 自定义 Git

### Git 显示颜色

`git config --global color.ui true`

### 忽略特殊文件

在工作区的根目录创建 .gitignore 文件  
将要忽略的文件名填进去 Git 就会自动忽略这些文件  
Github 的配置文件：<https://github.com/github/gitignore>

忽略文件的原则是：  
忽略操作系统自动生成的文件 比如缩略图等  
忽略编译生成的中间文件 可执行文件等  
也就是如果一个文件是通过另一个文件自动生成的 那自动生成的文件就没必要放进版本库  
比如 Java 编译产生的.class 文件  
忽略你自己的带有敏感信息的配置文件 比如存放口令的配置文件

如果想添加一个被忽略的文件 添加-f 参数强制添加到 Git  
`git add -f <file>`

检查.gitignore 文件是否正确  
`git check-ignore -v <file>`

### 配置别名

`git config --global alias.st status`  
（将 status 改成 st）

`git config --global alias.co checkout`  
（将 checkout 改成 co）

`git config --global alias.ci commit`  
（将 commit 改成 ci）

`git config --global alias.br branch`  
（将 branch 改成 br）

--global 参数时全局参数

`git config --global alias.unstage 'reset HEAD'`  
（将 `git reset HEAD <file>` 改成 `git unstage <file>` ）

`git config --global alias.last 'log -1'`  
（配置一个 git last 让其显示最后一次提交信息）

`git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`  
（大神操作 将 `git lg` 配置成查看分支图）

### 配置文件

配置 Git 的时候 加上 --global 是针对当前用户起作用的  
如果不加 那只针对当前的仓库起作用

每个仓库的 Git 配置文件都放在 .git/config 文件中

别名就在[alias]后面，要删除别名 直接把对应的行删掉即可

当前用户的 Git 配置文件放在用户主目录下的一个隐藏文件 .gitconfig 中

### 搭建 Git 服务器

参考：<https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664>

## 参考资料

廖雪峰的 Git 教程
