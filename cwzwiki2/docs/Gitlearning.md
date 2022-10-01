[时光机穿梭 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/896043488029600/896954074659008) 阅读笔记

```
git add file_name 把工作区文件添加到暂存区
git commit -m "注释" 把暂存区文件添加到repo分支 并加注释提示本次主要修改了什么 

git status  #看repo当前状态，是否有是否有更改后未commit的文件

git diff file_name #看文件修改记录
git diff HEAD -- file_name #查看工作区和版本库的最新区别

git checkout -- file_name 丢弃工作区修改，工作区修改版本会变成暂存区或者分支的版本
git reset HEAD file_name 丢弃暂存区修改，如果想丢弃工作区修改，可使用上一命令丢弃
git reset --head commit_id 回退到版本库以往版本，更改分支head指向的版本

rm file_name 删掉工作区文件
git rm file_name 
git commit -m "注释你删了文件"
版本库中删掉某文件

rm file_name 删掉工作区文件
git checkout --file_name
工作区的文件被误删，用版本库最新版本恢复工作区文件

```

-   `HEAD`指向的版本就是当前版本，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`
	- 也可以写`head^ head^^ head~100 `意思分别是回到上一个、上上个和前100个
-   穿梭前，`git log`查看提交历史，以便确定要回退到哪个版本。
-   要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。


**工作区 Working directory**
- 你建的文件夹
**版本库 Repository**
- .git
- 版本库中包括stage-暂存区 和 master分支 
- 
把工作区文件往git版本库添加：
1. git add 把文件从工作区添加到repo的暂存区
2. git commit 把暂存区内容添加到当前分支

默认分支：main

对于回退和往前往后修改版本，可以看该网站“撤销修改”部分


删除文件


本地库→远程库
```
git push origin main 把本地main分支的最新修改推送至GitHub(有可能branch是master 记得修改)
```

远程库→本地库
```
git clone <git ssl 地址>
```

分支
```
git checkout -b dev 创建并指向新的分支dev
git switch -c dev 创建并指向新的分支dev


git branch 查看当前分支

git checkout main 切换分支，指向main
git switch main 切换分支，指向main

git merge dev  指向分支main时，该命令使dev和main融合（不能在指向分支dev时使用该命令）
git branc -d dev 删除分支dev
```


！分支管理还有一部分没学习完

标签
```
git tag v1.0 给最新提交的commit标签v1.0
git tag v0.9 commit_id 给以往提交的commit标签v0.9

git log --pretty=oneline --abbrev-commit 查看commit_id和log

git tag 查看已有tag们
git tag -a v1.0 -m "注释" commit_id 给commit一个标签并注释
git show tag_name 可以看到tag的相关信息

git tag -d v1.0 删除标签v1.0
git push origin v1.0 把标签上传到github
git push origin --tags 把所有标签上传到github
git push origin :refs/tags/<tagname> 删除github标签，该命令前需要先删除本地该标签
```
