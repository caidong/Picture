---
title: 代理IP中间件
date: 2018-6-22 10:51:21
categories: 
- Scrapy
tags: 
- Scrapy
- 代理
---
1. 代码
 ```
 class my_proxy(object):
    def process_request(self,request,spider):
        request.meta['proxy'] = 'ip:port'
        proxy_name_pass = [name:pwd]
        encode_pass_name = base64.b64encode(proxy_name_pass)
        request.heades['proxy-Authorization'] = 'Basic' + encode_pass_name.decode() 
        
 ```
 2. settings.py 文件中开启
 
    DOWLOAD_MIDDLER = {
            module_name:priority_level
        }
