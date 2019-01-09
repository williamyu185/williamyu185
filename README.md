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
    <table class="highlight tab-size js-file-line-container" data-tab-size="8">
          <tbody><tr>
            <td id="L1" class="blob-num js-line-number" data-line-number="1"></td>
            <td id="LC1" class="blob-code blob-code-inner js-file-line"># git operate</td>
          </tr>
          <tr>
            <td id="L2" class="blob-num js-line-number" data-line-number="2"></td>
            <td id="LC2" class="blob-code blob-code-inner js-file-line">testBranch="test_20170420"</td>
          </tr>
          <tr>
            <td id="L3" class="blob-num js-line-number" data-line-number="3"></td>
            <td id="LC3" class="blob-code blob-code-inner js-file-line">currentGitBranch() {</td>
          </tr>
          <tr>
            <td id="L4" class="blob-num js-line-number" data-line-number="4"></td>
            <td id="LC4" class="blob-code blob-code-inner js-file-line">	# 获取git当前分支</td>
          </tr>
          <tr>
            <td id="L5" class="blob-num js-line-number" data-line-number="5"></td>
            <td id="LC5" class="blob-code blob-code-inner js-file-line">    	branch=''</td>
          </tr>
          <tr>
            <td id="L6" class="blob-num js-line-number" data-line-number="6"></td>
            <td id="LC6" class="blob-code blob-code-inner js-file-line">    	cd $PWD</td>
          </tr>
          <tr>
            <td id="L7" class="blob-num js-line-number" data-line-number="7"></td>
            <td id="LC7" class="blob-code blob-code-inner js-file-line">    	if [ -d '.git' ]; then</td>
          </tr>
          <tr>
            <td id="L8" class="blob-num js-line-number" data-line-number="8"></td>
            <td id="LC8" class="blob-code blob-code-inner js-file-line">        	output=`git symbolic-ref --short -q HEAD`</td>
          </tr>
          <tr>
            <td id="L9" class="blob-num js-line-number" data-line-number="9"></td>
            <td id="LC9" class="blob-code blob-code-inner js-file-line">        	if [ "$output" ]; then</td>
          </tr>
          <tr>
            <td id="L10" class="blob-num js-line-number" data-line-number="10"></td>
            <td id="LC10" class="blob-code blob-code-inner js-file-line">            		branch="${output}"</td>
          </tr>
          <tr>
            <td id="L11" class="blob-num js-line-number" data-line-number="11"></td>
            <td id="LC11" class="blob-code blob-code-inner js-file-line">        	fi</td>
          </tr>
          <tr>
            <td id="L12" class="blob-num js-line-number" data-line-number="12"></td>
            <td id="LC12" class="blob-code blob-code-inner js-file-line">    	fi</td>
          </tr>
          <tr>
            <td id="L13" class="blob-num js-line-number" data-line-number="13"></td>
            <td id="LC13" class="blob-code blob-code-inner js-file-line">	echo "$branch"</td>
          </tr>
          <tr>
            <td id="L14" class="blob-num js-line-number" data-line-number="14"></td>
            <td id="LC14" class="blob-code blob-code-inner js-file-line">}</td>
          </tr>
          <tr>
            <td id="L15" class="blob-num js-line-number" data-line-number="15"></td>
            <td id="LC15" class="blob-code blob-code-inner js-file-line">allBranch() {</td>
          </tr>
          <tr>
            <td id="L16" class="blob-num js-line-number" data-line-number="16"></td>
            <td id="LC16" class="blob-code blob-code-inner js-file-line">	local allBranchName=`git branch`</td>
          </tr>
          <tr>
            <td id="L17" class="blob-num js-line-number" data-line-number="17"></td>
            <td id="LC17" class="blob-code blob-code-inner js-file-line">	echo "$allBranchName"</td>
          </tr>
          <tr>
            <td id="L18" class="blob-num js-line-number" data-line-number="18"></td>
            <td id="LC18" class="blob-code blob-code-inner js-file-line">}</td>
          </tr>
          <tr>
            <td id="L19" class="blob-num js-line-number" data-line-number="19"></td>
            <td id="LC19" class="blob-code blob-code-inner js-file-line">pullOtherBranch() {</td>
          </tr>
          <tr>
            <td id="L20" class="blob-num js-line-number" data-line-number="20"></td>
            <td id="LC20" class="blob-code blob-code-inner js-file-line">	git pull origin $1</td>
          </tr>
          <tr>
            <td id="L21" class="blob-num js-line-number" data-line-number="21"></td>
            <td id="LC21" class="blob-code blob-code-inner js-file-line">}</td>
          </tr>
          <tr>
            <td id="L22" class="blob-num js-line-number" data-line-number="22"></td>
            <td id="LC22" class="blob-code blob-code-inner js-file-line">checkoutToTestAndPull() {</td>
          </tr>
          <tr>
            <td id="L23" class="blob-num js-line-number" data-line-number="23"></td>
            <td id="LC23" class="blob-code blob-code-inner js-file-line">	checkoutBranch $testBranch</td>
          </tr>
          <tr>
            <td id="L24" class="blob-num js-line-number" data-line-number="24"></td>
            <td id="LC24" class="blob-code blob-code-inner js-file-line">	pullCurrentBranch</td>
          </tr>
          <tr>
            <td id="L25" class="blob-num js-line-number" data-line-number="25"></td>
            <td id="LC25" class="blob-code blob-code-inner js-file-line">}</td>
          </tr>
          <tr>
            <td id="L26" class="blob-num js-line-number" data-line-number="26"></td>
            <td id="LC26" class="blob-code blob-code-inner js-file-line">checkoutToMasterAndPull() {</td>
          </tr>
          <tr>
            <td id="L27" class="blob-num js-line-number" data-line-number="27"></td>
            <td id="LC27" class="blob-code blob-code-inner js-file-line">	checkoutToOtherAndPull master</td>
          </tr>
          <tr>
            <td id="L28" class="blob-num js-line-number" data-line-number="28"></td>
            <td id="LC28" class="blob-code blob-code-inner js-file-line">}</td>
          </tr>
          <tr>
            <td id="L29" class="blob-num js-line-number" data-line-number="29"></td>
            <td id="LC29" class="blob-code blob-code-inner js-file-line">checkoutToOtherAndPull() {</td>
          </tr>
          <tr>
            <td id="L30" class="blob-num js-line-number" data-line-number="30"></td>
            <td id="LC30" class="blob-code blob-code-inner js-file-line">	checkoutBranch $1</td>
          </tr>
          <tr>
            <td id="L31" class="blob-num js-line-number" data-line-number="31"></td>
            <td id="LC31" class="blob-code blob-code-inner js-file-line">	pullCurrentBranch</td>
          </tr>
          <tr>
            <td id="L32" class="blob-num js-line-number" data-line-number="32"></td>
            <td id="LC32" class="blob-code blob-code-inner js-file-line">}</td>
          </tr>
          <tr>
            <td id="L33" class="blob-num js-line-number" data-line-number="33"></td>
            <td id="LC33" class="blob-code blob-code-inner js-file-line">pullCurrentBranch() {</td>
          </tr>
          <tr>
            <td id="L34" class="blob-num js-line-number" data-line-number="34"></td>
            <td id="LC34" class="blob-code blob-code-inner js-file-line">	currentGitBranch</td>
          </tr>
          <tr>
            <td id="L35" class="blob-num js-line-number" data-line-number="35"></td>
            <td id="LC35" class="blob-code blob-code-inner js-file-line">	pullOtherBranch $branch</td>
          </tr>
          <tr>
            <td id="L36" class="blob-num js-line-number" data-line-number="36"></td>
            <td id="LC36" class="blob-code blob-code-inner js-file-line">}</td>
          </tr>
          <tr>
            <td id="L37" class="blob-num js-line-number" data-line-number="37"></td>
            <td id="LC37" class="blob-code blob-code-inner js-file-line">checkoutBranch() {</td>
          </tr>
          <tr>
            <td id="L38" class="blob-num js-line-number" data-line-number="38"></td>
            <td id="LC38" class="blob-code blob-code-inner js-file-line">	if [ ! -n "$1" ]; then</td>
          </tr>
          <tr>
            <td id="L39" class="blob-num js-line-number" data-line-number="39"></td>
            <td id="LC39" class="blob-code blob-code-inner js-file-line">		echo "enter branch name"</td>
          </tr>
          <tr>
            <td id="L40" class="blob-num js-line-number" data-line-number="40"></td>
            <td id="LC40" class="blob-code blob-code-inner js-file-line">		return</td>
          </tr>
          <tr>
            <td id="L41" class="blob-num js-line-number" data-line-number="41"></td>
            <td id="LC41" class="blob-code blob-code-inner js-file-line">	fi</td>
          </tr>
          <tr>
            <td id="L42" class="blob-num js-line-number" data-line-number="42"></td>
            <td id="LC42" class="blob-code blob-code-inner js-file-line">	git checkout $1</td>
          </tr>
          <tr>
            <td id="L43" class="blob-num js-line-number" data-line-number="43"></td>
            <td id="LC43" class="blob-code blob-code-inner js-file-line">}</td>
          </tr>
          <tr>
            <td id="L44" class="blob-num js-line-number" data-line-number="44"></td>
            <td id="LC44" class="blob-code blob-code-inner js-file-line">commitAndPush() {</td>
          </tr>
          <tr>
            <td id="L45" class="blob-num js-line-number" data-line-number="45"></td>
            <td id="LC45" class="blob-code blob-code-inner js-file-line">	if [ ! -n "$1" ]; then</td>
          </tr>
          <tr>
            <td id="L46" class="blob-num js-line-number" data-line-number="46"></td>
            <td id="LC46" class="blob-code blob-code-inner js-file-line">		echo "commit message must be writed"</td>
          </tr>
          <tr>
            <td id="L47" class="blob-num js-line-number" data-line-number="47"></td>
            <td id="LC47" class="blob-code blob-code-inner js-file-line">		return</td>
          </tr>
          <tr>
            <td id="L48" class="blob-num js-line-number" data-line-number="48"></td>
            <td id="LC48" class="blob-code blob-code-inner js-file-line">	fi</td>
          </tr>
          <tr>
            <td id="L49" class="blob-num js-line-number" data-line-number="49"></td>
            <td id="LC49" class="blob-code blob-code-inner js-file-line">	currentGitBranch</td>
          </tr>
          <tr>
            <td id="L50" class="blob-num js-line-number" data-line-number="50"></td>
            <td id="LC50" class="blob-code blob-code-inner js-file-line">	git add .</td>
          </tr>
          <tr>
            <td id="L51" class="blob-num js-line-number" data-line-number="51"></td>
            <td id="LC51" class="blob-code blob-code-inner js-file-line">	git commit -m "$1"</td>
          </tr>
          <tr>
            <td id="L52" class="blob-num js-line-number" data-line-number="52"></td>
            <td id="LC52" class="blob-code blob-code-inner js-file-line">	pullCurrentBranch</td>
          </tr>
          <tr>
            <td id="L53" class="blob-num js-line-number" data-line-number="53"></td>
            <td id="LC53" class="blob-code blob-code-inner js-file-line">	git push origin $branch</td>
          </tr>
          <tr>
            <td id="L54" class="blob-num js-line-number" data-line-number="54"></td>
            <td id="LC54" class="blob-code blob-code-inner js-file-line">}</td>
          </tr>
          <tr>
            <td id="L55" class="blob-num js-line-number" data-line-number="55"></td>
            <td id="LC55" class="blob-code blob-code-inner js-file-line">pushTestAndCheckoutBranch() {</td>
          </tr>
          <tr>
            <td id="L56" class="blob-num js-line-number" data-line-number="56"></td>
            <td id="LC56" class="blob-code blob-code-inner js-file-line">	currentGitBranch</td>
          </tr>
          <tr>
            <td id="L57" class="blob-num js-line-number" data-line-number="57"></td>
            <td id="LC57" class="blob-code blob-code-inner js-file-line">	if [ "$branch" != $testBranch ]; then</td>
          </tr>
          <tr>
            <td id="L58" class="blob-num js-line-number" data-line-number="58"></td>
            <td id="LC58" class="blob-code blob-code-inner js-file-line">		echo `current branch must be ${testBranch}`</td>
          </tr>
          <tr>
            <td id="L59" class="blob-num js-line-number" data-line-number="59"></td>
            <td id="LC59" class="blob-code blob-code-inner js-file-line">		return</td>
          </tr>
          <tr>
            <td id="L60" class="blob-num js-line-number" data-line-number="60"></td>
            <td id="LC60" class="blob-code blob-code-inner js-file-line">	fi</td>
          </tr>
          <tr>
            <td id="L61" class="blob-num js-line-number" data-line-number="61"></td>
            <td id="LC61" class="blob-code blob-code-inner js-file-line">	if [ ! -n "$1" ]; then</td>
          </tr>
          <tr>
            <td id="L62" class="blob-num js-line-number" data-line-number="62"></td>
            <td id="LC62" class="blob-code blob-code-inner js-file-line">		echo "enter branch name"</td>
          </tr>
          <tr>
            <td id="L63" class="blob-num js-line-number" data-line-number="63"></td>
            <td id="LC63" class="blob-code blob-code-inner js-file-line">		return</td>
          </tr>
          <tr>
            <td id="L64" class="blob-num js-line-number" data-line-number="64"></td>
            <td id="LC64" class="blob-code blob-code-inner js-file-line">	fi</td>
          </tr>
          <tr>
            <td id="L65" class="blob-num js-line-number" data-line-number="65"></td>
            <td id="LC65" class="blob-code blob-code-inner js-file-line">	pullCurrentBranch</td>
          </tr>
          <tr>
            <td id="L66" class="blob-num js-line-number" data-line-number="66"></td>
            <td id="LC66" class="blob-code blob-code-inner js-file-line">	git push origin $testBranch</td>
          </tr>
          <tr>
            <td id="L67" class="blob-num js-line-number" data-line-number="67"></td>
            <td id="LC67" class="blob-code blob-code-inner js-file-line">	checkoutBranch $1</td>
          </tr>
          <tr>
            <td id="L68" class="blob-num js-line-number" data-line-number="68"></td>
            <td id="LC68" class="blob-code blob-code-inner js-file-line">}</td>
          </tr>
          <tr>
            <td id="L69" class="blob-num js-line-number" data-line-number="69"></td>
            <td id="LC69" class="blob-code blob-code-inner js-file-line">creatLocalAndRemoteBranch() {</td>
          </tr>
          <tr>
            <td id="L70" class="blob-num js-line-number" data-line-number="70"></td>
            <td id="LC70" class="blob-code blob-code-inner js-file-line">	if [ ! -n "$1" ]; then</td>
          </tr>
          <tr>
            <td id="L71" class="blob-num js-line-number" data-line-number="71"></td>
            <td id="LC71" class="blob-code blob-code-inner js-file-line">		echo "enter branch name"</td>
          </tr>
          <tr>
            <td id="L72" class="blob-num js-line-number" data-line-number="72"></td>
            <td id="LC72" class="blob-code blob-code-inner js-file-line">		return</td>
          </tr>
          <tr>
            <td id="L73" class="blob-num js-line-number" data-line-number="73"></td>
            <td id="LC73" class="blob-code blob-code-inner js-file-line">	fi</td>
          </tr>
          <tr>
            <td id="L74" class="blob-num js-line-number" data-line-number="74"></td>
            <td id="LC74" class="blob-code blob-code-inner js-file-line">	currentGitBranch</td>
          </tr>
          <tr>
            <td id="L75" class="blob-num js-line-number" data-line-number="75"></td>
            <td id="LC75" class="blob-code blob-code-inner js-file-line">	if [ "$branch" != "master" ]; then</td>
          </tr>
          <tr>
            <td id="L76" class="blob-num js-line-number" data-line-number="76"></td>
            <td id="LC76" class="blob-code blob-code-inner js-file-line">		echo "current branch must be master"</td>
          </tr>
          <tr>
            <td id="L77" class="blob-num js-line-number" data-line-number="77"></td>
            <td id="LC77" class="blob-code blob-code-inner js-file-line">		return</td>
          </tr>
          <tr>
            <td id="L78" class="blob-num js-line-number" data-line-number="78"></td>
            <td id="LC78" class="blob-code blob-code-inner js-file-line">	fi</td>
          </tr>
          <tr>
            <td id="L79" class="blob-num js-line-number" data-line-number="79"></td>
            <td id="LC79" class="blob-code blob-code-inner js-file-line">	git checkout -b $1</td>
          </tr>
          <tr>
            <td id="L80" class="blob-num js-line-number" data-line-number="80"></td>
            <td id="LC80" class="blob-code blob-code-inner js-file-line">	git push --set-upstream origin $1</td>
          </tr>
          <tr>
            <td id="L81" class="blob-num js-line-number" data-line-number="81"></td>
            <td id="LC81" class="blob-code blob-code-inner js-file-line">}</td>
          </tr>
          <tr>
            <td id="L82" class="blob-num js-line-number" data-line-number="82"></td>
            <td id="LC82" class="blob-code blob-code-inner js-file-line">delLocalAndRemoteBranch() {</td>
          </tr>
          <tr>
            <td id="L83" class="blob-num js-line-number" data-line-number="83"></td>
            <td id="LC83" class="blob-code blob-code-inner js-file-line">	if [ ! -n "$1" ]; then</td>
          </tr>
          <tr>
            <td id="L84" class="blob-num js-line-number" data-line-number="84"></td>
            <td id="LC84" class="blob-code blob-code-inner js-file-line">		echo "enter branch name"</td>
          </tr>
          <tr>
            <td id="L85" class="blob-num js-line-number" data-line-number="85"></td>
            <td id="LC85" class="blob-code blob-code-inner js-file-line">		return</td>
          </tr>
          <tr>
            <td id="L86" class="blob-num js-line-number" data-line-number="86"></td>
            <td id="LC86" class="blob-code blob-code-inner js-file-line">	fi</td>
          </tr>
          <tr>
            <td id="L87" class="blob-num js-line-number" data-line-number="87"></td>
            <td id="LC87" class="blob-code blob-code-inner js-file-line">	git branch -D $1</td>
          </tr>
          <tr>
            <td id="L88" class="blob-num js-line-number" data-line-number="88"></td>
            <td id="LC88" class="blob-code blob-code-inner js-file-line">	git push origin -d $1</td>
          </tr>
          <tr>
            <td id="L89" class="blob-num js-line-number" data-line-number="89"></td>
            <td id="LC89" class="blob-code blob-code-inner js-file-line">}</td>
          </tr>
          <tr>
            <td id="L90" class="blob-num js-line-number" data-line-number="90"></td>
            <td id="LC90" class="blob-code blob-code-inner js-file-line">containCommitIdBranchs() {</td>
          </tr>
          <tr>
            <td id="L91" class="blob-num js-line-number" data-line-number="91"></td>
            <td id="LC91" class="blob-code blob-code-inner js-file-line">	if [ ! -n "$1" ]; then</td>
          </tr>
          <tr>
            <td id="L92" class="blob-num js-line-number" data-line-number="92"></td>
            <td id="LC92" class="blob-code blob-code-inner js-file-line">		echo "enter commit id"</td>
          </tr>
          <tr>
            <td id="L93" class="blob-num js-line-number" data-line-number="93"></td>
            <td id="LC93" class="blob-code blob-code-inner js-file-line">		return</td>
          </tr>
          <tr>
            <td id="L94" class="blob-num js-line-number" data-line-number="94"></td>
            <td id="LC94" class="blob-code blob-code-inner js-file-line">	fi</td>
          </tr>
          <tr>
            <td id="L95" class="blob-num js-line-number" data-line-number="95"></td>
            <td id="LC95" class="blob-code blob-code-inner js-file-line">	git branch -a --contains $1</td>
          </tr>
          <tr>
            <td id="L96" class="blob-num js-line-number" data-line-number="96"></td>
            <td id="LC96" class="blob-code blob-code-inner js-file-line">}</td>
          </tr>
    </tbody></table>
    <p>
        <br />
        <p>5.代码合并时产生冲突也是经常遇到的问题,解决冲突是一个比较复杂和考虑较多的过程,最好通过查找冲突点,查看log日志,询问相关代码提交人等方式手动解决,故未对冲突操作进行shell封装.</p>
    	<br />
    6.共同学习,一起进步.错漏处在所难免,敬请谅解.希望本文在提高项目管理效率方面对你有所帮助.<br />
    </p>
</body>
</html>
