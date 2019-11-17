title: selenium实现web自动化
author: 躲不掉的风
date: 2018-09-24 23:27:30
tags:
---
#### web自动化
- pom模式：pageobjectmodel我理解是底层逻辑用例的分层

      1.底层封装：即driver层的封装如常用的定位元素方法、输入、键盘鼠标、切换frame以及测试报告HTMLTestRunner模板等

      2.page层：根据页面，写页面测试用例如输入登录名，输入密码，点击登录，获取信息后断言

      3.用例层：利用unittest框架，结合page页面功能结合生成一条条case
      继承TestCase基类创建用例，方法名均以test开头

- 每个测试的关键是：调用assertEqual()来检查预期的输出；assertTure或assertFalse()来验证一个条件，调用assertRaise()抛出特定异常。从而产生最终结果报告。

- unittest.main()提供了一个命令行接口

- setUp()方法前置方法，teatDown()方法用于最后的清理工作。每个测试用例(方法)运行时这两个方法以及__init__()都会被调用一次，setUpclass()和tearDownClass()在所有用例执行前后分别执行。 

- 跳过测试：@unittest.skip(reason)

  跳过被此装饰器装饰的测试。 reason 为测试被跳过的原因。

#### 1.环境搭建 
安装python环境，使用

  ```
  pip install selenium  
  pip show selenium   #验证

  ```
下载对应浏览器版本驱动 http://npm.taobao.org/，
配置该驱动（可执行文件）到环境变量。完成后执行代码：
```
#coding=utf-8 
from selenium import webdriver
driver = webdriver.Chrome() 
driver.get("http://www.baidu.com")
driver.find_element_by_id("kw").send_keys("Selenium2") 
driver.find_element_by_id("su").click() 
driver.quit()
```
若正常运行则成功。  
#### 2.主要语法
  ##### 2.1  定位页面元素的方法

          find_element_by_id() 通过元素的id来查找元素
          find_element_by_name() – 通过元素的name来查找元素
          find_element_by_class_name() – 通过class 来查找
          find_element_by_tag_name() – 通过元素的类型来查找，一般不用这种方式
          find_element_by_link_text() – 通过链接地址来查找元素

          find_element_by_partial_link_text()
          find_element_by_xpath() – 通过xpath来查找元素
          find_element_by_css_selector() – 通过css样式来查找元素
          
 若需定位一组元素:

 ```
 #coding=utf-8 
 from selenium import webdriver 
 import os
driver = webdriver.Firefox() 
file_path = 'file:///' + os.path.abspath('checkbox.html') 
driver.get(file_path)
#通过XPath找到type=checkbox的元素 
#checkboxes = driver.find_elements_by_xpath("//input[@type='checkbox']")
#通过CSS找到type=checkbox的元素 
checkboxes = driver.find_elements_by_css_selector('input[type=checkbox]') 
for checkbox in checkboxes: 
	checkbox.click()
# 打印当前页面上type为checkbox的个数 
print len(checkboxes)
# 把页面上最后1个checkbox的勾给去掉 driver.find_elements_by_css_selector('input[type=checkbox]').pop().click()
driver.quit()
 ```
【注意点】  
 1. 查找元素的两种方式对比   
 2. pop使用，pop(1)是第二个

##### 2.2  元素常用操作

          clear() – 清除元素内容，一般是清除输入框中的数据
          send_keys() – 在元素中模拟按键输入
          click() – 点击元素
          submit()  -- 提交表单
##### 2.3 获取元素信息
    size：元素的大小
    text：元素内文本

    is_displayed( )  ：元素是否可见
    is_enabled()： 元素是否可用（一般用于判断按钮是否置灰）
    is_selected( ) ： 元素是否被选中（一般用于表单中的单选框和复选框）
    get_attribute ( ) ： 元素的属性（可以获取到所选标签内的属性信息）


