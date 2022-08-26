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

### 2
