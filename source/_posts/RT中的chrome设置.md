title: RT中的chrome设置
author: 躲不掉的风
date: 2020-02-02 17:09:21
tags:
---
##### jenkins在执行远程docker chrome hub运行RT时候报错：
	
	selenium.common.exceptions.WebDriverException: Message: unknown error: session deleted because of page crash
	from unknown error: cannot determine loading status
	from tab crashed

一个解决办法就是要添加chrome参数Add the following chrome_options:

	chrome_options.add_argument('--no-sandbox')   
参考：https://stackoverflow.com/questions/53902507/unknown-error-session-deleted-because-of-page-crash-from-unknown-error-cannot
    
###### 那么这些参数都有哪些意义呢？
```
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
chrome_options = Options()
chrome_options.add_argument('--no-sandbox')#解决DevToolsActivePort文件不存在的报错
chrome_options.add_argument('window-size=1920x3000') #指定浏览器分辨率
chrome_options.add_argument('--disable-gpu') #谷歌文档提到需要加上这个属性来规避bug
chrome_options.add_argument('--hide-scrollbars') #隐藏滚动条, 应对一些特殊页面
chrome_options.add_argument('blink-settings=imagesEnabled=false') #不加载图片, 提升速度
chrome_options.add_argument('--headless') #浏览器不提供可视化页面. linux下如果系统不支持可视化不加这条会启动失败
chrome_options.binary_location = r"C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" #手动指定使用的浏览器位置

driver=webdriver.Chrome(chrome_options=chrome_options)
driver.get('https://www.baidu.com')

print('hao123' in driver.page_source)
driver.close() #切记关闭浏览器，回收资源
```
###### 如何在RT中应用？

RT中添加需要转化成capabilities这样才可在使用远程driver的时候应用。

参考大神:https://stackoverflow.com/questions/46812155/how-to-run-headless-remote-chrome-using-robot-framework/46817149
```
Headless Chrome - Open Browser
    ${chrome_options} =     Evaluate    sys.modules['selenium.webdriver'].ChromeOptions()    sys, selenium.webdriver
    Call Method    ${chrome_options}   add_argument    headless
    Call Method    ${chrome_options}   add_argument    disable-gpu
    ${options}=     Call Method     ${chrome_options}    to_capabilities     

    Open Browser    http://cnn.com    browser=chrome    remote_url=http://localhost:4444/wd/hub     desired_capabilities=${options}

    Maximize Browser Window
    Capture Page Screenshot
```