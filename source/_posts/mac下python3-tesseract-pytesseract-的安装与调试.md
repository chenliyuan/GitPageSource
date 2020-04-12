title: ' tesseract 的环境安装'
author: 躲不掉的风
date: 2020-03-21 12:15:51
tags:
---
###### windows 10 安装:
第一步在虚拟环境或者正式环境pip安装：

	 pip install pytesseract
 	 pip install pillow
第二步 安装Tesseract-OCR	

- 下载安装包官网：
注意版本之前由于使用3.0.2一直报错，后来换成5.0才好
	
	https://github.com/UB-Mannheim/tesseract/wiki
一路安装时可选择下语言包
- 系统环境Path变量配置：

	![upload successful](\images\pasted-106.png)

	https://www.cnblogs.com/chenshengkai/p/11318272.html

	验证： 
    	tesseract --version
- 在python文件下搜索pytesseract.py文件并修改路径：
tesseract_cmd = 'C:\\Program Files\\Tesseract-OCR\\tesseract.exe'

![upload successful](\images\pasted-104.png)

   参考
   https://www.cnblogs.com/chenshengkai/p/11318387.html
	https://blog.csdn.net/qq_38161040/article/details/90649497
    
   ```
  import pytesseract
  from PIL import Image

  image = Image.open("data/enzh.png")
  code = pytesseract.image_to_string(image,lang='chi_sim')
  print(code)
	 ```
###### mac安装
1. 		
    pip install pytesseract
    pip install tesseract  
   
	之后运行demo一直报这个错：

        NotADirectoryError: [Errno 20] Not a directory: 'tesseract'

 后来看到帖子说mac必须得用brew安装才行(救星)，又重新安装了一次，果然成功了
 		
     brew install tesseract
        
   参考： https://blog.csdn.net/weixin_38246633/article/details/82993678

2. 紧接着又报错：

        pytesseract.pytesseract.TesseractError: (1, 'Error opening data file /usr/local/share/tessdata/chi_sim.traineddata Please make sure the TESSDATA_PREFIX environment variable is set to your "tessdata" directory. Failed loading language \'chi_sim\' Tesseract couldn\'t load any languages! Could not initialize tesseract.')
   感谢文章： 
      https://blog.csdn.net/magicianofcodes/article/details/79401622

	即根据错误提示：

  1）.  需要设置环境变量    

          export TESSDATA_PREFIX=/usr/local/Cellar/tesseract/3.05.01/share/tessdata
          export PATH=$PATH:$TESSDATA_PREFIX

  2）. 下载语言包

    https://github.com/tesseract-ocr/tessdata

   下载相应缺少的语言包(这里是chi_sim)，然后放在/usr/local/Cellar/tesseract/3.05.01/share/tessdata之下
   
   至此，成功 ~