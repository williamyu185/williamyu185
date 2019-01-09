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
    # git batch operate&nbsp; &nbsp; &nbsp; by dingyu<br />
    testBranch="test_20170420"<br />
    currentGitBranch() {<br />
    <span> </span># 获取git当前分支<br />
    &nbsp; &nbsp; <span> </span>branch=''<br />
    &nbsp; &nbsp; <span> </span>cd $PWD<br />
    &nbsp; &nbsp; <span> </span>if [ -d '.git' ]; then<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span> </span>output=`git symbolic-ref --short -q HEAD`<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span> </span>if [ "$output" ]; then<br />
    &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; <span> </span>branch="${output}"<br />
    &nbsp; &nbsp; &nbsp; &nbsp; <span> </span>fi<br />
    &nbsp; &nbsp; <span> </span>fi<br />
    <span> </span>echo "$branch"<br />
    }<br />
    allBranch() {<br />
    <span> </span>
    <blockquote>
    	local allBranchName=`git branch`<br />
    echo "$allBranchName"
    </blockquote>
    }<br />
    pullOtherBranch() {<br />
    <span> </span>
    <blockquote>
    	git pull origin $1
    </blockquote>
    }<br />
    checkoutToTestAndPull() {<br />
    <span> </span>
    <blockquote>
    	checkoutBranch $testBranch<br />
    pullCurrentBranch
    </blockquote>
    }<br />
    checkoutToMasterAndPull() {<br />
    <span> </span>
    <blockquote>
    	checkoutToOtherAndPull master
    </blockquote>
    }<br />
    checkoutToOtherAndPull() {<br />
    <span> </span>
    <blockquote>
    	checkoutBranch $1<br />
    pullCurrentBranch
    </blockquote>
    }<br />
    pullCurrentBranch() {<br />
    <span> </span>
    <blockquote>
    	currentGitBranch<br />
    pullOtherBranch $branch
    </blockquote>
    }<br />
    checkoutBranch() {<br />
    <span> </span>
    <blockquote>
    	if [ ! -n "$1" ]; then<br />
    </blockquote>
    <blockquote>
    	<blockquote>
    		echo "enter branch name"
    	</blockquote>
    	<blockquote>
    		return
    	</blockquote>
    </blockquote>
    <blockquote>
    	fi<br />
    git checkout $1
    </blockquote>
    }<br />
    commitAndPush() {<br />
    <span> </span>
    <blockquote>
    	if [ ! -n "$1" ]; then<br />
    </blockquote>
    <blockquote>
    	<blockquote>
    		echo "commit message must be writed"
    	</blockquote>
    	<blockquote>
    		return
    	</blockquote>
    </blockquote>
    <blockquote>
    	fi<br />
    currentGitBranch<br />
    git add .<br />
    git commit -m "$1"<br />
    pullCurrentBranch<br />
    git push origin $branch
    </blockquote>
    }<br />
    pushTestAndCheckoutBranch() {<br />
    <span> </span>
    <blockquote>
    	currentGitBranch<br />
    if [ "$branch" != $testBranch ]; then<br />
    </blockquote>
    <blockquote>
    	<blockquote>
    		echo `current branch must be ${testBranch}`
    	</blockquote>
    	<blockquote>
    		return
    	</blockquote>
    </blockquote>
    <blockquote>
    	fi<br />
    if [ ! -n "$1" ]; then<br />
    </blockquote>
    <blockquote>
    	<blockquote>
    		echo "enter branch name"
    	</blockquote>
    	<blockquote>
    		return
    	</blockquote>
    </blockquote>
    <blockquote>
    	fi<br />
    pullCurrentBranch<br />
    git push origin $testBranch<br />
    checkoutBranch $1
    </blockquote>
    }<br />
    creatLocalAndRemoteBranch() {<br />
    <span> </span>
    <blockquote>
    	if [ ! -n "$1" ]; then<br />
    </blockquote>
    <blockquote>
    	<blockquote>
    		echo "enter branch name"
    	</blockquote>
    	<blockquote>
    		return
    	</blockquote>
    </blockquote>
    <blockquote>
    	fi<br />
    currentGitBranch<br />
    if [ "$branch" != "master" ]; then<br />
    </blockquote>
    <blockquote>
    	<blockquote>
    		echo "current branch must be master"
    	</blockquote>
    	<blockquote>
    		return
    	</blockquote>
    </blockquote>
    <blockquote>
    	fi<br />
    git checkout -b $1<br />
    git push --set-upstream origin $1
    </blockquote>
    }<br />
    delLocalAndRemoteBranch() {<br />
    <span> </span>
    <blockquote>
    	if [ ! -n "$1" ]; then<br />
    </blockquote>
    <blockquote>
    	<blockquote>
    		echo "enter branch name"
    	</blockquote>
    	<blockquote>
    		return
    	</blockquote>
    </blockquote>
    <blockquote>
    	fi<br />
    git branch -D $1<br />
    git push origin -d $1
    </blockquote>
    }<br />
    containCommitIdBranchs() {<br />
    <span> </span>
    <blockquote>
    	if [ ! -n "$1" ]; then<br />
    </blockquote>
    <blockquote>
    	<blockquote>
    		echo "enter commit id"
    	</blockquote>
    	<blockquote>
    		return
    	</blockquote>
    </blockquote>
    <blockquote>
    	fi<br />
    git branch -a --contains $1
    </blockquote>
    }<br />
    <p>
        <br />
        <p>5.代码合并时产生冲突也是经常遇到的问题,解决冲突是一个比较复杂和考虑较多的过程,最好通过查找冲突点,查看log日志,询问相关代码提交人等方式手动解决,故未对冲突操作进行shell封装.</p>
    	<br />
    6.共同学习,一起进步.错漏处在所难免,敬请谅解.希望本文在提高项目管理效率方面对你有所帮助.<br />
    </p>
</body>
</html>
