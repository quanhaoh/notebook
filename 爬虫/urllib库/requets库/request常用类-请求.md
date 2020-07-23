### urllib.request.Request(url, data=None, headers={}, origin_req_host=None, unverifiable=False, method=None)

抽象的URL请求  

**1、参数**  
- url：URL字符串

- data：二进制数据/可迭代对象  
> 当有data参数时，此时为http的post请求，否则为get请求。仅适用于http请求 

- headers：字典
> 带有User-Agent等请求头数据

- methond:如GET、POST、HEAD等
> 选择http的请求方法      [详细教程链接](https://www.runoob.com/http/http-methods.html)

- origin_req_host：
> 请求的原始主机

- unverifiable：False/True
> 表明请求是否是RFC 2965定义的无法验证的 
 

**2、Request对象的方法**

- full_url
> 返回传入Request对象的完整url

- get_full_url()
> 返回传入Request对象的完整url

- type
> 返回url的类型，如https、http

- host
> 返回url的主机名host部分，如 www.baidu.com，可能会包含端口号port

- origin_req_host
> 返回url的主机名host部分，如 www.baidu.com，不包含端口号port

- data
> 返回请求的主体，否则为None

- unverifiable
> 返回True/False

- get_method()
> 返回本次http请求的方法

- add_header(key, val)
> 添加http请求头

- add_unredirected_header(key, header)
> 添加不会用于重定向的http请求头

- has_header(header)
> 判断是否有请求头header

- remove_header(header)
> 移除请求头header

- .get_header(header_name, default=None)
> 返回header_name的value值，若无则返回默认值None

- header_items()
> 返回所有(header_name, header_value)，以元组的形式

- set_proxy(host, type)
> 设置代理服务器来请求  
```
Request.set_proxy('122.227.139.170:3128','http')
```

- Request.selector
> 返回URI路径；当请求使用代理时，将返回发送给代理服务器的URL
