### urllib.request.HTTPCookieProcessor(cookiejar=None)
处理HTTP Cookies

在内存中读取、保存cookie
```
# 创建CookieJar()对象
cookiejar = cookiejar.CookieJar()
# 构建Cookie处理器对象，处理Cookie请求
cookie_handler = request.HTTPCookieProcessor(cookiejar)
# 构建自定义opener，发送请求后可以自动记录cookie到cookiejar
opener = request.build_opener(cookie_handler)
# 发送请求
opener.open("http://www.baidu.com/")
```

在文件中读取、保存cookie
```
filename = 'cookie.txt'
cookiejar = cookiejar.MozillaCookieJar(filename)
cookie_handler = request.HTTPCookieProcessor(cookiejar)
opener = request.build_opener(cookie_handler)
resp = opener.open("http://www.baidu.com")
cookiejar.save()
```


