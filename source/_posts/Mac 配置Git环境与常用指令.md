title: Mac 配置Git环境与常用指令

date: 2018-09-12

------

#### 一、Git安装

- 方法一：下载安装包[Git下载地址](https://link.jianshu.com/?t=https://git-scm.com/download)。
- 方法二：安装[Homebrew](https://link.jianshu.com/?t=https://brew.sh)，然后通过指令安装`brew install git`。

#### 二、生成密钥

    Git关联远端仓库时候需要提供公钥，本地保存私钥，每次与远端仓库交互时候，远端仓库会用公钥来验证交互者身份。
    生成密钥`ssh-keygen -t rsa -C "email address"`，根据提示需要选择密钥存放路径。
    生成密钥后，在路径下生成两个文件`id_rsa`、`id_rsa.pub`，其中`id_rsa`文件保存的是私钥，放在本地，`id_rsa.pub`文件是公钥，需要将公钥内容上传到远端仓库，Mac 下直接用文本编辑打开公钥文件。

#### 三、配置提交文件时的用户信息

```
git config --global user.name "name"
git config --global user.email "email address"
```

配置信息也可以修改，指令与上面的指令相同。使用`git config --list`查看Git的配置信息。

#### 四、本地关联远端仓库

1. 打开本地文件夹，执行`git init`命令，初始化文件夹作为本地的一个仓库。
2. 将远端文件 clone 到本地目录，`git clone 远端文件URL`。

#### 五、常用指令



![img](//pic.oldzhg.com/uPic/QOk2Kt.png)



- **名词解释：**
  Workspace：工作区
  Index / Stage：暂存区
  Repository：仓库区（或本地仓库）
  Remote：远程仓库
- **生成密钥**
  `ssh-keygen -t rsa -C "email address"`
- **配置**

```
// 显示当前Git 配置
git config --list 

// 编辑Git配置文件
git config -e --global
 
// 配置提交文件时的用户信息
git config --global user.name "name"
git config --global user.email "email address"
```

- **新建一个仓库**

```
// 在当前目录新建一个 Git 仓库
git init 

// 下载项目
git clone 远端文件URL
```

- **添加/删除文件**

```
// 添加指定文件到暂存区
git add filename1 filename2 ..... 

// 添加指定目录及其子目录到暂存区
git add dir

// 删除工作区文件，并且将这次删除放入暂存区
git rm filename1 filename2 .....

// 停止追踪指定文件，但该文件会保留在工作区
git rm --cached filename
```

- **代码提交**

```
// 提交暂存区到仓库区
git commit -m "message"

// 提交暂存区的指定文件到仓库区
git commit  filename1 filename2 .....  -m "message"

// 提交工作区自上次commit之后的变化，直接到仓库区
git commit -a

// 提交时显示所有diff信息
git commit -v

// 使用一次新的commit，替代上一次提交
// 如果代码没有任何新变化，则用来改写上一次commit的提交信息
git commit --amend -m "message"

// 重做上一次commit，并包括指定文件的新变化
git commit --amend filename1 filename2 ..... 
```

- **分支**

```
// 列出所有本地分支
git branch

// 列出所有远程分支
git branch -r

// 列出所有本地分支和远程分支
git branch -a

// 新建一个分支，但依然停留在当前分支
git branch branchname

// 新建一个分支，并切换到该分支
git checkout -b branchname

// 新建一个分支，指向指定commit
git branch branchname commitname

// 新建一个分支，与指定的远程分支建立追踪关系
git branch --track branchname remotebranch

// 切换到指定分支，并更新工作区
git checkout branchname

// 切换到上一个分支
git checkout -

// 建立追踪关系，在现有分支与指定的远程分支之间
git branch --set-upstream branchname remotebranch

// 合并指定分支到当前分支
git merge branchname

// 选择一个commit，合并进当前分支
git cherry-pick commitname

// 删除分支
git branch -D branchname

// 删除远程分支
git push origin --delete branchname
git branch -dr [remote/branch]
```

- **查看 log**

```
// 显示有变更的文件
git status

// 显示当前分支的版本历史
git log

// 显示commit历史，以及每次commit发生变更的文件
git log --stat

// 搜索提交历史，根据关键词
git log -S keyword

// 显示某个文件的版本历史，包括文件改名
git log --follow filename
git whatchanged filename

// 显示指定文件相关的每一次diff
git log -p filename

// 显示过去5次提交
git log -5 --pretty --oneline

// 显示所有提交过的用户，按提交次数排序
git shortlog -sn

// 显示指定文件是什么人在什么时间修改过
git blame filename

// 显示暂存区和工作区的差异
git diff

// 显示暂存区和上一个commit的差异
git diff --cached filename

// 显示工作区与当前分支最新commit之间的差异
git diff HEAD

// 显示两次提交之间的差异
git diff [first-branch]...[second-branch]

// 显示今天你写了多少行代码
git diff --shortstat "@{0 day ago}"

// 显示某次提交的元数据和内容变化
git show [commit]

// 显示某次提交发生变化的文件
git show --name-only [commit]

// 显示某次提交时，某个文件的内容
git show [commit]: filename

// 显示当前分支的最近几次提交
git reflog
```

- **远程同步**

```
// 下载远程仓库的所有变动
git fetch [remote]

// 显示所有远程仓库
git remote -v

// 显示某个远程仓库的信息
git remote show [remote]

// 增加一个新的远程仓库，并命名
git remote add [shortname] [url]

// 取回远程仓库的变化，并与本地分支合并
git pull [remote] branchname

// 上传本地指定分支到远程仓库
git push [remote] branchname

// 强行推送当前分支到远程仓库，即使有冲突
git push [remote] --force

// 推送所有分支到远程仓库
git push [remote] --all
```

- **撤销**

```
// 恢复暂存区的指定文件到工作区
git checkout filename

// 恢复某个commit的指定文件到暂存区和工作区
git checkout [commit] filename

// 恢复暂存区的所有文件到工作区
git checkout .

// 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
git reset filename

// 重置暂存区与工作区，与上一次commit保持一致
git reset --hard

// 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
git reset [commit]

// 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
git reset --hard [commit]

// 重置当前HEAD为指定commit，但保持暂存区和工作区不变
git reset --keep [commit]

// 新建一个commit，用来撤销指定commit
// 后者的所有变化都将被前者抵消，并且应用到当前分支
git revert [commit]

// 暂时将未提交的变化移除，稍后再移入
git stash
git stash pop
```

#### 六、遇到的问题

1. 拒绝访问

```
Permission denied (publickey).
fatal: Could not read from remote repository.
Please make sure you have the correct access rights
and the repository exists.
```

解决方法：`ssh-add 私钥路径`，其中`ssh-add`命令是把专用密钥添加到ssh-agent的高速缓存中。

1. 执行 `git branch -a`看不到新创建的分支
   原因：这条命令并没有每一次都从远程更新仓库信息。
   解决方法：手动刷新仓库信息`git fetch origin`。

## 七、参考链接

- [Git官方中文手册](https://link.jianshu.com/?t=https://git-scm.com/book/zh/v2)
- [阮一峰的网络日志 - 常用 Git 命令清单](https://link.jianshu.com/?t=http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)


