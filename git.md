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



### 4 .gitignore不起作用，可能是因为缓存造成的
```
删除指定文件的缓存
git rm -r --cached src/main/resources/application-local.yml

修改.gitignore文件中，将application-local.yml文件ignore掉
在.gitignore文件中添加:
src/main/resources/application-local.yml

提交.gitignore文件的修改
 git add .gitignore
 git commit -m "清除git缓存，解决gitignore问题"
 git push

// 上面是针对某个文件的内容，可以直接将所有cached清空
git rm -r --cached .
git add .
git commit -m 'update .gitignore'

```


### 5 git 删除分支
 ```
 删除本地分支
 git branch -a
 git branch -D amatookeptx
 
 删除远程分支
 ？
 ```
 
### 6 git 摘取某个分支的commit到指定分支

在测试分支提交了n多，想要把其中的一个commit 合并到master
```
1 现在test 分支 git log 查看指定分支的commitid copy
2 git checkout master
3 git cherry-pick commitid
即可
```

### 7 merge
```
git pull origin 
git merge origin/test

git pull upstream 
git merge upstream/master
在哪个分支上面执行就是合并到当前分支
```

### 8 git add了错误文件如何撤销
```
git status // 先看一下add 中的文件 
git reset HEAD // 如果后面什么都不跟的话，就是上一次 add 里面的内容全部撤销
git reset HEAD XXX/XXX/XXX.java // 就是对某个文件进行撤销
```
撤销完成之后，会变成add之前的样子，代码修改还存在

### 9 git开发分支错了，如何切换，将修改的内容保存；
```
比如正常在test分支开发，结果开发中需要在master分支解决一个bug上线，结果就忘记切换了，下次打开直接就进行了开发，开发完后提交发现不应该在master分支；如果将内容移动到test分支上；
未add的内容直接：
git stash save
git checkout test
git stash pop

已经add的内容：（可以执行8之后在执行9，未实践）
????

```

### 10 git 开发了好几个版本已经add commit push到远程了，想要删掉最近几次的commit
```
git log //查看要回退到的制定的commitid
git reset --soft commitid       ||        git reset --hard commitid (近几次提交的变化直接被清空)

git log  // 查看不到进几次的提交
git status   // 近几次提交的变化任然存在 如下所示：
```
![image](https://user-images.githubusercontent.com/31762176/206104404-c6d431ae-5d61-4e20-9d20-3a7e376455bd.png)

```
git restore --staged .   // 从暂存区删除，变为如下样子：

```
![image](https://user-images.githubusercontent.com/31762176/206104862-d926152f-4017-4ab7-a264-51cfd02e4d16.png)

```
git restore .   // 丢弃工作区的改动
git status
位于分支 test
您的分支落后 'origin/test' 共 4 个提交，并且可以快进。
  （使用 "git pull" 来更新您的本地分支）

无文件要提交，干净的工作区
```
以上只是将本地代码恢复到了想要的状态，但是不能直接 push到远程，只是回提示 本地落后远程几个分支；
```
git push origin test 会报错如下：
```
![image](https://user-images.githubusercontent.com/31762176/206106276-4ae21fcc-0386-4e68-a492-73369b483c51.png)

需要使用：
```
git push origin test --force  //强制提交当前版本号。
```
至此，撤销push提交完成

[参考文献](https://blog.csdn.net/qq_45503196/article/details/126089133)


### 11 git 更换文件名大小写无法提交问题

```
1.使用 git config core.ignorecase命令查看当前git是否忽略了大小写
git config core.ignorecase
如果显示为true，则忽略大小写

2.使用git config core.ignorecase false命令,  设置为false

git config core.ignorecase false

3.再次提交文件时就能够将更换了大小写名称的文件提交上去了
```
