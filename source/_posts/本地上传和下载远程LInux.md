title: 本地上传和下载远程LInux
author: 躲不掉的风
date: 2020-01-28 12:41:23
tags:
---
上传命令：
```
scp path/filename userName@sseverName:path
```

下载：调换位置即可
```
scp userName@sseverName:path path/filename
```
我的执行命令

```
sudo scp /Users/liyuanchen/Documents/workspace/JenkinsTask.zip  xxxxqa@10.12.77.89:/home

```
【报错】
1. xxxxqa@10.12.77.89: Permission denied (publickey,password). 

  解决方法，在同账户下把A的公钥放到B上，注意你scp的账户，添加在该账户下。
  具体操作：
  A：cat .ssh/id_rsa.pub   
  B: cat .ssh/authorized_keys （没有该文件创建一个）   
	把A的公钥贴过来即可
    
2. scp: /home/setuptools/JenkinsTask.zip: Permission denied

  还是报错，按照网上的折腾了一番/etc/ssh/sshd/sshd_config中的PasswordAuthentication和AuthorizedKeysFile,放开了注释保证是开通状态；   
  还是不行，在命令行前加sudo。                  
  还是不行，直到更改目标文件夹到xxxxqa，因为原文件夹权限是root，使用xxxxqa用户当然不行啦。
  ```
  drwxr-xr-x  8 root     root     4096 1月  21 14:40 setuptools
  drwxr-xr-x 17 xxxxqa   xxxxqa   4096 1月  27 22:14 xxxxqa
```