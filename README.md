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
    1.安装iterm2.命令行模式: brew cask install iterm2 或 到官网  https://www.iterm2.com/downloads.html  下载.<br />
    <br />
    2.vim ~/.zshrc,将shell.txt中的代码粘贴到.zshrc文件中,保存并退出.以下为shell.txt中的源码展示.<br />
    <br />
    <p>
    	#git operate <br /> testBranch = "test_20170420"currentGitBranch() {#获取git当前分支branch = ''cd $PWD<br />
    &nbsp; &nbsp; if [ - d '.git'];<br />
    &nbsp; &nbsp; then output = `git symbolic - ref--short - q HEAD`<br />
    &nbsp; &nbsp; if ["$output"];<br />
    &nbsp; &nbsp; then branch = "${output}"fi fi echo "$branch"<br />
    }<br />
    allBranch() {<br />
    &nbsp; &nbsp; local allBranchName = `git branch`echo "$allBranchName"<br />
    }<br />
    pullOtherBranch() {<br />
    &nbsp; &nbsp; git pull origin $1<br />
    }<br />
    checkoutToTestAndPull() {<br />
    &nbsp; &nbsp; checkoutBranch $testBranch pullCurrentBranch<br />
    }<br />
    checkoutToMasterAndPull() {<br />
    &nbsp; &nbsp; checkoutToOtherAndPull master<br />
    }<br />
    checkoutToOtherAndPull() {<br />
    &nbsp; &nbsp; checkoutBranch $1 pullCurrentBranch<br />
    }<br />
    pullCurrentBranch() {<br />
    &nbsp; &nbsp; currentGitBranch pullOtherBranch $branch<br />
    }<br />
    checkoutBranch() {<br />
    &nbsp; &nbsp; if [!-n "$1"];<br />
    &nbsp; &nbsp; then echo "enter branch name"<br />
    &nbsp; &nbsp; return fi git checkout $1<br />
    }<br />
    commitAndPush() {<br />
    &nbsp; &nbsp; if [!-n "$1"];<br />
    &nbsp; &nbsp; then echo "commit message must be writed"<br />
    &nbsp; &nbsp; return fi currentGitBranch git add.git commit - m "$1"pullCurrentBranch git push origin $branch<br />
    }<br />
    pushTestAndCheckoutBranch() {<br />
    &nbsp; &nbsp; currentGitBranch<br />
    &nbsp; &nbsp; if ["$branch" != $testBranch];<br />
    &nbsp; &nbsp; then echo`current branch must be $ {<br />
    &nbsp; &nbsp; &nbsp; &nbsp; testBranch<br />
    &nbsp; &nbsp; }`<br />
    &nbsp; &nbsp; return fi<br />
    &nbsp; &nbsp; if [!-n "$1"];<br />
    &nbsp; &nbsp; then echo "enter branch name"<br />
    &nbsp; &nbsp; return fi pullCurrentBranch git push origin $testBranch checkoutBranch $1<br />
    }<br />
    creatLocalAndRemoteBranch() {<br />
    &nbsp; &nbsp; if [!-n "$1"];<br />
    &nbsp; &nbsp; then echo "enter branch name"<br />
    &nbsp; &nbsp; return fi currentGitBranch<br />
    &nbsp; &nbsp; if ["$branch" != "master"];<br />
    &nbsp; &nbsp; then echo "current branch must be master"<br />
    &nbsp; &nbsp; return fi git checkout - b $1 git push--set - upstream origin $1<br />
    }<br />
    delLocalAndRemoteBranch() {<br />
    &nbsp; &nbsp; if [!-n "$1"];<br />
    &nbsp; &nbsp; then echo "enter branch name"<br />
    &nbsp; &nbsp; return fi git branch - D $1 git push origin - d $1<br />
    }<br />
    containCommitIdBranchs() {<br />
    &nbsp; &nbsp; if [!-n "$1"];<br />
    &nbsp; &nbsp; then echo "enter commit id"<br />
    &nbsp; &nbsp; return fi git branch - a--contains $1<br />
    }<br />
    </p>
    <br />
    在实际开发中,将正在开发分支代码合并到测试分支应该是最常进行的操作.笔者专门针对测试环境封装了几个常用指令.上面的test_20170420为笔者所在部门所使用测试环境分支名,用户可自行替换成自己所在开发环境的测试分支名<br />
    <br />
    3.终端下执行 &nbsp;&nbsp; source ~/.zshrc &nbsp;使粘入的代码生效.执行&nbsp;&nbsp;chmod 777 ~/.zshrc 确保脚本有读写执行权限.重启iterm2就可使用啦<br />
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
    		2)pullOtherBranch &lt;branchName&gt; &nbsp;拉取远程其他分支代码合并到当前分支.示例:pullOtherBranch test_20170420
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
    		9)pushTestAndCheckoutBranch &lt;branchName&gt; &nbsp;需在其他分支合并到测试分支后使用该指令.该指令将合并后的代码推送到远程测试分支,并切换到指定分支.注:为避免推送错分支,该指令只能在测试分支下使用,在其他分支使用该指令无效.示例:pushTestAndCheckoutBranch otherBranchName
    	</p>
    	<p>
    		10)creatLocalAndRemoteBranch &lt;branchName&gt; 创建一个新的本地和远程分支.注:由于本地创建的新分支是当前分支的一个镜像,为保证新分支代码本地master分支的一致,该指令只能在master分支下使用.示例:creatLocalAndRemoteBranch newBranchName
    	</p>
    	<p>
    		11)delLocalAndRemoteBranch &lt;branchName&gt; &nbsp;删除本地分支和远程分支.示例:delLocalAndRemoteBranch delBranchName
    	</p>
    	<p>
    		12)containCommitIdBranchs &lt;commitId&gt; &nbsp;笔者所在部门分支由测试人员负责上线,有时本地积累了大量分支,由于不确定分支是否已经上线,在删除本地和远程分支是难免有很多顾虑.使用该指令可解决这个问题.切换到想要删除的分支,git log查看日志,查找到最新一次提交的commitId并复制,通过containCommitIdBranchs <commitId>查看打印结果,如果打印结果中,该commitId出现在master分支中,说明分支代码已上线,可放心删除.注:使用该指令前先切换到master分支并拉取最新的master分支远程代码.示例:containCommitIdBranchs 06b04c182af87d8464b27cf54cc1ccb4e7e2b2c3
    	</p>
    </blockquote>
    <p>
        <br />
        <p>5.代码合并时产生冲突也是经常遇到的问题,解决冲突是一个比较复杂和考虑较多的过程,最好通过查找冲突点,查看log日志,询问相关代码提交人等方式手动解决,故未对冲突操作进行shell封装.</p>
    	<br />
    6.共同学习,一起进步.错漏处在所难免,敬请谅解.希望本文在提高项目管理效率方面对你有所帮助.<br />
    </p>
</body>
</html>