##### 2.4 浏览器操作和属性
		操作方法：    
        back（） 返回上一个页面
        forward（）进入下一个页面
        close（）关闭当前标签页
        quit（）关闭浏览器
        set_window_size(480,800) 设置浏览器大小（传参输入浏览器长、宽）
        maximize_window()  最大化浏览器
        refresh()  刷新页面

        属性：
          current_url :当前页面的URL路径
          title：当前页面的title名称
          name：当前浏览器名称
          page_source：当前html页面源码

        
        
![upload successful](\images\pasted-71.png)
参考：https://blog.csdn.net/CCGGAAG/article/details/75309315
##### 2.4 鼠标事件  ActionChains  

使用前引入模块：from selenium.webdriver.common.action_chains import ActionChains


      ActionChains 类提供的鼠标操作的常用方法：
       perform() 执行所有 ActionChains 中存储的行为  
       context_click() 右击 
       double_click() 双击 
       drag_and_drop() 拖动 
       move_to_element() 鼠标悬停
举例：
```
#引入ActionChains类 from selenium.webdriver.common.action_chains import ActionChains
...
#定位到要右击的元素 
right_click =driver.find_element_by_id("xx") #对定位到的元素执行鼠标右键操作 
ActionChains(driver).context_click(right_click).perform() 
```
##### 2.5 键盘事件
   引入模块 from selenium.webdriver.common.keys import Keys

      fromselenium.webdriver.common.keys importKeys
      在使用键盘按键方法前需要先导入 keys 类包。
      下面经常使用到的键盘操作：
      send_keys(Keys.BACK_SPACE) 删除键（BackSpace）
      send_keys(Keys.SPACE) 空格键(Space)
      send_keys(Keys.TAB) 制表键(Tab)
      send_keys(Keys.ESCAPE) 回退键（Esc）
      send_keys(Keys.ENTER) 回车键（Enter）
      《Selenium2Python 自动化测试实战》样张
      81
      send_keys(Keys.CONTROL,'a') 全选（Ctrl+A）
      send_keys(Keys.CONTROL,'c') 复制（Ctrl+C）
      send_keys(Keys.CONTROL,'x') 剪切（Ctrl+X）
      send_keys(Keys.CONTROL,'v') 粘贴（Ctrl+V）
      send_keys(Keys.F1) 键盘 F1
      ……
      send_keys(Keys.F12) 键盘 F12
      
##### 2.6 切换frame

- 切换到一个frame中

      switch_to_frame(frame_reference)

      其中frame_reference：id、name、element(定位的某个元素)、索引
    
常用实例 ：   
      # 定位父类层级iframe
      ele_framest = driver.find_element_by_css_selector("#result > iframe")
      # 切换到父类层级iframe-通过元素切换
      driver.switch_to_frame(ele_framest)
      或
       driver.switch_to_frame(1)


- 切换到主界面

      switch_to_default_content()：
    注：driver.switch_to_frame(None)等同于driver.switch_to_default_content()
#### 2.7 下拉框
需引用：from selenium.webdriver.support.ui import Select

![upload successful](\images\pasted-72.png)

select包内的方法详解

1.获取option元素

    options：获取包含select下拉框内所有option项element的列表

    all_selected_options: 获取当前选中项element的列表

    first_selected_option:获取所有下拉选项中的第一个选项的element（或者获取当前选中的这一项）

 

2.选择option

    select_by_value(values)：选择option标签中value属性为：values的选项

    select_by_index(index_number)：选择索引为index_number的选项（索引从0开始）

    select_by_visible_text(text)：选择option选项内容为：text的选项

 

3.复选select的情况（select标签中，multiple="multiple"时，即可多选的select选择框）

    deselect_all: 取消所有已选择的选项

    deselect_by_value(values):取消选择option标签中value属性为：values的选项

    deselect_by_index(index_number)：取消选择索引为index_number的选项（索引从0开始）

    deselect_by_visible_text(text)：取消选择option选项内容为：text的选项

实例片段：

    options=Select(ele_select).options #定位select元素
    for option in options:
        print(option.text)
    Select(ele_select).select_by_value("2") #选择值是2的项
    sleep(1)
    first_selected_option=Select(ele_select).first_selected_option
    print(first_selected_option.text)#输出第一个选中项文案

