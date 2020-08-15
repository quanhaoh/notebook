### urllib.request.urlopen(url, data=None, [timeout, ]*, cafile=None, capath=None, cadefault=False, context=None) 

请求URL，读取并返回对象

**1、参数**
- url：URL字符串/Request对象

- data：二进制数据/可迭代对象  
> 当有data参数时，此时为http的post请求，否则为get请求。仅适用于http请求

- timeout：数字
> 以second为单位，定时阻塞（断开）如连接请求等，连接超时则返回异常。使用默认时，将使用全局定义的timeout设置

- calife、capath、cadefault、context：在https请求时使用，与SSL相关  

**2、函数返回**  
- 返回对象可使用的方法：  
> geturl()，返回检索到的资源的URL，用于重定向  
> info()，返回一个httplib.HTTPMessage对象，表示远程服务器返回的头信息 
> getcode()，返回http状态码

- HTTP和HTTPS请求的返回对象：http.client.HTTPResponse对象
> read()、readline()、readlines()，读取并返回响应正文  
> getheader(name,defalut=None)，返回响应头name的值，若无则返回默认值None  
> getheaders()，返回(header,value)元组  
> status，返回http状态码  
> reason，返回连接错误原因
> closed()，若连接已关闭则返回TRUE
> version，返回服务器的http协议版本

- FTP请求的返回对象： urllib.response.addinfourl对象


### urllib.request.pathname2url(path)
将系统文件名转化为URL
### urllib.request.url2pathname(path)
将URL转化为系统文件名

### urllib.request.build_opener([handler, ...])
创建自定义的opener

### urllib.request.install_opener(opener)
将自定义的opner定义为全局opener，即可正常使用urlopen方法

### urllib.request.getproxies()



