title: python爬虫获取cookie
author: 躲不掉的风
date: 2020-04-21 17:52:54
tags:
---
https://www.cnblogs.com/sticker0726/articles/10703682.html
以上文章有多种方法，实践实现下面一种：

```

import requests

url = 'https://www.processon.com/login'
login_email = '286933867@qq.com'
login_password = 'ZZZ05382881391'
# 创建一个session,作用会自动保存cookie
session = requests.session()
data = {
    'login_email': login_email,
    'login_password': login_password
}
# 使用session发起post请求来获取登录后的cookie,cookie已经存在session中
session.post(url = url,data=data)

# 用session给个人主页发送请求，因为session中已经有cookie了
index_url = 'https://www.processon.com/diagrams'
index_page = session.get(url=index_url).text
print(index_page)
```