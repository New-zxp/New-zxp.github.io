---
title: Git笔记
date: 2021-06-10
tags: Git
categories: 笔记
description: Git笔记
cover: https://zhuxinpeng.oss-cn-hangzhou.aliyuncs.com/11.jpg
---

遇到密码验证时在设置里生成个人访问令牌。

更换主题：git clone -b master + http + themes/名字
①hexo clean
②hexo g
③hexo d
更改后记得清除缓存

ssh -T git@github.com时要输入密码直接输入：
git config --global http.proxy http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890



Failed to connect to 127.0.0.1 port:

$ git config --global --unset http.proxy



！！！切换分支之前先提交本地修改

Git Gui：图形界面工具
Git Bash：命令行工具

touch 文件名称.文件类型   创建文件

git config --global user.name "XXX" # 设置用户名
git config --global user.email "XXXX" # 设置邮箱

为操作取别名：
①创建.bashrc文件：touch ~/.bashrc
②在创建的文件中输入：

用于输出git提交日志

alias git-log='git log --pretty=oneline --all --graph --abbrev-commit'
#用于输出当前目录所有文件及基本信息
alias ll='ls -al'

解决GitBsh乱码问题：
①在GItBash执行：git config --global core.quotepath flase
②在安装目录/etc/bash.bashrc文件加入：
export LANG="zh_CN.UTF-8"
export LC_ALL=zh_CN.UTF-8"

获取本地仓库：在一个目录中打开Git bash，执行：git init

工作区（workspace）→ 暂存区（index）→ 仓库（repository）

新创建的文件处于”未跟踪状态“通过git add转为”已暂存“状态再通过git commit提交进仓库，增加一条提交记录
修改已有文件文件处于”未暂存状态“通过git add转为”已暂存“状态再通过git commit提交进仓库，增加一条提交记录

git status：查看文件状态
git add 文件名：把文件添加到暂存区
git add .  ：把所有文件添加到暂存区
git commit -m "标记信息"：添加到仓库（有空格）
vi 文件名：进入vi编辑器
vi退出编辑时，按esc，输入冒号（英文），然后切换到最后一行模式，最后一行模式决定是否保存文件。例如输入wq保存并退出。
ll：输出当前目录所有文件及基本信息
ls -al：输出当前目录所有文件及基本信息
git log --all：显示所有分支
git log --pretty=online：将提交信息显示为一行
git log --abbrev-commit：使得输出的commited更简短
git log --graph：以图的形式显示
git log --pretty=oneline --all --graph --abbrev-commit：叠加使用
注意：上边已经起了别名git-log='git log --pretty=oneline --all --graph --abbrev-commit'
git reset --hard commitID：版本切换 commitID可以用git-log或git log查看
选中默认就是赋值！！！然后鼠标单机滚轮就是粘贴
git reflog：获取操作记录

添加文件至忽略列表(添加后记得刷新提交目录)：
在工作目录创建.gitignore文件，列出要忽略的文件模式，例如：file.txt（文件名）或通配符 *.a（a类型的文件）
创建好后需要添加并提交

分支：几乎所有的版本控制系统都以某种形式支持分支。 使用分支意味着你可以把你的工作从开发主线上分离开来进行重大的Bug修改、开发新的功能，以免影响开发主线。

HEAD指向的是当前分支
git branch：查看本地分支
git branch 分支名：创建本地分支
git checkout 分支名：切换分支
有多个分支时，只能对一个分支进行修改，称为当前分支
git checkout -b 分支名：直接切换到一个不存在的分支（创建并切换）
git merge 分支名：一个分支上的提交可以合并到另一个分支（需要添加并提交）
删除分支：不能删除当前分支，只能删除其他分支
git branch -d 分支名：删除分支时，需要做各种检查
git branch -D 分支名：不做任何检查，强制删除

在开发中分支使用原则与流程：
master（生产）分支：线上分支，主分支，中小规模项目作为线上运行的应用对应的分支；
develop（开发）分支：是从master创建的分支，一般作为开发部门的主要开发分支，如果没有其他并行开发不同期上线要求，都可以在此版本进行开发，阶段开发完成后，需要是合并到master分支，准备上线。
feature/xxxx分支：从develop创建的分支，一般是同期并行开发，但不同期上线时创建的分支，分支上的研发任务完成后合并到develop分支，之后该分支可以删除。
hotfix/xxxx分支：从master派生的分支，一般作为线上bug修复使用，修复完成后需要合并到master、test、develop分支。
master和develop是固定的，不要删除


ls -al ~/.ssh：查看现有公钥
Gitee使用
①在gitee创建仓库（下边的不要勾选）
②在本地仓库输入：
git config --global user.name "zhaobaogang"
git config --global user.email "11632818+zhaobaogang@user.noreply.gitee.com"
③生成SSH公钥：ssh-keygen -t rsa
④获取公钥：cat ~/.ssh/id_rsa.pub（一直回车）
⑤在gitee设置中输入公钥
⑥验证是否配置成功：ssh -T git@gitee.com
⑦选择ssh复制，在本地输入：git remote add 名字（一般为origin） + ssh
⑧输入git remote获取远程仓库查看是否添加成功
⑨把本地代码同步到远程仓库：git push	 origin master
1.完整代码为 git push [-f] [--set-upstream] [远端名称] [本地分支] [远端分支名]
2.如果远端分支名和本地分支名相同，可以只写本地分支
3.-f表示强制覆盖
4.--set-upstream推送到远端的同时并且建立起和远端分支的联系
5.如果本地已经和远端分支关联，则可以省略分支名和远端名

git branch -vv：获取本地分支与远端分支的关联关系

克隆
①复制要克隆仓库的SSH地址
②在文件夹中git clone + ssh地址 [仓库名称]      如果不设置仓库名称就默认和复制的仓库名一样

从远程仓库抓取和拉取：远程分支和本地分支一样，我们可以进行marge操作，只是需要把远程仓库的更新都下载到本地，在进行操作
抓取命令：git fetch [remote name] [branch name]
①抓取指令就是将仓库里的更新都抓取到本地，不会进行合并
②如果不指定远端名称和分支名，则抓取所有分支
拉取命令：git pull [remote name] [branch name]
①拉去指令就是将远端仓库的修改拉取到本地并自动进行合并，等同于fetch+marge
②如果不指定远端名称和分支名，则抓取并更细当前分支

IDEA中移除本项目git配置：
File --> Settings --> Version Control → Directory Mappings 并移除当前项目的git配置
Idea使用Git（失败时点击CommitAnyway）:
①在Idea中设置Git
②在gitee新建仓库并复制ssh
③在Idea中添加gitignore
④在idea创建Git仓库：Git → create Git Repository
⑤连接远程仓库

Idea克隆：Git → clone 输入克隆ssh

Idea创建分支：
①第一种方法：右下角 → New Branch
②第二种方法：在log中的修改记录上New Branch

Idea合并分支：右下角选择你要和当前分支合并的分支

Idea切换分支：右下角选择分支→checkout

蓝色箭头：git pull
绿色箭头：git push
绿色对勾：git commit
fetch：Git → fetch