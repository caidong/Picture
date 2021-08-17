---
title: User-Agent中间件
date: 2018-6-25 10:51:21
categories: 
- Scrapy
tags: 
- Scrapy
---
1. 代码


```
class my_useragent(object):
    def process_request(self, request, spider):
        USER_AGENT_LIST = [
        Mozilla/5.0(Macintosh;U;IntelMacOSX10_6_8;en-us)AppleWebKit/534.50(KHTML,likeGecko)Version/5.1Safari/534.50,
        Mozilla/5.0(Windows;U;WindowsNT6.1;en-us)AppleWebKit/534.50(KHTML,likeGecko)Version/5.1Safari/534.50
        ]
        agent = random.choice(USER_AGENT_LIST)
        request.headers['user-agent'] = agent 
```
2. 在settings.py 启用
 
    DOWLOAD_MIDDLER = {
        module_name:priority_level
} 
