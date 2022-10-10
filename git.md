## 开发过程中遇到的相关的git问题

### 1 多人开发如何管理远程库及merge代码

a 多人开发时fork自己git，将原git添加到upstream
```
git remote add <shortname> <url> 添加一个新的远程 Git 仓库

git remote add upstream http://**
git remote -v  (查看仓库)

```
b fetch远程代码并merge
```
git fetch upstream

git merge upstream/test
```
c 解决本地一些冲突等在提交代码到远处

d 若想删除（或者add远程库错误了）需要先删除远程库

```
git remote rm origin
git remote rm upstream
```

### 2 已经push了的版本想要将其删除

1 git log 查看，找到要删除内容的前一个节点id
2 git reset --hard id
3 查看本地结果，发现已没有中间提交过的内容
对于已经push到远程了的需要执行下面的，若只是commit了 执行到上一步就可以了
4 git push origin test --force


### 3 简单的操作创建和删除分支

1 git checkout -a 查看全部分支
2 git checkout -b newName 切换到newName分支，没有该分支的话就创建 (无-b直接切分支)
3 git branch -d name 删除本地指定分支
4 git push remoteName --delete remoteBranchName 删除远程分支


