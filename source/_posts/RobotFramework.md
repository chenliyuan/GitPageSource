title: RT使用语法和技巧
author: 躲不掉的风
date: 2020-01-29 11:27:02
tags:
---
官方文档：https://robotframework.org/#/documentation

######  架构&变量
参考： https://www.jianshu.com/p/dc9dea0741d5
```
*** Variables ***
# 创建scalar变量
${NAME}             Robot Framework
${VERSION}         3.0
${ROBOT}            ${NAME}  ${VERSION}
${NULL_VAR}         # 空字符串被赋值给了${NULL_VAR}
${EXAMPLE}          This value is joined together with a space
...                 1122333333333333333333333333333333
${LONG_WORDS}       1111111111111111
# 创建列表变量
@{list_data}        1    2    3    4    5
@{list_data2}       a    b    c    d    e    f
#创建字典变量
&{dict_data}        a=b    e=w   y=1    asd=123
&{dict_data1}       a=1    b=2    c=3    d=4
...                 r=7    u=90    # 列表元素太长时使用... 分割

*** Test Cases ***
test_01
    log    ${NAME}
    log    ${VERSION}
    log   @{list_data} [0]
    log many    &{dict_data}[a]    &{dict_data1}[a]
    log    &{dict_data}[a]
```
###### 全局变量怎么用
问题：想执行一个文件夹下所有的suite，但是需要通过suite1的返回值供其他suite使用
1. 定义变量后，直接使用下面接收函数返回值，然后全局赋值？No

        ${download_dir}    校验已下载模板到本地
        set global variable    ${download_dir}
2. 差点放弃的时候，发现使用set variable赋值替代接收的函数会有效果

      
######  数字运算

自增运算
```
test_05
    :FOR   ${i}    IN RANGE     4
    \    ${i}    run keyword if   ${i}>2   evaluate    ${i}-1
```

数字运算不使用evaluate
如 ${i+1}

 range范围同python 比如range 4,则i 取值为0，1，2，3
###### 条件语句
```
*** Test Cases ***

test case8
    ${a}    Set variable    59
    run keyword if    ${a}>=90    log    优秀
    ...    ELSE IF    ${a}>=70    log    良好
    ...    ELSE IF    ${a}>=60    log    及格
    ...    ELSE    log    不及格
```
条件判断赋值
```
 ${ele}    run keyword if    ${i}<${pagescount}        Get WebElement    ${ele_path}
```
一个条件多个执行,注意AND大写
```
run keyword if    ${variable}   run keywords  点击按钮    ${homepage_ele}[vacationmode_on]    AND    获取文本    ${homepage_ele}
```
条件判断字符串不存在的时候,字符串变量必须外边必须加引号用
```
\    run keyword if    '${temp}'!=''   exit for loop
```
可以使用and和or连接。连接两个条件经常报错，可能是空格的问题，and两边必须保持一个空格
```
Run Keyword If   '${B_name}'=='${B}' and '${C}'== '0'
```
数字使用或操作
```
${0|0|0|1|0}
```
###### For循环

```
test case9
:FOR    ${i}    IN RANGE     1      ${trsize}
\    ${ele}    Catenate    SEPARATOR=${i+1}   css=tbody tr:nth-of-type(    )>.transaction-escrow-status
\    log    ${ele}
```
若for循环体中嵌套了条件判断则else前无需加\，否则报错
```
\    Run Keyword If    '${get_value}'!='register'    获取文本校验    ${ele}    ${get_value}
     ...    ELSE    获取文本校验  ${ele_register}    Register/Login
```
###### 字符串使用

- 三目运算
```
${seller}    set variable if    ${seller}    ${seller}    ${test_data}[seller][account]
```
- 移除部分字符串
```
${t}    remove string     ShopeePay.ps_reports_wallet_all.20191125_20191225.csv    .csv
```
```
${after}=     remove string     ${test}       ,     ${SPACE}
```
- 字符串连接 Catenate（与变量连接直接放一起也可以）
![upload successful](/images/pasted-79.png)

字符串转换成数字，可直接使用${${numstr}}或者int(${numstr})

- 字符串格式化

###### 列表操作

所有列表操作举例：cnblogs.com/ronyjay/p/11598107.html 

对列表进行遍历的时候IN后面需用@

如果通过“@{}”去定义列表的话，可以通过“log many”关键字进行打印 ,当然也可以选择不这么做默认还是用$
列表切片
```
${expectedlist}    get slice from list     ${expected}    1
```
列表元素是整形

	| ${ints} =   | Create List | ${1} | ${2} | ${3} |
    
列表列举定义的时候用@,传参的时候需要用$

```
*** Variables ***
@{sales_info}         Sales Info     價格及庫存     mass_update_sales

*** Test Cases ***
test_01
    我是关键词    ${sales_info}
    
*** keywords ***
我是关键词
    [Arguments]    ${x}
    log   ${x}[1]

```

