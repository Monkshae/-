
###1.git的简单使用流程
git分为工作区（working directory）、暂存区（index或者stage）、本地仓库 （local）

1. 新建一个gitDemo文件夹,mkdir gitDemo

2. 然后在当前目录下输入命令，执行
git init 即是建立了一个本地仓库;

3. 提交变并写上注释
```
git add . 文件加入暂存区
git status  查看暂存区的文件状态
git commit -am 'first commit'， //提交变更到本地
git status可以查看当前工作局状态，提交成功或者是失败;
git show  #显示某次commit提交的内容 git show $id 
```
4. 本地创建一个新分支develop
git checkout -b develop

5. 关联远程分支
git remote add origin git@git.gengmei.cc:gengmeiios/gengmeiios.git
如果是老项目clone下来的文件夹中，想要重新绑定url，使用
git remote set-ul origin git@git.gengmei.com:gengmeiios/gengmeiios.git
在push之前，加上这一句，我们创建的新仓库就与新项目建立了连接

6. 本地分支和远程分支关联
git branch --set-upstream-to=origin/master master

7. 拉取远程代码
git pull --rebase

8. 本地代码push到远程
git push

9. 查看你当前项目远程连接的是哪个仓库地址 
git remote -v

10. 如果是要clone的仓库，团队协作的时候，创建了一个origin，两个人分别clone 
分别做完全不同的提交 
第一个人git push成功 
第二个人在执行git pull的时候,提示 
fatal: refusing to merge unrelated histories 
解决方法: 
git pull --allow-unrelated-histories 

__注意__
```
本地创建一个分支后，必须要做远程分支关联，如果没有关联，git会在下面的操作中提示你显示的添加关联。关联的目的是如果在本地分支下操作：
如使用git pull, git push不需要指定在命令行指定远程分支 
关联远程分支 
git branch --set-upstream my_branch origin/my_branch 
该语法等价与在第一次提交分支时
git push --set-upstream  origin my_branch 
```

###2.gitignore文件
vi .gitignore  这是一个隐藏文件，打开文本后写入不想提交的问价，然后commit这个文件 

###3.git diff
```
git diff <file> # 比较当前文件和暂存区文件差异 git diff 
git diff <$id1> <$id2> # 比较两次提交之间的差异 
git diff <branch1>..<branch2> # 在两个分支之间比较 
git diff --staged # 比较暂存区和版本库差异 
git diff --cached # 比较暂存区和版本库差异 
git diff --stat # 仅仅比较统计信息 
diff SDKTypeDef.h SDKTypeDef1.h  #可以用来比较小的文件
```
###4.git log
```
git log -n 2 显示最新的2次提交 
git log -oneline 仅仅显示每次提交的message 
git log --stat 显示每次提交的change数量 
git log -p 显示文件修改细节 
git log --author="Richard" 查看当前分支下该menber的提交 
git log -n 2 --author="Richard" 显示该用户的最近的2次提交记录 
git log PhotoDataTestCase 显示该文件修改后的提交记录 
git checkout HEAD~1 //移动HEAD也可以查看记录 
git log --graph —all 显示git树 
```
git log可以查看提交历史，以便确定要回退到哪个版本。
git reflog查看命令历史，以便确定要回到未来的哪个版本。
git log --tags --simplify-by-decoration --pretty="format:%ai %d" | grep tag //看tag时间 

###git fetch
git fetch 实际上将本地仓库中的远程分支更新成了远程仓库相应分支最新的状态

git fetch 并不会改变你本地仓库的状态。它不会更新你的master分支，也不会修改你磁盘上的文件。

理解这一点很重要，因为许多开发人员误以为执行了 git fetch 以后，他们本地仓库就与远程仓库同步了。它可能已经将进行这一操作所需的所有数据都下载了下来，但是并没有修改你本地的文件。

###git merge
git merge用来合并分支，为了push新变更到远程仓库，你要做的就是将远程仓库中最新变更合并到自己的分支上。
为什么在操作远程分支时很多人不喜欢用 merge 呢？分支合并除了mege 还可以用rebase,至于具体是用 rebase 还是 merge，并没有限制。

在开发社区里，有许多关于 merge 与 rebase 的讨论。以下是关于 rebase 的优缺点：
优点:
	•	Rebase 使你的提交树变得很干净, 所有的提交都在一条线上
缺点:
	•	Rebase 修改了提交树的历史
比如, 提交 C1 可以被 rebase 到 C3 之后。这看起来 C1 中的工作是在 C3 之后进行的，但实际上是在 C3 之前。
一些开发人员喜欢保留提交历史，因此更偏爱 merge。而其他人（比如我自己）可能更喜欢干净的提交树，于是偏爱rebase。仁者见仁，智者见智。 

###git pull
git pull 等于 git fetch + git merge
远程分支上工作会有大量的合并提交。使用 git rebase可以避免这些提交。
尽量避免快速合并，通过添加--no-ff参数避免快速合并,区别如下图所示
git pull --no-ff  //抓取远程仓库所有分支更新并合并到本地，不要快进合并 
git merge --no-ff #不要Fast-Foward合并，这样可以生成merge提交 

