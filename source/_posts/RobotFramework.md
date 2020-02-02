title: RT使用语法记录
author: 躲不掉的风
date: 2020-01-29 11:27:02
tags:
---
##### 语法记录
官方文档：https://robotframework.org/#/documentation
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
三目运算
```
${seller}    set variable if    ${seller}    ${seller}    ${test_data}[seller][account]
```
移除部分字符串
```
${t}    remove string     ShopeePay.ps_reports_wallet_all.20191125_20191225.csv    .csv
```
```
${after}=     remove string     ${test}       ,     ${SPACE}
```
字符串连接 Catenate（与变量连接直接放一起也可以）
![upload successful](/images/pasted-79.png)

字符串转换成数字，可直接使用${${numstr}}或者int(${numstr})
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