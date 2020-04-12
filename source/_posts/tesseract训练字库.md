title: tesseract训练字库
author: 躲不掉的风
date: 2020-04-12 13:50:29
tags:
---

#### 准备条件：
1. tesseract-OCR   exe  我使用的是c5.0
2. jdk
3. jTessBoxEditor 

#### 步骤
1. 单张图片可直接使用如下命令生成box文件：
tesseract myfontlab.normal.exp01.png myfontlab.normal.exp01 -l eng+chi_sim  batch.nochop makebox

  【语法】：tesseract [lang].[fontname].exp[num].tif [lang].[fontname].exp[num] batch.nochop makebox  
  【语法】：
  lang为语言名称，为了不影响本有的语言eng，chi_sim等等，取成别的,比如数字可以是num,中文myfontlab myeng等
  fontname为你对这门语言某一字体，你可以填任何你记得住的比如 trumpsb，normal;
  num为序号；
  在tesseract中，一定要注意格式

  多张图片，需先使用jTessBoxEditor 的train.bat 合并所有图片为一个tif文件然后使用命令生成box文件
  
  tesseract myfontlab.normal.exp01.tif myfontlab.normal.exp01 -l eng+chi_sim  batch.nochop makebox

2. 用jTessBoxEditor 的train.bat打开tif文件，然后根据实际情况修改box文件，去除多余空格，修改识别文字。
3. 产生字符特征文件，生成.tr文件。命令
单张推荐：
tesseract  myfontlab.normal.exp01.png  myfontlab.normal.exp01  nobatch box.train
多张推荐：
tesseract myfontlab.normal.exp01.tif myfontlab.normal.exp0 nobatch box.train  

4. 生成字符集文件，生产unicharset文件。命令
unicharset_extractor myfontlab.normal.exp01.box
5. 新建一个font_properties文件。注意，文件的名字就是font_properties，它没有.txt后缀。
里面内容写入 normal 0 0 0 0 0 表示默认普通字体
6. 生成shape文件，生成五个文件
```
  shapeclustering -F font_properties -U unicharset myfontlab.normal.exp01.tr

  mftraining -F font_properties -U unicharset -O unicharset myfontlab.normal.exp01.tr
  cntraining myfontlab.normal.exp01.tr
```
7. 在这五个文件前加上normal.
8. 合并五个文件，执行如下命令，会生成目标文件normal.traineddata，
combine_tessdata normal.
9. 该文件就是训练好的字库。将它复制到你安装的Tesseract程序目录下的“tessdata”目录下即可。
10. 验证：tesseract myfontlab.normal.exp01.png out –l normal  查看生成的out.txt文件



#### 总结
1. 中英文一块识别率很低？使用+连接
如： tesseract 05.png 05 -l chi_sim+eng
eng+chi_sim效果更好？试了一个案例验证没区别，不知道大数据是不是真的。
2. 最后有一个识别失败的案例，比如A竟然没有识别出来(box调试的时候已经识别出来了)
原因是Box里面A的范围不准确，有两个框，把最外面的删掉，剩余最内部分框框就好了。
3.  Other case a of A is not in unicharset
事实证明这个报错对识别结果未有影响
4. 语法说明：

tesseract imagename outputbase [-l lang] [-psm pagesegmode] [configfile...]

tesseract 图片名 输出文件名 -l 字库文件 -psm pagesegmode 配置文件
例如：
tesseract test.jpg result -l eng -psm 7 nobatch

-l eng 表示用英文文字库（默认使用英文。如需要下载中文字库文件，解压后，存放到tessdata目录下去,字库文件扩展名为 .raineddata 简体中文字库文件名为: chi_sim.traineddata，命令为：chi_sim）

-psm 7 表示告诉tesseract test.jpg图片是一行文本 这个参数可以减少识别错误率. 默认为 3

configfile 参数值为tessdata\configs 和 tessdata\tessconfigs 目录下的文件名.

————————————————
版权声明：本文为CSDN博主「_Cassie」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/wangyongxia921/article/details/52809079


参考

https://blog.csdn.net/qq_31112205/article/details/100159963

https://www.cnblogs.com/wj-1314/p/9454656.html

https://www.cnblogs.com/pyweb/p/11457519.html  多样本