若元组传参,这时候必须用@了，不然按照列表的方式认为只能传一个列表变量
```
test_01
    我是关键词   1   2   3    4

*** keywords ***
我是关键词
    [Arguments]    @{x}
    log   @{x}[1]
```
【总结】在RobotFramework中，标量指的是${}，链表指的是@{}，大括号中间的变量名如果是一样的，那么就是一个变量。区别是标量->整个值就会是一个整体，链表->代表多个值

###### 正则表达式
需要双杠
```
${rt}=    Get Regexp Matches        ${test}     \\$\\d,\\d+
should match regexp    ${get_amount}    \\$(\\d)*,?(\\d)+
```
模糊匹配
```
File Should Exist    /Users/liyuanchen/Downloads/weekly_report*

```
###### 其他关键字
- **Get WebElement 和Get WebElements的区别**：前者会中断执行，后者返回空。可使用来判断元素是否存在

![upload successful](/images/pasted-76.png)
```
${temp}    GetWebElements    css=.username
run keyword if  ${temp}   click button   css=.username
```

- 对输入框执行ENTER操作可使用**press keys**
```
press keys   css=.shopee-input__input    RETURN
```
- 生成指定数据范围的随机数字(包括左边，也包括右边)
```
${pagenum}    Evaluate    random.randint(1, ${pagescount+1})    random
log    ${pagenum}
```

- robot中，断言有时会失败，但不想影响后面语句的执行，这时候要用到

	Run Keyword And Continue On Failure
    
- 计算可使用Evaluate关键字

	${rt}    Evaluate    ${num}+1
	或${i+1} 
    
  ```
  ${if_visible_download}    Run_keyword_and_return_status    Element_Should_Contain    Xpath=//button[@class='shopee-button shopee-button--primary shopee-button--normal']    Download
  ```


- 其他：

	run keyword and return status
    
    Run Keyword And Ignore Error  handle alert
#可接收浏览器alert默认接收，即选择OK

-  不支持的操作

	循环嵌套、条件判断和循环直接组合(可通过定义关键字调用)
    
##### 为解决有些操作失败重新执行：
###### 1.添加关键字
```
若找不到元素则重新执行
    [Arguments]    ${locator}    ${keyword}
    ${ele}    get webelements    ${locator}
    run keyword unless     ${ele}     ${keyword}
```

######  2.case后添加teardown
```
03BlanceAccountCB
    [Documentation]  CB seller进入balance页面，能够展示cb卡片信息,同Bank页面
    [Tags]  all
    [Setup]    获取配置数据
    登录SellerCenter    seller=cbseller
    进入cb_balance模块
    comment  cb账号进如balance校验cb卡片信息
    校验CB卡片信息
    [Teardown]    run keyword if test failed    校验CB卡片信息
```
也可以放在setting中声明，调用关键字如teststop

```
*** Settings ***
Test Teardown     teststop
```
   3.登陆的时候经常不定时出现某些弹窗影响下一步的case？封装关键字轮询查找弹窗
登陆弹窗轮询查找
```
    [Arguments]    ${timeout}=5
    :FOR    ${i}    IN RANGE    ${timeout}
    \   ${servererror}    弹出框存在则点击    ${homepage_ele}[alert_servererror]     ${timeout}
    \   ${version}    弹出框存在则点击    ${login_ele}[version]     ${timeout}
    \   ${navigation}    弹出框存在则点击    ${login_ele}[navigation]     ${timeout}
    \   ${mainaccount_tips}    弹出框存在则点击    ${login_ele}[mainaccount_tips]     ${timeout}
    \   ${mainaccount_tips}    弹出框存在则点击    ${login_ele}[mainaccount_tips]     ${timeout}
    \   弹出框存在则点击    ${homepage_ele}[alert_servererror]     ${timeout}
    \   exit for loop if   ${${servererror}|${version}|${navigation}|${mainaccount_tips}|${mainaccount_tips}}==0
```

##### 窗口问题：
当有两个窗口如果选择新打开的窗口则用select window   NEW   老窗口换成MAIN，但是如果是三个窗口取中间的窗口呢？

使用Handles解决：

1、删除第三个窗口  
2、获取当前Handle,选择最后一个（实践证明，不可用New，若使用可能会认为被关掉的那个才是new，故不生效）
```
${handels_bf}    get window handles
close window
${handels}    get window handles
${currenthd}    get from list    ${handels}    -1
select window    ${currenthd}
```

#### 多个元素校验--使用或判断


    ${status1}   run keyword and return status     wait until element is visible    ${homepage_ele}[button_in_market_continue]
    ${status2}   run keyword and return status     wait until element is visible  ${homepage_ele}[vari_marketingcentre]
    校验为真  ${status1} or ${status1}
    
或

    校验包含任何一个值
        [Arguments]    ${str}    @{veluelist}
        run keyword and continue on failure    should contain any    ${str}    @{veluelist}

####  定位元素至可见

    execute javascript    document.documentElement.scrollTop=200
    
  若是向上滚动，则数值使用负数