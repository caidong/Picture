---
title: Scrapy 获取浏览器Cookie中间件
date: 2018-08-17 23:12:43
tags:
- Scrapy
- 笔记
categories: 
- Scrapy
---
#### 1. 获取Cookie
Python库 browsercookie 可获取浏览器中Cookie
- 安装
```
pip3 install  browsercookie
pip3 install pycryptodome
```

访问浏览器Cookie
```
import browsercookie
# 获取chrome 的cookie
chrome_cookiejar = browsercookie.chrome()
firefox_cookiejar = browsercookie.firefox()
type(chrome_cookiejar)
http.cookiehar.CookieJar
for cookie in firefox_cookiejar:
    print(cookie)
```

实现BrowserCookieMiddleware
```
import browsercookie 
from scrapy.downloadermiddlerwares.cookies import CookiesMiddlerware
class BrowserCookieMiddleware(CookieMiddleware):
    def __init__(self,debug=False):
        super().__init__(debug)
        self.load_browser_cookies()
    
    def load_browser_cookies(self):
        # 加载Chrome 浏览器中的Cookie 
        jar =self.jars['chrome']
        chrome_cookiejar = browsercookie.chrome()
        for cookie in chrome_cookiejar:
            jar.set_cookie(cookie)
        # 加载 Firefox 浏览器中的cookie
        jar = self.jars['firefox'] 
        firefox_cookiejar =browsercookie.firefox()
        for cookie in firefox_cookiejar:
            jar.set_cookie(cookie)
            
            
setting.py中启用中间件
 DOWNLOADER_MIDDLEWARES ={
 #开启自定义中间件,关闭系统自带中间件
     'scrapy.downloadermiddlerwares.cookies.CookieMiddleware':None
     'browser_cookie.middlewares.BrowserCookiesMiddleware':701,
 }
 
 构建 Request对象时
 Request(url, meta={
     'cookiejar':'chrome'
 })
```

