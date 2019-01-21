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
    # git operate&nbsp; &nbsp; &nbsp;<br />
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
    containSomeoneAllBranchs() {<br />
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
    git branch -a --contains $1
    </blockquote>
    }<br />
    <p>
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
                       9)pushTestAndCheckoutBranch &lt;branchName&gt; &nbsp;将其他分支合并到测试分支后,使用该指令,可自行将合并后的代码推送到远程测试分
        支,并切换到指定指定分支.注:为避免推送错分支,该指令只能在测试分支下使用,在其他分支使用该指令无效.示例:pushTestAndCheckoutBranch otherBranchName
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
                       9)pushTestAndCheckoutBranch &lt;branchName&gt; &nbsp;将其他分支合并到测试分支后,使用该指令,可自行将合并后的代码推送到远程测试分支,并切换到指定指定分支.
        注:为避免推送错分支,该指令只能在测试分支下使用,在其他分支使用该指令无效.示例:pushTestAndCheckoutBranch otherBranchName
               </p>
               <p>
                       10)creatLocalAndRemoteBranch &lt;branchName&gt; 创建一个新的本地和远程分支.注:由于本地创建的新分支是当前分支的一个镜像,为保证新分支代码本地master分支的
        一致,该指令只能在master分支下使用.示例:creatLocalAndRemoteBranch newBranchName
               </p>
               <p>
                       11)delLocalAndRemoteBranch &lt;branchName&gt; &nbsp;删除本地分支和远程分支.示例:delLocalAndRemoteBranch delBranchName
               </p>
               <p>
                       12)containCommitIdBranchs &lt;branchName&gt; &nbsp;笔者所在部门分支由测试人员负责上线,有时本地积累了大量分支,由于不确定分支是否上线,在删除本地和远程分支是难免有很多忧虑,使用该指令可解决这个问题.切换到想要删除的分支,通过containCommitIdBranchs branchName查看打印结果,如果打印结果中,该branchName出现在master分支中,说明分支代码已上线,可放心删除.示例:containCommitIdBranchs branchName
               </p>
            </blockquote>
            <p>
               <br />
            5.共同学习,一起进步.有错漏地方在所难免,敬请指出.希望本文对你有所帮助<br />
            </p>
        </body>
        </html>
        ~
        ~
        (END)
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
                       9)pushTestAndCheckoutBranch &lt;branchName&gt; &nbsp;将其他分支合并到测试分支后,使用该指令,可自行将合并后的代码推送到远程测试分
        支,并切换到指定指定分支.注:为避免推送错分支,该指令只能在测试分支下使用,在其他分支使用该指令无效.示例:pushTestAndCheckoutBranch otherBranchName
               </p>
               <p>
                       10)creatLocalAndRemoteBranch &lt;branchName&gt; 创建一个新的本地和远程分支.注:由于本地创建的新分支是当前分支的一个镜像,为保证新
        分支代码本地master分支的一致,该指令只能在master分支下使用.示例:creatLocalAndRemoteBranch newBranchName
               </p>
               <p>
                       11)delLocalAndRemoteBranch &lt;branchName&gt; &nbsp;删除本地分支和远程分支.示例:delLocalAndRemoteBranch delBranchName
               </p>
               <p>
                       12)containSomeoneAllBranchs &lt;branchName&gt; &nbsp;笔者所在部门分支由测试人员负责上线,有时本地积累了大量分支,由于不确定分支是否上
        线,在删除本地和远程分支是难免有很多忧虑,使用该指令可解决这个问题.切换到想要删除的分支,,通过containCommitIdBranchs 06b04c182af87d8464b27cf54cc1ccb4e7e2b2c3查看打印结果,如果打印结果中,该branchName出现在master分支中,说明分支代码已上线,可放心删除.示例:containSomeoneAllBranchs branchName
               </p>
            </blockquote>
        <br />
        <p>5.代码合并时产生冲突也是经常遇到的问题,解决冲突是一个比较复杂和考虑较多的过程,最好通过查找冲突点,查看log日志,询问相关代码提交人等方式手动解决,故未对冲突操作进行shell封装.</p>
    	<br />
    6.共同学习,一起进步.错漏处在所难免,敬请谅解.希望本文在提高项目管理效率方面对你有所帮助.<br />
    </p>
</body>
</html>
