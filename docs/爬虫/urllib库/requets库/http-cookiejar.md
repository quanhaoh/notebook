#### session
通过在服务器端记录的信息确认用户的身份

#### cookie
通过客户端记录的信息确认用户身份

Cookie为http请求报头的属性，包括：
- Name：Cookie名字
- Value：Cookie值
- Expires/Max-Age：Cookie的过期时间
- Path：Cookie作用路径
- Dmain：Cookie所在域名
- Secure：使用Cookie进行安全连接

### http-cookiejar
处理HTTP cookies的模块

##### cookiejar.LoadError

返回FileCookieJar类在从文件中读取cookie时抛出的异常

##### 类：

###### 1、cookiejar.CookiePolicy  

决定cookie是否从服务器端接受/返回给服务器

###### 2、cookiejar.DefaultCookiePolicy(blocked_domains=None,allowed_domains=None,...)

###### 3、cookiejar.CookieJar(policy=None) 

从HTTP请求中提取cookie，在HTTP响应中返回cookie

==CookieJar对象的方法==：

- extract_cookies(response, request)
> 从HTTP响应中提取cookie，存在CookieJar中

- clear([domain[, path[, name]]])
> 根据三个参数，删除CookieJar中对应的cookie

- clear_session_cookies()
> 丢弃所有session cookies

###### 4、FIleCookieJar(filename, delayload=None, policy=None)

在文件中读取、保存cookie

==FIleCookieJar对象的方法==：

- save(filename=None,ignore_discard=False, ignore_expires=False)
> 保存cookie到文件中，覆盖式。
> ignore_discard和ignore_expires判断是否保存丢弃、过期的cookie

- load(filename=None,ignore_discard=False, ignore_expires=False)
> 从文件中读取cookie，旧cookie仍保存直到被新的cookie覆盖

- revert(filename=None,ignore_discard=False, ignore_expires=False)
> 清除旧cookie，从文件中读取新的cookie

###### 5、MozillaCookieJar(filename, delayload=None, policy=None)

FileCookieJar的子类。在Mozilla cookies.txt文件中读取、保存cookie

###### 6、LWPCookieJar(filename, delayload=None, policy=None)

FileCookieJar的子类