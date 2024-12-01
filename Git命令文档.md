# Git命令文档

## 1. Git基础命令

### git init：初始化一个Git仓库

### git add：添加文件到暂存区

1. git add .：添加所有文件到暂存区。

### git commit：提交文件到本地仓库

1. git commit -m "commit message"：提交所有文件并添加注释

2. git commit -amend：修改上次提交

### 4. git status：查看仓库状态

### 5. git log：查看提交历史

### git diff：查看文件差异

### git help：查看帮助

### git config：配置Git

### git pull：拉取远程分支到本地仓库

1. git pull
拉去远端仓库所以内容包括分支
是git fetch 和git merge的组合操作
2. git pull --rebase
是git fetch 和 git rebase 远端分支 的组合操作

### git clone：克隆远程仓库

### git fetch：拉取远程仓库

拉取远端仓库的所有内容包括分支
只是下载了所有差异到本地 ，并不会修改任何本地仓库，本地仓库的HEAD指针依然指向git fetch操作前的提交记录

1. git fetch origin <source>:<destination>
   将指定的远端提交记录 拉取到 本地一个分支上
   当source没有时，会在本地创建一个 destination分支

### git push：推送本地分支到远程仓库

推送本地数据到远端 不带参数的 git push命令会默认走<push.default>配置文件的相关设定

1. git push origin <source>:<destination>
   将指定的本地提交记录 推送到 远端的一个 分支上
   当source为空时，会去远端删除destination分支 （特别注意）

### reset：回退版本

用于回退本地版本

### revert：撤销提交

### git bisect：二分查找

## 2. Git分支操作

### git branch：查看分支

1. git branch branch_name：创建分支。
git branch <name>  <ref> 在指定位置创建分支

2. git branch -r: 查看远程分支

3. git branch -a: 查看所有分支

4. git branch -f branch_name hash_code：强制创建分支。

### git checkout：切换分支

1. git checkout branch_name : 切换分支
2. git checkout -b branch_name： 创建分支并切换到分支
3. git checkout hash_code：讲HEAD指针分离到hash_code提交
4. git checkout HEAD^：将头指针指向上一个提交
当一个节点有两个parent节点时，^可以在两个节点中选择其一
^~可以连续调用，例如：HEAD~^2~
5. git checkout HEAD~2：将头指针指向上2个提交

### git merge：合并分支

1. git merge branch_name ： 将branch_name分支合到当前分支

### git rebase : 变基

1. git rebase branch_name:将当前分支合并到branch_name分支
2. git rebase -i hash_code:交互式变基，新启一个分支，将HEAD到hash_code的所有提交重新提交到新的分支
3. git rebase branch1 branch2：将branch2 变基到 branch1

### git cherry-pick：部分合并

1. git cherrt-pick c2 c3： 将hash为c2 ,c3的两个提交合并到当前分支（注：c2,c3不可为当前分支的上行）

### git remote：管理远程仓库

## 3. Git 标签

 标签的作用：永久地将某个特定的提交命名为里程碑，可以像分支一样引用

### git tag： 标签

 1. git tag v1 hash_code：将hash_code所在提交定为v1标签
 2. git describe <ref>：输出距离<hash>分支最近的tag
 输出结果：<tag>_<numCommits>_g<hash>： tag 表示的是离 ref 最近的标签， numCommits 是表示这个 ref 与 tag 相差有多少个提交记  录， hash 表示的是你所给定的 ref 所表示的提交记录哈希值的前几位

## 4. Git高级命令

###

###

### 锁定的Main：Locked Main

策略选择 锁定main时 无法直接push当前修改到远端
需要新建一个分支feature, 推送到远程服务器. 然后reset你的main分支和远程服务器保持一致
使用push requset命令推送

### 分支跟踪

1. git checkout -b <branch-name>  <origin-name>
   使用 checkout 命令创建一个新的分支并跟踪一个远端分支
2. git branch -u <origin-name> <branch-name>
   使用branch -u命令 跟踪一个远端分支

跟踪分支之后，可以在新的分支中提交和拉取远端分支

<!-- stash：暂时保存工作区的修改。 -->

<!-- 6. git bisect：二分查找。

8. git blame：查看文件历史记录。
9. git grep：搜索代码。
10. git archive：创建打包文件。
11. git gc：压缩仓库。
12. git fsck：检查仓库。
13. git filter-branch：过滤提交。
14. git instaweb：在浏览器中查看仓库。
15. git cvs：导入CVS仓库。
16. git svn：导入SVN仓库。
17. git fast-import：导入Git仓库。
18. git gc --aggressive：压缩仓库。
19. git gc --auto：压缩仓库。
20. git gc --prune：压缩仓库。
21. git gc --quiet：压缩仓库。
22. git gc --aggressive --prune：压缩仓库。
23. git gc --auto --prune：压缩仓库。
24. git gc --quiet --prune：压缩仓库。 -->

## Git 多分支开发 合并操作流程

  当本地存在多个分支需要提交到远端的时候

1. 使用rebase

- git pull --rebase
- git rebase main side1
- git rebase side1 side2
- git rebase side2 side3
- git push

2. 使用merge

- git pull
- git merge side1
- git merge side2
- git merge side3
- git push