##### 2.8 等待
具体参考原文：https://blog.csdn.net/CCGGAAG/article/details/86763952
  - sleep是强制等待   
  - implicitly_wait()隐形等待 也称为全局等待 

    隐形等待是设置了一个最长等待时间，如果在规定时间内网页加载完成，则执行下一步，否则一直等到时间截止，然后执行下一步。注意这里有一个弊端，那就是程序会一直等待整个页面加载完成，也就是一般情况下你看到浏览器标签栏那个小圈不再转，才会执行下一步

            driver.implicitly_wait(30)  # 隐性等待，最长等30秒

 
  - WebDriverWait() 显示等待 

    满足条件后继续执行，否则在设置时间过后抛出异常，一般由 until()（或 until_not()）方法配合使用。具体格式如下： 
		WebDriverWait(driver, timeout, poll_frequency=POLL_FREQUENCY, ignored_exceptions=None)
        
    - driver 所创建的浏览器driver

    - timeout 最长时间长度（默认单位：秒）
    - poll_frequency 间隔检测时长（每）默认0.5秒
    - ignored_exceptions  方法调用中忽略的异常，默认只抛出：找不到元素的异常

    大部分返回True/False，极少数返回元素:
```
      #需导入库
      from selenium.webdriver.support.ui import WebDriverWait
      ...
      #部分关于元素定位的判断返回类型为：元素
      assert_ele = WebDriverWait(driver, 15, 0.5).until(Expect.presence_of_element_located(("id", "wd1")))
      print("返回类型 %s" % assert_ele)
```
   
#### 2.9 until / until_not
直到调用的方法返回值为True

until(method, message='')

- method：expected_conditions库中定义的方法
- message ：自定义报错信息
直到调用的方法返回值为False

until_not(method, message='')

- method：expected_conditions库中定义的方法
- message ：自定义报错信息

#### 2.10 expected_conditions库title_is(title)

    title：期望的页面标题
    判断当前页面标题是否包含title
    title_contains(title)

    title：期望的页面标题
    判断此定位的元素是否存在
    presence_of_element_located(locator)

    locator：元素的定位信息
    判断页面网址中是否包含url
    url_contains(url)

    url：期望的页面网址
    判断页面网址是否为url
    url_to_be(url)

    url：期望的页面网址
    判断页面网址不是url
    url_changes(url)

    url：期望的页面网址
    判断此定位的元素是否可见
    visibility_of_element_located(locator)

    locator：元素的定位信息
    判断此元素是否可见
    visibility_of(element)

    element：所获得的元素
    判断此定位的一组元素是否至少存在一个
    presence_of_all_elements_located(locator)

    locator：元素的定位信息
    判断此定位的一组元素至少有一个可见
    visibility_of_any_elements_located(locator)

    locator：元素的定位信息
    判断此定位的一组元素全部可见
    visibility_of_all_elements_located(locator)

    locator：元素的定位信息
    判断此定位中是否包含text_的内容
    text_to_be_present_in_element(locator, text_)

    locator：元素的定位信息
    text_：期望的文本信息
    判断此定位中的value属性中是否包含text_的内容
    text_to_be_present_in_element_value(locator, text_)

    locator：元素的定位信息
    text_：期望的文本信息
    判断定位的元素是否为frame，并直接切换到这个frame中
    frame_to_be_available_and_switch_to_it(locator)

    locator：元素的定位信息
    判断定位的元素是否不可见
    invisibility_of_element_located(locator)

    locator：元素的定位信息
    判断此元素是否不可见
    invisibility_of_element(element)

    element：所获得的元素
    判断所定位的元素是否可见且可点击
    element_to_be_clickable(locator)

    locator：元素的定位信息
    判断此元素是否不可用
    staleness_of(element)

    element：所获得的元素
    判断该元素是否被选中
    element_to_be_selected(element)

    element：所获得的元素
    判断定位的元素是否被选中
    element_located_to_be_selected(locator)

    locator：元素的定位信息
    判断该元素被选中状态是否和期望状态相同
    element_selection_state_to_be(element,Boolean)

    element：所获得的元素
    Boolean：期望的状态（True/False）
    判断定位的元素被选中状态是否和期望状态相同
    element_located_selection_state_to_be(locator,Boolean)

    locator：元素的定位信息
    Boolean：期望的状态（True/False）
    判断当前浏览器页签数量是否为num
    number_of_windows_to_be(num)

    num：期望的页签数量
    判断此handles页签不是唯一打开的页签
    new_window_is_opened(handles)

    handles：页签
    判断是否会出现alert窗口警报
    alert_is_present()
 
 示例：
     # WebDriverWait大部分方法返回类型为True\False
    assert_judge = WebDriverWait(driver, 15, 0.5).until_not(Expect.title_is("网易"))
    print("返回类型 %s" % assert_judge)

    # 判断失败后会报错
    assert_judge = WebDriverWait(driver, 5, 0.5).until(Expect.title_is("网易"), "错误信息：网页标题不是网易")





