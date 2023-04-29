Git分布式版本控制系统，从主仓库拉取代码的主机就是服务器，保存了仓库的历史记录，可以分享给其他人。集中式版本控制(svn,cvs),本机只保存一份当前信息，回滚等操作需要服务器支持,git不需要。
Git基本组成框架：
	Workspace：开发者工作区:当前写代码的目录
	Index/Stage:缓存区/暂存区:缓存区指的是git add中添加的文件，commit提交的就是缓存区的内容。暂存区是git stash保存的工作状态。
	Repository:仓库区（本地仓库）
	Remote:远程仓库
	

### 初始化
git clone ***  克隆一个项目
git init  初始化项目
### 提交
git add   *** 添加到缓存区中
git stash  保存当前工作状态
git stash pop 恢复保存的工作状态内容到本地
git stash list 查看保存的工作状态
git diff ** 比较命令
git status  工作区改动的内容
git commit -m  “信息” 将缓存区中的内容提交到本地仓库
git commit --amend 修改最近提交的内容描述
### 提交历史
git log   分支的提交历史
git log ***  文件的提交历史 
### 分支
git branch -av  所有现有的分支及最近提交记录
git checkout -b 生成并切换分支
git checkout 切换分支
git branch  本地所有分支
git branch -d/D 分支名  删除分支名
git tag 标签名 标签标记当前提交
git tag -l  本地所有标签

git remote add 仓库地址 添加远程存储库地址
git remote -v (fetch拉取仓库，push提交仓库)
git fetch -p  拉取远端分支到本地
git pull origin 远端分支  拉取代码
git push origin 远端分支  提交代码
### 合并 
git merge 分支名：合并分支名的代码到本地，但有合并记录，不同的commit记录在不同的分支上。
git rebase 分支名 :合并分支名的代码到本地，但没有合并记录，会记录在本分支主干上
git cherry-pick commitID :合并指定commitid的代码到本地,commitID可以是多个
### 撤销
git reset --hard commitId  回退到指定commit号
git reset --hard HEAD(~n) 回退当前版本的n次
git submodule add  url 添加一个子模块

--no-ff：不使用fast-forward方式合并，保留分支的commit历史
--squash：使用squash方式合并，把多次分支commit历史压缩为一次