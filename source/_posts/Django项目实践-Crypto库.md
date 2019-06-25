title: Django项目实践-Crypto库
---
背景：
	由于公司接口有加密解密，自定义接口脚本需要调取Crypto模块，之前用的是php调用python脚本实现的结构体，过于繁琐，打算搭建Django框架实现

## 问题一：

但是搭建后发现调用Crypto模块经常会报错，主要报错是：
from Crypto.Cipher import _AES
ImportError: DLL load failed: 找不到指定的模块。
这个问题困扰了我很久，原来代码目录
![在这里插入图片描述
](https://img-blog.csdnimg.cn/201903111457336.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)

1、一直认为引用这个模块使用的是目录调用，然后验证路径调用是否正确，怎么验证也没发现什么毛病
2、文件里引用当前目录的_AES,最开始以为是AES.py文件本身（单下划线私有变量）；后来知道.pyd文件实际上就是个库文件（DLL），才转移到是引用的_ASE.pyd，但是也没有找出找不到它的理由
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190311150008620.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)
解决：
后来尝试在原来PHP框架里删除该引用文件，发现依旧可用，于是明确一点：该实现并不依赖该路径，而是与系统安装的pycrypto路径文件有关，但是wondows本地一直有问题，于是直接从当前正在使用PHP linux服务器上调试。做法：删除本地目录（直接用系统的，经之前项目验证系统的没问题，可以用），然后启动Django服务就不报错了，后来又调试了下报错都是些其他的问题了，困扰我两周的问题终于终于落幕了，可以收下心做正事了~~

## 问题2：

虽然在linux上的这个问题莫名其妙的解决了，但是在台式机win7（py3.7）系统上还是有问题，使用django框架吊起来没有问题，但是直接执行python脚本还是会报一样的错误。忍了一段时间后终于忍不下去了。
最开始听人说在linux环境可行，于是在windows下打开git bash确实可行，但是还是会报其他的错，可能是因为版本换了，因为他走的是Anaconda里面的插件，后来干脆还是修复本地的吧
1、卸载本地的pycrypto 
卸载失败提示:Cannot uninstall ''. It is a distutils installed project and thus we cannot accurately.
解决：直接全局搜索对应文件，具体包括 "package name" 文件夹 和 "package name".egg-info ，找到后直接删除即可
2、重新安装pip install pycrypto 
直接安装报错，使用源码安装？一样报错
error: command 'C:\\Program Files (x86)\\Microsoft Visual Studio 14.0\\VC\\BIN\\
x86_amd64\\cl.exe' failed with exit status 2
解决：从网上找的经验
1）添加VC环境变量，设置用户环境变量，这里划重点！！！是用户环境变量，不是系统环境变量 
变量名：VCINSTALLDIR 
（变量值为vs安装路径下的VC，默认是这个） 
变量值：C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC 
 win+R运行cmd，执行命令set CL=/FI”%VCINSTALLDIR%\INCLUDE\stdint.h” %CL% 

2）安装twisted https://blog.csdn.net/jiangyunsheng147/article/details/80449556

还会报错warning: GMP or MPIR library not found; Not building Crypto.PublicKey._fastmath.
building 'Crypto.Random.OSRNG.winrandom' extension
error: Unable to find vcvarsall.bat

3）降低版本3.7-->3.6
还是报楼上错误，最后看到有pycrypto-2.6.1.win-amd64-py3.4.exe可行，但是需要3.4.于是降到3.4，刚开始用的exe事32位的不行，后来下了64的果然可以了，搞了一天的环境终于在最后搞定了，也不辜负这一天的工时。
插曲：在降低版本的时候，出现了明明已经降到3.4，但是python -V的时候一直仍然显示3.7，后来又是删注册表，又是卸载程序的，我怀疑跟我安装的Anaconda有关系。