- 截图
		except : 
			driver.get_screenshot_as_file("D:\\baidu_error.jpg") 

- 基础api汇总

      #导入selenium
      from selenium import webdriver
      #创建chrome驱动实例,打开浏览器
      driver=webdriver.Chrome()
      #浏览器最大化
      driver.maximize_window()
      #浏览器最小化
      driver.minimize_window()
      #获取浏览器当前窗口大小
      size=driver.get_window_size()
      #设置浏览器窗口大小
      driver.set_window_size(400,400)
      #打开指定网页
      driver.get("http://www.so.com")
      #获取当前页面的链接地址
      url=driver.current_url
      driver.get("http://baike.so.com")
      #后退
      driver.back()
      #前进
      driver.forward()
      #浏览器退出
      driver.close()
      driver.quit()
      #截图
      driver.get_screenshot_as_png()
      driver.get_screenshot_as_base64()
      driver.get_screenshot_as_file("filename")
      driver.save_screenshot("filename")
      #切换到当前被操作元素
      ele=driver.switch_to.active_element
      #切换alert、confirm、prompt框
      alert = driver.switch_to.alert
      #切换到默认页面
      driver.switch_to.default_content()
      #切换iframe
      driver.switch_to.frame('frame_name')
      driver.switch_to.frame(1)
      driver.switch_to.frame(driver.find_elements_by_tag_name("iframe")[0])
      driver.switch_to.parent_frame()
      #获取浏览器所有句柄
      handles=driver.window_handles
      #获取当前句柄
      current_handle=driver.current_window_handle
      driver.switch_to.window()
      #执行js脚本
      driver.execute_script('script')
- 什么是Page Object模型

  创建一个PO模型对应创建页面上一个应用，该页面上的操作和熟悉可以基于这个模型创建对应的页面模型，这样可以减少代码冗余，增强代码的可读性和可维护性。
  
#### 3. 常见问题
###### 3.1. 定位元素时候报错or无法正确定位？

解决：
- 由于新页面没加载出来导致元素定位不到，可以加等待时间，推荐用try catch添加隐式等待。
- 查看元素外边是否被frame包围，若有则先切换到该frame下再定位元素
###### 3.2. css定位class元素报错？
需要注意的是：如果 class属性值 里带空格，用.来代替空格(下例中第二个点)
		driver.find_element_by_css_selector(".search-btn.tjclick").click()
###### 3.3. element click intercepted: Element <span class="textC">...</span> is not clickable at point (453, 328). Other element would receive the click: ......
意思是有其他的元素掩盖了需要点击的元素
解决：

        element = driver.find_element_by_css('div[class*="loadingWhiteBox"]')
        driver.execute_script("arguments[0].click();", element)

        element = driver.find_element_by_css('div[class*="loadingWhiteBox"]')
        webdriver.ActionChains(driver).move_to_element(element ).click(element ).perform()

原文链接：https://blog.csdn.net/WanYu_Lss/article/details/84137519