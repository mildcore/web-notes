

[TOC]

# Git

## 概览

```json
日志
git log --graph --pretty=oneline --abbrev-commit -<number>
git reflog

git config --global user.name "Your Name"
git config --global user.email "email@example.com"

git init
git add .
git commit -m 'msg'

```

## 远程仓库

```json
1. 创建密钥SSH Key
ssh-keygen -t rsa -C "email@example.com"  // 在用户主目录.ssh目录下，生成公钥私钥对id_rsa和id_rsa.pub
2. GitHub ->　Account settings -> SSH Keys -> Add SSH Key	// 公钥添加进来

远程
git remote -v

git remote add origin git@server-name:path/repo-name.git  // 添加远程仓库
git push -u origin master	// 第一次push到远程仓库
git push origin master		// 后续push

git pull
git clone

```

## 版本控制与文件操作

```json
           	 |      版本库         |
   工作区    |  暂存区  |         |
             |         |         |
#　版本控制
1. 版本 commit id
$ git log --pretty=oneline
1094adb7	// HEAD
e475afc9	// HEAD^
eaadf4e3	// HEAD^^  ||  HEAD~2

2. 版本回退
git reset --hard HEAD^

# 文件操作
1.添加到索引/暂存区
git add .	// 注意后续修改不会自动添加到stage, 需再次add

git add -A  // 所有   
git add .   // 添加新文件和编辑过的文件, 不包括删除的文件
git add -u  // 添加删除的和编辑过的文件, 不包括新添加的文件

2.unstage	// 暂存区回到工作区，不撤销更改
git restore --staged <file>	
git reset HEAD <file>

3.撤销修改   // 未添加暂存区，回到版本库当前状态; 已添加暂存区，回到添加到暂存区时状态
git restore <file>
git checkout -- <file> 
	   
4.单文件回退	 // 从历史版本检出某文件，相当于当前工作区作了修改
git restore -s HEAD^ <file>

5.删除
rm <file>	// 只删物理文件
git rm <file>
gir rm --cached <file>	// 只删索引区

```

## 分支管理

```json
1. 查看
查看分支:	git branch
git branch -v
git branch -vv
git branch --set-upstream-to origin/dev [dev]	// 建立本地分支与远程分支的连接后push/pull可简写（当前分支为dev, 可以省略[dev]）
git branch --unset-upstream [dev]
git checkout -b dev origin/dev 			// 创建远程仓库非master分支到本地, 或者直接在本地创建同名branch也能看到

2. 创建
创建分支:	git branch <name>

3. 切换
切换分支:	git checkout <name>  ||  git switch <name>
创建+切换:	git checkout -b <name>  || git switch -c <name>

4. 删除
删除分支:	git branch -d <name>	// -D 强制删除

5. 合并
合并分支<name>到当前分支:	
git merge <name> 			  // 快进模式，看不到分支合并痕迹
git merge <name> --no-ff [-m '']  // 禁用快进模式，创建一个新commit, 显示出合并痕迹

6. 保存当前工作
git stash		// 保存快照，暂时去进行其他工作， 结束后再回来继续
git stash pop	// 取回并删除顶部快照
git stash apply stash@{0}	// 不删除， 删除需另外用git stash drop
git stash drop stash@{0}
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge

7. 复制一个commit的修改
git cherry-pick <commit>		// 通常用来修复master分支一个bug后，将bug修改的commit复制到dev上

8. git rebase

```

## 标签管理

```json
1. 新建
git tag -a v0.1 -m 'tag info' [commit id]

2. 查看
git show v0.1  // 指定标签
git tag  // 所有标签

3. 上传
git push origin v0.1
git push origin --tags // 推送全部未推送过的本地标签

4. 删除
git tag -d v0.1
git push origin :refs/tags/v0.1  // 删除远程标签

*5.rename a tag
// 复制一个新tag, 删除老tag
git tag new old
git tag -d old
git push origin :refs/tags/old
git push --tags
// 其他人删除本地tag
git pull --prune --tags
// 如果是annotated tag, 使用 old^{} 代替old. 以指向commit而不是老的annotated tag object
git tag -a new old^{} -m 'new info'

```

## 比较文件Diff

```json
1. 比较
git diff HEAD <file> // 查看工作区相对于版本库最新版本的区别, 省略file比较全部文件

// 工作区相对于暂存区
git diff
// 工作区相对于版本库
git diff HEAD
git diff HEAD^
git diff HEAD~1
git diff <commit id>
// 暂存区相对于版本库
git diff --staged
git diff --staged <commit id>
// 版本库相对于版本库, 注意顺序, 前者为变化前, 后者为变化后
git diff <cmid1> <cmid2>

// 涉及分支比较, 分支为最新commit版本
git diff master 	// 相对于master最新版本的区别
git diff dev
git diff master dev // dev相对于master的区别 

2. diff结果解读
三种不同diff  ' https://www.cnblogs.com/alfayed/p/4682780.html
正常格式(normal diff)        // diff; 4c4 第4行(c:change, a:add, d:delete)第4行. <表示删除行, >表示增加行
上下文格式(context diff)		// diff -c; 前: * 开始行,行数. 后: - ...
合并格式(unified diff)		 // diff -u; 前: -开始行,行数. 后: + ...

3. GIT DIFF结果是合并格式的变体
diff --git a/a.text b/a.text		// a/..表示改变前; b/..表示改变后
index cbe0187..98ded77 100644		// hash值..hash值（不是版本id） 普通文件644权限
--- a/a.text
+++ b/a.text
@@ -1,2 +1,3 @@					   // 改变前:第1行开始, 共2行; 改变后:第1行开始, 共3行 
 hello git 1.					   // 第一行没有变化
-line 2 end.					   // -表示改变前的行, 可以理解为删除
+line 2 add-master-1.			    // +表示改变后的行, 可以理解为增加
+line 3 add-master-2.			    // +增加第3行

```

##　其他

```json
.gitignore
git check-ignore -v <file>

别名
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch

日志
git config --global alias.lg "log --graph --pretty=oneline --abbrev-commit"
git config --global alias.lgv "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

```

##　Practices

### rebase

```json
// rebase可以将commit记录线性化
合并分支
git rebase dev	// 在master使用，可以将dev分支rebase到master分支

拉取远程数据
git pull --reabse  // = git fetch + git rebase FETCH_HEAD

出现冲突时
1.手动解决后git add, git rebase –continue继续
2.或者git rebase –skip忽略冲突

```



