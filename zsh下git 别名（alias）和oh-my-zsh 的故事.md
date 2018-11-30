#zsh下git 别名（alias）和oh-my-zsh 的故事

##命令设置别名

不同的 shell, alias 配置文件是不同的，你执行 alias显示的也会不同。比如 Mac 默认的 bash，其配置文件可能是 /etc/bashrc、~/bash_profile 或者/etc/profile;zsh 默认是 ~/.zshrc。

alias设置别名，有临时别名和全局别名两种方式，命令如下:
```
//设置临时别名,如果你重启了终端就不生效了
alias la="ls -al" 
//设置全局别名，重启后终端还生效，本质是写入到了配置文件中，zsh下，写入了~/.gitconfig中
git config --global alias.st status
```
##修改配置文件设置别名
###3.git配置
设置用户名和密码，只需要设置一次 
```
git config —list 检查你的配置 
git config user.name  //获取用户名 
git config use.name richard //将用户名设置为richard
git config user.email //获取用户邮件 
git config user.email richard@163.com//将用户邮件设置成richard@163.com 
git  help   config 或者 git  config —help  查看帮助方式
```
###配置文件中的alias
因为用的是zsh，就在~/.zshrc配置文件下
alias st = "status"
然后退出vi，保存，其效果和命令行
git config --global alias.st status是一样的
这里是默认写入~/.zshrc,那如果要写进其他的配置文件，比如/etc/profile呢

* ./etc/gitconfig 文件

	包含了适用于系统所有用户和所有库的值。如果你传递参数选项'--system' 给 git config，它将明确的读和写这个文件，例如我们设置用户名和邮箱。

		git config --system user.name "xxxx"  
		git config --system user.email "xxxxxxxx@163.com"
	
* ~/.gitconfig文件

	具体到你的特定用户。你可以通过传递--global 选项使 Git 读或写这个特定的文件。
	
		git config --global user.name "xxxx"  
		git config --global user.email "xxxxxxxx@163.com"
* .git/config
	位于 git 目录的 config 文件 (也就是 .git/config) ：无论你当前在用的库是什么，特定指向该单一的库。每个级别重写前一个级别的值。因此，在.git/config中的值覆盖了在/etc/gitconfig中的同一个值。

>因此在配置别名的时候，如果指定 --system ，将会对所有的用户生效。  指定 --global 的时候，会对当前用户生效。 没有指定--system 或者--global 的时候，只在当前仓库生效
	
###直接修改文件设置别名可能会报错
例如：
alias gst='git status'
alias gcm='git commit -m '
结果在执行`gcm` 的时候,运行不正确。用alias gcm查看该别名设置：
alias gcm
gcm='git checkout master'
原来是配置成了其他命令。
但是奇怪的是，在.zshrc 文件中并没有找到相关配置。经过仔细阅读，最终发现了下面这段内容
```
# Which plugins would you like to load? (plugins can be found in ~/.oh-my-zsh/plugins/*)
# Custom plugins may be added to ~/.oh-my-zsh/custom/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(git)
```
恍然大悟因为我用的是oh-my-zsh，那我们按照注释说明，到那个插件下看一下吧。
```
cd ~/.oh-my-zsh/plugins/git
ls
README.md      git.plugin.zsh
```
可以看到一个 git.plugin.zsh 文件，vi 打开，看到一些 functions 和 N 多的 alias。如果你有兴趣，终端下执行一下 alias 就列出所有可用别名,你会看到有些小惊喜~，简单列举一下
```
-='cd -'
...=../..
....=../../..
.....=../../../..
......=../../../../..
1='cd -'
2='cd -2'
3='cd -3'
4='cd -4'
5='cd -5'
6='cd -6'
7='cd -7'
8='cd -8'
9='cd -9'
_=sudo
afind='ack -il'
d='dirs -v | head -10'
g=git
ga='git add'
gaa='git add --all'
gap='git apply'
gapa='git add --patch'
gau='git add --update'
gav='git add --verbose'
gb='git branch'
gbD='git branch -D'
gba='git branch -a'
gbd='git branch -d'
```
如果你没有安装 zsh 和 oh-my-zsh，个人觉得这些别名的设置还是可以借鉴一下的，毕竟有些alias我们默认用了很多年的。

###其他bash下如果处理alias
mac下默认的终端是bash shell，配置文件是.bash_profile，如果没有.bash_profile文件，那就直接创建一个.bash_profile，然后进行编辑。

当你配置了zsh之后，打开新的终端不会按照bash的方式走 .bash_profile，source ~/.bash_aliases没有执行，因此发现就没有起作用。而会走了.zshrc文件。可以直接在.zshr当中可以设置alias。

如果想让两个shell终端下都生效
在.zshrc文件的开头添加这样一行语句即可
 ```
 test -f ~/.bash_profile  && source ~/.bash_profile
 ```

更多请参考:
<http://www.cnblogs.com/cspku/articles/Git_cmds.html>


