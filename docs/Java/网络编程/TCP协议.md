## TCP协议

客户端和服务器通信需要4个IO流对象

### 1. 服务器

服务器使用客户端的IO流与客户进行交互

![image-20200407225357340](https://gitee.com/quanhaoh/blogImage/raw/master/img/20200407225648.png)

#### ServerSocket类

创建ServerSocket对象，表示开启一个服务（套接字），并等待客户端的连接

##### 构造方法

- `ServerSocket()`

创建未绑定的服务器套接字

- `ServerSocket(int port)`

创建绑定到指定端口的服务器套接字

##### 常用方法

- `Scoket accept()`

监听来自客户端的连接请求，返回客户端的套接字

### 2. 客户端

#### Socket类

创建Socket对象，表示向服务器发出连接请求，服务器响应请求则建立连接，开始通信

##### 构造方法

- `Socket(String host, int port)`

创建流套接字并将其连接到指定主机上的指定端口号

- `Socket(Proxy proxy)`

创建一个未连接的套接字，指定应该使用的代理类型

##### 常用方法

- `void close()`

关闭套接字

- `InputStream getInputStream()`

返回套接字的字节输入流

- `OutputStream getOutputStream()`

返回套接字的字节输出流