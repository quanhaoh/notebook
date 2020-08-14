### error库包含了由urllib.request请求引起的异常

#### error.URLError

OSError的子类

**方法**
- reason
> 返回错误的具体原因


#### error.HTTPError

URLError的子类

**方法**
- code
> 返回HTTP状态码

- reason
> 返回错误的具体原因

- headers
> 返回引起错误的服务器响应头部信息


#### error.ContentTooShortError(msg, content)

当服务器响应的data小于所期待的数据量时，会引起该异常