![git pull](/Users/richard/Desktop/files/ff.png)

####git pull --rebase 

这是变更基线一个非常有用的特殊用法就是。举个例子，假设你正在master分支的一个本地版本上工作，你已经向仓库提交了一小部分变更。与此同时，也有人向master分支提交了他一周的工作成果。当你尝试推送本地变更时，git提示你需要先运行一下 git pull ， 来解决冲突。作为一个称职的工程师，你运行了一下 git pull ，并且git自动生成了如下的提交信息。
Merge remote-tracking branch ‘origin/master’
尽管这不是什么大问题，也完全安全，但是不太有利于历史记录的聚合。
这种情况下，git pull --rebase 是一个不错的选择。
这个命令会迫使git将远程分支上的变更同步到本地，然后将尚未推送的提交重新应用到这个最新版本，就好象它们刚刚发生一样。这样就可以避免合并以及随之而来的丑陋的合并信息了。
在实际开发中这个命令十分有效。

###git rebase
rebase是另一种合并分支的方式，使用 git rebase 可以避免这些提交分支错综复杂。
另外交互式 rebase 指的是使用带参数 --interactive 的 rebase 命令, 简写为 -
git rebase -i HEAD~4

如果你在命令后增加了这个选项, Git 会打开一个 UI 界面并列出将要被复制到目标分支的备选提交记录，它还会显示每个提交记录的哈希值和提交说明，提交说明有助于你理解这个提交进行了哪些更改。
在实际使用时，所谓的 UI 窗口一般会在文本编辑器 —— 如 Vim —— 中打开一个文件

* 调整提交记录的顺序（通过鼠标拖放来完成）
* 删除你不想要的提交（通过切换 pick 的状态来完成，关闭就意味着你不想要这个提交记录）
*	合并提交

1. 如果你的改动还在工作区或者暂存区，也就是没有打commit到历史区，可以用下面的命令
	```
	git stash
	git pull --no-ff
	git stash apply
	git commit -am 'xxx'
	git push
	```
	如果你已经提交到历史区有了commitId
	只需要将本地commit回退到你认为回退的地方
	git reset --soft commitId
	这条命令是从历史区拿出更改放到暂存区，然后在继续上面的命令就可以了。

2. 很明显上面的操作很复杂，其实那该如何解决这个问题呢？很简单，你需要做的就是使你的工作基于最新的远程分支。不过最直接的方法就是通过 git pull --rebase。
   我们说过git pull 就是 fetch 和 merge 的简写，类似的 git pull --rebase 就是 fetch 和 rebase 的简写！
   
git rebase 如果遇到冲突先解决冲突,然后执行 git rebase --continue 


###git stash
git stash push -um 'xxx' 推荐使用，可以单独指定路径更改，放入暂存区, -u表示未被跟踪的文件 -a包含所有文件，如.gitignore...
g，并且将stash放入list栈中
git stash save后面跟文件着路径无效，并不能精确储藏指定路径下的修改文件，会将工作区的所有更改都放进暂存区

git stash pop stash@{id}命令会在执行后将对应的stash id 从stash list里删除
git stash apply stash@{id} 命令则会继续保存stash id

恢复改动
git stash list查看stash列表，从中选择你想要pop的stash，运行命令
git stash pop stash@{id}
或者 
git stash apply stash@{id} 即可
省略stash@{id}表示应用的是最近的一次的commit

查看指定stash的diff
git stash show
git stash show stash@{id}

删除stash
git stash drop <stash@{id}> 如果不加stash编号，默认的就是删除最新的，也就是编号为0的那个，加编号就是删除指定编号的stash。
git stash clear 是清除所有stash

>实际使用，一般我们会成对的使用
git stash 
git stash apply

###git commit --amend
假设提交之后，你意识到自己犯了一个拼写错误。你可以重新提交一次，并附上描述你的错误的提交信息。但是，还有一个更好的方法：
如果提交尚未推送到远程分支，那么按照下面步骤简单操作一下就可以了：
1.	修复你的拼写错误
2.	将修正过的文件暂存，通过git add some-fixed-file.js
3.	运行 git commit –amend 命令，将会把最近一次的变更追加到你最新的提交。同时也会给你一个编辑提交信息的机会。
4.	准备好之后，将干净的分支推送到远程分支。
![git commit amend](/Users/richard/Desktop/files/git-commit-amend.gif)
###git reset

####git中的文件状态：

1.	没有暂存
2.	暂存并准备提交
3.	已经提交

