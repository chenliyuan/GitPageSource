title: RT框架简介
author: 躲不掉的风
date: 2020-04-24 17:49:49
tags:
---

参考链接：https://www.jianshu.com/p/dc9dea0741d5


#### robotframework目录文件结构

    robotframework是以project为单位进行管理的  
    一个project可以包含多个Test Suite
    一个Test Suite可以包含多个测试用例
    一个Test Suite有四部分组成：Settings、Variables、Test Cases、Keywords

#### 查看报告文件
    用例执行完毕后会生成三个文件分别是：log.html、output.xml、report.html
    
    output.xml：记录的测试结果是XML文件，根据特定的需要可以编写脚本读取XML文件并生成特定的测试报告
    log.html：会记录Robotframework运行的每一步操作，主要用于编写测试脚本的过程查看
    report,html：为测试报告，整理性的展示测试用例的运行情况
    
#### 结构及关键字

```
    *** Settings ***
    Documentation           这个是当前Test Suite说明文字
    Library                 当前Test Suite需要使用的库
    Resource                当前Test Suite需要加载使用的资源，可能是参数也可能是用例文件
    Metadata                定义元数据
    Variables               引用变量文件
    Suite Setup             Test Suite执行前的动作
    Suite Teardown          Test Suite执行后的动作
    Test Setup              Test Case执行前的动作
    Test Teardown           Test Case执行后的动作
    Force Tags              Test Suite下的所有测试用例都会被打上这个tag
    Default Tags            Test Suite的用例如果没有打上tag，就会用这个默认tag，如果打了tag，就会用自己的tag
    Test Timeout            设置每一个测试用例的超时时间，只要超过这个时间就会失败，并停止案例运行
    ...                     这是防止某些情况导致案例一直卡住不动，也不停止也不是失败
    Test Template           数据驱动模板（很有用的一个参数）


    *** Variables ***
    ${SCALAR_VARS}          创建scalar参数
    @{LIST_VARS}            创建list参数
    &{DICT_VARS}            a=创建dictionary参数


    *** Test Cases ***
    Test_01
        [Documentation]     测试用例说明……
        [Template]          数据驱动模板，每条用例只有一个模板
        [Tags]              测试用例标签
        [Setup]             测试用例执行前的动作
        [Teardown]          测试用例执行后的动作
        [Timeout]           测试用例的超时时间
        My Keyword One  

    Test_02
        [Documentation]     。。。
        [Template]          。。。
        [Tags]              。。。
        [Setup]             。。。
        [Teardown]          。。。
        [Timeout]           。。。
        My Keyword Two


    *** Keywords ***
    My Keyword One
        [Documentation]     关键字描述
        [Arguments]         自定义参数设置
        [Return]            将返回值抛出
        [Timeout]           关键字流程执行超时时间
        [Tags]              标签
        [Teardown]          关键字流程结束动作
        log                 ${SCALAR_VARS}
        log Mang            @{LIST_VARS}
        log                 ${DICT_VARS}

    My Keyword Two
        log                 ${SCALAR_VARS}
        log Mang            @{LIST_VARS}
        log                 ${DICT_VARS}
```