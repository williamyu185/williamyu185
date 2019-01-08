<!DOCTYPE html>
<html lang="zh-cn">
<head>
<meta charset="utf-8"/>
<meta name="viewport" content="width=device-width, initial-scale=1" />
<meta name="referrer" content="origin" />
</head>
<body>
    <p>
    	<h2 style="text-align:center;">
    		在iterm2终端下,使用shell封装几个实用git项目管理指令
    	</h2>
    <br />
    笔者操作系统为mac,用户可参考以下说明,配置可在自己开发环境下运行的shell指令
    </p>
    <p>
    	<br />
    1.安装iterm2.实用命令行模式: brew cask install iterm2 或 到官网  https://www.iterm2.com/downloads.html  下载.<br />
    <br />
    2.vim ~/.zshrc,将shell.txt中的代码粘贴到.zshrc文件中,保存并退出.以下为shell.txt中的源码展示.<br />
    <br />
    # git operate<br />
    testBranch="test_20170420"<br />
    currentGitBranch() {<br />
    # 获取git当前分支<br />
    &nbsp; &nbsp; 	branch=''<br />
    &nbsp; &nbsp; 	cd $PWD<br />
    &nbsp; &nbsp; 	if [ -d '.git' ]; then<br />
    &nbsp; &nbsp; &nbsp; &nbsp; 	output=`git symbolic-ref --short -q HEAD`<br />
    &nbsp; &nbsp; &nbsp; &nbsp; 	if [ "$output" ]; then<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 		branch="${output}"<br />
    &nbsp; &nbsp; &nbsp; &nbsp; 	fi<br />
    &nbsp; &nbsp; 	fi<br />
    echo "$branch"<br />
    }<br />
    allBranch() {
    </p>
    <blockquote>
    	<p>
    		local allBranchName=`git branch`
    	</p>
    	<p>
    		echo "$allBranchName"
    	</p>
    </blockquote>
    <p>
    	}<br />
    pullOtherBranch() {
    </p>
    <blockquote>
    	<p>
    		git pull origin $1
    	</p>
    </blockquote>
    <p>
    	}<br />
    checkoutToTestAndPull() {
    </p>
    <blockquote>
    	<p>
    		git checkout $testBranch
    	</p>
    	<p>
    		git pull origin $testBranch
    	</p>
    </blockquote>
    <p>
    	}<br />
    checkoutToMasterAndPull() {
    </p>
    <blockquote>
    	<p>
    		git checkout master
    	</p>
    	<p>
    		git pull origin master
    	</p>
    </blockquote>
    <p>
    	}<br />
    checkoutToOtherAndPull() {
    </p>
    <blockquote>
    	<p>
    		checkoutBranch $1
    	</p>
    	<p>
    		pullCurrentBranch
    	</p>
    </blockquote>
    <p>
    	}<br />
    pullCurrentBranch() {
    </p>
    <blockquote>
    	<p>
    		currentGitBranch
    	</p>
    	<p>
    		git pull origin $branch
    	</p>
    </blockquote>
    <p>
    	}<br />
    checkoutBranch() {
    </p>
    <blockquote>
    	<p>
    		if [ ! -n "$1" ]; then
    	</p>
    </blockquote>
    <blockquote>
    	<blockquote>
    		<p>
    			echo "enter branch name"
    		</p>
    	</blockquote>
    </blockquote>
    <blockquote>
    	<p>
    		return
    	</p>
    	<p>
    		fi
    	</p>
    	<p>
    		git checkout $1
    	</p>
    </blockquote>
    <p>
    	}<br />
    commitAndPush() {
    </p>
    <blockquote>
    	<p>
    		if [ ! -n "$1" ]; then
    	</p>
    </blockquote>
    <blockquote>
    	<blockquote>
    		<p>
    			echo "commit message must be writed"
    		</p>
    	</blockquote>
    	<blockquote>
    		<p>
    			return
    		</p>
    	</blockquote>
    </blockquote>
    <blockquote>
    	<p>
    		fi
    	</p>
    	<p>
    		currentGitBranch
    	</p>
    	<p>
    		git add .
    	</p>
    	<p>
    		git commit -m "$1"
    	</p>
    	<p>
    		git pull origin $branch
    	</p>
    	<p>
    		git push origin $branch
    	</p>
    </blockquote>
    <p>
    	}<br />
    pushTestAndCheckoutBranch() {
    </p>
    <blockquote>
    	<p>
    		currentGitBranch
    	</p>
    	<p>
    		if [ "$branch" != $testBranch ]; then
    	</p>
    </blockquote>
    <blockquote>
    	<blockquote>
    		<p>
    			echo `current branch must be ${testBranch}`
    		</p>
    	</blockquote>
    	<blockquote>
    		<p>
    			return
    		</p>
    	</blockquote>
    </blockquote>
    <blockquote>
    	<p>
    		fi
    	</p>
    	<p>
    		if [ ! -n "$1" ]; then
    	</p>
    </blockquote>
    <blockquote>
    	<blockquote>
    		<p>
    			echo "enter branch name"
    		</p>
    	</blockquote>
    	<blockquote>
    		<p>
    			return
    		</p>
    	</blockquote>
    </blockquote>
    <blockquote>
    	<p>
    		fi
    	</p>
    	<p>
    		git pull origin $testBranch
    	</p>
    	<p>
    		git push origin $testBranch
    	</p>
    	<p>
    		git checkout $1
    	</p>
    </blockquote>
    <p>
    	}<br />
    creatLocalAndRemoteBranch() {
    </p>
    <blockquote>
    	<p>
    		if [ ! -n "$1" ]; then
    	</p>
    </blockquote>
    <blockquote>
    	<blockquote>
    		<p>
    			echo "enter branch name"
    		</p>
    	</blockquote>
    	<blockquote>
    		<p>
    			return
    		</p>
    	</blockquote>
    </blockquote>
    <blockquote>
    	<p>
    		fi
    	</p>
    	<p>
    		git checkout -b $1
    	</p>
    	<p>
    		git push --set-upstream origin $1
    	</p>
    </blockquote>
    <p>
    	}<br />
    delLocalAndRemoteBranch() {
    </p>
    <blockquote>
    	<p>
    		if [ ! -n "$1" ]; then
    	</p>
    </blockquote>
    <blockquote>
    	<blockquote>
    		<p>
    			echo "enter branch name"
    		</p>
    	</blockquote>
    	<blockquote>
    		<p>
    			return
    		</p>
    	</blockquote>
    </blockquote>
    <blockquote>
    	<p>
    		fi
    	</p>
    	<p>
    		currentGitBranch
    	</p>
    	<p>
    		if [ "$branch" != "master" ]; then
    	</p>
    </blockquote>
    <blockquote>
    	<blockquote>
    		<p>
    			echo "current branch must be master"
    		</p>
    	</blockquote>
    	<blockquote>
    		<p>
    			return
    		</p>
    	</blockquote>
    </blockquote>
    <blockquote>
    	<p>
    		fi
    	</p>
    	<p>
    		git branch -D $1
    	</p>
    	<p>
    		git push origin -d $1
    	</p>
    </blockquote>
    <p>
    	}<br />
    containCommitIdBranchs() {
    </p>
    <blockquote>
    	<p>
    		if [ ! -n "$1" ]; then
    	</p>
    </blockquote>
    <blockquote>
    	<blockquote>
    		<p>
    			echo "enter commit id"
    		</p>
    	</blockquote>
    	<blockquote>
    		<p>
    			return
    		</p>
    	</blockquote>
    </blockquote>
    <blockquote>
    	<p>
    		fi
    	</p>
    	<p>
    		git branch -a --contains $1
    	</p>
    </blockquote>
    <p>
    	}<br />
    <br />
    在实际开发中,将正在开发分支代码合并到测试分支应该是最常进行的操作.笔者专门针对测试环境封装了几个常用指令.上面的test_20170420为笔者所在部门所使用测试环境分支名,用户可自行替换成自己所在开发环境的测试分支名<br />
    <br />
    3.终端下执行source ~/.zshrc &nbsp;使粘入的代码生效,执行chmod 777 ~/.zshrc 确保脚本有读写执行权限.重启iterm2就可使用啦<br />
    <br />
    4.指令文档说明
    </p>
    <blockquote>
    	<p>
    		1)allBranch 打印所有本地分支
    	</p>
    </blockquote>
    <blockquote>
    	<p>
    		2)pullOtherBranch &lt;branchName&gt; &nbsp;拉取远程其他分支代码并合并到当前分支.示例:pullOtherBranch test_20170420
    	</p>
    	<p>
    		3)checkoutToTestAndPull &nbsp;自行从当前分支切换到测试分支,并拉取远程测试分支代码.
    	</p>
    	<p>
    		4)checkoutToMasterAndPull &nbsp;切换到主分支并拉取远程主分支代码
    	</p>
    	<p>
    		5)checkoutToOtherAndPull &lt;branchName&gt; 切换到其他分支,切换后拉取所在分支远程代码.示例:checkoutToOtherAndPull test_20170420
    	</p>
    	<p>
    		6)pullCurrentBranch &nbsp;拉取当前分支远程代码
    	</p>
    	<p>
    		7)checkoutBranch &lt;branchName&gt; &nbsp;切换到其他分支.示例:checkoutBranch test_20170420
    	</p>
    	<p>
    		8)commitAndPush &lt;commitMsg:String&gt; 提交本地代码到当前分支的本地仓库,并推送到当前分支的远程仓库.示例:commitAndPush "commitMsg"
    	</p>
    	<p>
    		9)pushTestAndCheckoutBranch &lt;branchName&gt; &nbsp;将其他分支合并到测试分支后,使用该指令,可自行将合并后的代码推送到远程测试分支,并切换到指定指定分支.注:为避免推送错分支,该指令只能在测试分支下使用,在其他分支使用该指令无效.示例:pushTestAndCheckoutBranch otherBranchName
    	</p>
    	<p>
    		10)creatLocalAndRemoteBranch &lt;branchName&gt; 创建一个新的本地和远程分支.注:由于本地创建的新分支是当前分支的一个镜像,为保证新分支代码本地master分支的一致,该指令只能在master分支下使用.示例:creatLocalAndRemoteBranch newBranchName
    	</p>
    	<p>
    		11)delLocalAndRemoteBranch &lt;branchName&gt; &nbsp;删除本地分支和远程分支.示例:delLocalAndRemoteBranch delBranchName
    	</p>
    	<p>
    		12)containCommitIdBranchs &lt;commitId&gt; &nbsp;笔者所在部门分支由测试人员负责上线,有时本地积累了大量分支,由于不确定分支是否上线,在删除本地和远程分支是难免有很多忧虑,使用该指令可解决这个问题.切换到想要删除的分支,git log查看日志,查找到最新一次提交的commitId并复制,使用checkoutToMasterAndPull指令切换会主分支,粘入复制的commitId,通过containCommitIdBranchs 06b04c182af87d8464b27cf54cc1ccb4e7e2b2c3查看打印结果,如果打印结果中,该commitId出现在master分支中,说明分支代码已上线,可放心删除.示例:containCommitIdBranchs 06b04c182af87d8464b27cf54cc1ccb4e7e2b2c3
    	</p>
    </blockquote>
    <p>
    	<br />
    5.共同学习,一起进步.错漏处在所难免,敬请谅解.<br />
    </p>
</body>
</html>