```
在Git中，用HEAD表示当前版本，也就是最新的提交commit id,上一个版本就是HEAD^，上上一个版本就是HEAD^^,当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id

使用相对引用最多的就是移动分支。选项让分支指向另一个提交
git branch -f myBranch HEAD~3
上面的命令会将 master 分支强制指向 HEAD 的第3级父提交
如果myBranch是不存在的分支则会以当前分支的HEAD，创建一个叫myBranch，再强制将myBranch分支移动到指定的节点上

```
####重置文件
在git中，有3种类型的重置。重置是让文件回到git历史中的一个特定版本。
1.	git reset –hard {{some-commit-hash}} #回退到一个特定的历史版本。丢弃这次提交之后的所有变更。
2.	git reset {{some-commit-hash}} #回滚到一个特定的历史版本。将这个版本之后的所有变更移动到“未暂存”的阶段。这也就意味着你需要运行 git add . 和 git commit 才能把这些变更提交到仓库.
3.	git reset –soft {{some-commit-hash}} #回滚到一个特定的历史版本。将这次提交之后所有的变更移动到暂存并准备提交阶段。意味着你只需要运行 git commit 就可以把这些变更提交到仓库。

###git revert

git revert 撤销 某次操作，此次操作之前和之后的commit和history都会保留，并且把这次撤销 作为一次最新的提交     * git revert HEAD                  撤销前一次 commit     * git revert HEAD^               撤销前前一次 commit     * git revert commit （比如：fa042ce57ebbe5bb9c8db709f719cec2c58ee7ff）撤销指定的版本，撤销也会作为一次提交进行保存。 git revert是提交一个新的版本，将需要revert的版本的内容再反向修改回去， 版本会递增，不影响之前提交的内容

git revert -n
如果打算撤销之前一次或者两次的提交，查看这些提交都做了哪些变更，哪些变更又有可能引发问题，这个命令非常方便。
通常，git revert 会自动将回退的文件提交到仓库，需要你写一个新的提交信息。-n 标志告诉git先别急着提交，因为我只是想看一眼罢了。




###删除分支

删除本地分支:

git branch -d/-D <name> 
对于没有被合并过的分支，使用通过git branch -D <name>强行删除

删除远程分支:
git push origin --delete <name> 
推送一个空分支到远程分支，其实就相当于删除远程分支： 
git push origin :<name> 


###删除tag

删除本地tag：
git tag -d <tagname>  //删除本地tab 

删除远程tag：
git push origin --delete tag <tagname> 
推送一个空tag到远程tag, 其实就相当于删除远程tag 
git push origin :refs/tags/<tagname>

例如:
git push origin :refs/tags/0.1.0 
git push origin --delete tag 0.1.0
两种语法作用完全相同。 

###容易混淆的两对命令

####git checkout file 和 git reset HEAD file区别
git checkout  readme.txt //丢弃工作区的修改
命令git checkout readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
一种是readme.txt自修改后还没有被放到暂存区，现在撤销修改就回到和版本库一模一样的状态；
一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
总之，就是让这个文件回到最近一次 __git commit或git add时的状态__

*git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本*
git reset HEAD readme.txt   //把暂存区的修改撤销掉,重新放回工作区
一般如果想讲本地的某个文件的改动，直接回滚到未改动之前，需要执行下面命令:
git reset HEAD file   
git checkout file 


####git revert 和 git reset的区别 
1. git revert是用一次新的commit来回滚之前的commit，git reset是直接删除指定的commit。  
2. 在回滚这一操作上看，效果差不多。但是在日后继续merge以前的老版本时有区别。因为git revert是用一次逆向的commit“中和”之前的提交，因此日后合并老的branch时，导致这部分改变不会再次出现，但是git reset是之间把某些commit在某个branch上删除，因而和老的branch再次merge时，这些被回滚的commit应该还会被引入。  
3. git reset 是把HEAD向后移动了一下，而git revert是HEAD继续前进，只是新的commit的内容和要revert的内容正好相反，能够抵消要被revert的内容。
4. git reset 很方便，但是这种“改写历史”的方法对大家一起使用的远程分支是无效的;为了撤销更改并分享给别人，我们需要使用 git revert

###小技巧
git config --global http.postBuffer 524288000   //调整postbuffer为500M,远程仓库文件过大不调整会clone失败
git rm --cached file  //当我们需要删除暂存区或分支上的文件, 但本地又需要使用, 只是不希望这个文件被版本控制, 可以使用

git remote prune --dry-run origin  //查看哪些会被彻底清理，但是不会执行清理 
git remote prune origin //执行彻底清理 

更改远程远程仓库地址url
git remote remove origin 
git remote add origin  <oldUrl>
上面两句和等价下面一句
git remote set-url origin <newUrl>

*我自己配置的alias*
```
[alias]
	co = checkout
	st = status
	sl = stash list
  	cm = commit -am
  	br = branch
	reh = reset --hard  HEAD
  	res = reset --soft HEAD
	last = log -1
  	pr = pull --rebase #更加安全的pull方式
  	mnf = merge --no-ff #非快速合并
	lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
  	reignore = git rm -r --cached . && git add .
  	whyignore = git check-ignore -v
	cpk = cherry-pick
	tlg = log --tags --simplify-by-decoration --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset'  #查看tag时间
	blg = "remote prune --dry-run origin" #查看哪些会被彻底清理，但是不会执行清理
	brm = "remote prune origin" #执行彻底清理
```
git小游戏:<http://pcottle.github.io/learnGitBranching>
参考文章:<http://blog.jobbole.com/96088/>










