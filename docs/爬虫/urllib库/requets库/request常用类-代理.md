### urllib.request.ProxyHandler(proxies=None)

设置代理服务器

**1、参数**
- proxies：字典
> proxies字典形式为{'http':"http://proxie_id:port"}
```
{'http': '114.239.145.137:808',
'https': '123.163.97.29:9999	'}
```


### urllib.request.OpenerDirector

与BaseHandlers链一起打开URL

**1、OpenerDirector的方法**

- add_handler(handler)
- open(url, data=None[, timeout])
> 与urlopen用法相同，ulropen本质上是特殊的open方法。  
> 往后会按顺序调用handler链的多个方法，如set_proxy()、default_open(req)、protocol_open(req)、protocol_request(req)、http_error_nnn(req, fp, code, msg, hdrs)等
- error(proto, *args)

### urllib.request.BaseHandler
所有已注册处理程序的基类



### 使用代理请求URL

思路
1. 使用类ProxyHandle创建对象，传入代理
2. 使用方法build_opener()创建自定义opender对象
3. 使用自定义的opener对象，发送请求
- 方法1：直接调用open方法发送请求
```
proxy = "114.239.145.137:808"
proxy_support=urllib.request.ProxHandler({'http':proxy})
opener = urllib.request.build_opener(proxy_support)
response = opener.open(url)
```

- 方法2：使用方法install_opener()将自定义的opener对象定义为全局opener，之后使用urlopen，意味着都使用这个全局opener

```
proxy="114.239.145.137:808"
# Build ProxyHandler object by given proxy
proxy_support=urllib.request.ProxyHandler({'http':proxy})
# Build opener with ProxyHandler object
opener = urllib.request.build_opener(proxy_support)
# Install opener to request
urllib.request.install_opener(opener)
# Open url
r = urllib.request.urlopen('http://icanhazip.com',timeout = 1000)
```
