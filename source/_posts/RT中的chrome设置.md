title: RT中的chrome设置
author: 躲不掉的风
tags: []
categories: []
date: 2020-02-02 17:09:00
---
##### 1.jenkins在执行远程docker chrome hub运行RT时候报错：

	selenium.common.exceptions.WebDriverException: Message: unknown error: session deleted because of page crash
	from unknown error: cannot determine loading status
	from tab crashed
    
-  解决方法一

一个解决办法就是要添加chrome参数Add the following chrome_options:

	chrome_options.add_argument('--no-sandbox')   
参考：https://stackoverflow.com/questions/53902507/unknown-error-session-deleted-because-of-page-crash-from-unknown-error-cannot
    
 ######   那么这些参数都有哪些意义呢？
```
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
chrome_options = Options()
chrome_options.add_argument('--no-sandbox')#停止使用沙箱模式（沙箱模式就是一个封闭的独立的环境。）,也可解决DevToolsActivePort文件不存在的报错
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


解决方法二：方法一并没有有效的解决问题，以下贴出完整的问题log

```
[ WARN ] Keyword 'Capture Page Screenshot' could not be run on failure: WebDriverException: Message: Session [0c86405952403c3719bb6d9e8467b7f9] was terminated due to BROWSER_TIMEOUT
Stacktrace:
    at org.openqa.grid.internal.ActiveTestSessions.getExistingSession (ActiveTestSessions.java:115)
    at org.openqa.grid.internal.DefaultGridRegistry.getExistingSession (DefaultGridRegistry.java:387)
    at org.openqa.grid.web.servlet.handler.RequestHandler.getSession (RequestHandler.java:241)
    at org.openqa.grid.web.servlet.handler.RequestHandler.process (RequestHandler.java:123)
    at org.openqa.grid.web.servlet.DriverServlet.process (DriverServlet.java:85)
    at org.openqa.grid.web.servlet.DriverServlet.doGet (DriverServlet.java:63)
    at javax.servlet.http.HttpServlet.service (HttpServlet.java:687)
    at javax.servlet.http.HttpServlet.service (HttpServlet.java:790)
    at org.seleniumhq.jetty9.servlet.ServletHolder.handle (ServletHolder.java:865)
    at org.seleniumhq.jetty9.servlet.ServletHandler.doHandle (ServletHandler.java:535)
    [ Message content over the limit has been removed. ]
    at org.seleniumhq.jetty9.io.AbstractConnection$ReadCallback.succeeded (AbstractConnection.java:305)
    at org.seleniumhq.jetty9.io.FillInterest.fillable (FillInterest.java:103)
    at org.seleniumhq.jetty9.io.ChannelEndPoint$2.run (ChannelEndPoint.java:118)
    at org.seleniumhq.jetty9.util.thread.strategy.EatWhatYouKill.runTask (EatWhatYouKill.java:333)
    at org.seleniumhq.jetty9.util.thread.strategy.EatWhatYouKill.doProduce (EatWhatYouKill.java:310)
    at org.seleniumhq.jetty9.util.thread.strategy.EatWhatYouKill.tryProduce (EatWhatYouKill.java:168)
    at org.seleniumhq.jetty9.util.thread.strategy.EatWhatYouKill.run (EatWhatYouKill.java:126)
    at org.seleniumhq.jetty9.util.thread.ReservedThreadExecutor$ReservedThread.run (ReservedThreadExecutor.java:366)
    at org.seleniumhq.jetty9.util.thread.QueuedThreadPool.runJob (QueuedThreadPool.java:765)
    at org.seleniumhq.jetty9.util.thread.QueuedThreadPool$2.run (QueuedThreadPool.java:683)
    at java.lang.Thread.run (Thread.java:748)
| FAIL |
WebDriverException: Message: unknown error: session deleted because of page crash
from unknown error: cannot determine loading status
from tab crashed
  (Session info: chrome=79.0.3945.79)
```
解决方法：增加共享内存
虽然通过df -h 可以看到/dev/shm有7个多G的空间，但是容器默认配置共享空间是64M："default-shm-size": "64M",使用docker for python 可以通过添加--shm-size参数来修改,官方参数说明

```
shm_size (str or int) – Size of /dev/shm (e.g. 1G).

```
参考                                   
https://github.com/elgalu/docker-selenium/issues/20
https://www.zhihu.com/question/40125229?sort=created
https://blog.csdn.net/isea533/article/details/95197468
https://www.cnblogs.com/FZfangzheng/p/10851762.html
https://docker-py.readthedocs.io/en/stable/containers.html

Finally:
```
client.containers.run('selenium/hub', name=hub_name, detach=True, tty=True, stdin_open=True,publish_all_ports=True,shm_size='1G')
                               
```
###### 2. chrome远程下载

  解决：使用谷歌驱动获取远程下载路径文件列表
  
```
  from openapi_utils import split_order, init_forder
  from robot.libraries.BuiltIn import BuiltIn
  def get_downloaded_files():
      seleniumlib = BuiltIn().get_library_instance('SeleniumLibrary')
      driver = seleniumlib.driver
      if not driver.current_url.startswith("chrome://downloads"):
          driver.get("chrome://downloads/")

      return driver.execute_script( \
      "return downloads.Manager.get().items_   "
      "  .filter(e => e.state === 'COMPLETE')  "
      "  .map(e => e.filePath || e.file_path); " )
```

###### 3. 使用以上js下载脚本时一直提示这个错 

JavascriptException: Message: javascript error: Cannot read property 'get' of undefined
  (Session info: chrome=80.0.3987.122)
  
  原因是chrome版本的问题，可能是80版本不兼容，将版本将至79且禁止浏览器更新~