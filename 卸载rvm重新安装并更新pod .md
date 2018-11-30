
卸载rvm重新安装并更新pod

rvm  implode
另外为了保险起见最好执行下
gem uninstall rvm
还有目录需要自己手动删除
cd ~
rm -rf .rvm/ 
rm -rf /etc/rvmrc
最后还需要去bash下注释环境变量
 #[[-s "$HOME/.rvm/scripts/rvm"]] && . "$HOME/.rvm/scripts/rvm" # Load RVM function
  
我安装了zsh所的分别手动去下面三个配置文件检查
vi .bashrc
vi .bash_profile
vi .zshrc
 
rvm list known 查看已知的ruby版本
rvm install 2.5.1 安装2.5.1版本
ruby -v  查看当前安装版本
gem -v   查看gem版本
gem update --system 2.7.6 安装gem2.7.6版本
gem sources -l 查看当前gem源
gem install cocoapods -v 1.4.0 安装cocoapods 1.4.0

更新pod版本
gem install cocoapods -v 0.38.0 
 
 



