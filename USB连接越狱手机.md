##如何越狱
自己从网上淘了一个手机，系统是10.3.2 只能是不完美越狱，越狱步骤简单分成两步
1. 在自己的mac电脑上下载[doubleH3lix-RC8.ipa](https://doubleh3lix.tihmstar.net)并保存
2.下载[Cydia Impactor](http://www.cydiaimpactor.com/)当前的版本是Impactor_0.9.51，将其安装在mac上。
3. 数据线连上自己的越狱手机，打开Impactor,会弹出如下弹窗
![impactor](/Users/richard/Documents/files/impactor.png)

4. 找到第1步下载的doubleH3lix-RC8.ipa安装包，单指按住拖入弹窗中间会，会出现另一个界面
![appID](/Users/richard/Documents/files/your_appId.png)

__注意__:
这个appid必须去掉双重验证，否则是无法通过Impactor安装ipa到越狱手机上的，安装过程中弹出的错误不用管。
5. 安装完成越狱手机桌面会发现一个叫doubleH3lix,点击启动App，然后点击"run uicache"按钮，等待手机越狱，重启后就越狱成功了。
错误参看文献：<https://yalujailbreak.net/http-osx-cpp-131-error/>
6.出现了诡异的错误，试试重启电脑，手机，断开数据线多试几次，经验之谈，因为出现了太多问题，有时候越狱后通过手机的终端，ls命令都不能用了，必须重新越狱
7.不完美越狱后会出现很多异常，ls命令不能用之后我登录了[icloud](https://www.icloud.com)选择了抹除此iphone,然后重复上面的步骤，再次登录手机，ls命令可以用了，但是Cydia出问题了，每次启动cydia都会出现这个界面，也无法在越狱去下载，不过本身是为了学习。所以这个也就可有可无了，先凑合用吧。

##10.3.2的各种越狱异常
###DPKG_LOCKED错误
10.3.2的系统上面这个错误，在已安装软件后就会出现令人头疼DPKG_LOCKED错误 
![cydia](/Users/richard/Documents/files/your_appId.png)
解决办法：
注册Dropbox帐号下载lib.zip并解压,也可以从我的[网盘](https://pan.baidu.com/s/19NbDtPlWWBMCruc7hdsvzg)下载
打开ifunbox，将上一步解压后的lib文件夹拖入Books文件夹中
打开手机终端，输入命令
```
su
```
输入密码alpine
然后用终端将刚刚的lib文件夹拷贝到/var/目录下：
```
cp -R /var/moblie/Media/Books/lib /var
```
参考文档:<https://www.jianshu.com/p/e18d44152dc3>
 
###没有scp命令
1. 通过ifudBox将scp拷贝到/var/mobile/Media/Books/scp目录
通过cp命令：
 cp -R /var/mobile/Media/Books/scp  /usr/lib
复制到/usr/lib目录下就可以 使用scp
scp命令的包和上面的lib包都在我的[网盘](https://pan.baidu.com/s/19NbDtPlWWBMCruc7hdsvzg)

2. 使用scp拷贝远程文件
使用scp要ssh用登录，USB登录暂时不知道怎么怎么搞
输入
```
scp -r root@192.168.16.219:/var/root/test.txt ~/Desktop/GO
```
会弹出下面这句先验证登录
```
root@192.168.16.219's password:
```
然后输入密码alpine，就会将文件test.txt从手机复制到Mac的 ~/Desktop/GO的目录下面
其中192.168.16.219是越狱手机的IP
/var/root/test.txt是路径，必须是完整路径

###Cydia无法连接网络
在解决完DPKG_LOCKED后又出现了一个新问题，就是Cydia连不上网了，我的手机卸载cydia，然后发现又重装不上，找了很久的解决方案，后来查证发现因为我的系统是10.3.2，Cydia可以正常打开但是无法连接到网络，是因为系统没有打开Cydia的联网权限

1. 用windows电脑下载电脑版PP助手或者爱思助手(MAC上是没有对应dmg)，然后通过助手下载乐网app安装到自己的越狱手机（红色图标的）
2. 打开安装好的乐网开启全局拦截，然后你会发现电池栏会出现vpn，这时候Cydia还是不能联网的，因为乐网app安装完都不能弹出授权联网请求
3. 重点！很多老的教程都忽略了这点，进入设置->通用->还原->还原网络设置，然后手机会还原网络重启。启动后打开乐网app开启全局拦截，然后**再次进行越狱(重要的话就讲一遍！)**,越狱成功后，手机启动会直接加载已经开启的VPN。再次进入cydia就能直接加载。
4. 更加重点！(见证奇迹的时刻),打开Cydia后点重新加载可以加载首页了，然后就添加雷锋源(http://apt.abccydia.com/), 我顺便还添加了一个威锋源(http://apt.so/), 搜索下载ConditionWIFI2限制插件，安装好
5. 打开Cydia网络权限
 * 设置->无线局域网 ->打开对Cydia的网络请求开关(就是打开那个绿色小开关)，就打开局域网的联网权限
 * 设置->无线局域网 ->使用无线局域网与蜂窝移动的应用->Cydia->选中"无线局域网与蜂窝移动数据"，就打开了蜂窝移动网络联网权限。
 *  以上两步操作好了，以后再4G和WIFI下Cydia就都可以连上网络了。
 
  
参考文章:[ios10.3.3越狱无法连接到cydia？一招解决](https://baijiahao.baidu.com/s?id=1590073886388913266&wfr=spider&for=pc&isFailFlag=1)

##电脑终端连接越狱手机

##SSH连接
ssh root@192.168.16.219，第一次登陆会让你确认，直接输入yes，然后会让你输入密码
直接输入alpine
就可以通过ssh无线连接手机了，虽然直接可以比较方便的使用scp，但是不太稳定，稳定的连接方式是用USB。

###USB连接
首先通过如下命令安装libmobiledevice,然后使用工具里提供的iproxy把本地的端口2222映射到设备的TCP端口22，这样就可以通过本地的2222端口建立连接了。
```
brew install libmobiledevice
iproxy 2222  22
ssh root@localhost -p 2222
```

每次通过命令来进行端口转换比较麻烦，可以把这个端口转换命令写到开机启动项加载中。创建文件~/Library/LaunchAgents/com.usbmux.iproxy.plist,然后将一下内容写入改文件，然后运行命令lauchct1 load ~/Library/LaunchAgents/com.usbmux.plist。
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>Label</key>
	<string>com.usbmux.iproxy</string>
	<key>ProgramArguments</key>
	<array>
	  <string>/usr/local/bin/iproxy</string>
	  <string>2222</string>
	  <string>22</string>
	</array>
	<key>RunAtLoad</key>
	<true/>
	<key>KeepAlive</key>
	<true/>
</dict>
</plist>

```

这样每次在终端输入
ssh root@localhost -p 2222命令就可以建立连接了。但是仍然觉得很麻烦怎么办？别慌，我们可以使用终极大招。给这条命令指定一个名字，然后进行SSH连接。打开~/.ssh.config文件（如果没有新建一个），写入以下内容。完成这些操作以后。每次直接终端输入ssh iP6命令就可以登录了
```
Host iP6
Hostname localhost
User root
Port 2222
```

通过uname -a可以看出iOS是基于Darwin Kernel的，而Darwin Kenel是一种UNIX-like操作系统,通过XCode的window->Devices and Simulators
![installed_apps](/Users/richard/Desktop/files/installed_apps.png)
点击下面的"+"号可以安装应用、"-"按钮删除应用,通过旁边的设置按钮可以浏览、下载和替换应用沙河文件（浏览需要多等待一会儿才能出现数据目录）

```
uname -a
Darwin Richardde-iPhone 16.6.0 Darwin Kernel Version 16.6.0: Mon Apr 17 17:33:35 PDT 2017; root:xnu-3789.60.24~24/MarijuanARM64_T7000 iPhone7,2
```

##越狱必工具
准备好越狱手机后，很多工具都是必须要安装的。除了前面提到的Apple File Condiut 2 和 Cydia Substrate。
1. adv-cmds：手机上经常会使用ps命令来查看当前运行的进程ID及可执行的文件的路径，初次使用可能会出现"-sh：ps:command not found"这样的错误，这是因为没有安装adv-comds。adv-cmds会提供ps命令
2. appsync：直接修改一个应用的结构或者文件会破解应用本身的签名信息，所以安装修改后的应用会出出现"Failed to verify code of XXX"这样的错误。这时候需要安装appsync，让系统不再验证应用的签名。在安装时要选择能够支持当前的系统的版本。
3. iFile：iFile 是手机上的文件管理器，用于管理手机中的文件、修改文件的权限。
4. scp：对于iOS以后的版本，使用yalu越狱后就没有scp这个工具。从网盘中下载到scp，使用iFunBox拷贝到/usr/bin目录，然后登陆越狱手机设备，执行如下命令：
```
cd /usr/bin/
ldid -S scp
chomd 777 scp
```
或者在Cydia中搜索并下载rsync来代替scp，方法如下
```
rsync -avze 'ssh -p 2222' root@localhost:/tmp/tmpfile
rsync -avze 'ssh -p 2222' ./utils.cy root@localhost:var/root/
```
5. Cycript:在cydia下载

##查看网易云
首先分清楚两个目录
应用的可执行文件目录（app在磁盘存放的目录）：/var/containers/Bundle/Application/AC015766-DD2A-425A-8D16-154092572CB6/neteasemusic.app/neteasemusic

应用的沙河用户目录：/var/mobile/Containers/Data/Application/1B85CC81-74F3-4866-8B88-EFA74AD4DF57
经过仔细查找发现下载音乐的存储目录是：
/var/mobile/Containers/Data/Application/1B85CC81-74F3-4866-8B88-EFA74AD4DF57/Documents/UserData/Download/done

获取BundleId的命令和编写的代码分别如下：
```
//输入
cat /var/containers/Bundle/Application/5F03DE15-ED4C-4C2A-9F0E-35226DED510F/QQMusic.app/Info.plist | grep CFBundleIdentifier -A 1
//输出：
    <key>CFBundleIdentifier</key>
    <string>com.tencent.QQMusic</string>
```

这次打开的是QQ音乐获取应用的沙河目录，这里获取的沙河目录
/var/mobile/Containers/Data/Application/91A0CEBF-8C91-4900-A74B-C92952C35261/Documents

将dylib复制到Documents目录下
1.先dumpdecrypted.dylib通过签名，cd到dumpdecrypted.dylib所在目录下
ldid -S dumpdecrypted.dylib
2.使用scp命令把生成的dumpdecrypted.dylib文件复制到Documents目录下，在电脑终端执行，具体如下。
```
scp -p 2222 ~/Documents/Jailbreak/dumpdecrypted.dylib root@192.168.199.106:/var/mobile/Containers/Data/Application/91A0CEBF-8C91-4900-A74B-C92952C35261/Documents
```
解密，登录到手机终端，执行如下命令
```
DYLD_INSERT_LIBRARIES=dumpdecrypted.dylib /var/containers/Bundle/Application/5F03DE15-ED4C-4C2A-9F0E-35226DED510F/QQMusic.app/QQMusic

Class BeaconNetFlowRecorder is implemented in both /var/containers/Bundle/Application/5F03DE15-ED4C-4C2A-9F0E-35226DED510F/QQMusic.app/QQMusic (0x103ceb358) and /var/containers/Bundle/Application/5F03DE15-ED4C-4C2A-9F0E-35226DED510F/QQMusic.app/QQMusic (0x103ceb358). One of the two will be used. Which one is undefined.
mach-o decryption dumper

DISCLAIMER: This tool is only meant for security research purposes, not for application crackers.

[+] detected 64bit ARM binary in memory.
[+] offset to cryptid found: @0x10007ce28(from 0x10007c000) = e28
[+] Found encrypted data at address 00004000 of length 54280192 bytes - type 1.
[+] Opening /private/var/containers/Bundle/Application/5F03DE15-ED4C-4C2A-9F0E-35226DED510F/QQMusic.app/QQMusic for reading.
[+] Reading header
[+] Detecting header type
[+] Executable is a plain MACH-O image
[+] Opening QQMusic.decrypted for writing.
[+] Copying the not encrypted start of the file
[+] Dumping the decrypted data into the file
[+] Copying the not encrypted remainder of the file
[+] Setting the LC_ENCRYPTION_INFO->cryptid to 0 at offset e28
[+] Closing original file
[+] Closing dump file

```
解密之后会在当前目录下面生成TargetApp.decryted文件。赶紧的把榻复制到Mac中备用吧，如果将动态库复制一个非沙河目录下，例如~/Desktop/,运行后就会报如下错误。
```
scp -P 2222 root@localhost:/var/mobile/Containers/Data/Application/91A0CEBF-8C91-4900-A74B-C92952C35261/Documents/QQMusic.decrypted ~/Desktop
```

然后，使用如下命令查看加密标识。
otool -l TargetApp.decrypted | grep crypt

```
otool -l QQMusic.decrypted | grep crypt                   
QQMusic.decrypted:
     cryptoff 16384
    cryptsize 54280192
      cryptid 0
```   
  https://www.jianshu.com/p/cb2c2c65bd0f?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation
https://www.jianshu.com/p/57115e8cf06e


