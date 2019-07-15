title: Django模板template
author: 躲不掉的风
date: 2019-07-14 08:43:07
tags:
---
1. 模板标签
  - 循环标签（for标签）   
  这种适合返回的字典值是列表的情况
  ```
  {%for value in value_list %}
  	{{value}}
  {%endfor%}
  ```
  for遍历 
  除了普通列表遍历还可以实现:  
    - 对列表进行翻转
    ```
    {% for obj in list reversed%}
    ```
    - 循环遍历字典
    ```
    {%for key ,value in data.items %}
    		{{key}}:{{value}}
    {%endfor%}
    ```
  - 条件控制标签
  ```
  {% if condition1 %}
	...
{% elif condition2 %}
	...
{% else %}
	...
{% endif %}
  ```
 2. 过滤器  
  在原来值后面加上过滤的关键字，起到过滤效果
  ```
  {{somekeyname|upper|first|lower|length}}
  常用的过滤器：
  capfirst首字母大写
  cut：删除指定值，如{{value|cut:""}}
  date:格式化日期，如{{value|date:"D d M Y"}}
  default
  join拼接如{{value|join:"//"}}
  truncatewords 缩短内容长度
  ```