git配置用户名、邮箱

git config --global user.name 'FZiqian'

git config --global user.email 'xxx@qq.com'

git config --local	//只对某个仓库有效

git 三种状态

modified (已修改)

staged/tracked (已暂存)

committed (已提交)

//初始化git项目

git init 

git status

git add file

(use "git rm --cached <file>..." to unstage)

	// 将暂存区文件移出代码库的跟踪 ???

git rm README.md	//删除工作区文件和暂存区的文件跟踪，并在下次提交时不纳入版本管理

git rm  -f README.md	// 此方法为--force及强制删除工作区与暂存区的文件，用于当工作区与暂存区不一致时

以上rm方法都不会删除仓库里的文件


git commit -m 'msg'

git commit -am 'add and commit'

git commit add -u	//更新提交所有已经被git跟踪的文件

 (use "git reset HEAD <file>..." to unstage)

 	// 将暂存区的文件重置（不会改变工作区的内容）

"git checkout -- <file>..." to discard changes in working directory)

	// 本地文件会被版本库的文件覆盖

	// 这里在不想提交修改文件或者误删工作区文件后恢复文件时使用
git log

版本回退

git reset HEAD		//将暂存区恢复回版本库

git reset HEAD -- filename // 将指定暂存区的文件恢复为版本库

git reset HEAD^		//将仓库的版本倒退一个 

			//此时因为版本库倒退，本地的代码不变，所以暂存区会显示有unstaged的修改
			
			//此时如果想将工作区与仓库同步 可使用git checkout -- <file>

HEAD~2 上上个 HEAD~3 上上上个

HEAD^ 上一个 HEAD^^^ 上上上个

git reset 的参数

git reset [--mixed] HEAD^ （默认）效果同上

git reset --soft HEAD^ (此时会显示有staged但未commit的文件，相当于可以重新commit一次代码)

git reset --hard HEAD^ (此时工作区与仓库会同步回滚，no unstaged,no uncommitted)

			//hard方式有危险，因为会把工作区的内容消掉

git commit --amend	//更正最近一次的提交

git commit --amend -m 'msg'	// 加-m 不会使用vim打开编辑massage

//更新以前的提交massage

git rebase -i parentCommitID(hash)  //rebase 变基 -i 交互式

				// reword
				
				//使用rebase在本地修改未push的可以，但是最后不要修改已经push了的

//使用rebase将以前的多个commit合并

git rebase -i parentCommitID(hash) //squash

回滚指定id的快照（可向前/向后滚）

git reset a1fsda(快照的hash值) 

git reflog //可以查看所有的版本历史id（包括已经回滚的）

回滚个别文件 

git reset 版本快照id 文件名/路径

	//此方法不会修改整个版本库的head指针


注意上面回滚不加reset的参数，默认就是mixed方式回滚

diff命令	//比较不同状态或版本文件的不同

git diff	// 默认比较的是【工作区】与暂存区的不同

git diff 版本号1 版本号2	//比较两个版本的不同

git diff 版本号			//比较【工作区】与指定版本的不同

git diff --cached [版本号]	//--cached参数表示比较的是暂存区与版本库的不同

git diff -- filename		//比较指定文件

git mv 旧文件名 新文件名	//重命名文件

git branch			//查看分支

git branch -av			//查看所有分支，并显示对应的最新commit

git branch 分支名		//创建一个新的分支

git checkout 分支名		//切换一个分支

将上面两步合并

git checkout -b 分支名		//创建并切换到一个新的分支

git checkout -b 新分支 基础分支	//创建并切换到一个基于基础分支的新分支

				//如果基础分支不制定，则为当前分支

git log --decorate		//decorate 装饰
				
				//显示提交指向的分支(branch)和tag\

git log -n5 --oneline		//显示最近5条日志  -n5

git log --oneline tmp		//tmp 分支名 显示分支的历史

git help --web log		//使用浏览器打开 git log 命令的帮助文档

git log --decorate --oneline	//--oneline 只显示快照的id和说明

git log --decorate --oneline --graph --all	//--graph 以图表的方式显示   --all 显示所有的分支

git merge 分支名		//将指定的分支合并到本分支中

git branch -d(--delete) 分支名	//删除分支

git branch -D 分支名		//强制删除没有merge的分支

reset 与 checkout的区别

reset主要用于版本快照的切换

checkout主要用于切换分支

git checkout -- filename	//将指定文件从暂存区里恢复到工作区

git stash	//将暂存区的内容临时存放到stash 清空暂存区

git stash list	//查看stash列表

//提交紧急任务后恢复stash

git stash apply //将stash列表里的挑出来恢复工作区和暂存区

git stash pop   //将stash列表里的跳出来恢复后并将stash列表里的对应stash删除

.gitignore

//忽略doc已经doc文件夹下所有文件

doc/ 

git clone --bare /path/my/project/.git new.git //哑协议

git clone --bare file:///path/my/project/.git new.git //智能协议（带进度条，传输进行了压缩优化）

git remote //查看远程仓库

git remote -v //查看远程仓库详细信息

git remote add remoteName file:///path/my/project/new.git //添加一个远程仓库

git remote remove remoteName

git remote rename old new

git push --set-upstream remoteName/branchName branchName //将设置上游的远程仓库分支指向本地分支

git push	//push到远程仓库

git push remoteName --all //将所有的分支都push到远程仓库

git merge --allow-unrelated-histories branchName //合并允许无关联的版本仓库 有时候在github上面创建项目为非空，合并本地仓库时会用到

使用github创建版本库，上传本地代码

git remote add origin https://github.com/Fziqian/git-demo.git

git push -u origin master

使用github clone以后，使用git checkout -b branchName origin/branchName //将远端的分支clone并切换