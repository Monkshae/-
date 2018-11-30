###1. 配置文件加载顺序

OS X系统的配置文件，加载顺序为：

```
/etc/profile
/etc/paths
~/.bash_profile
~/.bash_login
~/.bashrc
```
`/etc/profile`和`etc/paths`是系统级别的，系统启动就会加载，后面几个是当前用户的环境变量。

`~/.bash_profile`，`~/.bash_login`，`~/.profile`按照从前往后的顺序读取，
如果`~/.bash_profile`文件存在，则后面的几个文件就会被忽略不读了，
如果`~/.bash_profile`文件不存在，才会以此类推读取后面的文件。

`~/.bashrc`没有上述规则，它是bash shell打开的时候载入的。

###2. PATH变量
设置PATH的语法为：
```
export PATH="$PATH:<PATH 1>:<PATH 2>:<PATH 3>:...:<PATH N>"
```
####注：

（1）环境变量更改后，重启后才可生效。如果想立刻生效，则可执行下面的语句：

```
source 相应的配置文件
```
（2）如果默认shell是bash，那么shell启动时会触发`.bashrc`，
如果默认shell是zsh，那么shell启动时会触发`.zshrc`

以下命令可以得到系统的默认shell，
```
echo $SHELL
/bin/zsh
```
（3）环境变量既可以加到`$PATH`头部，也可以加到`$PATH`尾部。
例如mac中自带`emacs`的位置在`/usr/local/emacs`
```
type emacs
emacs is /usr/local/emacs
```
如果我们想更改`emacs`的默认路径，就得将新路径加到`$PATH`前面。
（别忘了执行`source .zshrc`）
```
export PATH="/Applications/Emacs.app/Contents/MacOS:$PATH"
```
或者增加alias，
```
alias emacs="/Applications/Emacs.app/Contents/MacOS/Emacs"

```
否则，加载到`$PATH`后面，
`type emacs`仍然还是`emacs is /usr/local/emacs`

###3.zsh    
今天在Mac上试用了oh-my-zsh，发现效果果然不同凡响，颜色搭配也极为养眼。有兴趣的到GitHub上了解一下oh-my-zsh. https://github.com/robbyrussell/oh-my-zsh目前发现一个小问题是中文显示乱码，Google了一下，发现应该是zsh默认没有设为utf-8的原因，设置一下就没问题了 
cat ~/.zsh - >    
export LC_ALL=en_US.UTF-8       ① 
export LANG=en_US.UTF-8          ② 
补充：在切换到英文操作系统下，为了让git下的log显示中文，在终端下输入①或者②命令行都可以，但是最近xcode更新到6.1偶然用了下cocoapods，如果使用①，会有警告， 
WARNING: CocoaPods requires your terminal to be using UTF-8 encoding. 
See https://github.com/CocoaPods/guides.cocoapods.org/issues/26 for 
possible solutions. 
而且cocoapods安装不成功；但是如果使用②，则没有警告会安装成功 建议使用②

zsh源码安装地址https://github.com/robbyrussell/oh-my-zsh 并执行 
curl -L http://install.ohmyz.sh | sh即可安装zsh 

###4.终端下文件显示颜色 
先将该文件放到用户目录下（就是小房子那个目录） 
然后执行命令行 
mv 1.vimrc .vimrc 
ls -al 显示所有文件包括隐藏文件 
vi .vimrc 进入到最后一行 去掉set number前的“可以显示行号 

###5.切回bash的shell 
当我们需要返回到bash下的时候，就得卸载zsh 直接命令行  uninstall_oh_my_zsh 
如果发现卸载不成功 我们需要强制卸载解决冲突 原网址在github上，即https://github.com/bradtse/oh-my-zsh/blob/master/tools/uninstall.sh 
介绍卸载zsh详情博客http://hshao.diandian.com/post/2013-12-10/40060416499 

###6.查看是否安装了brew 
命令行 which brew ，如果没有安装http://www.jb51.net/os/MAC/101860.html 


 

