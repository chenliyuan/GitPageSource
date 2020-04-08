title: git使用篇
date: 2018-10-07 16:19:32
---
git命令基础操作这篇文章很清楚：https://www.cnblogs.com/tocy/p/git-command-line-manual.html
## 常见出错和问题

###### 1.  出现error: The following untracked working tree files would be overwritten by checkout
解决： $ git clean -d -fx ""
参考   http://blog.chinaunix.net/uid-23366077-id-3581375.html
###### 2. Git冲突：commit your changes or stash them before you can merge. 解决办法
出现这个问题的原因是其他人修改了xxx.php并提交到版本库中去了，而你本地也修改了xxx.php，这时候你进行git pull操作就好出现冲突了，解决方法，在上面的提示中也说的很明确了。
https://www.cnblogs.com/wenlj/p/5866356.html
1）直接commit本地的修改 ----也一般不用这种方法
2）通过git stash  ---- 通常用这种方法

	git stash
	git pull
	git stash pop
通过git stash将工作区恢复到上次提交的内容，同时备份本地所做的修改，之后就可以正常git pull了，git pull完成后，执行git stash pop将之前本地做的修改应用到当前工作区。

###### 3. github多人协作
1、创建组织--创建库，创建Team(邀请成员，设置每人owner权限，保证每人都可写)


参考：https://www.cnblogs.com/zhaoyanjun/p/5882784.html

###### 4.为GitLab帐号添加SSH keys并连接GitLab
https://blog.csdn.net/qq_21916259/article/details/81000723
密匙生成成功之后，复制 .ssh目录下的id_rsa.pub文件中的密匙，添加gitLab上后就可以使用git clone ...了
可以设置多个设备的秘钥，实现多设备共享。
###### 5. 删除内容
  ```
  $ git rm test.txt
  rm 'test.txt'

  $ git commit -m "remove test.txt"
  [master d46f35e] remove test.txt
   1 file changed, 1 deletion(-)
   delete mode 100644 test.txt
  ```
###### 6. git提交代码时，出现这个错误“error: The requested URL returned error: 403 Forbidden while accessing https”

   解决：编辑.git目录下的config文件即可。

  ```
  vim .git/config
  #修改对于的配置
  #原来的url = https://github.com/elitecodegroovy/PhoenixC.git

  url = https://elitecodegroovy@github.com/elitecodegroovy/PhoenixC.git
```
###### 7. git别人提交的时候冲突了，需要回退到之前某版本

   首先要git pull 否则直接按照以下的操作，最后还会提示pull

  1. 查找历史版本
       使用git log命令查看所有的历史版本，获取你git的某个历史版本的id

       假设查到历史版本的id是fae6966548e3ae76cfa7f38a461c438cf75ba965。

  2. 恢复到历史版本
        $ git reset --hard fae6966548e3ae76cfa7f38a461c438cf75ba965
  3. 把修改推到远程服务器
           $ git push -f -u origin master  
  出错：
```
remote: GitLab: You are not allowed to force push code to a protected branch on this project.
To git.17usoft.com:app_test/doc.git
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'git@git.17usoft.com:app_test/doc.git'
```
原因：一般默认会开启保护模式，保护模式下不允许任何人push
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019041712030931.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1cGVyX2NoZW5seQ==,size_16,color_FFFFFF,t_70)
解决：去掉保护模式
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190417120340626.png)

###### 8. [GIT] warning: LF will be replaced by CRLF问题解决方法

```
$ git config --gobal core.autocrlf false
```
###### 9. 由于某配置项错误配置导致push失败，解决：删除配置项目

    1) git config --list 或者git config -l查看配置项
    2) git  config --global unset +配置项全名
  
###### 10. 、git 分支拉取
   https://blog.51cto.com/13893093/2175764?source=dra
1. 查看本地分支
   $ git branch
2. 查看远程分支
$ git branch -r
3. 查看所有分支
$ git branch -a
4. 拉取远程分支
  新建一个空文件
初始化 git init
自己要与origin master建立连接（下划线远程仓库链接）
git remote add origin http://192.168.9.10:8888/root/game-of-life.git
把远程分支拉到本地（game-of-live-first_branch为远程仓库的分支名）
git fetch origin game-of-live-first_branch
在本地创建分支game-of-live-first_branch并切换到该分支
git checkout -b game-of-live-first_branch origin/game-of-live-first_branch
把game-of-live-first_branch远程分支上的内容都拉取到本地
git pull origin game-of-live-first_branch

###### 11.  You are in 'detached HEAD' state. 
You can look around, make experimental changes and commit them, and you can discard any commits you make in this state without impacting any branches by performing another checkout.

解决：git checkout master
```
LiyuandeMacBook-Pro:scqa_ui_auto_platform_projects liyuanchen$ git checkout master
Already on 'master'
Your branch is up to date with 'origin/master'.
```

