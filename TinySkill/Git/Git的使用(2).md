## Git 的使用(2) ##

#### 1. 查看 ####
`git satus` :要随时掌握工作区的状态，使用git status命令。
	
`git diff` :如果git status告诉你有文件被修改过，用git diff可以查看修改内容。

#### 2. 版本回退 ####
`git log` :命令显示从最近到最远的提交日志

`git log--pretty=oneline`:查看过滤后的日志,只显示`commit id`和`distributed`

Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交3628164...882e1e0（注意我的提交ID和你的肯定不一样），上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。

穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。

要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

#### 3. 创建分支 ####
首先，我们创建dev分支，然后切换到dev分支：

	$ git checkout -b dev
	Switched to a new branch 'dev'
`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

	$ git branch dev
	$ git checkout dev
	Switched to branch 'dev'

然后，用`git branch`命令查看当前分支：

	$ git branch
	* dev
	  master

`git branch`命令会列出所有分支，当前分支前面会标一个*号。

然后，我们就可以在dev分支上正常提交，比如对readme.txt做个修改，加上一行：

	Creating a new branch is quick.
然后提交：

	$ git add readme.txt 
	$ git commit -m "branch test"
	[dev fec145a] branch test
	 1 file changed, 1 insertion(+)
现在，dev分支的工作完成，我们就可以切换回master分支：

	$ git checkout master
	Switched to branch 'master'

切换回master分支后，再查看一个readme.txt文件，刚才添加的内容不见了！因为那个提交是在dev分支上，而master分支此刻的提交点并没有变：

现在，我们把dev分支的工作成果合并到master分支上：

	$ git merge dev
	Updating d17efd8..fec145a
	Fast-forward
	 readme.txt |    1 +
	 1 file changed, 1 insertion(+)
`git merge`命令用于合并指定分支到当前分支。合并后，再查看readme.txt的内容，就可以看到，和dev分支的最新提交是完全一样的。

注意到上面的Fast-forward信息，Git告诉我们，这次合并是“快进模式”，也就是直接把master指向dev的当前提交，所以合并速度非常快。

当然，也不是每次合并都能Fast-forward，我们后面会讲其他方式的合并。

合并完成后，就可以放心地删除dev分支了：

	$ git branch -d dev
	Deleted branch dev (was fec145a).
删除后，查看branch，就只剩下master分支了：
	
	$ git branch
	* master
因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。

**总结:**
Git鼓励大量使用分支：

	查看分支：git branch
	
	创建分支：git branch <name>
	
	切换分支：git checkout <name>
	
	创建+切换分支：git checkout -b <name>
	
	合并某分支到当前分支：git merge <name>
	
	删除分支：git branch -d <name>