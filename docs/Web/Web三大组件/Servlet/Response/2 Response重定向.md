## Response重定向 

### 设置响应消息

1. 设置响应行
   1. 格式：HHTP/1.1 200 ok
   2. 设置状态码：setStatus(int sc)
2. 设置响应头：setHeader(String name, Stiing value)
3. 设置响应体：
   1. 获取输出流
      1. 字符输出流：PrintWriter getWriter()
      2. 字节输出流：ServletOutputStream getOutPutStrem()
   2. 使用输出流，将数据输出到客户端

### 实现重定向

1. 方法1：
   - response.setStatus(302)
   - response.setHeader("location", String path)
2. 方法2
   - response.setRedirect(String path)

#### 重定向的特点

- 客户端浏览器地址栏会发生变化
- 可以访问其他站点的资源
- 重定向是两次请求，不能使用request对象来共享数据

#### 转发的特点

- 客户端浏览器地址栏不变
- 转发只能访问当前服务器的资源
- 转发是一次请求，可以共享数据

## 常见的面试题，即说明redirect和forward的区别