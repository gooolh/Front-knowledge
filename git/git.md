## git (分布式版本管理)

```
svn vs git 
svn 是集中式版本管理， 由于是集中式的，所有人都会连接这个版本库，优点：一定程度的可以看到项目的其他人在干什么，缺点：如果中央节点出现故障的话 会导致谁也没法提交和更新
git 是分布式版本管理，由于是分布式的，每个人都有自己的一个版本库，
```

### 工作区（Working Directory）

```
我们所工作的目录，叫做工作区 ，它可以直观看到的目录
```

### 版本库（Repository）

```
工作区有个隐藏目录.`.git` 是git的版本库，版本库存有许多东西，其中一个叫做暂存区，还有git创建的一个默认分区master,以及指向HEAD的指针
```



### 暂存区（stage）

```
作用：分阶段提交，如果修改了某个文件了，只提交到暂存区，相当于对这个文件做了一次快照，然后又对这个文件做了修改，git commit 上去只是第一次add的文件，分批次提交，降低了commit的颗粒度，方便版本回退，参考文献（https://www.zhihu.com/question/19946553）
git add 命令是把文件的修改提交到暂存区
git commit 命令是把暂存区的文件提交到当前分支
```



### 分支管理

```
git默认情况下会创建一个默认分支master,HEAD指针指向master分支最新提交的内容，
git branch dev  //创建dev分支 指向当前master的相同提交。-b参数代表创建并切换到dev，这是HEAD 指向dev分支
git checkout dev //切换dev分支
git branch //查看当前分支
git merge dev //将dev分区与master合并

使用事项

如果开发到一半 想checkout 到master主分区修复bug，但又不想commit 
git stash //将修改的内容保存至堆栈区 
git stash pop //需要时将保存在堆栈区的pop出
git stash apply //指定pop内容
git stash drop //删除指定内容
git stash clear //清除堆栈区的内容
```

### 版本回退

```
如果尚未commit 只是add到暂存区 回退到未添加到暂存区的版本
git reset <file>
如果已经commit 尚未git push到远程仓库
git reset head^ //放弃上一次提交，回到上一次修改的情况
如果已经push上去了，使用reset 虽然会回退版本，并且要使用--force 才能提交上去，但是head指向之前的版本,别人就无法从服务器更新代码下来
git revert -n 版本号 //会创建一个新的版本，版本的内容和回退的版本一样，只不过head指向新版本，只有其他人就可以pull代码下来了

回到历史的版本
git log --oneline //查看版本号 --oneline参数方便显示
git reset --hard 版本号 //回退到指定版本 不写版本号默认上一次commit
git reset --sort //保留工作区和暂存区的修改
git reset --mixed //只保留工作区修改
这时如果回退后 不满意想回到未来
使用git log 是看不到未来的版本号的 要使用
git reflog //可以查看所有分支的所有操作记录
```

