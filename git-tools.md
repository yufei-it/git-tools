# git-tools  
参考文章：[Git教程|菜鸟教程](http://www.runoob.com/git/git-workspace-index-repo.html)  

Git是一个开源的分布式版本控制系统,它能够记录所有文件的所有版本，可以有效地追踪文件的变化。  

Git分为 工作区（workspace）、暂存区（index）、本地仓库（local repository），当然还有远程仓库（remote repository）。远程仓库为我们保存一份代码拷贝，如github。而工作区、暂存区和本地仓库都在本地，这就是为什么没有网络我们也照样使用git提交（commit）代码更新，因为提交仅是提交到本地仓库，待有网络之后可以再推送（push）到远程仓库。  

图中左侧为工作区，右侧为版本库(index+local _repository)。在版本库中标记为 "index" 的区域是暂存区（stage/index），标记为 "master" 的是 master 分支所代表的目录树。  
图中的 objects 标识的区域为 Git 的对象库，实际位于 ".git/objects" 目录下，里面包含了创建的各种对象及内容。  
当对工作区修改（或新增）的文件执行 "git add" 命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。  
当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。  

### 1. Git安装  
[Windows平台安装包下载](https://git-for-windows.github.io/)

	1. 配置用户名和电子邮件地址  

		$ git config --global user.name "wirelessqa" 
        $ git config --global user.email wirelessqa.me@gmail.com 

	2. 配置Git log输出界面  
		打开C:\Users\Administrator\.gitconfig，添加以下代码:
			[alias]
			lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit  
		查看git日志时，执行 $ git lg 命令。

	3. 配置秘钥信息

		1. 执行 $ ssh-keygen -t rsa -C "youremail@example.com" 命令，生成 SSH Key 。
		2. 打开C:\Users\Administrator.YUFEI-NEW-PC\.ssh\id_rsa.pub，复制所有内容。
		3. 登陆个人GitHub-->Account Settings-->SSH keys-->Add SSH key，粘贴id_rsa.pub文件内的key。
		4. 执行 $ ssh -T git@github.com 命令，验证是否配置成功。
		5. 若是验证通过，输出结果如下: Hi yufei-it! You've successfully authenticated, but GitHub does not provide shell access。

	4. Git新建本地仓库

		1. 本地新建仓库
			$ git init    新建文件夹初始化
			$ git remote add origin <url>    将远程仓库与本地仓库关联
			$ git pull origin master    同步远程仓库文件到工作区
			如果出现错误，请查看 http://blog.csdn.net/god_wot/article/details/10522405
			
		2. 从现有仓库克隆
			$ git clone <url>
		
### 2. Git工作流程  

		1. 克隆远程Git项目仓库作为工作区。
		2. 添加或修改工作区文件。
		3. 提交前更新(pull)工作区。
		4. 解决工作区内的冲突文件。
		5. 添加(add),提交(commit)修改内容到本地仓库。
		6. 上传(push)到远程仓库。
		7. 如果发现错误，可以撤回(reset)提交内容，执行2——6操作步骤。
  
### 3. Git常用命令  

> **基本操作**  

	    $ git add .        添加当前项目的所有文件
	    $ git add <file1> <file2>    添加指定文件到暂存区
	
	    $ git status    查看在你上次提交暂存之后，工作区有哪些文件被修改
		$ git status --s    查看在你上次提交暂存之后，文件被修改日志精简版
	
	    $ git diff    比较的是工作区和暂存区的数据
	    $ git diff --stat    比较的是工作区和暂存区有差异的文件名称
	    $ git diff –cached    比较的是暂存区和版本库中的数据
	    $ git diff HEAD    比较的是工作区和版本库的数据
	
	    $ git pull    同步远程仓库文件到本地(等同于$ git fetch + $ git merge)
	
	    $ git commit -m <description>    将暂存区的内容提交到版本库
	    $ git commit -am <description>    将修改的内容添加到暂存区并提交道版本库
	
	    $ git push origin master    将版本库最新内容推送到github远程仓库
	
	    $ git log     列出详细历史提交记录
	    $ git log -1    根据int参数，可以限制输出的提交记录数目
	    $ git log --online 命令查看历史记录精简版
	    $ git log --graph 命令查看历史分支建立、分支合并等记录。
	    $ git log --author=<user.name>     根据user.name可以查看指定用户的提交日志
	 
	    $ git tag -a <version> -m "注解"       如果你达到一个重要的阶段，并希望永远记住那个特别的提交快照，你可以使用 git tag 给它打上标签。
	    $ git tag 命令可以查看所有的标签

> **版本回退**  

		本地代码回退
	    $ git reset --soft <commitcode>    仅本地仓库回退到某个版本，工作目录和暂存区不变
	    $ git checkout HEAD .    会用 HEAD 指向的 master 分支中的全部文件替换暂存区和以及工作区中的文件
	    $ git checkout HEAD <file>    用 HEAD 指向的 master 分支中的指定文件替换暂存区和以及工作区中的文件
	    $ git checkout <file>    会用暂存区的指定文件替换工作区的指定文件
	
	    本地和远程代码都回退
	    $ git reset --hard <commitcode>    彻底回退到某个版本(工作区+本地仓库+远程仓库)
	    $ git push origin master -f        强制推送
	
	    $ git reset HEAD    暂存区的目录树被重写，被 master 分支指向的目录树所替换，工作区不受影响
	    $ git reset HEAD <file>        暂存区指定目录被重写
	
	    $ git rm <file>    文件从暂存区和工作区中删除
	    $ git rm --cached    文件从暂存区中删除，工作区会保留
	
	    $ git revert 此次操作之前和之后的commit和history都会保留，并且把这次撤销
	
> **Git分支管理**  

	    如果你对分支在本地是如何存储感兴趣的话，看看工作区文件夹内的以下文件：
		    .git/refs/head/     [本地分支]
		    .git/refs/remotes/     [正在跟踪的分支]
	
	    $ git branch <branchname>    创建分支
	    $ git push origin <branchname>  推送分支
	
	    $ git checkout <branchname>    工作区切换到当前分支
	    $ git checkout -b <branchname>    创建新分支并立即切换到该分支
	
	    $ git branch    查看本地分支
	    $ git branch -d <branchname>    删除本地分支
	
	    $ git branch -r    查看远程分支
	    $ git branch -r -d origin/branchname    删除远程分支
	    $ git push origin :branchname    删除结果推送到github
	
	    $ git fetch origin <branchname>    同步远程分支到本地仓库，所取回的更新，在本地主机上要用”远程主机名/分支名”的形式读取
	    $ git merge <branchname>    将任何分支合并到当前分支中去。冲突解决后，执行 git add 要告诉 Git 文件冲突已经解决。

### 4. 个人总结
